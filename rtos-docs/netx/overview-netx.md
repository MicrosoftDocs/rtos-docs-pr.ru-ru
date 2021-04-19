---
title: Основные сведения о NetX в ОСРВ Azure
description: NetX в ОСРВ Azure — это высокопроизводительная реализация стандартов протокола TCP/IP, полностью интегрированная с ThreadX в ОСРВ Azure и доступная для всех поддерживаемых процессоров.
author: philmea
ms.author: philmea
ms.date: 5/19/2020
ms.service: rtos
ms.topic: overview
ms.openlocfilehash: 029f73b755d5c40279125fb010ec35baaaf7f38d
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104814443"
---
# <a name="overview-of-azure-rtos-netx"></a>Общие сведения о NetX в ОСРВ Azure

NetX в ОСРВ Azure — это сетевой стек TCP/IP промышленного класса с внедренным протоколом IPv4, предназначенный специально для глубоко встраиваемых приложений, работающих в реальном времени, и приложений Интернета вещей. NetX в ОСРВ Azure — это оригинальный сетевой стек IPv4 от Майкрософт. Он фактически является подмножеством ОСРВ Azure. NetX предоставляет встраиваемым приложениям базовые сетевые протоколы, такие как IPv4, TCP и UDP, а также полный набор дополнительных подключаемых протоколов. Небольшой объем занимаемой памяти, высокая производительность и непревзойденная простота использования делают NetX в ОСРВ Azure идеальным вариантом при использовании самых требовательных встраиваемых приложений Интернета вещей.

## <a name="api-protocols"></a>Протоколы API

### <a name="telnet"></a>TELNET

* Минимум занимаемой памяти — 0,5 КБ и 0,3 КБ в ОЗУ.
* Поддержка клиентов и серверов.
* Интуитивно простые API-интерфейсы Telnet: *nx_telnet_\** .

### <a name="auto-ip"></a>Auto IP

* Автоматическое назначение IPv4-адресов.
* Минимум занимаемой памяти — 1,2 КБ и 300 байт в ОЗУ.
* Интуитивно простые API-интерфейсы AutoIP: *nx_autoip_\** .

### <a name="http---hypertext-transfer-protocolhttp"></a>HTTP

* Минимум занимаемой памяти — флэш-память: 2,8–4,8 КБ; ОЗУ: 0,4 –1,0 КБ.
* Поддержка клиентов и серверов.
* Интуитивно простые API-интерфейсы HTTP: *nx_http_\** .

### <a name="smtp---simple-mail-transfer-protocol-smtp"></a>SMTP

* Минимум занимаемой памяти — 4,1 КБ и 0,6 КБ в ОЗУ.
* Поддержка клиентов.
* Интуитивно простые API-интерфейсы SMTP: *nx_smtp_\** .

### <a name="dhcp---dynamic-host-configuration-protocol-dhcp"></a>DHCP (протокол динамической конфигурации узла)

* Минимум занимаемой памяти — флэш-память: 3,6–4,6 КБ; ОЗУ: 2,7 КБ.
* Поддержка клиентов и серверов.
* Поддержка IPv4.
* Интуитивно простые API-интерфейсы DHCP: *nx_dhcp_\** .

### <a name="p0p3---post-office-protocol-version-3-pop3"></a>POP3

* Минимум занимаемой памяти — 8,1 КБ и 1,4 КБ в ОЗУ.
* Поддержка клиентов.
* Интуитивно простые API-интерфейсы P0P3: *nx_pop3_\** .

### <a name="snmp---simple-network-management-protocol-snmp"></a>SNMP

* Минимум занимаемой памяти — 10,9 КБ и 2,6 КБ в ОЗУ.
* Поддержка агентов для VI, V2 и V3.
* Интуитивно простые API-интерфейсы SNMP: *nx_snmp_\** .

### <a name="ftp-tftp---file-transfer-protocol-ftp-trivial-file-transfer-protocol-tftp"></a>FTP и TFTP

* Минимум занимаемой памяти FTP — флэш-память: 1,8–7,2 КБ; ОЗУ: 0,6–2,1 КБ.
* Минимум занимаемой памяти TFTP — флэш-память: 1,7–2,4 КБ; ОЗУ: 0,3–1,8 КБ.
* Поддержка клиентов и серверов.
* Интуитивно простые API-интерфейсы FTP и TFTP: <i>nx_ftp_ *</i> или <i>nx_tftp_* </i>.

### <a name="ppp---polnt-to-point-protocol-ppp"></a>PPP

* Минимум занимаемой памяти — 7,1 КБ и 3,8 КБ в ОЗУ.
* Интуитивно простые API-интерфейсы PPP: *nx_ppp_\** .

### <a name="sntp---simple-network-time-protocol-sntp"></a>SNTP

* Минимум занимаемой памяти — 4 КБ и 0,5 КБ в ОЗУ.
* Поддержка клиентов.
* Интуитивно простые API-интерфейсы SNTP: *nx_sntp_\** .

### <a name="azure-rtos-netx-api"></a>API NetX в ОСРВ Azure

* Интуитивно простой и согласованный API.
* Соглашение об именовании с использованием конструкции "существительное — глагол".
* Быстрая реализация API без копирования.
* Все функции API имеют префикс <i>nx_*</i> для простой идентификации принадлежности к NetX в ОСРВ Azure.
* Блокирующие функции API дополнительно поддерживают ожидание потока в определенном интервале времени.
* Дополнительный уровень BSD для переноса старого кода сокетов.

### <a name="igmp---internet-group-management-protocol-igmp"></a>IGMP

* Минимум занимаемой флэш-памяти — 2,5 КБ.
* Поддержка групп многоадресной рассылки IPv4.
* Пройдена проверка IXIA IxANVL.
* Статистика IGMP (необязательно).
* Трассировка на уровне системы через TraceX для ОСРВ Azure.
* Интуитивно простые API-интерфейсы IGMP: *nx_igmp_\** .

### <a name="udp---user-datagram-protocol-udp"></a>UDP

* Минимум занимаемой памяти — флэш-память: 2,5 КБ; ОЗУ: 124 байт на сокет.
* Быстрая обработка пакетов TCP на уровне проводной сети:
* RX на уровне 95 Мбит/с в сети Ethernet со скоростью 100 Мбит/с при частоте MCU @100MHz и использовании MCU на уровне 14 %.
* TX на уровне 94 Мбит/с в сети Ethernet со скоростью 100 Мбит/с при частоте MCU @100MHz и использовании MCU на уровне 10 %.
* Технология UDP Fast Path™.
* Отсутствие ограничений на число UDP.
* Пройдена проверка IXIA IxANVL.
* Приостановка при получении в сокет (необязательно).
* Время ожидания при всех приостановках (необязательно).
* Статистика UDP (необязательно).
* Трассировка на уровне системы через TraceX для ОСРВ Azure.
* Интуитивно простые API-интерфейсы UDP: *nx_udp_\** .

### <a name="tcp---transmission-control-protocol-tcp"></a>TCP

* Минимум занимаемой памяти — флэш-память: 10,5–12,5 КБ; ОЗУ: 280 байт на сокет.
* Быстрая обработка пакетов TCP на уровне проводной сети:
* RX на уровне 93 Мбит/с в сети Ethernet со скоростью 100 Мбит/с при частоте MCU @100MHz и использовании MCU на уровне 20 %.
* TX на уровне 94 Мбит/с в сети Ethernet со скоростью 100 Мбит/с при частоте MCU @100MHz и использовании MCU на уровне 27 %.
* Надежное подключение.
* Отсутствие ограничений на число TCP-сокетов.
* Пройдена проверка IXIA IxANVL.
* Приостановка при получении или отправке в сокете (необязательно).
* Время ожидания при всех приостановках (необязательно).
* Статистика TCP (необязательно).
* Трассировка на уровне системы через TraceX для ОСРВ Azure.
* Интуитивно простые API-интерфейсы TCP: *nx_tcp_\** .

### <a name="icmp---internet-control-message-protocol-icmp"></a>ICMP

* Минимум занимаемой флэш-памяти — 2,5 КБ.
* Поддержка IPv4.
* Пройдена проверка IXIA IxANVL.
* Запрос проверки связи и ответ на нее.
* Приостановка потока при запросах проверки связи (необязательно).
* Время ожидания при всех приостановках (необязательно).
* Статистика ICMP (необязательно).
* Трассировка на уровне системы через TraceX для ОСРВ Azure.
* Интуитивно простые API-интерфейсы ICMP: *nx_icmp_\** .

### <a name="ipv4---internet-protocol-ip"></a>IPv4 (IP)

* Минимум занимаемой памяти — флэш-память: 3,5–8,5 КБ; ОЗУ: 2–3 КБ.
* Архитектура Piconet™.
* Высокая производительность на уровне проводной сети.
* Поддержка нескольких интерфейсов
* Поддержка многосетевых интерфейсов.
* Поддержка статической маршрутизации.
* Поддержка фрагментирования и пересборки IP.
* Поддержка IPv4.
* Пройдена проверка IXIA IxANVL.
* Сертификация с логотипом Phase II Ready.
* Статистика IP (необязательно).
* Хорошо определенный, интуитивно простой интерфейс драйвера физического уровня.
* Трассировка на уровне системы через TraceX для ОСРВ Azure.
* Интуитивно простые API-интерфейсы уровня IP: *nx_ip_\** , *nxd_ip_\** .
* Предварительная сертификация TUV и UL на соответствие стандартам IEC 61508 SIL 4, IEC 62304 (класс C), ISO 26262 ASIL D и EN 50128 SW-SIL4.

### <a name="arprarp---address-resolution-protocol-arp-reverse-address-resolution-protocol-rarp"></a>ARP и RARP

* Минимум занимаемой памяти — флэш-память и ОЗУ: 1,7 КБ.
* Динамическое разрешение 32-разрядных IPv4-адресов и 48-разрядных MAC-адресов.
* Пройдена проверка IXIA IxANVL.
* Гибкий, определяемый пользователем кэш ARP.
* Поддержка самообращенных запросов ARP.
* Статистика ARP/RARP, определяемая приложением (необязательно).
* Трассировка на уровне системы через TraceX для ОСРВ Azure.
* Интуитивно простые API-интерфейсы ARP/RARP: *nx_arp_\** , *nx_rarp_\** .

### <a name="ethernet-wifi-bluetooth-le-154-etc"></a>ETHERNET, WiFi, BLUETOOTH LE, 15.4 и т. д.

## <a name="small-footprint"></a>Небольшой объем занимаемой памяти

NetX в ОСРВ Azure занимает всего 9–15 КБ при базовой поддержке IP и UDP. Для функций TCP требуются дополнительно 10–13 КБ памяти области инструкций. NetX в ОСРВ Azure обычно занимает в ОЗУ от 2,6 до 3,6 КБ, а также некоторый объем памяти пула пакетов, который определяется приложением. Как и с ThreadX в ОСРВ Azure, размер NetX в ОСРВ Azure масштабируется автоматически в зависимости от служб, используемых приложением. Это практически устраняет потребность в сложной настройке и параметрах сборки, что упрощает работу разработчика.

## <a name="fast-execution"></a>Быстрое выполнение

NetX в ОСРВ Azure предоставляет реализацию отправки и получения пакетов без копирования. Решение тесно интегрировано со ThreadX в ОСРВ Azure для достижения максимальной возможной производительности. Например, NetX в ОСРВ Azure может достигать скоростей передачи данных, сравнимых с проводной сетью, с процессором с частотой 80 МГц (или менее), используя лишь незначительную часть его ресурсов.

## <a name="simple-easy-to-use"></a>Простота использования

NetX в ОСРВ Azure очень легко использовать. API NetX в ОСРВ Azure предоставляет интуитивно простые, но широкие функции. Имена API состоят из реальных слов, а не из набора букв или сокращенных имен, которые часто используются для других сетевых продуктов. Все API-интерфейсы NetX в ОСРВ Azure имеют префикс *nx_* и соответствуют соглашению об именовании с использованием конструкций "существительное — глагол". Более того, для API обеспечивается функциональная согласованность. Например, для всех функций API, которые поддерживают приостановку работы, реализована необязательная возможность использовать время ожидания. Возможность функционирует одинаково для всех функций. Для устаревших приложений NetX в ОСРВ Azure предлагает дополнительный уровень, совместимый с сокетами BSD. Этот уровень помогает разработчикам переносить крупные сетевые приложения.

## <a name="interoperability-verification"></a>Подтвержденная совместимость

NetX в ОСРВ Azure соответствует стандартам RFC и обеспечивает полную совместимость с устройствами большинства поставщиков. Кроме того, NetX в ОСРВ Azure использует стандартную отраслевую библиотеку IxANVL для базовой реализации протокола TCP/IP в NetX в ОСРВ Azure.

## <a name="advanced-technology"></a>Передовая технология

NetX в ОСРВ Azure — эта передовая технология, включающая следующее:

* архитектуру Piconet™;
* автоматическое масштабирование
* технологию UDP Fast-Path™;
* гибкое управление пакетами;
* API и реализациб без копирования;
* поддержку многосетевых интерфейсов;
* время ожидания при всех приостановках (необязательно);
* поддержку статической маршрутизации;
* поддержку системного анализа TraceX для ОСРВ Azure.

## <a name="fastest-time-to-market"></a>Минимальное время выхода на рынок

NetX в ОСРВ Azure легко устанавливать, изучать, использовать, отлаживать, проверять, сертифицировать и обслуживать. Поэтому NetX в ОСРВ Azure является одним из самых популярных стеков TCP/IP для встраиваемых устройств Интернета вещей, в том числе для множества интегральных систем от Broadcom, Gainspan и т. д. Наше преимущество по стабильно малому времени выхода на рынок обусловлено следующим:

* документация высокого качества — ознакомьтесь с нашим [руководством пользователя по NetX в ОСРВ Azure](about-this-guide.md) и убедитесь в этом сами;
* полная доступность исходного кода;
* простые в использовании API-интерфейсы;
* комплексный и широкий набор функций.

## <a name="one-simple-license"></a>Одна простая лицензия

Использование и тестирование исходного кода не требуют каких-либо затрат, как и лицензии для рабочей среды при развертывании на предварительно лицензированных устройствах. Для всех других устройств требуется лишь простая лицензия сроком на один год.

## <a name="full-highest-quality-source-code"></a>Полный и высококачественный исходный код

Уже несколько лет NetX в ОСРВ Azure задает стандарт качества и простоты понимания. Кроме того, соглашение по назначению каждой функции отдельного файла упрощает навигацию по исходному коду.

## <a name="supports-most-popular-architectures"></a>Поддержка большинства популярных архитектур

NetX в ОСРВ Azure работает с большинством популярных 32/64-разрядных микропроцессоров. Этот стек не требует дополнительной настройки, полностью протестирован и поддерживается в том числе в следующих передовых архитектурах:

**Analog Devices**: SHARC, Blackfin, CM4xx;

**Andes Core**: RISC-V;

**Ambiqmicro**: микроконтроллеры Apollo;

**ARM**: ARM7, ARM9, ARM11, Cortex-M0/M3/M4/M7/A15/A5/A7/A8/A9/A5x 64-bi/A7x 64-bit/R4/R5, TrustZone ARMv8-M;

**Cadence**: Xtensa, Diamond;

**CEVA**: PSoC, PSoC 4, PSoC 5, PSoC 6, FM0+, FM3, MF4, WICED WiFi;

**Cypress**: RISC-V;

**EnSilica**: eSi-RISC;

**Infineon**: XMC1000, XMC4000, TriCore;

**Intel; ППВМ Intel**: x36/Pentium, XScale, NIOS II, Cyclone, Arria 10;

**Microchip**: AVR32, ARM7, ARM9, Cortex-M3/M4/M7, SAM3/4/7/9/A/C/D/E/G/L/SV, PIC24/PIC32;

**Microsemi**: RISC-V;

**NXP**: LPC, ARM7, ARM9, PowerPC, 68 K, i.MX, ColdFire, Kinetis Cortex-M3/M4;

**Renesas**: SH, HS, V850, RX, RZ, Synergy;

**Silicon Labs**: EFM32;

**Synopsys**: ARC 600, 700, ARC EM, ARC HS;

**ST**: STM32, ARM7, ARM9, Cortex-M3/M4/M7;

**Tl**: C5xxx, C6xxx, Stellaris, Sitara, Tiva-C;

**Wave Computing**: MIPS32 4K, 24 K, 34 K, 1004 K, MIPS64 5K, microAptiv, interAptiv, proAptiv, M-Class;

**Xilinx**: MicroBlaze, PowerPC 405, ZYNQ, ZYNQ UltraSCALE.

*Все приведенные сведения о времени ожидания и размере могут отличаться от фактических. Это зависит от платформы разработки.*