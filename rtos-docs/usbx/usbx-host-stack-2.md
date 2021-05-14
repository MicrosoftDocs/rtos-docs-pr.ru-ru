---
title: Глава 2. Установка стека узлов ОСРВ Azure USBX
description: Узнайте, как установить стек узлов USBX.
author: philmea
ms.author: philmea
ms.date: 5/19/2020
ms.service: rtos
ms.topic: article
ms.openlocfilehash: 4c33f95b8ac268c557fd947a1303ec3af315a37e
ms.sourcegitcommit: d8edbb3207fe99f8afb431597dac063e73383e68
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/05/2021
ms.locfileid: "106377090"
---
# <a name="chapter-2---azure-rtos-usbx-host-stack-installation"></a>Глава 2. Установка стека узлов ОСРВ Azure USBX

## <a name="host-considerations"></a>Рекомендации по размещению

### <a name="computer-type"></a>Тип компьютера

Разработка внедренных решений обычно выполняется на главных компьютерах с Windows или Unix. После компиляции, связывания и размещения на узле приложение скачивается на целевое оборудование для выполнения.

### <a name="download-interfaces"></a>Интерфейсы скачивания

Как правило, скачивание на целевое оборудование выполняется через последовательный интерфейс RS-232, хотя становятся все более популярными параллельные интерфейсы, USB и Ethernet. Доступные варианты представлены в документации по инструменту разработки.

### <a name="debugging-tools"></a>Инструменты отладки

Отладка обычно выполняется по той же ссылке, что и скачивание образа программы. Существует множество отладчиков, от небольших программ мониторинга, выполняющихся на целевом объекте, до инструментов Background Debug Monitor (BDM) и In-Circuit Emulator (ICE). Инструмент ICE обеспечивает наиболее надежную отладку фактического целевого оборудования.

### <a name="required-hard-disk-space"></a>Требуемое место на жестком диске

Исходный код USBX поставляется в формате ASCII и требует примерно 500 КБ пространства на жестком диске главного компьютера.

## <a name="target-considerations"></a>Рекомендации по целевому оборудованию

Для USBX требуется от 24 до 64 КБ доступной только для чтения памяти (ПЗУ) на целевом оборудовании в режиме узла. Необходимый объем памяти зависит от типа используемого контроллера и классов USB, связанных с USBX. Для глобальных структур данных и пула памяти USBX требуется еще 32 КБ памяти (ОЗУ) на целевом оборудовании. Этот пул памяти также может быть скорректирован в зависимости от ожидаемого количества устройств на шине USB и типа контроллера USB. На стороне устройства USBX требуется примерно 10–12 КБ ПЗУ, в зависимости от типа контроллера устройства. Использование памяти ОЗУ зависит от типа класса, эмулируемого устройством.

USBX также использует семафоры, мьютексы и потоки ThreadX для защиты многопоточности, как и приостановку ввода-вывода и периодическую обработку для мониторинга топологии шины USB.

### <a name="product-distribution"></a>Дистрибутивы продукта

ОСРВ Azure USBX можно получить из нашего репозитория общедоступного исходного кода по адресу <https://github.com/azure-rtos/usbx/>.

Ниже приведен список важных файлов в этом репозитории.

- ***ux_api.h***: это файл заголовка C, который содержит все системные равенства, структуры данных и прототипы служб.
- ***ux_port.h***: это файл заголовка C, который содержит все определения и структуры данных, относящиеся к конкретным инструментам разработки.
- ***ux.lib***: это двоичная версия библиотеки C для USBX. Она распространяется с помощью стандартного пакета.
- ***demo_usbx.c*** — это файл C, содержащий простую демоверсию для USBX.

Все имена файлов указаны в нижнем регистре. Такое соглашение об именовании упрощает преобразование команд для платформ разработки UNIX.

## <a name="usbx-installation"></a>Установка USBX

USBX устанавливается путем клонирования репозитория GitHub на локальный компьютер. Ниже приведен типичный синтаксис для создания клона репозитория USBX на вашем компьютере.

```powershell
    git clone https://github.com/azure-rtos/usbx
```

Вы также можете скачать копию репозитория с помощью соответствующей кнопки на главной странице GitHub.

Кроме того, вы можете найти инструкции по созданию библиотеки USBX на главной странице веб-репозитория.

Приведенные ниже общие инструкции применимы практически к любой установке.

1. Используйте тот же каталог на жестком диске узла, в который вы ранее установили ThreadX. Все имена USBX уникальны и не повлияют на предыдущую установку USBX.
2. Добавьте вызов ***ux_system_initialize** _ в начало _ *_tx_application_define_.* * На этом этапе инициализируются ресурсы USBX.
3. Добавьте вызов ***ux_host_stack_initialize*** .
4. Добавьте один или несколько вызовов для инициализации требуемого экземпляра USBX.
5. Добавьте один или несколько вызовов для инициализации хост-контроллеров, доступных в системе.
6. Может потребоваться изменить файл tx_low_level_initialize.c, чтобы добавить инициализацию низкого уровня для оборудования и маршрутизацию векторов прерывания. Это относится к особенностям аппаратной платформы и не будет рассматриваться в этом документе.
7. Выполните компиляцию исходного кода приложения и связывание с библиотеками времени выполнения USBX и ThreadX — ux.a (или ux.lib) и tx.a (или tx.lib) (могут также потребоваться FileX и (или) NetX, если требуется скомпилировать класс хранения USB и (или) классы сети USB). Полученный пакет можно скачать на целевое оборудование и выполнить.

## <a name="configuration-options"></a>Параметры конфигурации

Существует ряд параметров конфигурации для сборки библиотеки USBX. Все параметры находятся в файле ***ux_user.h***.

Эти параметры подробно описаны в списке ниже.


- **UX_PERIODIC_RATE**: это значение представляет число тактов в секунду для конкретной аппаратной платформы. Значение по умолчанию — 1000, оно задает 1 такт на миллисекунду.
- **UX_MAX_CLASS_DRIVER**: это максимальное число классов, которое может быть загружено USBX. Это значение представляет контейнер классов, а не число экземпляров класса. Например, если конкретной реализации USBX требуется класс концентраторов, класс принтеров и класс хранилищ, то значение UX_MAX_CLASS_DRIVER может быть равно 3 независимо от числа устройств, принадлежащих к этим классам.
- **UX_MAX_HCD**: это значение представляет число разных хост-контроллеров, доступных в системе. Для поддержки USB 1.1 это значение обычно равно 1. Для поддержки USB 2.0 это значение может быть больше 1. Это значение представляет количество одновременно работающих хост-контроллеров. Например, если выполняются два экземпляра OHCI или один контроллер EHCI и один контроллер OHCI, то для UX_MAX_HCD должно быть задано значение 2. 
- **UX_MAX_DEVICES**: это значение представляет максимальное число устройств, которые могут быть подключены к USB. Как правило, теоретически максимальное число устройств, которые можно подключить к одному USB-устройству, равно 127. Это значение можно уменьшить, чтобы уменьшить снизить потребление памяти. Следует отметить, что это значение представляет общее количество устройств независимо от числа шин USB в системе.
- **UX_MAX_ED**: это значение представляет максимальное число дескрипторов ED в пуле контроллеров. Это число назначается только одному контроллеру. Если имеется несколько экземпляров контроллера, то для каждого контроллера используется отдельное значение.
- **UX_MAX_TD и UX_MAX_ISO_TD**: эти значения представляют максимальное число обычных и изохронных дескрипторов передачи (TD) в пуле контроллера. Это число назначается только одному контроллеру. Если имеется несколько экземпляров контроллера, то для каждого контроллера используется отдельное значение.
- **UX_THREAD_STACK_SIZE**: это размер стека в байтах для потоков USBX. Обычно это может быть 1024 байт или 2048 байт, в зависимости от используемого процессора и хост-контроллера.
- **UX_HOST_ENUM_THREAD_STACK_SIZE**: это размер стека потоков перечисления узлов USB. Если этот параметр не задан, то размер стека потоков перечисления узлов USBX задается равным **UX_THREAD_STACK_SIZE**. 
- **UX_HOST_HCD_THREAD_STACK_SIZE**: это размер стека потоков HCD узлов USB. Если этот параметр не задан, то размер стека потоков HCD узлов USBX задается равным **UX_THREAD_STACK_SIZE**.
- **UX_THREAD_PRIORITY_ENUM**: это значение приоритета ThreadX для потоков перечисления USBX, которые отслеживают топологию шины.
- **UX_THREAD_PRIORITY_CLASS**: это значение приоритета ThreadX для стандартных потоков USBX.
- **UX_THREAD_PRIORITY_KEYBOARD**: это значение приоритета ThreadX для класса HID-клавиатур USBX.
- **UX_THREAD_PRIORITY_HCD**: это значение приоритета ThreadX для потока хост-контроллера.
- **UX_NO_TIME_SLICE**: это значение фактически определяет временной срез, который будет использоваться для потоков. Например, если задано значение 0, то целевой порт ThreadX не будет использовать временные срезы.
- **UX_MAX_HOST_LUN**: это значение представляет максимальное число логических устройств SCSI, представленных в драйвере класса хранилищ узла.
- **UX_HOST_CLASS_STORAGE_INCLUDE_LEGACY_PROTOCOL_SUPPORT**: если этот параметр определен, то он включает в себя код для работы с устройствами хранения, использующими протокол CB или CBI (например, это могут быть гибкие диски). По умолчанию он отключен, так как эти протоколы устарели. Их заменил протокол BOT, который используют практически все современные устройства хранения.
- **UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE**: если этот параметр определен, то служба ux_host_class_hid_keyboard_key_get будет сообщать только об изменениях состояния клавиш, то есть об их нажатии и отпускании. По умолчанию она сообщает только о нажатии клавиш.
- **UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE_REPORT_KEY_DOWN_ONLY**: используется, только если определен параметр **UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE**. Если этот параметр определен, то служба ux_host_class_hid_keyboard_key_get сообщает только о нажатии клавиш, но не об их отпускании.
- **UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE_REPORT_LOCK_KEYS**: используется, только если определен параметр **UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE**. Если этот параметр определен, то служба ux_host_class_hid_keyboard_key_get сообщает об изменения состояния клавиш переключения режима (CAPS LOCK, NUM LOCK, SCROLL LOCK).
- **UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE_REPORT_MODIFIER_KEYS**: используется, только если определен параметр **UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE**. Если этот параметр определен, то служба ux_host_class_hid_keyboard_key_get сообщает об изменении состояния клавиш-модификаторов (CTRL, ALT, SHIFT, GUI).
- **UX_HOST_CLASS_CDC_ECM_NX_PKPOOL_ENTRIES**: если этот параметр определен, его значение представляет количество пакетов в классе узлов CDC-ECM. Значение по умолчанию равно 16.

## <a name="source-code-tree"></a>Дерево исходного кода

Файлы USBX предоставлены в нескольких каталогах.

![Дерево исходного кода](media/usbx-host-stack/source-code-tree.png)

Чтобы файлы можно было распознавать по именам, принято приведенное ниже соглашение.

| Имя суффикса файла | Описание файла                          |
| ---------------- | ----------------------------------------- |
| ux_host_stack    | Основные файлы стека узлов USBX                |
| ux_host_class    | Файлы классов стека узлов USBX             |
| ux_hcd           | Файлы драйвера контроллера стека узлов USBX   |
| ux_device_stack  | Основные файлы стека устройств USBX              |
| ux_device_class  | Файлы классов стека устройств USBX           |
| ux_dcd           | Файлы драйвера контроллера стека устройств USBX |
| ux_otg           | Файлы, связанные с драйвером контроллера OTG для USBX  |
| ux_pictbridge    | Файлы PictBridge для USBX                     |
| ux_utility       | Служебные функции USBX                    |
| demo_usbx        | Демонстрационные файлы для USBX              |

## <a name="initialization-of-usbx-resources"></a>Инициализация ресурсов USBX

В USBX имеется собственный диспетчер памяти. Память для USBX необходимо выделить перед инициализацией узла или устройства USBX. Диспетчер памяти USBX поддерживает системы с кэшированием памяти.

Приведенная ниже функция инициализирует ресурсы памяти USBX: 128 КБ обычной памяти без отдельного пула для некэшируемой памяти:

```c
/* Initialize USBX Memory */

ux_system_initialize(memory_pointer,(128*1024),UX_NULL,0);
```

Прототип для ux_system_initialize выглядит показан ниже.

```c
UINT ux_system_initialize( 
    VOID *regular_memory_pool_start,
    ULONG regular_memory_size,
    VOID *cache_safe_memory_pool_start,
    ULONG cache_safe_memory_size);
```

### <a name="input-parameters"></a>Входные параметры:

- **regular_memory_pool_start**: начало обычного пула памяти.
- **regular_memory_size**: размер обычного пула памяти.
- **cache_safe_memory_pool_start**: начало пула некэшируемой памяти.
- **cache_safe_memory_size**: размер пула некэшируемой памяти.    |

Не для всех систем нужно определять некэшируемую память. В такой системе во время инициализации для указателя памяти будут переданы значения UX_NULL, а размер пула будет равен 0. После этого USBX будет использовать стандартный пул памяти вместо некэшируемого пула.

В системе, где обычная память не является некэшируемой и для контроллера требуется память с прямым доступом (например, контроллеры OHCI, EHCI и пр.), необходимо определить пул памяти в некэшируемой зоне.

## <a name="uninitialization-of-usbx-resources"></a>Отмена инициализации ресурсов USBX

Работу экземпляра USBX можно завершить, освободив его ресурсы. Перед завершением экземпляра USBX нужно правильно завершить все ресурсы классов и контроллеров. Приведенная ниже функция отменяет инициализацию ресурсов памяти USBX.

```c
/* Unitialize USBX Resources */

ux_system_uninitialize();
```

Прототип для ux_system_initialize выглядит показан ниже.

```c
UINT ux_system_uninitialize(VOID);
```

## <a name="definition-of-usb-host-controllers"></a>Определение хост-контроллеров USB

Для работы USBX в режиме узла необходимо определить по крайней мере один хост-контроллер USB. Это определение должно находиться в файле инициализации приложения. Следующая строка выполняет определение универсального хост-контроллера.

```c
ux_host_stack_hcd_register("ux_hcd_controller",
        ux_hcd_controller_initialize, 0xd0000, 0x0a);
```

Прототип ux_host_stack_hcd_register приведен ниже.

```c
UINT ux_host_stack_hcd_register(
    UCHAR *hcd_name,
    UINT (*hcd_initialize_function)(struct UX_HCD_STRUCT *),
    ULONG hcd_param1, ULONG hcd_param2);
```

Функция ux_host_stack_hcd_register использует следующие параметры.

- **hcd_name**: строка имени контроллера.
- **hcd_initialize_function**: функция инициализации контроллера.
- **hcd_param1**: обычно это значение ввода-вывода или объем памяти, используемой контроллером.
- **hcd_param2**: обычно это значение IRQ, используемое контроллером.

В предыдущем примере:

- ux_hcd_controller: имя контроллера.
- ux_hcd_controller_initialize: подпрограмма инициализации хост-контроллера.
- 0xd0000 — это адрес, по которому регистры хост-контроллера отображаются в памяти.
- 0x0A — это значение IRQ, используемое хост-контроллером.

Ниже приведен пример инициализации USBX в режиме узла с одним хост-контроллером и несколькими классами.

```c
UINT status;

/* Initialize USBX. */
ux_system_initialize(memory_ptr, (128*1024),0,0);

/* The code below is required for installing the USBX host stack. */
status = ux_host_stack_initialize(UX_NULL);

/* If status equals UX_SUCCESS, host stack has been initialized. */

/* Register all the host classes for this USBX implementation. */
status = ux_host_class_register("ux_host_class_hub", ux_host_class_hub_entry);

/* If status equals UX_SUCCESS, host class has been registered. */
status = ux_host_class_register("ux_host_class_storage", ux_host_class_storage_entry);

/* If status equals UX_SUCCESS, host class has been registered. */
status = ux_host_class_register("ux_host_class_printer", ux_host_class_printer_entry);

/* If status equals UX_SUCCESS, host class has been registered. */
status = ux_host_class_register("ux_host_class_audio", ux_host_class_audio_entry);

/* If status equals UX_SUCCESS, host class has been registered. */

/* Register all the USB host controllers available in this system. */ 
status = ux_host_stack_hcd_register("ux_hcd_controller", ux_hcd_controller_initialize, 0x300000, 0x0a);

/* If status equals UX_SUCCESS, USB host controllers have been registered. */
```

## <a name="definition-of-host-classes"></a>Определение классов узлов

Необходимо определить один или несколько классов узлов с помощью USBX. Класс USB необходим для управления USB-устройством после того, как оно будет настроено в стеке USB. Класс USB зависит от устройства. Для работы с USB-устройством может потребоваться один или несколько классов в зависимости от числа интерфейсов, указанных в дескрипторах USB-устройства.

Ниже приведен пример регистрации класса концентраторов.

```c
status = ux_host_stack_class_register("ux_host_class_hub", ux_host_class_hub_entry);
```

Ниже приведен прототип функции ux_host_class_register.

```c
UINT ux_host_stack_class_register(
    UCHAR *class_name, 
    UINT (*class_entry_address) (struct UX_HOST_CLASS_COMMAND_STRUCT *));
```

- **class_name**: имя класса.
- **class_entry_address**: точка входа класса.

В примере инициализации класса концентраторов:

- ux_host_class_hub: имя класса концентратора;
- ux_host_class_hub_entry: точка входа класса концентраторов.

## <a name="troubleshooting"></a>Устранение неполадок

USBX поставляется с демонстрационным файлом и окружением моделирования. Рекомендуется сначала запустить демонстрационную систему — на целевом оборудовании или соответствующей демонстрационной платформе.

Если демонстрационная система не работает, попробуйте выполнить приведенные ниже действия, чтобы устранить проблему.

## <a name="usbx-version-id"></a>Идентификатор версии USBX

Сведения о текущей версии USBX доступны во время выполнения как пользователю, так и программному обеспечению приложения. Программист может получить данные о версии USBX, ознакомившись с файлом ***ux_port.h** _. Кроме того, этот файл содержит журнал версий для соответствующей платформы. Программное обеспечение приложения может получить версию USBX, проанализировав глобальную строку _ *_ _ux_version_id_* _, которая определена в файле _*_ux_port.h_**.