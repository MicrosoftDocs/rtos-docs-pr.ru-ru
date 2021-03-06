---
title: Глава 1. Введение в протоколы HTTP и HTTPS
description: В этой главе описывается модуль ОСРВ Azure NetX Duo HTTP/HTTPS для Интернета.
author: philmea
ms.author: philmea
ms.date: 07/24/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: e5f50419be3171d3df8544d1b34d603822f339785923f8a8199dc5b5ddcac281
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2021
ms.locfileid: "116801762"
---
# <a name="chapter-1---introduction-to-http-and-https"></a>Глава 1. Введение в протоколы HTTP и HTTPS

Протокол HTTP предназначен для передачи содержимого в Интернете. HTTP — это простой протокол, который использует для передачи содержимого надежные службы протокола TCP. Благодаря этому HTTP считается очень надежным протоколом для обмена содержимым. Также HTTP является одним из самых часто используемых протоколов приложений. Все операции в Интернете используют протокол HTTP.

HTTPS — это безопасная версия протокола HTTP, которая реализует протокол HTTP с использованием протокола TLS для защиты базового TCP-подключения. За исключением дополнительной конфигурации, необходимой для настройки TLS, использование протокола HTTPS по сути не отличается от протокола HTTP.

## <a name="general-http-requirements"></a>Общие требования для протокола HTTP

Для правильной работы пакета NetX Web HTTP требуется установить NetX Duo 5.10 или более поздней версии. Кроме того, должен быть создан экземпляр IP, для которого включено использование протокола TCP. Для поддержки HTTPS также необходимо установить NetX Secure TLS 5.11 или более поздней версии (см. следующий раздел). Этот процесс показан в демонстрационном файле в разделе "Пример небольшой системы" **главы 2**.

Для HTTP-клиента из пакета NetX Web HTTP больше нет дополнительных требований.

Но HTTP-сервер из пакета NetX Web HTTP определяет еще несколько дополнительных требований. Во-первых, ему требуется полный доступ к *известному TCP-порту 80* для обработки всех запросов HTTP-клиента (приложение может указать любой другой допустимый порт TCP). HTTP-сервер также разработан для работы с внедренной файловой системой FileX. Если система FileX недоступна, пользователь может перенести используемые разделы FileX в собственную среду. Этот процесс рассматривается в последующих разделах этого руководства.

## <a name="https-requirements"></a>Требования для протокола HTTPS

Для правильной работы протокола HTTPS на основе пакета NetX Web HTTP требуется, чтобы были установлены NetX Duo 5.10 или более поздней версии и NetX Secure TLS 5.11 или более поздней версии. Кроме того, должен быть создан экземпляр IP, для которого включено использование протокола TCP для работы с протоколом TLS. Сеанс TLS необходимо будет инициализировать с помощью соответствующих криптографических процедур и сертификата доверенного ЦС. Кроме того, потребуется выделить пространство для сертификатов, которые будут предоставляться удаленными узлами сервера во время подтверждения TLS. Этот процесс показан в демонстрационном файле в разделе "Пример небольшой системы HTTPS" **главы 2**.

Для HTTPS-клиента из пакета NetX Web HTTP больше нет дополнительных требований.

Но HTTPS-сервер из пакета NetX Web HTTP определяет еще несколько дополнительных требований. Во-первых, ему требуется полный доступ к *известному TCP-порту 443* для обработки всех HTTPS-запросов клиента (как и в случае протокола HTTP без шифрования, приложение может изменить этот порт). Во-вторых, потребуется инициализировать сеанс TLS с помощью соответствующих криптографических процедур и сертификата удостоверения сервера (или общего ключа). HTTPS-сервер также разработан для работы с внедренной файловой системой FileX. Если система FileX недоступна, пользователь может перенести используемые разделы FileX в собственную среду. Использование FileX рассматривается в последующих разделах этого руководства.

Дополнительные сведения о параметрах конфигурации TLS см. в документации по NetX Secure.

Если не указано иное, все функции HTTP, описанные в этом документе, также относятся к протоколу HTTPS.

## <a name="http-and-https-constraints"></a>Ограничения протоколов HTTP и HTTPS

NetX Web HTTP реализует стандарт HTTP 1.1. Но существует ряд ограничений, которые приведены ниже:

1. не поддерживаются конвейеры обработки запросов;
1. HTTP-сервер поддерживает обычную проверку подлинности и дайджест-проверку подлинности MD5, но не поддерживает MD5-sess; в настоящее время HTTP-клиент поддерживает только обычную проверку подлинности; при использовании протокола TLS для протокола HTTPS можно использовать проверку подлинности HTTP;
1. не поддерживается сжатие содержимого;
1. не поддерживаются запросы TRACE, OPTIONS и CONNECT;
1. пул пакетов, связанный с HTTP-сервером или HTTP-клиентом, должен быть достаточно большим для размещения заголовка HTTP целиком;
1. службы HTTP-клиента предназначены только для передачи содержимого, а средства для отображения в этом пакете не предоставляются.

## <a name="http-url-resource-names"></a>URL-адрес HTTP (имена ресурсов)

Протокол HTTP разработан для передачи содержимого через Интернет. Запрашиваемое содержимое определяется URL-адресом. Это основной компонент каждого HTTP-запроса. URL-адреса всегда начинаются с символа "/" и обычно обозначают определенные файлы на HTTP-сервере. Ниже приведены типичные расширения файлов, используемые с протоколом HTTP:

- **HTM** (или **HTML**): HTML-файлы (HTML);
- **TXT**: открытый текст ASCII;
- **GIF**: двоичное изображение GIF;
- **XBM**: двоичное изображение Xbitmap.

## <a name="http-client-requests"></a>Запросы HTTP-клиента

В протоколе HTTP есть простой механизм для запроса содержимого в Интернете. Доступен стандартные набор команд HTTP, которые отправляются клиентом после успешного установления подключения по *стандартному TCP-порту 80 (или порту 443 для HTTPS)* . Ниже приведены некоторые основные команды HTTP.

- **GET *ресурс* HTTP/1.1**: получение указанного ресурса.
- **POST *ресурс* HTTP/1.1**: получение указанного ресурса и передача вложенных входных данных на HTTP-сервер.
- **HEAD *ресурс* HTTP/1.1**: выполняется так же, как GET, но HTTP-сервер не возвращает содержимое.
- **PUT *ресурс* HTTP/1.1**: размещение ресурса на HTTP-сервере.
- **DELETE *ресурс* HTTP/1.1**: удаление ресурса с сервера.

Эти команды ASCII обычно генерируются веб-браузерами и службами клиента NetX Web HTTP для выполнения операций HTTP на HTTP-сервере.

Обратите внимание на то, что приложение HTTP-клиента должно использовать порт 80 (или порт 443, если используется протокол HTTPS). Интерфейсы API HTTP-клиента и HTTP-сервера принимают порт в качестве параметра. Для удобства использования определены соответствующие макросы, NX_WEB_HTTP_SERVER_PORT (порт 80) и NX_WEB_HTTPS_SERVER_PORT (порт 443). Порт HTTP-сервера также может быть изменен во время выполнения с помощью службы *nx_web_http_client_set_connect_port()* . Дополнительные сведения об этой службе см. в главе 4.

## <a name="http-server-responses"></a>Ответы HTTP-сервера

HTTP-сервер использует для отправки ответов на команды клиента тот же самый *известный TCP-порт 80 (или порт 443 для HTTPS)* . Когда HTTP-сервер завершает обработку команды клиента, он возвращает строку ответа ASCII, которая включает в себя 3-значный числовой код состояния. Программное обеспечение клиента по этому числовому ответу определяет, была ли операция выполнена успешно или завершилась сбоем. Ниже приведен список ответов HTTP-сервера на команды клиента.

- **200**: запрос выполнен успешно.
- **400**: запрос не сформирован должным образом.
- **401**: несанкционированный запрос, клиент должен отправить данные проверки подлинности.
- **404**: указанный в запросе ресурс не найден.
- **500**: внутренняя ошибка HTTP-сервера.
- **501**: запрос не реализован HTTP-сервером.
- **502**: служба недоступна.

Например, в ответ на успешно выполненный запрос PUT клиента для файла test.htm будет возвращено сообщение "HTTP/1.1 200 OK".

## <a name="http-communication"></a>Взаимодействие по протоколу HTTP

Как упоминалось ранее, HTTP-сервер принимает запросы от клиентов через *известный TCP-порт 80 (или порт 443 для HTTPS)* . HTTP-клиенты могут использовать любой доступный TCP-порт для исходящих подключений. Общая последовательность событий HTTP выглядит следующим образом.

**HTTP-запрос GET**

1. Клиент отправляет запрос на TCP-подключение к порту 80 сервера (или 443 для HTTPS).
1. Если используется протокол HTTPS, то после установления TCP-подключения следует подтверждение TLS, чтобы проверить подлинность сервера и установить безопасный канал.
1. Клиент отправляет запрос **GET *ресурс* HTTP/1.1** и другие сведения заголовка.
1. Сервер создает сообщение **HTTP/1.1 200 OK** с дополнительной информацией, за которой следует содержимое запрошенного ресурса (при наличии).
1. Сервер отключается от клиента (протокол TLS завершает работу, если используется протокол HTTPS).
1. Клиент отключается от сокета (протокол TLS завершает работу после оповещения об отключении от сервера).

**HTTP-запрос PUT**

1. Клиент отправляет запрос на TCP-подключение к порту 80 (или 443) сервера.
1. Если используется протокол HTTPS, то после установления TCP-подключения следует подтверждение TLS, чтобы проверить подлинность сервера и установить безопасный канал.
1. Клиент отправляет запрос PUT <ресурс> HTTP/1.1 и другие сведения заголовка, за которыми следует содержимое ресурса.
1. Сервер создает сообщение "HTTP/1.1 200 OK" с дополнительной информацией, за которой следует содержимое запрошенного ресурса.
1. Сервер разрывает подключение.
1. Клиент разрывает подключение.

> [!NOTE]
> Как уже упоминалось, HTTP-сервер может изменить стандартный порт подключения (80 или 443) на любой другой порт с помощью службы *nx_web_http_client_set_connect_port()* , что позволяет веб-серверам, использующим альтернативные порты, подключаться к клиентам.

## <a name="http-authentication"></a>Проверка подлинности HTTP

Проверка подлинности HTTP является необязательной, то есть требуется не для всех веб-запросов. Существуют две разновидности проверки подлинности: *обычная* и *на основе дайджеста*. Обычная проверка подлинности по *имени* и *паролю* работает точно так же, как во многих других протоколах. При обычной проверке подлинности HTTP имя и пароли объединяются в одну строку и кодируются в формате Base64. Основным недостатком обычной проверки подлинности является то, что имя и пароль передаются в запросе в открытом виде. Это позволяет достаточно легко похищать такие имена и пароли. Дайджест-проверка подлинности устраняет эту проблему, так как при ней имя и пароль не передаются вместе с запросом. Вместо этого применяется специальный механизм вычисления 128-разрядного дайджеста по имени пользователя, паролю и некоторым другим параметрам. Сервер NetX Web HTTP поддерживает стандартный алгоритм дайджестов MD5.

Когда нужна проверка подлинности? HTTP-сервер самостоятельно решает, требуется ли проверка подлинности для запрошенного ресурса. Если проверка подлинности нужна, но в запросе от клиента нет необходимых данных проверки подлинности, то клиенту возвращается ответ "HTTP/1.1 401 Unauthorized" с указанием требуемого типа проверки подлинности. Ожидается, что клиент в этом случае сформирует новый запрос с правильными данными проверки подлинности.

При использовании протокола HTTPS HTTPS-сервер по-прежнему может использовать проверку подлинности HTTP. В этом случае для шифрования всего трафика HTTP используется протокол TLS, поэтому использование *обычной* проверки подлинности HTTP не обуславливает угрозу безопасности. *Дайджест*-проверка подлинности также допускается, но не обеспечивает значительного повышения безопасности по сравнению с обычной проверкой подлинности на основе протокола TLS.

## <a name="http-authentication-callback"></a>Обратный вызов проверки подлинности HTTP

Как уже упоминалось, проверка подлинности HTTP является необязательной, то есть используется не при любой передаче данных через Интернет. Кроме того, проверка подлинности обычно зависит от конкретного ресурса. Один и тот же сервер может требовать проверку подлинности для доступа к некоторым ресурсам и не требовать для доступа к другим. Пакет сервера NetX Web HTTP позволяет приложению указать (с помощью вызова ***nx_http_server_create***) подпрограмму обратного вызова проверки подлинности, которая будет вызываться в начале обработки каждого запроса от HTTP-клиента.

Эта подпрограмма обратного вызова предоставляет серверу NetX Web HTTP строковые значения имени пользователя, пароля и области, которые связаны с конкретным ресурсом, и возвращает необходимый тип проверки подлинности. Если для ресурса не требуется проверка подлинности, обратный вызов проверки подлинности должен возвращать значение **NX_WEB_HTTP_DONT_AUTHENTICATE**. Если для указанного ресурса требуется обычная проверка подлинности, эта подпрограмма должна возвращать **NX_WEB_HTTP_BASIC_AUTHENTICATE**. Наконец, если требуется дайджест-проверка подлинности MD5, подпрограмма обратного вызова должна возвращать **NX_WEB_HTTP_DIGEST_AUTHENTICATE**. Если ни для одного из ресурсов, предоставляемого HTTP-сервером, не требуется проверка подлинности, обратный вызов можно не указывать, передав в вызов для создания HTTP-сервера пустой указатель.

Формат подпрограммы обратного вызова проверки подлинности для приложения достаточно прост и определен ниже.

```C
UINT nx_web_http_server_authentication_check(NX_WEB_HTTP_SERVER *server_ptr,
    UINT request_type, CHAR *resource,
    CHAR **name, CHAR **password,
    CHAR **realm);
```

Входные параметры определяются следующим образом.

- **request_type**: указывает запрос HTTP-клиента, который может иметь одно из следующих значений:
  - **NX_WEB_HTTP_SERVER_GET_REQUEST**
  - **NX_WEB_HTTP_SERVER_POST_REQUEST**
  - **NX_WEB_HTTP_SERVER_HEAD_REQUEST**
  - **NX_WEB_HTTP_SERVER_PUT_REQUEST**
  - **NX_WEB_HTTP_SERVER_DELETE_REQUEST**
- **resource**: запрашиваемый ресурс.
- **name**: указатель на требуемое имя пользователя.
- **password**: указатель на требуемый пароль.
- **realm**: указатель на область определения приложений для данной проверки подлинности.

Возвращаемое значение подпрограммы проверки подлинности указывает, требуется ли проверка подлинности. Указатели на имя, пароль и область определения приложения не используются, если подпрограмма обратного вызова проверки подлинности возвращает значение **NX_WEB_HTTP_DONT_AUTHENTICATE**. В противном случае разработчик HTTP-сервера должен убедиться, что значения **NX_WEB_HTTP_MAX_USERNAME** и **NX_WEB_HTTP_MAX_PASSWORD**, определенные в файле *nx_web_http_server.h*, достаточно велики для размещения имени пользователя и пароля, указанных в обратном вызове проверки подлинности. По умолчанию оба значения равны 20 символам.

## <a name="http-invalid-usernamepassword-callback"></a>Обратный вызов для недопустимых значений имени пользователя или пароля HTTP

Необязательный обратный вызов для недопустимых значений имени пользователя или пароля на сервере NetX Web HTTP вызывается в тех случаях, когда HTTP-сервер получает в запросе от клиента недопустимое сочетание имени пользователя и пароля. Если приложение HTTP-сервера зарегистрирует обратный вызов, то указанная подпрограмма будет вызываться в случае ошибки обычной проверки подлинности или дайджест-проверки подлинности в *nx_web_http_server_get_process()* , *nx_web_http_server_put_process()* или *nx_web_http_server_delete_process()* .

Чтобы зарегистрировать обратный вызов на HTTP-сервере, используется приведенная ниже служба, которая определена на сервере NetX Web HTTP.

```C
UINT nx_web_http_server_invalid_userpassword_notify_set(
    NX_WEB_HTTP_SERVER *http_server_ptr,
    UINT (*invalid_username_password_callback)
        (CHAR *resource, ULONG *client_nx_address,
        UINT request_type));
```

Определены следующие типы запроса:

- **NX_WEB_HTTP_SERVER_GET_REQUEST**
- **NX_WEB_HTTP_SERVER_POST_REQUEST**
- **NX_WEB_HTTP_SERVER_HEAD_REQUEST**
- **NX_WEB_HTTP_SERVER_PUT_REQUEST**
- **NX_WEB_HTTP_SERVER_DELETE_REQUEST**

## <a name="http-insert-gmt-date-header-callback"></a>Обратный вызов для добавления заголовка даты GMT в HTTP

Этот необязательный обратный вызов на сервере NetX Web HTTP позволяет добавлять в ответные сообщения заголовок со значением даты. Он вызывается, когда HTTP-сервер отвечает на запрос PUT или GET.

Чтобы зарегистрировать обратный вызов для добавления даты GMT на HTTP-сервере, определена приведенная ниже служба.

```C
UINT nx_web_http_server_gmt_callback_set(
    NX_WEB_HTTP_SERVER *server_ptr,
    VOID (*gmt_get)(NX_WEB_HTTP_SERVER_DATE *date);
```

Тип данных NX_WEB_HTTP_SERVER_DATE определяется следующим образом:

```C
typedef struct NX_WEB_HTTP_SERVER_DATE_STRUCT
{
    USHORT nx_web_http_server_year; /* Year */
    UCHAR nx_web_http_server_month; /* Month */
    UCHAR nx_web_http_server_day; /* Day */
    UCHAR nx_web_http_server_hour; /* Hour */
    UCHAR nx_web_http_server_minute; /* Minute */
    UCHAR nx_web_http_server_second; /* Second */
    UCHAR nx_web_http_server_weekday; /* Weekday */
} NX_WEB_HTTP_SERVER_DATE;
```

## <a name="http-cache-info-get-callback"></a>Обратный вызов для получения сведений из кэша HTTP

HTTP-сервер поддерживает обратный вызов для запроса ограничений по возрасту и датам для определенного ресурса в приложении HTTP. Эти сведения позволяют определить, будет ли HTTP-сервер отправлять всю страницу клиенту по запросу GET. Если в запросе клиента нет строки "if modified since" (если изменено позднее) или это значение не совпадает с датой "last modified" (последнее изменение), полученной в обратном вызове запроса сведений из кэша, то клиенту отправляется вся страница.

Чтобы зарегистрировать обратный вызов на HTTP-сервере, определена приведенная ниже служба.

```C
UINT nx_web_http_server_cache_info_callback_set(
    NX_WEB_HTTP_SERVER *server_ptr,
    UINT (*cache_info_get)
    (CHAR *, UINT *, NX_WEB_HTTP_SERVER_DATE *));
```

## <a name="http-chunked-transfer-coding-support"></a>Поддержка поблочного кодирования HTTP

Если определить общую длину сообщения HTTP перед отправкой невозможно, то можно использовать функцию поблочного кодирования для отправки сообщений в виде серий блоков без поля заголовка Content-Length. Эта функция поддерживается во всех сообщениях HTTP-запросов и HTTP-ответов. Эта функция поддерживается на стороне получателя, а заголовок блока автоматически обрабатывается внутренней логикой. На стороне отправителя клиентом и сервером должны вызываться интерфейсы API *nx_web_http_client_request_chunked_set* и *nx_web_http_server_response_chunked_set* соответственно.

```C
UINT nx_web_http_client_request_chunked_set(NX_WEB_HTTP_CLIENT *client_ptr,
    UINT chunk_size,
    NX_PACKET *packet_ptr);

UINT nx_web_http_server_response_chunked_set(NX_WEB_HTTP_SERVER *server_ptr,
    UINT chunk_size,
    NX_PACKET *packet_ptr);
```

Дополнительные сведения об использовании этих служб можно найти в главе 3 "Описание служб HTTP".

## <a name="http-multipart-support"></a>Поддержка многокомпонентных сообщений HTTP

Протокол MIME изначально предназначался для взаимодействия с протоколом SMTP, но теперь он используется и с протоколом HTTP. Протокол MIME позволяет включать в одно сообщение смешанные типы данных (например, image/jpg и text/plain). Сервер NetX Web HTTP включает в себя службы для определения типа содержимого в полученных от клиента сообщениях HTTP, содержащих данные MIME. Чтобы включить поддержку многокомпонентных сообщений HTTP и использовать эти службы, необходимо определить параметр конфигурации **NX_WEB_HTTP_MULTIPART_ENABLE**.

```C
UINT nx_web_http_server_get_entity_header(NX_WEB_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_pptr,
    UCHAR *entity_header_buffer,
    ULONG buffer_size);

UINT nx_web_http_server_get_entity_content(NX_WEB_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_pptr,
    ULONG *available_offset,
    ULONG *available_length);
```

Дополнительные сведения об использовании этих служб можно найти в главе 3 "Описание служб HTTP".

## <a name="http-multi-thread-support"></a>Поддержка многопоточности HTTP

Службы клиента NetX Web HTTP можно вызывать из нескольких потоков одновременно. Но запросы на чтение или запись для конкретного экземпляра HTTP-клиента должны выполняться последовательно из одного потока.

При использовании протокола HTTPS службы клиента NetX Web HTTP могут вызываться из нескольких потоков, но ввиду повышенной сложности базовых функций TLS каждый поток должен использовать отдельный, независимый экземпляр HTTP-клиента (структуру управления NX_WEB_HTTP_CLIENT).

## <a name="http-rfcs"></a>Соответствие протокола HTTP положениям документов RFC

NetX Web HTTP соответствует требованиям документов RFC 1945 "Hypertext Transfer Protocol/1.0" (Протокол передачи гипертекста, версия 1.0), RFC 2616 "Hypertext Transfer Protocol/1.1" (Протокол передачи гипертекста, версия 1.1), RFC 2581 "TCP Congestion Control" (Контроль перегрузки TCP), RFC 1122 "Requirements for Internet Hosts" (Требования к Интернет-узлам) и других связанных с ними документов RFC.

Реализация протокола HTTPS в NetX Web HTTP соответствует требованиям документа RFC 2818 "HTTP over TLS" (Передача данных HTTP по протоколу TLS).
