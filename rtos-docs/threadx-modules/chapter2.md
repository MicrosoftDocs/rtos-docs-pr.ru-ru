---
title: Глава 2. Требования к модулям
description: Эта статья содержит описание требований при создании модуля ThreadX.
author: philmea
ms.author: philmea
ms.date: 06/08/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 32288d78ceffb74ab088a1d720dbac657f6d3ed4
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104814383"
---
# <a name="chapter-2---module-requirements"></a>Глава 2. Требования к модулям

Модуль ThreadX содержит преамбулу, которая определяет основные характеристики модуля. За преамбулой следует область инструкций модуля. Модули могут выполняться на месте, или же они могут быть загружены диспетчером модулей в область памяти модуля перед выполнением. Единственное требование заключается в том, чтобы преамбула всегда находилась по первому адресу модуля. На рисунке 2 показан базовый макет модуля.

| Макет модуля |
|:---:|
| \[преамбула модуля\]         |
| \[область инструкций модуля\] |
| \[область памяти модуля\]         |

**Рисунок 2**. Макет модуля

> [!NOTE]
> Модули должны быть построены с соответствующим положением независимого кода и параметров компилятора или компоновщика данных. Это позволяет выполнять модуль в любой области памяти.

При создании потока модуля выделяется второе пространство стека для использования в случаях, когда поток находится в ядре с защищенной памятью. Размер пространства стека ядра в потоке можно настраивать с помощью параметра **TXM_MODULE_KERNEL_STACK_SIZE** в файле **_txm_module_port.h_ *_. Это позволяет использовать меньший размер стека при создании потока модуля, так как стек, указанный пользователем при вызове _* _tx_thread_create_**, используется только в модуле.

> [!NOTE]
> В верхней части стека потоков модуля содержится структура сведений о записи потока (**TXM_MODULE_THREAD_ENTRY_INFO**), поэтому доступный размер стека уменьшается на размер этой структуры. При создании потока в модуле увеличьте размер его стека, по крайней мере, на размер этой структуры.

Следующие шаги необходимы для создания и сборки модуля ThreadX (более подробное описание каждого шага приводится ниже).

1. Все файлы C в модуле должны содержать директиву #define **TXM_MODULE** до включения **_txm_module.h_**. Это можно сделать в компилируемом исходном файле или в составе параметров проекта. Это приведет к повторному перераспределению вызовов API ThreadX с зависящей от модуля версией API, которая вызывает функцию диспетчеризации в резидентном модуле диспетчера для выполнения вызова самой функции API.
2. Каждый модуль должен иметь преамбулу в первом адресе области инструкций, которая определяет характеристики и потребность в ресурсах модуля.
3. Каждый модуль должен связывать преамбулу в начале области инструкций модуля.
4. Каждый модуль должен связываться с библиотекой модулей (***txm.a***), которая содержит функции для конкретного модуля, используемые для взаимодействия с ThreadX.

## <a name="module-sources"></a>Исходный код модулей

Модули ThreadX имеют собственный набор исходных файлов, которые должны быть связаны и размещены непосредственно с исходным кодом модуля. Эти файлы предоставляют мост между отдельным модулем и диспетчером резидентных модулей. Файлы модуля приведены ниже.

| Имя файла | Содержимое |
|---|---|
| **txm_module.h** | Подключаемый файл, определяющий сведения о модуле. |
| **txm_module_port.h** | Подключаемый файл, определяющий сведения о модуле, специфичные для конкретного порта. |
| **txm_module_user.h** | Определения и значения, которые пользователь может настраивать. |
| **txm_module_initialize.s [1][3]** | Вызов встроенных функций для модуля запуска. |
| **txm_module_preamble.\{s/S/68\}** | Файл сборки преамбулы модуля. Этот файл определяет различные атрибуты конкретного модуля и связывается с кодом приложения модуля. |
| **txm_module_application_request.c [1]** | Функция запроса приложения модуля отправляет в резидентный код запрос, зависящий от приложения. |
| **txm_module_callback_request_thread_entry.c&nbsp;[1]** | Поток обратного вызова модуля, отвечающий за обработку обратных вызовов, запрошенных модулем, включая таймеры и обратные вызовы уведомлений. |
| **txm_*.c [1][2]** | Стандартные службы API ThreadX, которые вызывают диспетчер ядра.
| **txm_module_object_allocate.c [1]** | Функция модуля для выделения памяти для объектов модуля, расположенных в пуле памяти диспетчера. |
| **txm_module_object_deallocate.c [1]** | Функция модуля для освобождения памяти объектов модуля, расположенных в пуле памяти диспетчера. |
| **txm_module_object_pointer_get.c [1]** | Функция модуля для получения указателя на системный объект. |
| **txm_module_object_pointer_get_extended.c [1]** | Функция модуля для получения указателя на системный объект с безопасной длиной имени. |
| **txm_module_thread_shell_entry.c [1]** | Функция входа в поток модуля. |
| **txm_module_thread_system_suspend.c [1]** | Функция модуля для приостановки потока. |

**[1]** Расположен в библиотеке **_txm.a_**.

**[2]** Эти файлы имеют то же имя, что и файлы API ThreadX с префиксом **txm_** вместо префикса **tx_** .

**[3]** Файл **txm_module_initialize.s** предназначен только для портов, использующих средства ARM.

## <a name="module-preamble"></a>Преамбула модуля

Преамбула модуля определяет характеристики и ресурсы модуля. Такие сведения, как начальная функция входа в поток и начальная область памяти, связанная с потоком, определяются в преамбуле. Примеры преамбулы, относящиеся к конкретному порту, находятся в [приложении](appendix.md). На рисунке 3 показан пример преамбулы модуля ThreadX для универсального целевого объекта (строки, начинающиеся с символа *, — это значения, которые обычно изменяются приложением).

```c
    AREA Init, CODE, READONLY

    /* Define public symbols. */
    EXPORT __txm_module_preamble

    /* Define application-specific start/stop entry points for the module. */
    IMPORT demo_module_start

    /* Define common external refrences. */
    IMPORT _txm_module_thread_shell_entry
    IMPORT _txm_module_callback_request_thread_entry
    IMPORT |Image$$ER_RO$$Length|
    IMPORT |Image$$ER_RW$$Length|

__txm_module_preamble
    DCD     0x4D4F4455                                  ; Module ID
    DCD     0x6                                         ; Module Major Version
    DCD     0x1                                         ; Module Minor Version
    DCD     32                                          ; Module Preamble Size in 32-bit words
*   DCD     0x12345678                                  ; Module ID (application defined)
*   DCD     0x01000001                                  ; Module Properties where:
                                                        ; Bits 31-24: Compiler ID
                                                        ;   0 -> IAR
                                                        ;   1 -> ARM
                                                        ;   2 -> GNU
                                                        ;   Bits 23-1: Reserved
                                                        ;   Bit 0: 0 -> Privileged mode execution (no MMU protection)
                                                        ;          1 -> User mode execution (MMU protection)
    DCD     _txm_module_thread_shell_entry - . + .      ; Module Shell Entry Point
*   DCD     demo_module_start - . + .                   ; Module Start Thread Entry Point
    DCD     0                                           ; Module Stop Thread Entry Point
*   DCD     1                                           ; Module Start/Stop Thread Priority
*   DCD     2048                                        ; Module Start/Stop Thread Stack Size
    DCD     _txm_module_callback_request_thread_entry - . + . ; Module Callback Thread Entry
    DCD     1                                            ; Module Callback Thread Priority
    DCD     2048                                         ; Module Callback Thread Stack Size
    DCD     |Image$$ER_RO$$Length|                       ; Module Code Size
    DCD     |Image$$ER_RW$$Length|                       ; Module Data Size
    DCD     0                                            ; Reserved 0
    DCD     0                                            ; Reserved 1
    DCD     0                                            ; Reserved 2
    DCD     0                                            ; Reserved 3
    DCD     0                                            ; Reserved 4
    DCD     0                                            ; Reserved 5
    DCD     0                                            ; Reserved 6
    DCD     0                                            ; Reserved 7
    DCD     0                                            ; Reserved 8
    DCD     0                                            ; Reserved 9
    DCD     0                                            ; Reserved 10
    DCD     0                                            ; Reserved 11
    DCD     0                                            ; Reserved 12
    DCD     0                                            ; Reserved 13
    DCD     0                                            ; Reserved 14
    DCD     0                                            ; Reserved 15
    END
```

**Рис. 3**

В большинстве случаев разработчику необходимо только определить начальный поток модуля (смещение 0x1C), идентификатор модуля (смещение 0x10), приоритет потока начала и окончания (смещение 0x24) и размер стека потока начала и окончания (смещение 0x28). Приведенная выше демонстрация настроена следующим образом: начальный поток модуля — ***demo_module_start** _, идентификатор модуля — _*_0x12345678_*_, начальный поток имеет приоритет _*_1_*_, а размер стека равен _ *_2048_** байт.

Некоторые приложения могут дополнительно определять поток остановки, который выполняется, когда диспетчер модулей останавливает модуль. Кроме того, некоторые приложения могут использовать поле свойства модуля, определенное следующим образом.

## <a name="module-properties-bit-map"></a>Битовая схема свойств модуля

В таблице ниже приведен пример использования битовой схемы свойств. Битовые схемы, специфичные для отдельных портов, приведены в [приложении](appendix.md).

| bit | Значение | Значение |
|---|---|---|
| 0 | 0<br />1 | Выполнение в привилегированном режиме<br />Выполнение в пользовательском режиме |
| 1 | 0<br />1 | Без защиты MPU<br />Защита MPU (необходимо выбрать пользовательский режим) |
| 2 | 0<br />1 | Отключение доступа к общей или внешней памяти<br />Включение доступа к общей или внешней памяти |
| [23–3] | 0 | Зарезервировано
| [31–24] | <br />0x01<br />0x02<br />0x03 | **Идентификатор компилятора**<br />IAR<br />ARM<br />GNU |


## <a name="module-linker-control-file"></a>Файл управления компоновщиком модулей

При построении модуля необходимо разместить преамбулу модуля перед любыми другими разделами кода. Модуль должен быть построен с использованием не зависящих от места кода и разделов данных. Примеры файлов компоновщика для конкретного порта находятся в [приложении](appendix.md).

## <a name="module-threadx-library"></a>Библиотека ThreadX модуля

Каждый модуль должен связываться со специальной модульной библиотекой ThreadX. Эта библиотека предоставляет доступ к службам ThreadX в резидентном коде. Большая часть доступа выполняется с помощью файлов ***txm_\*.c***. Ниже приведен пример вызова доступа к модулю для функции API ThreadX **_tx_thread_relinquish_* _ (в _*_ \_txm_thread_relinquish.c\****).

```c
(_txm_module_kernel_call_dispatcher)(TXM_THREAD_RELINQUISH_CALL, 0, 0, 0);
```

В этом примере указатель функции, предоставляемый диспетчером модулей, используется для вызова функции диспетчеризации диспетчера модулей с идентификатором, связанным со службой ***tx_thread_relinquish***. Эта служба не принимает параметров.

## <a name="module-example"></a>Пример модуля

Ниже приведен пример стандартной демонстрации ThreadX в форме модуля. Основные различия между стандартной демонстрацией ThreadX и демонстрацией модуля следующие.

1. Замена ***tx_api. h** _ на _ *_txm_module.h_**.
2. Добавление **#define TXM_MODULE** перед **_txm_module.h_**.
3. Замена ***main** _ и _ *tx_application_define** на **_demo_module_start_**.
4. Объявление *указателей* на объекты ThreadX, а не самих объектов.

```c
/* Specify that this is a module! */
#define TXM_MODULE

/* Include the ThreadX module header. */
#include "txm_module.h"

/* Define constants. */
#define DEMO_STACK_SIZE         1024
#define DEMO_BYTE_POOL_SIZE     9120
#define DEMO_BLOCK_POOL_SIZE    100
#define DEMO_QUEUE_SIZE         100

/* Define the pool space in the bss section of the module. ULONG is used to
   get word alignment. */
   
ULONG                   demo_module_pool_space[DEMO_BYTE_POOL_SIZE / 4];

/* Define the ThreadX object control block pointers. */

TX_THREAD               *thread_0;
TX_THREAD               *thread_1;
TX_THREAD               *thread_2;
TX_THREAD               *thread_3;
TX_THREAD               *thread_4;
TX_THREAD               *thread_5;
TX_THREAD               *thread_6;
TX_THREAD               *thread_7;
TX_QUEUE                *queue_0;
TX_SEMAPHORE            *semaphore_0;
TX_MUTEX                *mutex_0;
TX_EVENT_FLAGS_GROUP    *event_flags_0;
TX_BYTE_POOL            *byte_pool_0;
TX_BLOCK_POOL           *block_pool_0;


/* Define the counters used in the demo application. */
ULONG       thread_0_counter;
ULONG       thread_1_counter;
ULONG       thread_1_messages_sent;
ULONG       thread_2_counter;
ULONG       thread_2_messages_received;
ULONG       thread_3_counter;
ULONG       thread_4_counter;
ULONG       thread_5_counter;
ULONG       thread_6_counter;
ULONG       thread_7_counter;
ULONG       semaphore_0_puts;
ULONG       event_0_sets;
ULONG       queue_0_sends;

/* Define thread prototypes. */

void    thread_0_entry(ULONG thread_input);
void    thread_1_entry(ULONG thread_input);
void    thread_2_entry(ULONG thread_input);
void    thread_3_and_4_entry(ULONG thread_input);
void    thread_5_entry(ULONG thread_input);
void    thread_6_and_7_entry(ULONG thread_input);

/* Define notify functions. */

void semaphore_0_notify(TX_SEMAPHORE *semaphore_ptr)
{
    if (semaphore_ptr == semaphore_0)
        semaphore_0_puts++;
}


void event_0_notify(TX_EVENT_FLAGS_GROUP *event_flag_group_ptr)
{
    if (event_flag_group_ptr == event_flags_0)
        event_0_sets++;
}


void queue_0_notify(TX_QUEUE *queue_ptr)
{
    if (queue_ptr == queue_0)
        queue_0_sends++;
}

/* Define the module start function. */
void demo_module_start(ULONG id)
{
    CHAR *pointer;

    /* Allocate all the objects. In memory protection mode,
        modules cannot allocate control blocks within their
        own memory area so they cannot corrupt the resident
        portion of ThreadX by corrupting the control block(s). */
    txm_module_object_allocate(&thread_0, sizeof(TX_THREAD));
    txm_module_object_allocate(&thread_1, sizeof(TX_THREAD));
    txm_module_object_allocate(&thread_2, sizeof(TX_THREAD));
    txm_module_object_allocate(&thread_3, sizeof(TX_THREAD));
    txm_module_object_allocate(&thread_4, sizeof(TX_THREAD));
    txm_module_object_allocate(&thread_5, sizeof(TX_THREAD));
    txm_module_object_allocate(&thread_6, sizeof(TX_THREAD));
    txm_module_object_allocate(&thread_7, sizeof(TX_THREAD));
    txm_module_object_allocate(&queue_0, sizeof(TX_QUEUE));
    txm_module_object_allocate(&semaphore_0, sizeof(TX_SEMAPHORE));
    txm_module_object_allocate(&mutex_0, sizeof(TX_MUTEX));
    txm_module_object_allocate(&event_flags_0, sizeof(TX_EVENT_FLAGS_GROUP));
    txm_module_object_allocate(&byte_pool_0, sizeof(TX_BYTE_POOL));
    txm_module_object_allocate(&block_pool_0, sizeof(TX_BLOCK_POOL));

    /* Create a byte memory pool from which to allocate the thread stacks. */
    tx_byte_pool_create(byte_pool_0, "module byte pool 0",
        demo_module_pool_space, DEMO_BYTE_POOL_SIZE);

    /* Allocate the stack for thread 0. */
    tx_byte_allocate(byte_pool_0, (VOID **) &pointer,
        DEMO_STACK_SIZE, TX_NO_WAIT);

    /* Create thread 0. */
    tx_thread_create(thread_0, "module thread 0", thread_0_entry, 0,
        pointer, DEMO_STACK_SIZE,
        1, 1, TX_NO_TIME_SLICE, TX_AUTO_START);

    /* Allocate the stack for thread 1. */
    tx_byte_allocate(byte_pool_0, (VOID **) &pointer,
        DEMO_STACK_SIZE, TX_NO_WAIT);

    /* Create threads 1 and 2. These threads pass information through
        a ThreadX message queue. It is also interesting to note that
        these threads have a time slice. */
    tx_thread_create(thread_1, "module thread 1", thread_1_entry, 1,
        pointer, DEMO_STACK_SIZE,
        16, 16, 4, TX_AUTO_START);

    /* Allocate the stack and create thread 2. */
    tx_byte_allocate(byte_pool_0, (VOID **) &pointer,
        DEMO_STACK_SIZE, TX_NO_WAIT);
    tx_thread_create(thread_2, "module thread 2", thread_2_entry, 2,
        pointer, DEMO_STACK_SIZE,
        16, 16, 4, TX_AUTO_START);

    /* Allocate the stack for thread 3. */
    tx_byte_allocate(byte_pool_0, (VOID **) &pointer,
        DEMO_STACK_SIZE, TX_NO_WAIT);

    /* Create threads 3 and 4. These threads compete for a ThreadX
        counting semaphore. An interesting thing here is that both threads
        share the same instruction area. */
    tx_thread_create(thread_3, "module thread 3",
        thread_3_and_4_entry, 3,
        pointer, DEMO_STACK_SIZE,
        8, 8, TX_NO_TIME_SLICE, TX_AUTO_START);

    /* Allocate the stack and create thread 4. */
    tx_byte_allocate(byte_pool_0, (VOID **) &pointer,
        DEMO_STACK_SIZE, TX_NO_WAIT);
    tx_thread_create(thread_4, "module thread 4",
        thread_3_and_4_entry, 4,
        pointer, DEMO_STACK_SIZE,
        8, 8, TX_NO_TIME_SLICE, TX_AUTO_START);

    /* Allocate the stack for thread 5. */
    tx_byte_allocate(byte_pool_0, (VOID **) &pointer,
        DEMO_STACK_SIZE, TX_NO_WAIT);

    /* Create thread 5. This thread simply pends on an event flag which
        will be set by thread 0. */
    tx_thread_create(thread_5, "module thread 5", thread_5_entry, 5,
        pointer, DEMO_STACK_SIZE,
        4, 4, TX_NO_TIME_SLICE, TX_AUTO_START);

    /* Allocate the stack for thread 6. */
    tx_byte_allocate(byte_pool_0, (VOID **) &pointer,
        DEMO_STACK_SIZE, TX_NO_WAIT);

    /* Create threads 6 and 7. These threads compete for a ThreadX mutex. */
    tx_thread_create(thread_6, "module thread 6",
        thread_6_and_7_entry, 6,
        pointer, DEMO_STACK_SIZE,
        8, 8, TX_NO_TIME_SLICE, TX_AUTO_START);

    /* Allocate the stack and create thread 7. */
    tx_byte_allocate(byte_pool_0, (VOID **) &pointer,
        DEMO_STACK_SIZE, TX_NO_WAIT);
    tx_thread_create(thread_7, "module thread 7",
        thread_6_and_7_entry, 7,
        pointer, DEMO_STACK_SIZE,
        8, 8, TX_NO_TIME_SLICE, TX_AUTO_START);

    /* Allocate the message queue. */
    tx_byte_allocate(byte_pool_0, (VOID **) &pointer,
        DEMO_QUEUE_SIZE*sizeof(ULONG), TX_NO_WAIT);

    /* Create the message queue shared by threads 1 and 2. */
    tx_queue_create(queue_0, "module queue 0", TX_1_ULONG, pointer,
        DEMO_QUEUE_SIZE*sizeof(ULONG));

    /* Register queue send callback. */
    tx_queue_send_notify(queue_0, queue_0_notify);

    /* Create the semaphore used by threads 3 and 4. */
    tx_semaphore_create(semaphore_0, "module semaphore 0", 1);

    /* Register semaphore put callback. */
    tx_semaphore_put_notify(semaphore_0, semaphore_0_notify);

    /* Create the event flags group used by threads 1 and 5. */
    tx_event_flags_create(event_flags_0, "module event flags 0");

    /* Register event flag set callback. */
    tx_event_flags_set_notify(event_flags_0, event_0_notify);

    /* Create the mutex used by thread 6 and 7 without priority
        inheritance. */
    tx_mutex_create(mutex_0, "module mutex 0", TX_NO_INHERIT);

    /* Allocate the memory for a small block pool. */
    tx_byte_allocate(byte_pool_0, (VOID **) &pointer,
        DEMO_BLOCK_POOL_SIZE, TX_NO_WAIT);

    /* Create a block memory pool. */
    tx_block_pool_create(block_pool_0, "module block pool 0",
        sizeof(ULONG), pointer, DEMO_BLOCK_POOL_SIZE);

    /* Allocate a block. */
    tx_block_allocate(block_pool_0, (VOID **) &pointer,
        TX_NO_WAIT);

    /* Release the block back to the pool. */
    tx_block_release(pointer);

}

/* Define all the threads. */

void thread_0_entry(ULONG thread_input)
{
    UINT status;

    /* This thread simply sits in while-forever-sleep loop. */
    while(1)
    {
        /* Increment the thread counter. */
        thread_0_counter++;

        /* Sleep for 10 ticks. */
        tx_thread_sleep(10);

        /* Set event flag 0 to wake up thread 5. */
        status = tx_event_flags_set(event_flags_0, 0x1, TX_OR);

        /* Check status. */
        if (status != TX_SUCCESS)
            break;
    }
}

void thread_1_entry(ULONG thread_input)
{
    UINT status;

    /* This thread simply sends messages to a queue shared by
       thread 2. */
    while(1)
    {
        /* Increment the thread counter. */
        thread_1_counter++;

        /* Send message to queue 0. */
        status = tx_queue_send(queue_0, &thread_1_messages_sent,
            TX_WAIT_FOREVER);

        /* Check completion status. */
        if (status != TX_SUCCESS)
            break;

        /* Increment the message sent. */
        thread_1_messages_sent++;
    }
}

void thread_2_entry(ULONG thread_input)
{
    ULONG received_message;
    UINT status;

    /* This thread retrieves messages placed on the queue by thread 1. */
    while(1)
    {
        /* Increment the thread counter. */
        thread_2_counter++;

        /* Retrieve a message from the queue. */
        status = tx_queue_receive(queue_0, &received_message, TX_WAIT_FOREVER);

        /* Check completion status and make sure the message is what
           we expected. */
        if ((status != TX_SUCCESS) || (received_message != thread_2_messages_received))
            break;

        /* Otherwise, all is okay. Increment the received message count. */
        thread_2_messages_received++;
    }
}

void thread_3_and_4_entry(ULONG thread_input)
{
    UINT status;

    /* This function is executed from thread 3 and thread 4. As the loop
       below shows, these function compete for ownership of semaphore_0. */
    while(1)
    {
        /* Increment the thread counter. */
        if (thread_input == 3)
            thread_3_counter++;
        else
            thread_4_counter++;

        /* Get the semaphore with suspension. */
        status = tx_semaphore_get(semaphore_0, TX_WAIT_FOREVER);

        /* Check status. */
        if (status != TX_SUCCESS)
            break;

        /* Sleep for 2 ticks to hold the semaphore. */
        tx_thread_sleep(2);

        /* Release the semaphore. */
        status = tx_semaphore_put(semaphore_0);

        /* Check status. */
        if (status != TX_SUCCESS)
            break;
    }
}

void thread_5_entry(ULONG thread_input)
{
    UINT status;
    ULONG actual_flags;

    /* This thread simply waits for an event in a forever loop. */
    while(1)
    {
        /* Increment the thread counter. */
        thread_5_counter++;

        /* Wait for event flag 0. */
        status = tx_event_flags_get(event_flags_0, 0x1, TX_OR_CLEAR,
                                        &actual_flags, TX_WAIT_FOREVER);

        /* Check status. */
        if ((status != TX_SUCCESS) || (actual_flags != 0x1))
            break;
    }
}

void thread_6_and_7_entry(ULONG thread_input)
{
    UINT status;

    /* This function is executed from thread 6 and thread 7. As the loop
       below shows, these function compete for ownership of mutex_0. */
    while(1)
    {
        /* Increment the thread counter. */
        if (thread_input == 6)
            thread_6_counter++;
        else
            thread_7_counter++;

        /* Get the mutex with suspension. */
        status = tx_mutex_get(mutex_0, TX_WAIT_FOREVER);

        /* Check status. */
        if (status != TX_SUCCESS)
            break;

        /* Get the mutex again with suspension. This shows that an
           owning thread may retrieve the mutex it owns multiple times. */
        status = tx_mutex_get(mutex_0, TX_WAIT_FOREVER);

        /* Check status. */
        if (status != TX_SUCCESS)
            break;

        /* Sleep for 2 ticks to hold the mutex. */
        tx_thread_sleep(2);

        /* Release the mutex. */
        status = tx_mutex_put(mutex_0);

        /* Check status. */
        if (status != TX_SUCCESS)
            break;

        /* Release the mutex again. This will actually release ownership
           since it was obtained twice. */
        status = tx_mutex_put(mutex_0);

        /* Check status. */
        if (status != TX_SUCCESS)
            break;
    }
}
```

## <a name="building-modules"></a>Создание модулей

Создание модуля зависит от используемой цепочки инструментов. Примеры для конкретных портов см. в [приложении](appendix.md). К общим действиям для всех портов относятся следующие.

- Построение библиотеки модуля.
- Построение приложения модуля.

Каждый модуль должен иметь раздел **txm_module_preamble** (настроенный для конкретного модуля) и библиотеку модулей (например, **_txm.a_**).
