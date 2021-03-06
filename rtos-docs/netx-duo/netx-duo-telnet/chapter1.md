---
title: Глава 1. Введение в NetX Duo Telnet для ОСРВ Azure
description: NetX Duo Telnet для ОСРВ Azure — это протокол, предназначенный для передачи команд и ответов между двумя узлами в Интернете.
author: philmea
ms.author: philmea
ms.date: 06/04/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 50a2c4d8726863f4e609debadd9ddf2455cfd540219a476970756d3d250ec562
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2021
ms.locfileid: "116799036"
---
# <a name="chapter-1---introduction-to-the-azure-rtos-netx-duo-telnet"></a>Глава 1. Введение в NetX Duo Telnet для ОСРВ Azure

NetX Duo Telnet для ОСРВ Azure — это протокол, предназначенный для передачи команд и ответов между двумя узлами в Интернете. Telnet — это простой протокол, в котором для выполнения функции передачи используются надежные службы протокола TCP. Поэтому Telnet является высоконадежным протоколом передачи данных. Telnet также является одним из наиболее используемых протоколов приложений.

## <a name="telnet-requirements"></a>Требования для работы Telnet

Для правильной работы пакета NetX Duo Telnet требуется, чтобы экземпляр IP NetX уже был создан. Кроме того, на этом же экземпляре IP должен быть включен протокол TCP. Часть клиента в пакете NetX Duo Telnet не имеет дополнительных требований.

Часть сервера в пакете NetX Duo Telnet имеет одно дополнительное требование. Ему требуется полный доступ к *хорошо известному порту TCP 23* для обработки всех клиентских запросов Telnet.

## <a name="telnet-constraints"></a>Ограничения Telnet 

Протокол NetX Duo Telnet реализует стандарт Telnet. Однако за интерпретацию и ответ команд Telnet, обозначенных байтом со значением 255, отвечает приложение. Различные команды Telnet и их параметры определены в файлах *nxd_telnet_client.h и nxd_telnet_server.h*.

## <a name="telnet-communication"></a>Взаимодействие по протоколу Telnet

Как упоминалось ранее, сервер Telnet использует *хорошо известный порт TCP 23* для клиентских запросов. Клиенты Telnet могут использовать любой доступный TCP-порт.

## <a name="telnet-authentication"></a>Проверка подлинности Telnet

Проверка подлинности Telnet отвечает за функцию обратного вызова сервера Telnet в приложении. Ответный вызов "новое подключение" к серверу Telnet-сервера обычно запрашивает у клиента имя и (или) пароль. Затем на клиента возлагается ответственность за предоставление этой информации. Затем сервер будет обрабатывать информацию в обратном вызове "получение данных". Именно здесь код сервера приложений должен будет проверить подлинность информации и решить, является ли она допустимой.

## <a name="telnet-new-connection-callback"></a>Обратный вызов нового подключения Telnet

Сервер NetX Duo Telnet вызывает указанную приложением функцию обратного вызова при получении нового запроса клиента Telnet. Приложение указывает функцию обратного вызова при создании сервера Telnet с помощью функции ***nx_telnet_server_create***. Типичные действия обратного вызова "новое подключение" включают отправку баннера или запроса клиенту. Это вполне может включать запрос информации для входа в систему.

### <a name="prototype"></a>Прототип

```c
void  telnet_new_connection(NX_TELNET_SERVER *server_ptr, 
                            UINT logical_connection);
```

### <a name="input-parameters"></a>Входные параметры

- **server_ptr**: указатель на вызывающий сервер Telnet.
- **logical_connection**: внутреннее логическое соединение для сервера Telnet. Этот параметр может использоваться приложением в качестве индекса в буферах и (или) структурах данных, специфичных для каждого клиентского соединения. Его значение лежит в диапазоне от 0 до NX_TELNET_MAX_CLIENTS - 1.

## <a name="telnet-receive-data-callback"></a>Обратный вызов получения данных Telnet

Сервер NetX Duo Telnet вызывает указанную приложением функцию обратного вызова при получении новых данных клиента Telnet. Приложение указывает функцию обратного вызова при создании сервера Telnet с помощью функции ***nx_telnet_server_create***. Типичные действия обратного вызова "получение данных" включают возврат данных обратно и (или) анализ данных и предоставление данных в результате интерпретации команды от клиента.

Обратите внимание, что эта подпрограмма обратного вызова также должна освобождать указанный пакет.

### <a name="prototype"></a>Прототип

```c
void  telnet_receive_data(NX_TELNET_SERVER *server_ptr, 
                          UINT logical_connection, NX_PACKET *packet_ptr);
```
### <a name="input-parameters"></a>Входные параметры

- **server_ptr**: указатель на вызывающий сервер Telnet.
- **logical_connection**: внутреннее логическое соединение для сервера Telnet. Этот параметр может использоваться приложением в качестве индекса в буферах и (или) структурах данных, специфичных для каждого клиентского соединения. Его значение лежит в диапазоне от 0 до NX_TELNET_MAX_CLIENTS - 1.
- **packet_ptr**: указатель на пакет, содержащий данные от клиента.

## <a name="telnet-end-connection-callback"></a>Обратный вызов завершения подключения Telnet

Сервер NetX Duo Telnet вызывает указанную приложением функцию обратного вызова при завершении подключения клиентом Telnet. Приложение указывает функцию обратного вызова при создании сервера Telnet с помощью функции ***nx_telnet_server_create***. Типичные действия обратного вызова "завершение подключения" включают очистку всех специфичных для клиента структур данных, связанных с логическим подключением.

### <a name="prototype"></a>Прототип
```c
void  telnet_end_connection(NX_TELNET_SERVER *server_ptr, 
                            UINT logical_connection);
```

### <a name="input-parameters"></a>Входные параметры

- **server_ptr**: указатель на вызывающий сервер Telnet.
- **logical_connection**: внутреннее логическое соединение для сервера Telnet. Этот параметр может использоваться приложением в качестве индекса в буферах и (или) структурах данных, специфичных для каждого клиентского соединения. Его значение лежит в диапазоне от 0 до NX_TELNET_MAX_CLIENTS - 1.

## <a name="telnet-option-negotiation"></a>Согласование параметров Telnet

Сервер NetX Duo Telnet поддерживает ограниченный набор параметров Telnet, Echo и Suppress Go Ahead.

Для работы этой функции параметр NX_TELNET_SERVER_OPTION_DISABLE не должен быть определен. По умолчанию он не определен. Сервер Telnet создает пул пакетов в службе *nx_telnet_server_create*, из которой он выделяет пакеты для отправки запросов параметров Telnet клиенту. Сведения о настройке полезных данных пакета (NX_TELNET_SERVER_PACKET_PAYLOAD) и размера пула пакетов (NX_TELNET_SERVER_PACKET_POOL_SIZE) для этого пула пакетов см. в разделе "Параметры конфигурации". Этот пул пакетов будет удален при вызове службы *nx_telnet_server_delete*.

При установлении соединения с клиентом Telnet он отправляет ему этот набор параметров, если не получил следующие запросы параметров от клиента:

- will echo
- dont echo
- will sga

При получении данных Telnet от клиента сервер Telnet проверяет, является ли первый байт кодом "IAC". В этом случае он обрабатывает все параметры пакета клиента. Параметры, отсутствующие в списке выше, игнорируются.

По умолчанию сервер Telnet создает собственный внутренний пул пакетов, если параметр NX_TELNET_SERVER_OPTION_DISABLE не определен и ему требуется передать команды параметров Telnet. Пул пакетов сервера Telnet определяется параметрами NX_TELNET_SERVER_PACKET_PAYLOAD и NX_TELNET_SERVER_PACKET_POOLSIZE. Однако, если определен параметр NX_TELNET_SERVER_USER_CREATE_PACKET_POOL, то приложение должно создать пул пакетов сервера Telnet и задать его в качестве пула пакетов сервера Telnet, вызвав *_nx_telnet_server_packet_pool_set*. Дополнительные сведения об этой функции см. в главе 3 "Описание служб Telnet".

В отличие от сервера NetX Duo Telnet, поток задач клиента NetX Duo Telnet не выполняет отправку и не реагирует на полученные параметры от сервера Telnet автоматически. Это должно делать клиентское приложение Telnet.

## <a name="telnet-multi-thread-support"></a>Поддержка нескольких потоков в Telnet

Клиентские службы NetX Duo Telnet можно вызывать из нескольких потоков одновременно. Однако запросы на чтение или запись для конкретного экземпляра клиента Telnet должны выполняться последовательно из одного потока.

## <a name="telnet-rfcs"></a>Документы RFC по Telnet

NetX Duo Telnet совместим с RFC854 и связанными документами RFC.
