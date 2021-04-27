---
title: Глава 1. Знакомство с FileX в ОСРВ Azure
description: FileX в ОСРВ Azure — это полноценная система управления файлами и носителями в формате FAT для глубоко внедренных приложений.
author: philmea
ms.author: philmea
ms.date: 05/19/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: be7e6f9cd9fbc69ac0908d1de733dac1c4f73bf6
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104814399"
---
# <a name="chapter-1---introduction-to-azure-rtos-filex"></a>Глава 1. Знакомство с FileX в ОСРВ Azure

FileX в ОСРВ Azure — это полноценная система управления файлами и носителями в формате FAT для глубоко внедренных приложений. В этой главе представлены основные сведения о FileX, в том числе области применения и преимущества.

## <a name="filex-unique-features"></a>Уникальные функции FileX

Компонент FileX в ОСРВ Azure поддерживает неограниченное количество устройств хранения данных одновременно, включая электронные диски, диспетчеры флэш-памяти и непосредственно физические устройства. Он поддерживает 12-, 16- и 32-битные форматы FAT, а также exFAT, непрерывное распределение файлов и хорошо оптимизирован как с точки зрения размера, так и с точки зрения производительности. FileX также включает в себя поддержку отказоустойчивости, функции открытия/закрытия носителя и обратного вызова записи файла.

Предназначенный для удовлетворения растущих потребностей в устройствах флэш-памяти, FileX использует те же методы проектирования и программирования, что и ThreadX. Как и все продукты Майкрософт, FileX распространяется с полным исходным кодом ANSI C и не имеет лицензионных отчислений за время выполнения.

### <a name="product-highlights"></a>Особенности продукта

- Полная поддержка процессора ThreadX.
- Отсутствие лицензионных отчислений.
- Полный исходный код ANSI C.
- Производительность в режиме реального времени.
- Быстрая техническая поддержка.
- Неограниченные объекты FileX (носители, каталоги и файлы).
- Динамическое создание или удаление объектов FileX.
- Гибкое использование памяти.
- Автоматическое масштабирование размера.
- Малый объем памяти (всего 6 КБ), занимаемой областью хранения команд: 6–30 КБ.
- Полная интеграция с ThreadX.
- Независимость от порядка байтов.
- Простые в реализации драйверы ввода-вывода FileX.
- Поддержка 12-, 16- и 32-разрядных систем FAT.
- Поддержка exFAT.
- Поддержка длинных имен файлов.
- Внутренний кэш записей FAT.
- Поддержка имен в Юникоде.
- Непрерывное распределение файлов.
- Последовательные чтение и запись сектора и кластера.
- Внутренний кэш логических секторов.
- Готовая демонстрация электронного диска.
- Возможность форматирования носителя.
- Обнаружение ошибок и восстановление.
- Функции отказоустойчивости.
- Встроенная статистика производительности.
- Поддержка автономной работы (без ОСРВ Azure).

## <a name="safety-certifications"></a>Сертификация по безопасности

### <a name="tv-certification"></a>Сертификация TÜV

Решение FileX сертифицировано организацией SGS-TÜV Saar для использования в критических для безопасности систем в соответствии со стандартами IEC-61508 и IEC-62304. Такая сертификация гарантирует, что FileX можно использовать при разработке программного обеспечения с особыми требованиями к высочайшим уровням защиты целостности в соответствии со стандартами IEC-61508 и IEC-62304 для "функциональной безопасности электрических, электронных и программируемых электронных систем, связанных с безопасностью". Компания SGS-TÜV Saar, основанная совместно немецкими компаниями SGS-Group и TÜV Saarland, стала ведущей аккредитованной и независимой компанией, выполняющей тестирование, аудит, проверку и сертификацию встраиваемого ПО для связанных с безопасностью систем. Соответствие промышленному стандарту безопасности IEC 61508 и всем его производным стандартам, в том числе IEC-62304, гарантирует функциональную безопасность электрических, электронных и программируемых электронных медицинских изделий, систем управления процессами, промышленного оборудования и систем управления железнодорожными перевозками с высоким уровнем защиты.

Компания SGS-TÜV Saar сертифицировала FileX для использования в критических для безопасности системах автомобилестроения в соответствии со стандартом ISO 26262. Более того, решение FileX сертифицировано согласно классификации Automotive Safety Integrity Level D (ASIL), которая соответствует высочайшему уровню сертификации ISO 26262.

Кроме того, компания SGS-TÜV Saar сертифицировала FileX для использования в критических для безопасности приложениях для железнодорожного сообщения в соответствии со стандартом EN 50128 вплоть до уровня SW-SIL 4.

![Логотип SGS TUV Saar](./media/user-guide/sgs-tuv-saar-logo.png)

- IEC 61508 до уровня SIL 4
- IEC 62304 до уровня SW класса безопасности C
- ISO 26262 ASIL D
- EN 50128 SW-SIL 4

> [!IMPORTANT]
> Обратившись к нам, вы можете получить дополнительные сведения о версиях FileX, которые прошли сертификацию TÜV, а также копии доступных отчетов о тестах, сертификатов и других документов.*

### <a name="ul-certification"></a>Сертификация UL

Решение FileX сертифицировано организацией UL как соответствующее стандартам безопасности UL 60730-1 (приложение H), CSA E60730-1 (приложение H), IEC 60730-1 (приложение H), UL 60335-1 (приложение R), IEC 603351 (приложение R) и UL 1998 для программного обеспечения в программируемых компонентах. В дополнение к стандарту IEC/UL 60730-1, который в приложении H содержит требования к управлению с использованием программного обеспечения, стандарт IEC 60335-1 описывает в приложении R требования к программируемым электронным цепям. В IEC 60730 (приложение H) и IEC 60335-1 (приложение R) обсуждается безопасность программного и аппаратного обеспечения микроконтроллеров, используемых в разных устройствах, таких как стиральные, посудомоечные и сушильные машины, а также холодильные, морозильные и духовые шкафы.

![C RU US 2](./media/user-guide/c-ru-us-logo.png)

*UL/IEC 60730, UL/IEC 60335, UL 1998*

> [!IMPORTANT]
>*Обратившись к нам, вы можете получить дополнительные сведения о версиях FileX, которые прошли сертификацию UL, а также копии доступных отчетов о тестах, сертификатов и других документов.*

## <a name="powerful-services-of-filex"></a>Многофункциональные службы FileX

### <a name="multiple-media-management"></a>Управление несколькими носителями

FileX может поддерживать неограниченное число физических носителей. Каждый экземпляр носителя имеет собственную отдельную область памяти и соответствующий драйвер, указанный в вызове ***fx_media_open***. Дистрибутив FileX по умолчанию содержит простой драйвер электронного носителя (размещаемого в ОЗУ), а также демонстрационную систему, использующую этот электронный диск.

### <a name="logical-sector-cache"></a>Кэш логических секторов

Кэш логических секторов FileX значительно повышает производительность, снижая число передач полных секторов как на носитель, так и с него. FileX поддерживает кэш логических секторов для каждого открытого носителя. Глубина кэша логических секторов определяется объемом памяти, предоставленным FileX вызовом API ***fx_media_open***.

### <a name="contiguous-file-support"></a>Поддержка непрерывных файлов

FileX предоставляет поддержку непрерывную файлов с помощью службы API ***fx_file_allocate***, чтобы улучшить время доступа к файлу и сделать его детерминированным. Эта процедура принимает запрошенный объем памяти и ищет ряд смежных кластеров для удовлетворения запроса. Если такие кластеры найдены, они предварительно выделяются, что делает их частью цепочки выделенных кластеров для файла. При перемещении физического носителя поддержка непрерывных файлов FileX обеспечивает значительное повышение производительности и делает время доступа детерминированным.

### <a name="dynamic-creation"></a>Динамическое создание

FileX позволяет динамически создавать системные ресурсы. Это особенно важно, если приложение имеет различные или динамические требования к конфигурации. Кроме того, не существует заранее установленных ограничений на количество ресурсов FileX, которые можно использовать (носители или файлы). Кроме того, количество системных объектов не влияет на производительность.

## <a name="easy-to-use-api"></a>Простые в использовании API-интерфейсы

FileX предоставляет лучшую глубоко внедренную технологию файловой системы, которую при этом легко изучить и использовать. Программный интерфейс (API) FileX обеспечивает интуитивную простоту и согласованность при работе со службами. Вам не придется расшифровать службы из "набора букв", которые слишком часто встречаются в других файловых системах.

Полный список служб FileX версии 5 см. в [приложении A](appendix-a.md).

## <a name="exfat-support"></a>Поддержка exFAT

exFAT (расширенная таблица распределения файлов) — это файловая система, разработанная корпорацией Майкрософт и поддерживающая файлы размером свыше 2 ГБ, что было ограничением для файловой системы FAT32. Это файловая система по умолчанию для SD-карт емкостью свыше 32 ГБ. SD-карты или устройства флэш-памяти, отформатированные с использованием формата exFAT FileX, совместимы с Windows. exFAT поддерживает размер файла до одного эксабайта (ЭБ), который составляет примерно 1 000 000 000 ГБ.

Пользователи, желающие использовать exFAT, должны перекомпилировать библиотеку FileX с заданным символом ***FX_ENABLE_EXFAT** _. При открытии носителя FileX определяет его тип. Если носитель отформатирован с использованием exFAT, FileX осуществляет чтение и запись в файловой системе согласно стандарту exFAT. Чтобы отформатировать новый носитель с использованием exFAT, используйте службу _*_fx_media_exFAT_format_**. По умолчанию файловая система exFAT отключена.

## <a name="fault-tolerant-support"></a>Поддержка отказоустойчивости

Модуль отказоустойчивости FileX предназначен для предотвращения повреждения файловой системы, вызванного прерываниями при изменении файла или каталога. Например, при добавлении данных в файл FileX нужно изменить содержимое файла, запись каталога и, возможно, записи в файловой системе FAT. Если эта последовательность изменения была прервана (например, из-за сбоя питания или извлечения носителя в середине операции), файловая система находится в несогласованном состоянии, что может негативно повлиять на целостность всей файловой системы, вызвав повреждение других файлов.

Модуль отказоустойчивости FileX работает путем записи всех действий, необходимых для изменения файла или каталога. Эта запись журнала хранится на носителе в выделенных секторах (блоках), по которым FileX может выполнить поиск и к которым может обратиться. Доступ к расположению данных журнала можно получить даже без соответствующей файловой системы. Таким образом, если файловая система повреждена, FileX все равно сможет найти запись журнала и восстановить файловую систему в нормальном состоянии.

Записи журнала создаются по мере изменения файла или каталога решением FileX. После успешного завершения операции изменения записи журнала удаляются. Если записи журнала не были должным образом удалены после успешного изменения файла, а процесс восстановления определяет, что содержимое в записи журнала соответствует файловой системе, то никаких действий не требуется, а записи журнала можно очистить.

Если операция изменения файловой системы была прервана, при следующем подключении носителя с помощью FileX модуль отказоустойчивости анализирует записи журнала. Сведения в записях журнала позволяют FileX откатить частичные изменения, уже примененные к файловой системе (в случае сбоя на раннем этапе операции обновления файла), а если записи журнала содержат сведения о повторе, FileX может применить изменения, необходимые для завершения предыдущей операции.

Эта функция отказоустойчивости доступна для всех файловых систем FAT, поддерживаемых FileX, включая FAT12, FAT16, FAT32 и exFAT. По умолчанию отказоустойчивость в FileX отключена. Чтобы включить функцию отказоустойчивости, для FileX необходимо выполнить сборку с заданным символом **FX_ENABLE_FAULT_TOLERANT** и **FX_FAULT_TOLERANT**. Во время своего выполнения приложение запускает службу отказоустойчивости путем вызова **_fx_fault_tolerant_enable_**.
После запуска службы все операции записи файлов и каталогов проходят через модуль отказоустойчивости.

При запуске служба отказоустойчивости сначала определяет, защищен ли носитель модулем отказоустойчивости. Если это не так, FileX предполагает целостность файловой системы и активирует защиту, выделяя из файловой системы свободные блоки для ведения журнала и кэширования. Если в файловой системе найдены журналы модуля отказоустойчивости, решение анализирует записи журнала. FileX отменяет изменения предыдущей операции или повторяет предыдущую операцию в зависимости от содержимого записей журнала. После обработки всех предыдущих записей журнала файловая система становится доступной. Это гарантирует, что FileX начинает работу с известного рабочего состояния.

После защиты носителя с помощью модуля отказоустойчивости FileX этот носитель не будет обновляться другой файловой системой. В этом случае записи журнала в файловой системе не будут соответствовать содержимому в таблице FAT, записи каталога. Если носитель обновляется другой файловой системой до его переноса обратно в FileX с помощью модуля отказоустойчивости, это дает неопределенный результат.

## <a name="callback-functions"></a>Функции обратного вызова

В FileX добавлены три следующих функции обратного вызова:

- Обратный вызов открытия носителя
- Обратный вызов закрытия носителя
- Обратный вызов записи файла

После регистрации эти функции будут уведомлять приложение о возникновении таких событий.

## <a name="easy-integration"></a>Простота интеграции

FileX легко интегрируется практически с любым устройством флэш-памяти или носителем. Перенос FileX довольно прост. Данный процесс подробно описан в настоящем руководстве, при этом драйвер ОЗУ демонстрационной системы станет отличной отправной точкой.