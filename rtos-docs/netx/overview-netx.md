---
title: Основные сведения о NetX в ОСРВ Azure
description: NetX в ОСРВ Azure — это высокопроизводительная реализация стандартов протокола TCP/IP, полностью интегрированная с ThreadX в ОСРВ Azure и доступная для всех поддерживаемых процессоров.
author: philmea
ms.author: philmea
ms.date: 5/19/2020
ms.service: rtos
ms.topic: overview
ms.openlocfilehash: a5dd020daeb336f264bf611fc3a515e55dbc5dab6f9afbcfdbf3733baa66de26
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2021
ms.locfileid: "116782784"
---
# <a name="overview-of-azure-rtos-netx"></a>Общие сведения о NetX в ОСРВ Azure

NetX в ОСРВ Azure — это сетевой стек TCP/IP промышленного класса с внедренным протоколом IPv4, предназначенный специально для глубоко встраиваемых приложений, работающих в реальном времени, и приложений Интернета вещей. NetX в ОСРВ Azure — это оригинальный сетевой стек IPv4 от Майкрософт. Он фактически является подмножеством ОСРВ Azure. NetX предоставляет встраиваемым приложениям базовые сетевые протоколы, такие как IPv4, TCP и UDP, а также полный набор дополнительных подключаемых протоколов. Небольшой объем занимаемой памяти, высокая производительность и непревзойденная простота использования делают NetX в ОСРВ Azure идеальным вариантом при использовании самых требовательных встраиваемых приложений Интернета вещей.

## <a name="api-protocols"></a>Протоколы API
ОСРВ Azure NetX обеспечивает поддержку следующего.

### <a name="telnet"></a>TELNET

* Минимум занимаемой памяти — 0,5 КБ и 0,3 КБ в ОЗУ.
* Поддержка клиентов и серверов.

### <a name="auto-ip"></a>Auto IP

* Автоматическое назначение IPv4-адресов.
* Минимум занимаемой памяти — 1,2 КБ и 300 байт в ОЗУ.

### <a name="http---hypertext-transfer-protocolhttp"></a>HTTP

* Минимум занимаемой памяти — флэш-память: 2,8–4,8 КБ; ОЗУ: 0,4 –1,0 КБ.
* Поддержка клиентов и серверов.

### <a name="smtp---simple-mail-transfer-protocol-smtp"></a>SMTP

* Минимум занимаемой памяти — 4,1 КБ и 0,6 КБ в ОЗУ.
* Поддержка клиентов.

### <a name="dhcp---dynamic-host-configuration-protocol-dhcp"></a>DHCP (протокол динамической конфигурации узла)

* Минимум занимаемой памяти — флэш-память: 3,6–4,6 КБ; ОЗУ: 2,7 КБ.
* Поддержка клиентов и серверов.
* Поддержка IPv4.

### <a name="p0p3---post-office-protocol-version-3-pop3"></a>POP3

* Минимум занимаемой памяти — 8,1 КБ и 1,4 КБ в ОЗУ.
* Поддержка клиентов.

### <a name="snmp---simple-network-management-protocol-snmp"></a>SNMP

* Минимум занимаемой памяти — 10,9 КБ и 2,6 КБ в ОЗУ.
* Поддержка версий 1, 2 и 3.

### <a name="ftp-tftp---file-transfer-protocol-ftp-trivial-file-transfer-protocol-tftp"></a>FTP и TFTP

* Минимум занимаемой памяти FTP — флэш-память: 1,8–7,2 КБ; ОЗУ: 0,6–2,1 КБ.
* Минимум занимаемой памяти TFTP — флэш-память: 1,7–2,4 КБ; ОЗУ: 0,3–1,8 КБ.
* Поддержка клиентов и серверов.

### <a name="ppp---polnt-to-point-protocol-ppp"></a>PPP

* Минимум занимаемой памяти — 7,1 КБ и 3,8 КБ в ОЗУ.
* Отказоустойчивость и надежность.

### <a name="sntp---simple-network-time-protocol-sntp"></a>SNTP

* Минимум занимаемой памяти — 4 КБ и 0,5 КБ в ОЗУ.
* Поддержка клиентов.

### <a name="azure-rtos-netx-api"></a>API NetX в ОСРВ Azure

* Быстрая реализация API без копирования.
* Дополнительный уровень BSD для переноса старого кода сокетов.

### <a name="igmp---internet-group-management-protocol-igmp"></a>IGMP

* Минимум занимаемой флэш-памяти — 2,5 КБ.
* Поддержка групп многоадресной рассылки IPv4.
* Пройдена проверка IXIA IxANVL.
* Статистика IGMP (необязательно).
* Трассировка на уровне системы через TraceX для ОСРВ Azure.

### <a name="udp---user-datagram-protocol-udp"></a>UDP

* Минимум занимаемой памяти — флэш-память: 2,5 КБ; ОЗУ: 124 байт на сокет.
* Быстрая обработка пакетов TCP на уровне проводной сети:
* RX на уровне 95 Мбит/с в сети Ethernet со скоростью 100 Мбит/с при частоте MCU @100MHz и использовании MCU на уровне 14 %.
* TX на уровне 94 Мбит/с в сети Ethernet со скоростью 100 Мбит/с при частоте MCU @100MHz и использовании MCU на уровне 10 %.
* Технология UDP Fast Path™.
* Отсутствие ограничений на число UDP.
* Пройдена проверка IXIA IxANVL.
* Приостановка при получении в сокет (необязательно).
* Опциональное время ожидания при всех приостановках.
* Статистика UDP (необязательно).
* Трассировка на уровне системы через TraceX для ОСРВ Azure.

### <a name="tcp---transmission-control-protocol-tcp"></a>TCP

* Минимум занимаемой памяти — флэш-память: 10,5–12,5 КБ; ОЗУ: 280 байт на сокет.
* Быстрая обработка пакетов TCP на уровне проводной сети:
* RX на уровне 93 Мбит/с в сети Ethernet со скоростью 100 Мбит/с при частоте MCU @100MHz и использовании MCU на уровне 20 %.
* TX на уровне 94 Мбит/с в сети Ethernet со скоростью 100 Мбит/с при частоте MCU @100MHz и использовании MCU на уровне 27 %.
* Надежное подключение.
* Отсутствие ограничений на число TCP-сокетов.
* Пройдена проверка IXIA IxANVL.
* Приостановка при получении или отправке в сокете (необязательно).
* Опциональное время ожидания при всех приостановках.
* Статистика TCP (необязательно).
* Трассировка на уровне системы через TraceX для ОСРВ Azure.

### <a name="icmp---internet-control-message-protocol-icmp"></a>ICMP

* Минимум занимаемой флэш-памяти — 2,5 КБ.
* Поддержка IPv4.
* Пройдена проверка IXIA IxANVL.
* Запрос проверки связи и ответ на нее.
* Опциональная приостановка потока при запросах проверки связи.
* Опциональное время ожидания при всех приостановках.
* Статистика ICMP (необязательно).
* Трассировка на уровне системы через TraceX для ОСРВ Azure.

### <a name="ipv4---internet-protocol-ip"></a>IPv4 (IP)

* Минимум занимаемой памяти — флэш-память: 3,5–8,5 КБ; ОЗУ: 2–3 КБ.
* Архитектура Piconet™.
* Высокая производительность на уровне проводной сети.
* Поддержка нескольких интерфейсов.
* Поддержка множественной адресации.
* Поддержка статической маршрутизации.
* Поддержка фрагментирования и пересборки IP.
* Поддержка IPv4.
* Пройдена проверка IXIA IxANVL.
* Сертификация по программе Ready Logo Phase-2.
* Статистика IP (необязательно).
* Хорошо определенный, интуитивно простой интерфейс драйвера физического уровня.
* Трассировка на уровне системы через TraceX для ОСРВ Azure.

### <a name="arprarp---address-resolution-protocol-arp-reverse-address-resolution-protocol-rarp"></a>ARP и RARP

* Минимум занимаемой памяти — флэш-память и ОЗУ: 1,7 КБ.
* Динамическое разрешение 32-разрядных IPv4-адресов и 48-разрядных MAC-адресов.
* Пройдена проверка IXIA IxANVL.
* Гибкий кэш ARP, определяемый пользователем.
* Поддержка самообращенных запросов ARP.
* Статистика ARP и RARP, определяемая приложением (необязательно).
* Трассировка на уровне системы через TraceX для ОСРВ Azure.

### <a name="ethernet-wifi-bluetooth-le-154-etc"></a>ETHERNET, WiFi, BLUETOOTH LE, 15.4 и т. д.

## <a name="interoperability-verification"></a>Подтвержденная совместимость

ОСРВ Azure NetX соответствует стандартам RFC и обеспечивает полную совместимость с устройствами большинства поставщиков. Кроме того, NetX в ОСРВ Azure использует стандартную отраслевую библиотеку IxANVL для базовой реализации протокола TCP/IP в NetX в ОСРВ Azure.

## <a name="advanced-technology"></a>Передовая технология

NetX в ОСРВ Azure — эта передовая технология, включающая следующее:
* архитектуру Piconet™;
* автоматическое масштабирование;
* технологию UDP Fast-Path™;
* гибкое управление пакетами;
* API и реализацию без копирования;
* поддержку множественной адресации;
* время ожидания при всех приостановках (необязательно);
* поддержку статической маршрутизации;
* поддержку системного анализа TraceX для ОСРВ Azure.
