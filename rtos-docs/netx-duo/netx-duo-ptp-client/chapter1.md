---
title: Глава 1. Введение в PTP-клиент в NetX Duo для ОСРВ Azure
description: В этой главе приведен общий обзор PTP-клиента в NetX Duo для ОСРВ Azure.
author: v-condav
ms.author: v-condav
ms.date: 01/27/2021
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 5beec64bd6d74e3bed06be15255d6bd4a940ba64
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104814591"
---
# <a name="chapter-1---introduction-to-azure-rtos-netx-duo-ptp-client"></a>Глава 1. Введение в PTP-клиент в NetX Duo для ОСРВ Azure

PTP в NetX Duo для ОСРВ Azure реализует клиентскую часть протокола времени PTP (версия 2) согласно стандарту IEEE 1588-2008. Он используется для синхронизации часов между микроконтроллерами в локальной сети, которые взаимодействуют друг с другом через пакеты многоадресной рассылки IPv4 или IPv6.

## <a name="netx-duo-ptp-client-setup"></a>Настройка PTP-клиента в NetX Duo

Для правильной работы пакета PTP-клиента требуется, чтобы экземпляр IP-адреса в NetX Duo уже был создан.

По умолчанию PTP-клиент присоединяется к группе многоадресной рассылки IPv4. Чтобы запустить PTP-клиент в сети IPv6, нужно определить `NX_ENABLE_IPV6_MULTICAST` при сборке библиотеки NetX Duo.

При создании PTP-клиента приложение должно предоставить функцию обратного вызова для обработки меток времени входящих и исходящих пакетов. Для достижения высокого разрешения мы рекомендуем создавать метки времени с использованием таймера с высоким разрешением. Чтобы использовать PTP в симуляторе, предоставляется программная реализация `nx_ptp_client_soft_clock_callback`.

PTP-клиенту нужен пул пакетов для передачи сообщений PTP. Размер полезных данных пула пакетов должен быть не меньше `NX_UDP_PACKET + NX_PTP_CLIENT_PACKET_DATA_SIZE`, что составляет 108 байт для IPv4 или 128 байт для IPv6.

После создания PTP-клиента приложение может запустить этот PTP-клиент. Синхронизация выполняется во вспомогательном потоке PTP. Можно указать функцию обратного вызова для событий. Она будет вызываться, когда происходит одно из следующих событий:
* выбор основных часов; 
* калибровка времени;
* истечение времени ожидания основных часов.

## <a name="netx-duo-ptp-client-messages"></a>Сообщения PTP-клиента в NetX Duo

PTP-клиент в NetX Duo реализует только механизм запроса задержки и ответа. PTP-клиент открывает два UDP-порта: *319* для сообщений **событий** и *320* для **общих** сообщений. Существует пять типов сообщений для этого механизма:

* **Announce** (Объявление). Это сообщение события. Оно используется для выбора основных часов.
* **Sync** (Синхронизация). Это сообщение события. Оно используется для отправки отметок времени источника и вычисления задержки в пути от основных часов к клиенту.
* **Follow_Up** (Уточнение). Это общее событие. Оно является необязательным и используется для корретировки меток времени источника в связанном сообщении синхронизации. Оно отправляется только в том случае, если установлен флаг двухэтапной синхронизации в Sync.
* **Delay_Req** (Запрос задержки). Это сообщение события. Оно отправляется PTP-клиентом, чтобы при получении ответа Delay_Resp вычислить задержку в пути от клиента к основным часам.
* **Delay_Resp** (Ответ задержки). Это сообщение события. Оно отправляется от основных часов клиенту, чтобы он мог вычислить задержку в пути от клиента к основным часам.

*Обратите внимание, что в реализации отсутствует алгоритм выбора лучших основных часов. Когда PTP-клиент находится в состоянии прослушивания, он принимает только первый ответ от основных часов.*

## <a name="netx-duo-ptp-client-clock-callback"></a>Обратный вызов часов в PTP-клиенте в NetX Duo
Чтобы синхронизировать время по основным часам, PTP-клиенту требуются локальные часы. Функция обратного вызова часов передается в PTP-клиент во время его создания. Формат подпрограммы обратного вызова часов определен ниже.
```C
UINT ptp_clock_callback(
    NX_PTP_CLIENT *client_ptr, 
    UINT operation,
    NX_PTP_TIME *time_ptr, 
    NX_PACKET *packet_ptr,
    VOID *callback_data);
```
Входные параметры определяются следующим образом:
* *client_ptr* — указывает на PTP-клиент.
* *operation* — определяет операцию обратного вызова, допустимые значения которой приведены ниже.
  * **NX_PTP_CLIENT_CLOCK_INIT** — инициализация часов.
  * **NX_PTP_CLIENT_CLOCK_SET** — настройка текущей метки времени, которая задана `time_ptr`.
  * **NX_PTP_CLIENT_CLOCK_GET** — передача текущей метки времени в `time_ptr`.
  * **NX_PTP_CLIENT_CLOCK_PACKET_TS_EXTRACT** — извлечение метки времени из `packet_ptr` в `time_ptr`.
  * **NX_PTP_CLIENT_CLOCK_ADJUST** — корректировка текущей метки времени менее чем на *1* секунду.
  * **NX_PTP_CLIENT_CLOCK_PACKET_TS_PREPARE** — пометка `packet_ptr`, для которой нужно известить PTP-клиента о метке времени при ее передаче.
  * **NX_PTP_CLIENT_CLOCK_SOFT_TIMER_UPDATE** — обновление программного таймера. Может игнорироваться аппаратными часами.
* *time_ptr* — указывает на метку времени.
* *packet_ptr* — указывает на пакет.
* *callback_data* — указывает на непрозрачные данные обратного вызова.

Тип данных NX_PTP_TIME определяется следующим образом:
```C
typedef struct NX_PTP_TIME_STRUCT
{
    /* The MSB of the number of seconds */
    LONG  second_high;

    /* The LSB of the number of seconds */
    ULONG second_low;

    /* The number of nanoseconds */
    LONG  nanosecond;
} NX_PTP_TIME;
```

## <a name="netx-duo-ptp-client-event-callback"></a>Обратный вызов для события PTP-клиента в NetX Duo
Функция обратного вызова для события позволяет оповещать приложение о смене состояния PTP-клиента. Формат подпрограммы обратного вызова для события представлен ниже.
```C
UINT event_callback(
    NX_PTP_CLIENT *client_ptr, 
    UINT event, 
    VOID *event_data, 
    VOID *callback_data);
```
Принимаются следующие входные параметры:
* *client_ptr* — указывает на PTP-клиент.
* *event* — определяет событие для обратного вызова, для которого допускаются следующие значения:
  * **NX_PTP_CLIENT_EVENT_MASTER** — выбор основных часов;
  * **NX_PTP_CLIENT_EVENT_SYNC** — калибровка PTP-клиента;
  * **NX_PTP_CLIENT_EVENT_TIMEOUT** — истечение времени ожидания главных часов.
* *event_data* — определяет данные для конкретного события. Если это событие **NX_PTP_CLIENT_EVENT_MASTER**, event_data указывает на экземпляр `NX_PTP_CLIENT_MASTER`. Если это событие **NX_PTP_CLIENT_EVENT_SYNC**, event_data указывает на экземпляр `NX_PTP_CLIENT_SYNC`.

## <a name="netx-duo-ptp-client-communication"></a>Взаимодействие с PTP-клиентом в NetX Duo
Как уже упоминалось, PTP-клиент в NetX Duo поддерживает только механизм запроса задержки и ответа. Этот механизм измеряет задержку в пути между клиентом и основными часами следующим образом:
1. Клиент получает сообщение *Announce* от основных часов и сразу выбирает их как лучшие основные часы.
1. Клиент получает сообщение *Sync* от основных часов. Метка времени *t1* в этом сообщении обозначает метку времени источника. Метка времени *t2* заполняется по данным местных часов в момент получения сообщения.
1. Клиент получает сообщение *Follow_Up* от основных часов. Это необязательное сообщение, которое учитывается только при установленном флаге двухэтапной синхронизации в *Sync*. Теперь метка времени *t1* в этом сообщении заменяется меткой времени источника.
1. Клиент отправляет сообщение *Delay_Req* на основные часы. Метка времени *t3* заполняется по данным местных часов в момент отправки сообщения.
1. Клиент получает сообщение *Delay_Resp* от основных часов. Метка времени *t4* обозначает момент получения этого сообщения.

Вычисляется средняя задержка в пути, как показано ниже:
```C
<meanPathDelay>=[(t2-t1)+(t4-t3)]/2
```
Вычисляется смещение относительно основных часов, как показано ниже:
```C
<offsetFromMaster>=client_clock-master_clock
                  =(t2-t1)-<meanPathDelay>
```

> [!NOTE]
> *Значение ***correctionField** _ в этих вычислениях игнорируется._