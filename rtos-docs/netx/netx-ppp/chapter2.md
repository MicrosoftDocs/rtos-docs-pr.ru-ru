---
title: Глава 2. Установка и использование компонента Point-to-Point Protocol (PPP) NetX для ОСРВ Azure
description: В этой главе описываются различные проблемы, связанные с установкой, настройкой и использованием компонента Point-to-Point Protocol (PPP) NetX для ОСРВ Azure.
author: philmea
ms.author: philmea
ms.date: 06/04/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: b62ca837cadd5f7bee2686ab566ff133f1088ff7ba8b572e372e5051b7bbaab9
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2021
ms.locfileid: "116791097"
---
# <a name="chapter-2---installation-and-use-of-azure-rtos-netx-point-to-point-protocol-ppp"></a>Глава 2. Установка и использование компонента Point-to-Point Protocol (PPP) NetX для ОСРВ Azure

В этой главе описываются различные проблемы, связанные с установкой, настройкой и использованием компонента Point-to-Point Protocol (PPP) NetX для ОСРВ Azure.

## <a name="product-distribution"></a>Распространение продукта

Пакет Point-to-Point Protocol (PPP) NetX для ОСРВ Azure доступен по адресу <https://github.com/azure-rtos/netx>. В состав пакета входят следующие файлы.

- **nx_ppp.h**: файл заголовка для PPP для NetX
- **nx_ppp.c**: исходный файл для PPP для NetX
- **nx_ppp.pdf**: описание PPP для NetX в формате PDF
- **demo_netx_ppp.c**: демонстрация PPP для NetX

## <a name="ppp-installation"></a>Установка PPP

Чтобы использовать PPP для NetX, весь дистрибутив, упомянутый ранее, должен быть скопирован в тот же каталог, где установлен NetX. Например, если NetX установлен в каталоге *\threadx\arm7\green*, то файлы *nx_ppp.h* и *nx_ppp.c* необходимо скопировать в этот каталог.

## <a name="using-ppp"></a>Использование PPP

Использовать PPP для NetX просто. В код приложения следует добавить *nx_ppp.h* после *tx_api.h* и *nx_api.h*, чтобы использовать ThreadX и NetX соответственно. После добавления файла *nx_ppp.h* код приложения может выполнять вызовы функций PPP, указанные далее в этом разделе. В приложение также следует включить файл *nx_ppp.c* в процессе сборки. Этот файл должен быть скомпилирован так же, как и другие файлы приложения, а его объектная форма должна быть связана с файлами приложения. Это все, что необходимо для использования PPP NetX.

## <a name="using-modems"></a>Использование модемов

Если для подключения к Интернету требуется модем, для использования продукта PPP необходимо учитывать некоторые особенности. По сути, использование модема вводит дополнительную логику инициализации и логику потери связи. Кроме того, большая часть дополнительной логики модема реализуется вне контекста PPP NetX. Основной процесс использования PPP NetX с модемом выглядит примерно так, как описано ниже.

1. Инициализация модема

1. Установка связи с поставщиком услуг Интернета (ISP)

1. Ожидание подключения

1. Ожидание запроса на ввод идентификатора пользователя

1. Запуск PPP NetX [PPP работает]

1. Потеря связи

1. Остановка PPP NetX (или перезапуск с помощью nx_ppp_restart)

### <a name="initialize-modem"></a>Инициализация модема

Используя низкоуровневую подпрограмму последовательного вывода приложения, модем инициализируется с помощью ряда команд с ASCII-символами (дополнительные сведения см. в документации по модему).

### <a name="dial-internet-service-provider"></a>Установка связи с поставщиком услуг Интернета

Низкоуровневая подпрограмма последовательного вывода указывает модему установить связь с поставщиком услуг Интернета. Например, ниже приведена стандартная строка ASCII для установки связи с поставщиком услуг Интернета по номеру телефона 123-4567.

"ATDT123456\r"

### <a name="wait-for-connection"></a>Ожидание подключения

Теперь приложение ожидает от модема сообщения о том, что подключение установлено. Чтобы проверить состояние, нужно взглянуть на символы, выведенные низкоуровневой подпрограммой последовательного вывода приложения. Как правило, модемы возвращают строку ASCII "CONNECT", если подключение установлено.

### <a name="wait-for-user-id-prompt"></a>Ожидание запроса на ввод идентификатора пользователя

После установки подключения приложение ожидает начальный запрос имени входа от поставщика услуг Интернета. Обычно он имеет вид строки ASCII, например "Login?".

### <a name="start-netx-ppp"></a>Запуск PPP NetX

На этом этапе можно запустить PPP NetX. Для этого вызывается служба *nx_ppp_create*, а затем — служба *nx_ip_create*. Кроме того, могут потребоваться дополнительные службы для включения PAP и настройки IP-адресов PPP. Дополнительные сведения можно найти в следующих разделах этого руководства.

### <a name="loss-of-communication"></a>Потеря связи

После запуска PPP все сведения, не относящиеся к PPP, передаются в подпрограмму обработки недопустимых пакетов, указанную приложением в службе *nx_ppp_create*. Как правило, при потере связи с поставщиком услуг Интернета модемы отправляют строку ASCII, такую как "NO CARRIER". Когда приложение получает пакет, не относящийся к PPP, с такой информацией, оно должно либо остановить экземпляр PPP NetX, либо перезапустить конечный автомат PPP с помощью API *nx_ppp_restart*.

### <a name="stop-netx-ppp"></a>Остановка PPP NetX

Остановить PPP NetX довольно просто. По сути, все созданные сокеты должны быть освобождены и удалены. Затем следует удалить экземпляр IP-адреса с помощью службы *nx_ip_delete*. После удаления экземпляра IP необходимо вызвать службу *nx_ppp_delete*, чтобы завершить процесс остановки PPP. Теперь приложение может попытаться восстановить связь с поставщиком услуг Интернета.

## <a name="small-example-system"></a>Пример небольшой системы

Пример, демонстрирующий, насколько просто использовать PPP NetX, приведен в блоке 1.1 ниже. В этом примере файл включения PPP *nx_ppp.h* находится на строке 3. Затем PPP создается в *tx_application_define* в строке 56. Блок управления PPP *my_ppp* был определен ранее в качестве глобальной переменной в строке 9. 

>[!NOTE]
>PPP следует создать до создания экземпляра IP. После успешного создания PPP и IP поток *my_thread* ожидает перехода канала PPP в активное состояние в строке 98. В строке 104 и PPP, и NetX являются полностью работоспособными.

Одним из элементов, не показанных в этом примере, является получение последовательного байта ISR. Необходимо вызвать *nx_ppp_byte_receive* с параметром *my_ppp* и байтом, полученным в качестве входных параметров.

```c
#include   "tx_api.h"
#include   "nx_api.h"
#include   "nx_ppp.h"

#define     DEMO_STACK_SIZE         4096
TX_THREAD               my_thread;
NX_PACKET_POOL          my_pool;
NX_IP                   my_ip;
NX_PPP                  my_ppp;

/* Define function prototypes. */

void    my_thread_entry(ULONG thread_input);
void    my_serial_driver_byte_output(UCHAR byte);
void    my_invalid_packet_handler(NX_PACKET *packet_ptr);
 
/* Define main entry point. */
intmain()
{

    /* Enter the ThreadX kernel. */
    tx_kernel_enter();
 }


/* Define what the initial system looks like. */

void    tx_application_define(void *first_unused_memory)
{

CHAR    *pointer;
UINT    status;

/* Setup the working pointer. */
pointer =  (CHAR *) first_unused_memory;

/* Create “my_thread”. */
    tx_thread_create(&my_thread, "my thread", my_thread_entry, 0,  
                  pointer, DEMO_STACK_SIZE, 
                  2, 2, TX_NO_TIME_SLICE, TX_AUTO_START);
    pointer =  pointer + DEMO_STACK_SIZE;

    /* Initialize the NetX system. */
    nx_system_initialize();

    /* Create a packet pool. */
    status =  nx_packet_pool_create(&my_pool, "NetX Main Packet Pool", 
                                    1024, pointer, 64000);
    pointer = pointer + 64000;

    /* Check for pool creation error. */
    if (status)
        error_counter++;

    /* Create a PPP instance. */
    status = nx_ppp_create(&my_ppp, “My PPP”, &my_ip, pointer, 1024, 2, 
                           &my_pool, my_invalid_packet_handler, my_serial_driver_byte_output);
    pointer =  pointer + 1024;
    /* Check for PPP creation pool. */
    if (status)
        error_counter++;

    /* Create an IP instance with the PPP driver. */
    status = nx_ip_create(&my_ip,"My NetX IP Instance", 
                           IP_ADDRESS(216,2,3,1), 0xFFFFFF00, &my_pool, 
                           nx_ppp_driver, pointer, DEMO_STACK_SIZE, 1);
    pointer =  pointer + DEMO_STACK_SIZE;

    /* Check for IP create errors. */
    if (status)
        error_counter++;

    /* Enable ICMP for my IP Instance. */
    status =  nx_icmp_enable(&my_ip);

    /* Check for ICMP enable errors. */
    if (status)
        error_counter++;

    /* Enable UDP. */
    status =  nx_udp_enable(&my_ip);
    if (status)
        error_counter++;
}

/* Define my thread. */
void    my_thread_entry(ULONG thread_input)
{

UINT        status;
ULONG       ip_status;
NX_PACKET   *my_packet;

/* Wait for the PPP link in my_ip to become enabled. */
    status =  nx_ip_status_check(&my_ip,NX_IP_LINK_ENABLED,&ip_status,3000);

    /* Check for IP status error. */
    if (status) 
        return;

    /* Link is fully up and operational. All NetX activities 
    are now available. */

}
```
## <a name="configuration-options"></a>Параметры конфигурации

Существует ряд параметров конфигурации для использования PPP для NetX. Их подробное описание приводится ниже.

- **NX_DISABLE_ERROR_CHECKING** — если определен, этот параметр удаляет базовую проверку ошибок PPP. Обычно он используется после отладки приложения.
- **NX_PPP_PPPOE_ENABLE** — если определен, PPP может передавать пакеты через Ethernet.
- **NX_PPP_BASE_TIMEOUT** — определяет частоту периода (в тактах таймера) активации задачи потока PPP для проверки на наличие событий PPP. Значение по умолчанию — 1*NX_IP_PERIODIC_RATE (100 тактов).
- **NX_PPP_DISABLE_INFO** — если определен, сбор внутренних данных PPP отключен.
- **NX_PPP_DEBUG_LOG_ENABLE** — если определен, включен внутренний журнал отладки PPP.
- **NX_PPP_DEBUG_LOG_PRINT_ENABLE** — если определен, включен внутренний журнал отладки PPP для функции *printf* в *stdio*. Этот параметр допустим, только если также включен журнал отладки.
- **NX_PPP_DEBUG_LOG_SIZE** — размер журнала отладки (число записей в журнале отладки). При достижении последней записи запись отладки переносится к первой записи и перезаписывает все записанные ранее данные. Значение по умолчанию — 50.
- **NX_PPP_DEBUG_FRAME_SIZE** — максимальный объем данных, захваченных из полезных данных полученного пакета и сохраненных в выходных данных отладки. Значение по умолчанию — 50.
- **NX_PPP_DISABLE_CHAP** — если определен, удаляется внутренняя логика CHAP PPP, включая логику дайджеста MD5.
- **NX_PPP_DISABLE_PAP** — если определен, удаляется внутренняя логика PAP PPP.
- **NX_PPP_DNS_OPTION_DISABLE** — если определен, параметр DNS в ответе IPCP отключен.  По умолчанию этот параметр не определен (параметр DNS задан).
- **NX_PPP_DNS_ADDRESS_MAX_RETRIES** — указывает, сколько раз узел PPP будет запрашивать адрес DNS-сервера от однорангового узла в состоянии IPCP. Этот параметр не работает, если определен параметр NX_PPP_DNS_OPTION_DISABLE. Значение по умолчанию — 2.
- **NX_PPP_HASHED_VALUE_SIZE** — указывает размер строк хэшированного значения, используемых при проверке подлинности CHAP. Значение по умолчанию равно 16 байт, но может быть переопределено перед включением *nx_ppp.h*.
- **NX_PPP_MAX_LCP_PROTOCOL_RETRIES** — определяет максимальное число повторных попыток, если время ожидания PPP истекло до отправки другого сообщения запроса настройки LCP. При достижении этого значения подтверждение PPP прерывается, а канал переходит в нерабочее состояние. Значение по умолчанию — 20.
- **NX_PPP_MAX_PAP_PROTOCOL_RETRIES** — определяет максимальное число повторных попыток, если время ожидания PPP истекло до отправки другого сообщения запроса проверки подлинности PAP. При достижении этого значения подтверждение PPP прерывается, а канал переходит в нерабочее состояние. Значение по умолчанию — 20.
- **NX_PPP_MAX_CHAP_PROTOCOL_RETRIES** — определяет максимальное число повторных попыток, если время ожидания PPP истекло до отправки другого сообщения запроса CHAP. При достижении этого значения подтверждение PPP прерывается, а канал переходит в нерабочее состояние. Значение по умолчанию — 20.
- **NX_PPP_MAX_IPCP_PROTOCOL_RETRIES** — определяет максимальное число повторных попыток, если время ожидания PPP истекло до отправки другого сообщения запроса настройки IPCP. При достижении этого значения подтверждение PPP прерывается, а канал переходит в нерабочее состояние. Значение по умолчанию — 20.
- **NX_PPP_MRU** — указывает максимальный размер данных, передаваемых в пакете (MRU) для PPP. По умолчанию это значение равно 1500 байт (минимальное значение). Это определение можно задать в приложении перед включением *nx_ppp.h*.
- **NX_PPP_MINIMUM_MRU** — указывает минимальный объем MRU, полученный в сообщении запроса настройки LCP. По умолчанию это значение равно 1500 байт (минимальное значение). Это определение можно задать в приложении перед включением *nx_ppp.h*.
- **NX_PPP_NAME_SIZE** — указывает размер строк имени, используемых при проверке подлинности. Значение по умолчанию равно 32 байт, но может быть переопределено перед включением *nx_ppp.h.
- **NX_PPP_PASSWORD_SIZE** — указывает размер строк пароля, используемых при проверке подлинности. Значение по умолчанию равно 32 байт, но может быть переопределено перед включением *nx_ppp.h*.
- **NX_PPP_PROTOCOL_TIMEOUT** — определяет параметр ожидания (в секундах), в течение которого задача PPP получает ответ на сообщение запроса протокола PPP. Значение по умолчанию — 4 секунды.
- **NX_PPP_RECEIVE_TIMEOUTS** — определяет, сколько раз истекает время ожидания задачи потока PPP до получения следующего символа в потоке сообщений PPP. После этого протокол PPP освобождает пакет и начинает ждать получения следующего сообщения PPP. Значение по умолчанию — 4.
- **NX_PPP_SERIAL_BUFFER_SIZE** — указывает размер последовательного буфера получения символа. По умолчанию это значение равно 3000 байт. Это определение можно задать в приложении перед включением *nx_ppp.h*.
- **NX_PPP_TIMEOUT** — определяет параметр ожидания (в тактах таймера) для выделения пакетов для передачи данных, а также для добавления последовательных данных PPP в пакеты для отправки на уровень IP. Значение по умолчанию — 4*NX_IP_PERIODIC_RATE (400 тактов).
- **NX_PPP_THREAD_TIME_SLICE** — параметр временного среза для потоков PPP. Значением по умолчанию является TX_NO_TIME_SLICE. Это определение можно задать в приложении перед включением *nx_ppp.h*.
- **NX_PPP_VALUE_SIZE** — указывает размер строк значения, используемых при проверке подлинности CHAP. Значение по умолчанию равно 32 байт, но может быть переопределено перед включением nx_ppp.h.
