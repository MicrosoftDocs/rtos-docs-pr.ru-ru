---
title: Глава 5. API классов узлов USBX
description: Сведения об API классов узлов USBX.
author: philmea
ms.author: philmea
ms.date: 5/19/2020
ms.service: rtos
ms.topic: article
ms.openlocfilehash: 62750ab4a5540b243665a7a7d9000a0d60c4435313b6de2e1579ae7f1c20fe55
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2021
ms.locfileid: "116790672"
---
# <a name="chapter-5---usbx-host-classes-api"></a>Глава 5. API классов узлов USBX

В этой главе рассматриваются все предоставляемые API классов узлов USBX. Ниже подробно описаны следующие интерфейсы API для каждого класса.

- Класс HID.
- Класс CDC-ACM.
- Класс CDC-ECM.
- Класс хранения

## <a name="ux_host_class_hid_client_register"></a>ux_host_class_hid_client_register

Регистрация клиента HID для класса HID.

### <a name="prototype"></a>Прототип

```c
UINT ux_host_class_hid_client_register(
    UCHAR *hid_client_name,
    UINT (*hid_client_handler)
    (struct UX_HOST_CLASS_HID_CLIENT_COMMAND_STRUCT *));
```

### <a name="description"></a>Описание

Эта функция используется для регистрации клиента HID в классе HID. Классу HID необходимо найти соответствие между HID-устройством и клиентом HID перед запросом данных с этого устройства.

> [!NOTE]
> Строка C объекта hid_client_name должна завершаться нулем и ее длина (без конечного нуля) не должна быть больше, чем **UX_HOST_CLASS_HID_MAX_CLIENT_NAME_LENGTH**.

### <a name="parameters"></a>Параметры

- **hid_client_name** — указатель на имя клиента HID.
- **hid_client_handler** — указатель на обработчик клиента HID.

### <a name="return-values"></a>Возвращаемые значения

- **UX_SUCCESS** (0x00) — передача данных завершена
- **UX_MEMORY_INSUFFICIENT** (0x12) — сбой выделения памяти для клиента.
- **UX_MEMORY_ARRAY_FULL** (0X1a) — максимальное количество клиентов уже зарегистрировано.
- **UX_HOST_CLASS_ALREADY_INSTALLED** (0x58) — этот класс уже существует.

### <a name="example"></a>Пример

```c
UINT status;

/* The following example illustrates how to register a HID client, in
this case a USB mouse, to the HID class. */

status = ux_host_class_hid_client_register("ux_host_class_hid_client_mouse",
    ux_host_class_hid_mouse_entry);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_report_callback_register"></a>ux_host_class_hid_report_callback_register

Регистрация обратного вызова из класса HID.

### <a name="prototype"></a>Прототип

```c
UINT ux_host_class_hid_report_callback_register(
    UX_HOST_CLASS_HID *hid,
    UX_HOST_CLASS_HID_REPORT_CALLBACK *call_back);
```

### <a name="description"></a>Описание

Эта функция используется для регистрации обратного вызова из класса HID к клиенту HID при получении отчета.

### <a name="parameters"></a>Параметры

- **hid** — указатель на экземпляр класса HID
- **call_back** — указатель на структуру call_back.

### <a name="return-values"></a>Возвращаемые значения

- **UX_SUCCESS** (0x00) — передача данных завершена
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) — недопустимый экземпляр HID.
- **UX_HOST_CLASS_HID_REPORT_ERROR** (0x79) — ошибка при регистрации обратного вызова в отчете.

### <a name="example"></a>Пример

```c
UINT status;

/* This example illustrates how to register a HID client, in this case a USB mouse, to the HID class. In this case, the HID client is asking the HID class to call the client for each usage received in the HID report. */

call_back.ux_host_class_hid_report_callback_id = 0;
call_back.ux_host_class_hid_report_callback_function = ux_host_class_hid_mouse_callback;

call_back.ux_host_class_hid_report_callback_buffer = UX_NULL;
call_back.ux_host_class_hid_report_callback_flags = UX_HOST_CLASS_HID_REPORT_INDIVIDUAL_USAGE;

call_back.ux_host_class_hid_report_callback_length = 0;

status = ux_host_class_hid_report_callback_register(hid, &call_back);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_periodic_report_start"></a>ux_host_class_hid_periodic_report_start

Запуск периодической конечной точки для экземпляра класса HID.

### <a name="prototype"></a>Прототип

```c
UINT ux_host_class_hid_periodic_report_start(UX_HOST_CLASS_HID *hid);
```

### <a name="description"></a>Описание

Эта функция используется для запуска периодической (с прерываниями) конечной точки для экземпляра класса HID, привязанного к этому клиенту HID. Класс HID не может запустить периодическую конечную точку, пока клиент HID не будет активирован и, следовательно, клиент HID должен запустить эту конечную точку для получения отчетов.

### <a name="input-parameter"></a>Входной параметр

- **hid** — указатель на экземпляр класса HID.

### <a name="return-values"></a>Возвращаемые значения

- **UX_SUCCESS** (0x00) — периодическая отчетность успешно запущена.
- **ux_host_class_hid_PERIODIC_REPORT_ERROR** (0x7A) — ошибка в периодическом отчете.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) — экземпляр класса HID не существует.

### <a name="example"></a>Пример

```c
UINT status;

/* The following example illustrates how to start the periodic
endpoint. */

status = ux_host_class_hid_periodic_report_start(hid);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_periodic_report_stop"></a>ux_host_class_hid_periodic_report_stop

Остановка периодической конечной точки для экземпляра класса HID.

### <a name="prototype"></a>Прототип

```c
UINT ux_host_class_hid_periodic_report_stop(UX_HOST_CLASS_HID *hid);
```

### <a name="description"></a>Описание

Эта функция используется для остановки периодической (с прерываниями) конечной точки для экземпляра класса HID, привязанного к этому клиенту HID. Класс HID не может остановить периодическую конечную точку, пока клиент HID не будет деактивирован с освобождением всех его ресурсов и, следовательно, клиент HID должен запустить эту конечную точку для получения отчетов.

### <a name="input-parameter"></a>Входной параметр

- **hid** — указатель на экземпляр класса HID.

### <a name="return-values"></a>Возвращаемые значения

- **UX_SUCCESS** (0x00) — периодическая отчетность успешно остановлена.
- **ux_host_class_hid_PERIODIC_REPORT_ERROR** (0x7A) — ошибка в периодическом отчете.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) — экземпляр класса HID не существует

### <a name="example"></a>Пример

```c
UINT status;

/* The following example illustrates how to stop the periodic endpoint. */

status = ux_host_class_hid_periodic_report_stop(hid);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_report_get"></a>ux_host_class_hid_report_get

Получение отчета из экземпляра класса HID.

### <a name="prototype"></a>Прототип

```c
UINT ux_host_class_hid_report_get(
    UX_HOST_CLASS_HID *hid,
    UX_HOST_CLASS_HID_CLIENT_REPORT *client_report);
```

### <a name="description"></a>Описание

Эта функция используется для получения отчета непосредственно с устройства, не полагаясь на периодическую конечную точку. Этот отчет поступает от конечной точки элемента управления, но его обработка будет такой же, как если бы он поступал на периодическую конечную точку.

### <a name="parameters"></a>Параметры

- **hid** — указатель на экземпляр класса HID.
- **client_report** — указатель на отчет клиента HID.

### <a name="return-values"></a>Возвращаемые значения

- **UX_SUCCESS** (0x00) — отчет успешно получен.
- **UX_HOST_CLASS_HID_REPORT_ERROR** (0x70) — отчет клиента был недопустимым или произошла ошибка во время передачи.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) — экземпляр класса HID не существует.
- **UX_BUFFER_OVERFLOW** (0x5d) — предоставленный буфер недостаточно большой для размещения несжатого отчета.

### <a name="example"></a>Пример

```c
UX_HOST_CLASS_HID_CLIENT_REPORT input_report;

UINT status;

/* The following example illustrates how to get a report. */

input_report.ux_host_class_hid_client_report = hid_report;
input_report.ux_host_class_hid_client_report_buffer = buffer;
input_report.ux_host_class_hid_client_report_length = length;
input_report.ux_host_class_hid_client_flags = UX_HOST_CLASS_HID_REPORT_INDIVIDUAL_USAGE;

status = ux_host_class_hid_report_get(hid, &input_report);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_report_set"></a>ux_host_class_hid_report_set

Отправка отчета

### <a name="prototype"></a>Прототип

```c
UINT ux_host_class_hid_report_set(
    UX_HOST_CLASS_HID *hid,
    UX_HOST_CLASS_HID_CLIENT_REPORT *client_report);
```

### <a name="description"></a>Описание

Эта функция используется для отправки отчета непосредственно на устройство.

### <a name="parameters"></a>Параметры

- **hid** — указатель на экземпляр класса HID.
- **client_report** — указатель на отчет клиента HID.

### <a name="return-values"></a>Возвращаемые значения

- **UX_SUCCESS** (0x00) — отчет успешно отправлен.
- **UX_HOST_CLASS_HID_REPORT_ERROR** (0x70) — отчет клиента был недопустимым или произошла ошибка во время передачи.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) — экземпляр класса HID не существует.
- **UX_HOST_CLASS_HID_REPORT_OVERFLOW** (0x5d) — предоставленный буфер недостаточно большой для размещения несжатого отчета.

### <a name="example"></a>Пример

```c
/* The following example illustrates how to send a report. */

UX_HOST_CLASS_HID_CLIENT_REPORT input_report;

input_report.ux_host_class_hid_client_report = hid_report;
input_report.ux_host_class_hid_client_report_buffer = buffer;
input_report.ux_host_class_hid_client_report_length = length;
input_report.ux_host_class_hid_client_report_flags = UX_HOST_CLASS_HID_REPORT_INDIVIDUAL_USAGE;

status = ux_host_class_hid_report_set(hid, &input_report);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_mouse_buttons_get"></a>ux_host_class_hid_mouse_buttons_get

Получение кнопок мыши.

### <a name="prototype"></a>Прототип

```c
UINT ux_host_class_hid_mouse_buttons_get(
    UX_HOST_CLASS_HID_MOUSE *mouse_instance,
    ULONG *mouse_buttons);
```

### <a name="description"></a>Описание

Эта функция используется для получения кнопок мыши.

### <a name="parameters"></a>Параметры

- **mouse_instance** — указатель на экземпляр мыши HID.
- **mouse_buttons** — указатель на кнопки возврата.

### <a name="return-values"></a>Возвращаемые значения

- **UX_SUCCESS** (0x00) — кнопка мыши успешно извлечена.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) — экземпляр класса HID не существует.

### <a name="example"></a>Пример

```c
/* The following example illustrates how to obtain mouse buttons. */

UX_HOST_CLASS_HID_MOUSE *mouse_instance;

ULONG mouse_buttons;

status = ux_host_class_hid_mouse_button_get(mouse_instance, &mouse_buttons);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_mouse_position_get"></a>ux_host_class_hid_mouse_position_get

Получение расположения мыши.

### <a name="prototype"></a>Прототип

```c
UINT ux_host_class_hid_mouse_position_get(
    UX_HOST_CLASS_HID_MOUSE *mouse_instance,
    SLONG *mouse_x_position, 
    SLONG *mouse_y_position);
```

### <a name="description"></a>Описание

Эта функция используется для получения расположения мыши с координатами x и y.

### <a name="parameters"></a>Параметры

- **mouse_instance** — указатель на экземпляр мыши HID.
- **mouse_x_position** —указатель на координату x.
- **mouse_y_position** —указатель на координату y.

### <a name="return-values"></a>Возвращаемые значения

- **UX_SUCCESS** (0x00) — координаты X и Y успешно извлечены.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) — экземпляр класса HID не существует.

### <a name="example"></a>Пример

```c
/* The following example illustrates how to obtain mouse coordinates. */

UX_HOST_CLASS_HID_MOUSE *mouse_instance;

SLONG mouse_x_position;
SLONG mouse_y_position;

status = ux_host_class_hid_mouse_position_get(mouse_instance,
    &mouse_x_position, &mouse_y_position);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_keyboard_key_get"></a>ux_host_class_hid_keyboard_key_get

Получение клавиши и состояния клавиатуры.

### <a name="prototype"></a>Прототип

```c
UINT ux_host_class_hid_keyboard_key_get(
    UX_HOST_CLASS_HID_KEYBOARD *keyboard_instance,
    ULONG *keyboard_key, 
    ULONG *keyboard_state);
```

### <a name="description"></a>Описание

Эта функция используется для получения клавиши и состояния клавиатуры.

### <a name="parameters"></a>Параметры

- **keyboard_instance** — указатель на экземпляр клавиатуры HID.
- **keyboard_key** — указатель на контейнер клавиши клавиатуры.
- **keyboard_state** — указатель на контейнер состояния клавиатуры.

### <a name="return-values"></a>Возвращаемые значения

- **UX_SUCCESS** (0x00) — клавиша и состояние успешно извлечены.
- **UX_ERROR** (0xff) — данные для отчета отсутствуют.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) — экземпляр класса HID не существует.

Состояние клавиатуры может принимать указанные ниже значения.

- **UX_HID_KEYBOARD_STATE_KEY_UP** 0x10000
- **UX_HID_KEYBOARD_STATE_NUM_LOCK** 0x0001
- **UX_HID_KEYBOARD_STATE_CAPS_LOCK** 0x0002
- **UX_HID_KEYBOARD_STATE_SCROLL_LOCK** 0x0004
- **UX_HID_KEYBOARD_STATE_MASK_LOCK** 0x0007
- **UX_HID_KEYBOARD_STATE_LEFT_SHIFT** 0x0100
- **UX_HID_KEYBOARD_STATE_RIGHT_SHIFT** 0x0200
- **UX_HID_KEYBOARD_STATE_SHIFT** 0x0300
- **UX_HID_KEYBOARD_STATE_LEFT_ALT** 0x0400
- **UX_HID_KEYBOARD_STATE_RIGHT_ALT** 0x0800
- **UX_HID_KEYBOARD_STATE_ALT** 0x0a00
- **UX_HID_KEYBOARD_STATE_LEFT_CTRL** 0x1000
- **UX_HID_KEYBOARD_STATE_RIGHT_CTRL** 0x2000
- **UX_HID_KEYBOARD_STATE_CTRL** 0x3000
- **UX_HID_KEYBOARD_STATE_LEFT_GUI** 0x4000
- **UX_HID_KEYBOARD_STATE_RIGHT_GUI** 0x8000
- **UX_HID_KEYBOARD_STATE_GUI** 0xa000

### <a name="example"></a>Пример

```c
while (1)
{

    /* Get a key/state from the keyboard. */
    status = ux_host_class_hid_keyboard_key_get(keyboard,
        &keyboard_char, &keyboard_state);

    /* Check if there is something. */
    if (status == UX_SUCCESS)
    {

        #ifdef UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE
            if (keyboard_state & UX_HID_KEYBOARD_STATE_KEY_UP)
            {
                /* The key was released. */
            } else
            {
                /* The key was pressed. */
            }
        #endif

        /* We have a character in the queue. */
        keyboard_queue[keyboard_queue_index] = (UCHAR) keyboard_char;

        /* Can we accept more ? */
        if(keyboard_queue_index < 1024)
            keyboard_queue_index++;
    }

    tx_thread_sleep(10);

}
```

## <a name="ux_host_class_hid_keyboard_ioctl"></a>ux_host_class_hid_keyboard_ioctl

Выполнение функции IOCTL для клавиатуры HID.

### <a name="prototype"></a>Прототип

```c
UINT ux_host_class_hid_keyboard_ioctl(
    UX_HOST_CLASS_HID_KEYBOARD *keyboard_instance,
    ULONG ioctl_function, VOID *parameter);
```

### <a name="description"></a>Описание

Эта функция выполняет конкретную функцию IOCTL для клавиатуры HID. Вызов блокируется и возвращается только в случае ошибки или после завершения выполнения команды.

### <a name="parameters"></a>Параметры

- **keyboard_instance** — указатель на экземпляр клавиатуры HID.
- **ioctl_function** — функция IOCTL для выполнения. Допустимые функции IOCTL см. в приведенной ниже таблице.
- **parameter** — указатель на параметр, относящийся к IOCTL.

### <a name="return-values"></a>Возвращаемые значения

- **UX_SUCCESS** (0x00) — функция IOCTL успешно завершена.
- **UX_FUNCTION_NOT_SUPPORTED**: (0x54) — неизвестная функция IOCTL

### <a name="ioctl-functions"></a>Функции IOCTL

- UX_HID_KEYBOARD_IOCTL_SET_LAYOUT
- UX_HID_KEYBOARD_IOCTL_KEY_DECODING_ENABLE
- UX_HID_KEYBOARD_IOCTL_KEY_DECODING_DISABLE

### <a name="example--change-keyboard-layout"></a>Пример. Изменение раскладки клавиатуры

```c
UINT status;

/* This example shows usage of the SET_LAYOUT IOCTL function.
    USBX receives raw key values from the device (these raw values
    are defined in the HID usage table specification) and optionally
    decodes them for application usage. The decoding is performed
    based on a set of arrays that act as maps – which array is used
    depends on the raw key value (i.e. keypad and non-keypad) and
    the current state of the keyboard (i.e. shift, caps lock, etc.). */

/* When the shift condition is not present and the raw key value
    is not within the keypad value range, this array will be used to decode the raw key value. */

static UCHAR keyboard_layout_raw_to_unshifted_map[] =
{
    0,0,0,0,
    'a','b','c','d','e','f','g',
    'h','i','j','k','l','m','n',
    'o','p','q','r','s','t',
    'u','v','w','x','y','z',
    '1','2','3','4','5','6','7','8','9','0',
    0x0d,0x1b,0x08,0x07,0x20,'-','=','[',']',
    '\\','#',';',0x27,'`',',','.','/',0xf0,
    0xbb,0xbc,0xbd,0xbe,0xbf,0xc0,0xc1,0xc2,0xc3,0xc4,0xc5,0xc6,
    0x00,0xf1,0x00,0xd2,0xc7,0xc9,0xd3,0xcf,0xd1,0xcd,0xcd,0xd0,0xc8,0xf2,
    '/','*','-','+',
    0x0d,'1','2','3','4','5','6','7','8','9','0','.','\\',0x00,0x00,'=',
    0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,
};

/* When the shift condition is present and the raw key value
    is not within the keypad value range, this array will be used to decode the raw key value. */

static UCHAR keyboard_layout_raw_to_shifted_map[] =
{
    0,0,0,0,
    'A','B','C','D','E','F','G',
    'H','I','J','K','L','M','N',
    'O','P','Q','R','S','T',
    'U','V','W','X','Y','Z',
    '!','@','#','$','%','^','&','*','(',')',
    0x0d,0x1b,0x08,0x07,0x20,'_','+','{','}',
    '|','~',':','"','~','<','>','?',0xf0,
    0xbb,0xbc,0xbd,0xbe,0xbf,0xc0,0xc1,0xc2,0xc3,0xc4,0xc5,0xc6,
    0x00,0xf1,0x00,0xd2,0xc7,0xc9,0xd3,0xcf,0xd1,0xcd,0xcd,0xd0,0xc8,0xf2,
    '/','*','-','+',
    0x0d,'1','2','3','4','5','6','7','8','9','0','.','\\',0x00,0x00,'=',
    0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,
};

/* When numlock is on and the raw key value is within the keypad
    value range, this array will be used to decode the raw key value. */

static UCHAR keyboard_layout_raw_to_numlock_on_map[] =
{
    '/','*','-','+',
    0x0d,'1','2','3','4','5','6','7','8','9','0','.','\\',0x00,0x00,'=',
};

/* When numlock is off and the raw key value is within the keypad value
    range, this array will be used to decode the raw key value. */
static UCHAR keyboard_layout_raw_to_numlock_off_map[] =
{
    '/','*','-','+',
    0x0d,0xcf,0xd0,0xd1,0xcb,'5',0xcd,0xc7,0xc8,0xc9,0xd2,0xd3,'\\',0x00,0x
    00,'=',
};

/* Specify the keyboard layout for USBX usage. */
static UX_HOST_CLASS_HID_KEYBOARD_LAYOUT keyboard_layout =
{
    keyboard_layout_raw_to_shifted_map,
    keyboard_layout_raw_to_unshifted_map,
    keyboard_layout_raw_to_numlock_on_map,
    keyboard_layout_raw_to_numlock_off_map,
    /* The maximum raw key value. Values larger than this are discarded. */
    UX_HID_KEYBOARD_KEYS_UPPER_RANGE,
    /* The raw key value for the letter 'a'. */
    UX_HID_KEYBOARD_KEY_LETTER_A,
    /* The raw key value for the letter 'z'. */
    UX_HID_KEYBOARD_KEY_LETTER_Z,
    /* The lower range raw key value for keypad keys - inclusive. */
    UX_HID_KEYBOARD_KEYS_KEYPAD_LOWER_RANGE,
    /* The upper range raw key value for keypad keys. */
    UX_HID_KEYBOARD_KEYS_KEYPAD_UPPER_RANGE
};

/* Call the IOCTL function to change the keyboard layout. */
status = ux_host_class_hid_keyboard_ioctl(keyboard,
    UX_HID_KEYBOARD_IOCTL_SET_LAYOUT, (VOID *)&keyboard_layout);

/* If status equals UX_SUCCESS, the operation was successful. */
```

### <a name="example--disable-keyboard-key-decode"></a>Пример. Отключение декодирования клавиш клавиатуры

```c
UINT status;

/* The following example illustrates IOCTL function of Disable key decode from keyboard layout. */
status = ux_host_class_hid_keyboard_ioctl(keyboard,
    UX_HID_KEYBOARD_IOCTL_DISABLE_KEYS_DECODE, UX_NULL);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_hid_remote_control_usage_get"></a>ux_host_class_hid_remote_control_usage_get

Получение использования удаленного управления

### <a name="prototype"></a>Прототип

```c
UINT ux_host_class_hid_remote_control_usage_get(
    UX_HOST_CLASS_HID_REMOTE_CONTROL *remote_control_instance,
    LONG *usage, 
    ULONG *value);
```

### <a name="description"></a>Описание

Эта функция используется для получения использований удаленного управления.

### <a name="parameters"></a>Параметры

- **remote_control_instance** — указатель на экземпляр удаленного управления HID.
- **usage** — указатель на использование.
- **value** — указатель на значение для использования.

### <a name="return-values"></a>Возвращаемые значения

- **UX_SUCCESS** (0x00) — передача данных завершена.
- **UX_ERROR** (0xff) — данные для отчета отсутствуют.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) — экземпляр класса HID не существует.

Список всех возможных использований слишком длинный и не вмещается в этом руководстве пользователя. Полное описание можно просмотреть в файле ux_host_class_hid.h, в котором приведен полный набор возможных значений.

### <a name="example"></a>Пример

```c
/* Read usages and values as the user changes the vol/bass/treble buttons on the speaker */

while (remote_control != UX_NULL)
{
    status = ux_host_class_hid_remote_control_usage_get(remote_control, &usage, &value);
    if (status == UX_SUCCESS)
    {
        /* We have something coming from the HID remote control,
        we filter the usage here and only allow the
        volume usage which can be VOLUME, VOLUME_INCREMENT or VOLUME_DECREMENT */
        switch(usage)
        {
            case UX_HOST_CLASS_HID_CONSUMER_VOLUME :
            case UX_HOST_CLASS_HID_CONSUMER_VOLUME_INCREMENT :
            case UX_HOST_CLASS_HID_CONSUMER_VOLUME_DECREMENT :

            if (value<0x80)
            {
                if (current_volume + audio_control.ux_host_class_audio_control_res < 0xffff)
                    current_volume = current_volume + audio_control.ux_host_class_audio_control_res;
                } else {
                if (current_volume > audio_control.ux_host_class_audio_control_res)
                    current_volume = current_volumeaudio_control.ux_host_class_audio_control_res;
            }

            audio_control.ux_host_class_audio_control_channel = 1;
            audio_control.ux_host_class_audio_control = UX_HOST_CLASS_AUDIO_VOLUME_CONTROL;
            audio_control.ux_host_class_audio_control_cur = current_volume;
            status = ux_host_class_audio_control_value_set(audio, &audio_control);
            audio_control.ux_host_class_audio_control_channel = 2;
            audio_control.ux_host_class_audio_control = UX_HOST_CLASS_AUDIO_VOLUME_CONTROL;
            audio_control.ux_host_class_audio_control_cur = current_volume;
            status = ux_host_class_audio_control_value_set(audio, &audio_control);
            break;

        }
    }
    tx_thread_sleep(10);

}
```

### <a name="ux_host_class_cdc_acm_read"></a>ux_host_class_cdc_acm_read

Чтение из интерфейса cdc_acm.

### <a name="prototype"></a>Прототип

```c
UINT ux_host_class_cdc_acm_read(
    UX_HOST_CLASS_CDC_ACM *cdc_acm,
    UCHAR *data_pointer,
    ULONG requested_length,
    ULONG *actual_length);
```

### <a name="description"></a>Описание

Эта функция считывает данные из интерфейса cdc_acm. Вызов блокируется и возвращается только в случае ошибки или завершения передачи.

> [!Note]
> Эта функция считывает большой объем необработанных данных с устройства, поэтому она ожидает, пока буфер не будет заполнен или устройство не завершит передачу коротким пакетом (включая пакет нулевого размера). Дополнительные сведения см. в разделе [**Общие рекомендации по массовой передаче данных**](usbx-device-stack-5.md#general-considerations-for-bulk-transfer).

### <a name="parameters"></a>Параметры

- **cdc_acm** — указатель на экземпляр класса cdc_acm.
- **data_pointer** — указатель на адрес буфера полезных данных.
- **requested_length** — длина для получения.
- **actual_length** — фактическая полученная длина.

### <a name="return-values"></a>Возвращаемые значения

- **UX_SUCCESS** (0x00) — передача данных завершена.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) — недопустимый экземпляр cdc_acm.
- **UX_TRANSFER_TIMEOUT** (0x5c) — превышение времени ожидания передачи, чтение не завершено.

### <a name="example"></a>Пример

```c
UINT status;

/* The following example illustrates this service. */

status = ux_host_class_cdc_acm_read(cdc_acm, data_pointer,
    requested_length, &actual_length);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_cdc_acm_write"></a>ux_host_class_cdc_acm_write

Запись в интерфейс cdc_acm

### <a name="prototype"></a>Прототип

```c
UINT ux_host_class_cdc_acm_write(
    UX_HOST_CLASS_CDC_ACM *cdc_acm,
    UCHAR *data_pointer, 
    ULONG requested_length, 
    ULONG *actual_length);
```

### <a name="description"></a>Описание

Эта функция записывает данные в интерфейс cdc_acm. Вызов блокируется и возвращается только в случае ошибки или завершения передачи.

### <a name="parameters"></a>Параметры

- **cdc_acm** — указатель на экземпляр класса cdc_acm.
- **data_pointer** — указатель на адрес буфера полезных данных.
- **requested_length** — длина для отправки.
- **actual_length** — фактическая отправленная длина.

### <a name="return-values"></a>Возвращаемые значения

- **UX_SUCCESS** (0x00) — передача данных завершена.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) — недопустимый экземпляр cdc_acm.
- **UX_TRANSFER_TIMEOUT** (0x5c) — превышение времени ожидания передачи, запись не завершена.

### <a name="example"></a>Пример

```c
UINT status;

/* The following example illustrates this service. */

status = ux_host_class_cdc_acm_write(cdc_acm, data_pointer,
    requested_length, &actual_length);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_cdc_acm_ioctl"></a>ux_host_class_cdc_acm_ioctl

Выполнение функции IOCTL для интерфейса cdc_acm.

### <a name="prototype"></a>Прототип

```c
UINT ux_host_class_cdc_acm_ioctl(
    UX_HOST_CLASS_CDC_ACM *cdc_acm,
    ULONG ioctl_function, 
    VOID *parameter);
```

### <a name="description"></a>Описание

Эта функция выполняет конкретную функцию IOCTL для интерфейса cdc_acm. Вызов блокируется и возвращается только в случае ошибки или после завершения выполнения команды.

### <a name="parameters"></a>Параметры

- **cdc_acm** — указатель на экземпляр класса cdc_acm.
- **ioctl_function** — функция IOCTL для выполнения. Допустимые функции IOCTL см. в приведенной ниже таблице.
- **parameter** — указатель на параметр, относящийся к IOCTL

### <a name="return-value"></a>Возвращаемое значение

- **UX_SUCCESS** (0x00) — передача данных завершена.
- **UX_MEMORY_INSUFFICIENT** (0x12) — недостаточно памяти.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) — недопустимое состояние экземпляра CDC-ACM.
- **UX_FUNCTION_NOT_SUPPORTED** (0x54) — неизвестная функция IOCTL.

### <a name="ioctl-functions"></a>Функции IOCTL:

- UX_HOST_CLASS_CDC_ACM_IOCTL_SET_LINE_CODING
- UX_HOST_CLASS_CDC_ACM_IOCTL_GET_LINE_CODING
- UX_HOST_CLASS_CDC_ACM_IOCTL_SET_LINE_STATE
- UX_HOST_CLASS_CDC_ACM_IOCTL_SEND_BREAK
- UX_HOST_CLASS_CDC_ACM_IOCTL_ABORT_IN_PIPE
- UX_HOST_CLASS_CDC_ACM_IOCTL_ABORT_OUT_PIPE
- UX_HOST_CLASS_CDC_ACM_IOCTL_NOTIFICATION_CALLBACK
- UX_HOST_CLASS_CDC_ACM_IOCTL_GET_DEVICE_STATUS

```c
UINT status;

/* The following example illustrates this service. */

status = ux_host_class_cdc_acm_ioctl(cdc_acm,
    UX_HOST_CLASS_CDC_ACM_IOCTL_GET_LINE_CODING, (VOID *)&line_coding);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## <a name="ux_host_class_cdc_acm_reception_start"></a>ux_host_class_cdc_acm_reception_start

Начинает фоновый прием данных с устройства.

### <a name="prototype"></a>Прототип

```c
UINT ux_host_class_cdc_acm_reception_start(
    UX_HOST_CLASS_CDC_ACM *cdc_acm,
    UX_HOST_CLASS_CDC_ACM_RECEPTION *cdc_acm_reception);
```

### <a name="description"></a>Описание

С помощью этой функции USBX непрерывно считывает данные с устройства в фоновом режиме. По завершении каждой транзакции выполняется обратный вызов, указанный в **cdc_acm_reception**. Он необходим для того, чтобы приложение могло выполнять дальнейшую обработку данных транзакции.

> [!NOTE]
> Функцию **ux_host_class_cdc_acm_read** нельзя использовать, пока выполняется фоновый прием.

### <a name="parameters"></a>Параметры

- **cdc_acm** — указатель на экземпляр класса cdc_acm.
- **cdc_acm_reception** — указатель на параметр, который содержит значения, определяющие поведение фонового приема. Структура этого параметра выглядит следующим образом:

```c
typedef struct UX_HOST_CLASS_CDC_ACM_RECEPTION_STRUCT
{
    ULONG ux_host_class_cdc_acm_reception_state;
    ULONG ux_host_class_cdc_acm_reception_block_size;
    UCHAR *ux_host_class_cdc_acm_reception_data_buffer;
    ULONG ux_host_class_cdc_acm_reception_data_buffer_size;
    UCHAR *ux_host_class_cdc_acm_reception_data_head;
    UCHAR *ux_host_class_cdc_acm_reception_data_tail;
    VOID (*ux_host_class_cdc_acm_reception_callback)(struct UX_HOST_CLASS_CDC_ACM_STRUCT *cdc_acm,
        UINT status, UCHAR *reception_buffer, ULONG reception_size);
} UX_HOST_CLASS_CDC_ACM_RECEPTION;
```

### <a name="return-value"></a>Возвращаемое значение

- **UX_SUCCESS** (0x00) — фоновый прием успешно запущен.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) — неправильный экземпляр класса.

```c
UINT status;
UX_HOST_CLASS_CDC_ACM_RECEPTION cdc_acm_reception;

/* Setup the background reception parameter. */

/* Set the desired max read size for each transaction.
    For example, if this value is 64, then the maximum amount of
    data received from the device in a single transaction is 64.
    If the amount of data received from the device is less than this value,
    the callback will still be invoked with the actual amount of data received. */
cdc_acm_reception.ux_host_class_cdc_acm_reception_block_size = block_size;

/* Set the buffer where the data from the device is read to. */
cdc_acm_reception.ux_host_class_cdc_acm_reception_data_buffer = cdc_acm_reception_buffer;

/* Set the size of the data reception buffer.
    Note that this should be at least as large as ux_host_class_cdc_acm_reception_block_size. */
cdc_acm_reception.ux_host_class_cdc_acm_reception_data_buffer_size = cdc_acm_reception_buffer_size;

/* Set the callback that is to be invoked upon each reception transfer completion. */
cdc_acm_reception.ux_host_class_cdc_acm_reception_callback = reception_callback;

/* Start background reception using the values we defined in the reception parameter. */
status = ux_host_class_cdc_acm_reception_start(cdc_acm_host_data, &cdc_acm_reception);

/* If status equals UX_SUCCESS, background reception has successfully started. */
```

## <a name="ux_host_class_cdc_acm_reception_stop"></a>ux_host_class_cdc_acm_reception_stop

Останавливает фоновый прием пакетов.

### <a name="prototype"></a>Прототип

```c
UINT ux_host_class_cdc_acm_reception_stop(
    UX_HOST_CLASS_CDC_ACM *cdc_acm,
    UX_HOST_CLASS_CDC_ACM_RECEPTION *cdc_acm_reception);
```

### <a name="description"></a>Описание

С помощью этой функции USBX останавливает фоновый прием, ранее запущенный **ux_host_class_cdc_acm_reception_start**.

### <a name="parameters"></a>Параметры

- **cdc_acm** — указатель на экземпляр класса cdc_acm.
- **cdc_acm_reception** — указатель на тот же параметр, который использовался для запуска фонового приема. Структура этого параметра выглядит следующим образом:

```c
typedef struct UX_HOST_CLASS_CDC_ACM_RECEPTION_STRUCT
{
    ULONG ux_host_class_cdc_acm_reception_state;
    ULONG ux_host_class_cdc_acm_reception_block_size;
    UCHAR *ux_host_class_cdc_acm_reception_data_buffer;
    ULONG ux_host_class_cdc_acm_reception_data_buffer_size;
    UCHAR *ux_host_class_cdc_acm_reception_data_head;
    UCHAR *ux_host_class_cdc_acm_reception_data_tail;
    VOID (*ux_host_class_cdc_acm_reception_callback)(
            struct UX_HOST_CLASS_CDC_ACM_STRUCT *cdc_acm, UINT status,
            UCHAR *reception_buffer, ULONG reception_size);
} UX_HOST_CLASS_CDC_ACM_RECEPTION;
```

### <a name="return-value"></a>Возвращаемое значение

- **UX_SUCCESS** (0x00) — фоновый прием успешно остановлен.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) — неправильный экземпляр класса.

```c
UINT status;
UX_HOST_CLASS_CDC_ACM_RECEPTION cdc_acm_reception;

/* Stop background reception. The reception parameter should be the same
    that was passed to ux_host_class_cdc_acm_reception_start. */
status = ux_host_class_cdc_acm_reception_stop(cdc_acm, &cdc_acm_reception);

/* If status equals UX_SUCCESS, background reception has successfully stopped. */
```
