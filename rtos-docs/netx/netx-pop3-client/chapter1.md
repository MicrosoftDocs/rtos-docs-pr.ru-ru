---
title: Глава 1. Введение в POP3-клиент NetX для ОСРВ Azure
description: API POP3-клиента NetX для ОСРВ Azure предоставляет почтовую транспортную систему, позволяющую небольшим рабочим станциям обращаться к почтовым ящикам клиента на POP3-серверах для получения почты клиента.
author: philmea
ms.author: philmea
ms.date: 06/04/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 6abaa778da6203d0df367894165cb29ca629ab5e24403a35af1995f032cf0d4c
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2021
ms.locfileid: "116798815"
---
# <a name="chapter-1---introduction-to-azure-rtos-netx-pop3-client"></a>Глава 1. Введение в POP3-клиент NetX для ОСРВ Azure

Протокол POP3 (Post Office Protocol Version 3) — это протокол, предназначенный для предоставления почтовой транспортной системы, позволяющей небольшим рабочим станциям обращаться к почтовым ящикам клиента на POP3-серверах для получения почты клиента. Для передачи почты протокол POP3 использует службы протокола TCP. Поэтому POP3 является надежным протоколом для обмена содержимым.

Однако протокол POP3 не предоставляет комплексных операций по обработке почты. Как правило, почта скачивается клиентом, а затем удаляется из почтового ящика сервера.

## <a name="netx-pop3-client-requirements"></a>Требования к использованию POP3-клиента NetX

### <a name="client-requirements"></a>Требования клиента

Для использования API POP3-клиента NetX для OCPB Azure требуется экземпляр IP NetX, созданный с помощью *nx_ip_create*, и пул пакетов NetX, созданный с помощью *nx_packet_pool_create*. Так как POP3-клиент NetX применяет службы TCP, перед использованием служб POP3-клиента NetX на этом же экземпляре IP необходимо включить протокол TCP с помощью вызова *nx_tcp_enable*. POP3-клиент использует TCP-сокет для подключения к POP3-серверу через POP3-порт сервера. Обычно это *хорошо известный порт 110, хотя ни POP3-клиент, ни сервер не обязаны использовать именно его.*

Размер пула пакетов, используемого при создании POP3-клиента, настраивается пользователем с точки зрения полезных данных пакета и количества доступных пакетов. Если пакет используется только в службе создания POP3-клиента, полезная нагрузка пакета не должна превышать 100–120 байт в зависимости от длины имени пользователя и пароля или дайджеста APOP. Возможно, команда USER с именем пользователя локального узла является самым длинным сообщением, отправляемым POP3-клиентом. В nx_ip_create (пул пакетов IP по умолчанию) можно использовать тот же пул пакетов, так как для выполнения внутренних операций IP не требуется слишком большая полезная нагрузка пакетов для отправки и получения управляющих данных TCP.

Однако использование сетевым драйвером пула пакетов, совпадающего с пулом пакетов POP3-клиента, может быть нецелесообразным. Как правило, полезные данные пула пакетов получения задаются с помощью MTU экземпляра IP (обычно 1500 байт) сетевого интерфейса, размер которого значительно превышает размер сообщений POP3-клиента. Как правило, входящие сообщения POP3 имеют гораздо больший объем данных, чем исходящие сообщения POP3-клиента.

## <a name="netx-pop3-client-creation"></a>Создание POP3-клиента NetX

Служба создания POP3-клиента, *nx_pop3_client_create*, создает сокет TCP-сокет и подключается к POP3-серверу.

После подключения к POP3-серверу приложение POP3-клиента может вызвать службу *nx_pop3_client _mail_items_get*, чтобы получить количество почтовых элементов, находящихся в его почтовом ящике.

```c
UINT nx_pop3_client_mail_items_get(NX_POP3_CLIENT *client_ptr,
                                UINT *number_mail_items,
                                ULONG *maildrop_total_size)
```

Если в почтовом ящике клиента находится один элемент или несколько, приложение может получить размер определенного почтового элемента с помощью службы *nx_pop3_client_get_mail_item*:

```c
UINT nx_pop3_client_mail_item_get(NX_POP3_CLIENT *client_ptr,
                                UINT mail_item,
                                ULONG *item_size)
```

Первый почтовый элемент в почтовом ящике находится по индексу 1.

Чтобы получить реальное почтовое сообщение, приложение может вызывать службу *nx_pop3_client_mail_item_get_message_data* для извлечения пакетов почтовых сообщений, пока служба не сообщит о получении последнего пакета с помощью входного аргумента final_packet.

```c
UINT nx_pop3_client_mail_item_message_get(
                        NX_POP3_CLIENT *client_ptr,
                        NX_PACKET **recv_packet_ptr,
                        ULONG *bytes_retrieved,
                        UINT *final_packet)
```

Чтобы удалить конкретный почтовый элемент, приложение вызывает службу *nx_pop3_client_mail_item_delete* с тем же индексом, который использовался в предыдущем вызове службы *nx_pop3_client_get_mail_item*.

Клиент можно удалить с помощью службы *nx_pop3_client_delete*. Обратите внимание, что, если использовать пул пакетов POP3-клиента больше не требуется, приложение может удалить его помощью службы *nx_packet_pool_delete*.

## <a name="netx-pop3-client-constraints"></a>Ограничения для POP3-клиента NetX

В реализации POP3-клиента NetX существуют некоторые ограничения.

1. POP3-клиент NetX не поддерживает команду AUTH, хотя он реализует проверку подлинности APOP, используя DIGEST-MD5 для проверки подлинности клиента и сервера.

1. POP3-клиент NetX поддерживает не все команды POP3 (например, команды TOP или UIDL). Ниже приведен список поддерживаемых команд.
   - NOOP
   - RSET

## <a name="netx-pop3-client-login"></a>Имя входа POP3-клиента NetX

Для получения доступа к почтовому ящику POP3-клиент NetX должен подтвердить свою подлинность (имя входа) на POP3-сервере. Это можно сделать с помощью команд USER/PASS и указания имени пользователя и пароля, известных POP3-серверу, или с помощью команды APOP и дайджеста MD5, как описано ниже.

Имя пользователя обычно является полным доменным именем (содержит компонент "local-part" и имя домена, разделенные символом @). При использовании команд POP3 USER и PASS клиент отправляет свое имя пользователя и пароль в незашифрованном виде через Интернет.

Чтобы избежать угрозы безопасности, связанной с отправкой имени пользователя и пароля в виде открытого текста, POP3-клиент NetX можно настроить для использования проверки подлинности APOP, задав параметр *APOP_authentication* в службе *nx_pop3_client_create*.

```c
UINT  nxd_pop3_client_create(NX_POP3_CLIENT *client_ptr,
                            UINT APOP_authentication,
                            NX_IP *ip_ptr,
                            NX_PACKET_POOL *packet_pool_ptr,
                            NXD_ADDRESS *server_ip_address, ULONG server_port,
                            CHAR *client_name,
                            CHAR *client_password)
```

Или только для приложений IPv4 использовать службу *nx_pop3_client_create*.

```c
UINT  nx_pop3_client_create(NX_POP3_CLIENT *client_ptr,
                            UINT APOP_authentication,
                            NX_IP *ip_ptr,
                            NX_PACKET_POOL *packet_pool_ptr,
                            ULONG server_ip_address,
                            ULONG server_port, CHAR *client_name,
                            CHAR *client_password)
```

Когда клиент отправляет команду APOP, он принимает дайджест MD5, содержащий домен сервера, местное время и идентификатор процесса, извлеченные из приветственного сообщения сервера, а также пароль клиента. POP3-сервер создаст дайджест MD5, содержащий те же данные, и, если его дайджест MD5 совпадет с дайджестом MD5 клиента, клиент проходит проверку подлинности.

Если проверка подлинности APOP завершается сбоем, POP3-клиент NetX попытается пройти проверку подлинности с помощью команд USER/PASS.

## <a name="the-pop3-client-maildrop"></a>Почтовый ящик POP3-клиента

Клиентская почта хранится на POP3-сервере в почтовом ящике. Почтовый ящик клиента на POP3-сервере имеет вид списка почтовых элементов с отсчетом от 1. Это означает, что каждое сообщение указывается по его индексу в списке. Первый почтовый элемент находится по индексу 1 (а не по нулевому индексу). Команды POP3 ссылаются на определенные почтовые элементы по их индексу в этом списке.

## <a name="the-pop3-protocol-state-machine"></a>Конечный автомат протокола POP3

Для использования протокола POP3 требуется, чтобы клиент и сервер поддерживали состояние сеанса POP3. Сначала клиент пытается подключиться к POP3-серверу. В случае успеха он подключается по протоколу POP3, имеющему три различных состояния, определенных стандартом RFC 1939. Начальное состояние — это состояние авторизации, в котором клиент должен пройти идентификацию на сервере. В состоянии авторизации POP3-клиент может запускать только команды USER и PASS и только в таком порядке, или же команду APOP.

После проверки подлинности POP3-клиента сеанс клиента переходит в состояние транзакции. В этом состоянии клиент может скачивать и запрашивать удаление почты. В состоянии транзакции разрешено использовать команды LIST, STAT, RETR, DELE, RSET и QUIT. Обычно POP3-клиент отправляет команду STAT, за которой следует ряд команд RETR (по одной для каждого почтового элемента в его почтовом ящике).

Когда клиент выдает команду QUIT, сеанс POP3 переходит в состояние обновления, в котором он инициирует отключение TCP от сервера. Чтобы скачать почту позже, приложение POP3-клиента может в любое время вызвать `nx_pop3_client_mail_items_get` для проверки наличия новой почты в почтовом ящике.

### <a name="pop3-server-reply-codes"></a>Коды ответа POP3-сервера

- **+ OK**: сервер использует этот ответ для принятия команды клиента. Сервер может отправить дополнительные сведения после выдачи ответа "+ OK", но не может предположить, что клиент будет обрабатывать эту информацию, за исключением случаев скачивания данных почтового сообщения или команд LIST или DELE. В последнем случае "аргумент" после команды указывает на индекс почтового элемента в почтовом ящике клиента.

- **-ERR**: сервер использует этот ответ для отклонения команды клиента. Сервер может отправить дополнительные сведения после выдачи ответа "-ERR", но не может предположить, что клиент будет обрабатывать эту информацию.

### <a name="sample-pop3-client---server-session"></a>Пример сеанса POP3 для клиента и сервера

**Пример базового сеанса POP3 с использованием команд USER/PASS**

```c
S: <wait for connection on TCP port 110>
C: <open connection>
S: +OK POP3 server ready <1896.697170952@dbc.mtview.ca.us>
C: USER mrose
S: +OK mrose is valid
C: PASS mvan99
S: +OK mrose is logged in
C: STAT
S: +OK 2 320
C: RETR 1
S: +OK 120 octets
S: <the POP3 server sends message 1>
S: .
C: DELE 1
S: +OK message 1 deleted
C: RETR 2
S: +OK 200 octets
S: <the POP3 server sends message 2>
S: .
C: DELE 2
S: +OK message 2 deleted
C: QUIT
S: +OK POP3 server signing off (maildrop empty)
C: <close connection>
S: <wait for next connection>
```

**Пример базового сеанса POP3 с использованием команды APOP (и LIST вместо STAT)**

```c
S: <wait for connection on TCP port 110>
C: <open connection>
S: +OK POP3 server ready <1896.697170952@dbc.mtview.ca.us>
C: APOP mrose c4c9334bac560ecc979e58001b3e22fb
S: +OK mrose's maildrop has 2 messages (320 octets)
C: LIST
S: +OK 2 messages (320 octets)
S: 1 120
S: 2 200
S: .
C: RETR 1
S: +OK 120 octets
S: <the POP3 server sends message 1>
S: .
C: DELE 1
S: +OK message 1 deleted
C: RETR 2
S: +OK 200 octets
S: <the POP3 server sends message 2>
S: .
C: DELE 2
S: +OK message 2 deleted
C: QUIT
S: +OK dewey POP3 server signing off (maildrop empty)
C: <close connection>
S: <wait for next connection>
```

## <a name="rfcs-supported-by-netx-pop3-client"></a>Стандарты RFC, поддерживаемые POP3-клиентом NetX

Клиент-POP3 NetX соответствует стандарту RFC 1939.
