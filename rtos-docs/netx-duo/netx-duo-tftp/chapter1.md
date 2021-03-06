---
title: Глава 1. Введение в NetX Duo TFTP для ОСРВ Azure
description: Протокол TFTP — это нетребовательный к ресурсам протокол, созданный для передачи файлов.
author: philmea
ms.author: philmea
ms.date: 06/04/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 4854f96d8f230d7c1f21700ac731d6430854a950009d3ed51fbf90d37885f255
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2021
ms.locfileid: "116791981"
---
# <a name="chapter-1---introduction-to-azure-rtos-netx-duo-tftp"></a>Глава 1. Введение в NetX Duo TFTP для ОСРВ Azure 

Протокол TFTP — это нетребовательный к ресурсам протокол, созданный для передачи файлов. В отличие от более надежных протоколов, TFTP не выполняет развернутую проверку ошибок и может иметь ограниченную производительность, так как работает в режиме остановки и ожидания. Отправив пакет данных TFTP, отправитель ожидает подтверждения от получателя. Хотя это очень простая операция, она ограничивает общую пропускную способность TFTP. Пакет TFTP позволяет узлам использовать протокол TFTP в IP-сетях.

## <a name="tftp-requirements"></a>Требования протокола TFTP

Для надлежащей работы часть TFTP-клиента в пакете NetX Duo TFTP требует, чтобы экземпляр IP был уже создан. Кроме того, в таком экземпляре IP должен быть включен протокол UDP. Для части клиента в пакете NetX Duo TFTP нет других требований.

Часть TFTP-сервера в пакете NetX Duo TFTP имеет несколько дополнительных требований. Во-первых, ему требуется полный доступ к *хорошо известному порту UDP 69* для обработки всех клиентских запросов TFTP. TFTP-сервер также разработан для работы с внедренной файловой системой FileX. Если система FileX недоступна, пользователь может портировать используемые разделы FileX в собственную среду. Этот процесс рассматривается в последующих разделах этого руководства.

## <a name="tftp-file-names"></a>Имена файлов TFTP 

Имена файлов TFTP должны иметь формат целевой файловой системы. Они должны быть строками символов ASCII с завершающим нулем и сведениями о полном пути, если это необходимо. В реализации NetX Duo TFTP ограничение на размер имен файлов TFTP отсутствует.

## <a name="tftp-messages"></a>Сообщения TFTP

TFTP имеет очень простой механизм для открытия, чтения, записи и закрытия файлов. Под заголовком UDP просто указываются 2–4 байта заголовка TFTP. Определение для сообщений открытия файла в TFTP имеет следующий формат:

**oooof…f0OCTET0**

Где:

- **oooo** — 2-байтовое поле Opcode.  
0x0001 -> открыть для чтения.  
0x0002 -> открыть для записи.

- **f…f** — n-байтовое поле с именем файла.

- 0 — 1-байтовый завершающий символ NULL.

- **OCTET** — OCTET ASCII для указания передачи в двоичном формате.

- 0 — 1-байтовый завершающий символ NULL.

Определение сообщений о записи, подтверждении и ошибке в TFTP немного отличаются и имеют такой вид:

**oooobbbbd…d**

Где:

- **oooo** — 2-байтовое поле Opcode.  
0x0003 -> пакет данных.  
0x0004 -> подтверждение последней операции чтения.  
0x0005 -> условие ошибки.  

- **bbbb** — 2-байтовое поле с номером блока (1–n).

- **d…d** — n-байтовое поле данных.


- 0x0001 (чтение) — имя файла 0 OCTET 0.

- 0x0002 (запись) — имя файла 0 OCTET 0.

## <a name="tftp-communication"></a>Обмен данными по TFTP

TFTP-серверы используют хорошо известный порт UDP 69 для прослушивания запросов клиентов. Сокеты TFTP-клиенты могут привязываться к любому доступному порту UDP. Полезные данные пакета, включающие файл для передачи или скачивания, отправляются фрагментами по 512 байт при этом последний пакет может содержать менее 512 байт. Поэтому пакет, содержащий менее 512 байт, обозначает конец файла. Общая последовательность событий выглядит следующим образом.

Запросы TFTP на чтение файлов:

1.  Клиент отправляет запрос открытия для чтения с именем файла и ожидает ответ от сервера.

2.  Сервер отправляет первые 512 байт файла (или менее, если размер файла не превышает 512 байт).

3.  Клиент получает данные, отправляет подтверждение и ожидает следующий пакет от сервера для файлов, содержащих более 512 байт.

4.  Последовательность заканчивается, когда клиент получает пакет, содержащий менее 512 байт.

Запросы TFTP на запись:

1.  Клиент отправляет запрос открытия для записи с именем файла и ожидает подтверждения с номером блока 0 от сервера.

2.  Если сервер готов к записи в файл, он отправляет подтверждение с нулевым номером блока.

3.  Клиент отправляет первые 512 байт файла (или меньше для файлов размером не более 512 байт) на сервер и ожидает подтверждения.

4.  Сервер отправляет подтверждение после записи байтов.

5.  Последовательность заканчивается, когда клиент завершает запись пакета, содержащего менее 512 байт.
 

## <a name="tftp-server-session-timer"></a>Таймер сеанса TFTP-сервера

TFTP-сервер имеет ограниченное количество слотов для запросов клиента. Если сеанс клиента завершается, такой слот не может быть использован повторно. Но если включен параметр NX_TFTP_SERVER_RETRANSMIT_ENABLE, TFTP-сервер NetX Duo создает таймер сеанса, который отслеживает время ожидания для каждого сеанса клиента. По истечении времени ожидания сеанс завершается, а все открытые файлы закрываются. После этого слот становится доступен другому запросу TFTP-клиента.

Чтобы задать время ожидания, измените параметр конфигурации NX_TFTP_SERVER_RETRANSMIT_TIMEOUT, для которого по умолчанию задано значение в 200 тактов таймера. Интервал для проверки времени ожидания сеанса задается параметром NX_TFTP_SERVER_TIMEOUT_PERIOD, который по умолчанию имеет значение в 20 тактов таймера.

Если TFTP-клиент передал (записал) файлы на носитель FileX TFTP, такой носитель нужно очистить, чтобы данные можно было записать с электронного диска TFTP-сервера на базовый носитель (дисковая память TFTP-клиента). Это можно сделать с помощью службы fx_media_flush, если приложение не требует закрыть TFTP-сервер.

Но после закрытия TFTP-сервера приложению нужно (если оно больше не будет использовать носитель FileX) закрыть носитель с помощью службы fx_media_close. Это приведет к записи данных обратно на диск TFTP-клиента, закрытию открытых файлов и обновлению сведений о каталоге для носителя.

Демонстрацию этого можно найти в разделе "Небольшой пример" этой главы.

Третий вариант обновления носителя FileX — это компиляция библиотеки FileX с параметрами FX_FAULT_TOLERANT или FX_FAULT_TOLERANT_DATA вместо явного использования служб FileX. Если эти параметры определены, FileX автоматически передает запросы на запись драйверу носителя. Такие параметры могут ограничить производительность, но обеспечивают повышенную защиту от потерь данных файлов или кластеров. Дополнительные сведения и общие данные о FileX см. в руководстве пользователя Express Logic FileX.

## <a name="tftp-multi-thread-support"></a>Поддержка нескольких потоков в TFTP

Службы TFTP-клиента NetX Duo можно вызывать из нескольких потоков одновременно. Однако запросы на чтение или запись для конкретного экземпляра TFTP-клиента должны выполняться последовательно из одного потока.

## <a name="tftp-rfcs"></a>Стандарты RFC для протокола TFTP

NetX Duo TFTP совместим с RFC 1350 и связанными стандартами RFC.

