---
title: Глава 2. Установка и использование GUIX
description: В этой главе приведено описание различных проблем, связанных с установкой, настройкой и использованием высокопроизводительного GUIX продукта с пользовательским интерфейсом.
author: philmea
ms.author: philmea
ms.date: 05/19/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 6527227062fc667b3f527a798d6621914c374c5c
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104814980"
---
# <a name="chapter-2---installation-and-use-of-guix"></a>Глава 2. Установка и использование GUIX

В этой главе приведено описание различных проблем, связанных с установкой, настройкой и использованием высокопроизводительного GUIX продукта с пользовательским интерфейсом.  

## <a name="host-considerations"></a>Рекомендации по размещению

Разработка внедряемых решений обычно выполняется на главных компьютерах Windows или Linux (Unix). После компиляции приложения, его связывания и создания исполняемого файла на узле оно скачивается на целевое оборудование для выполнения.

Как правило, скачивание на целевой объект запускается из отладчика инструмента для разработки. После скачивания отладчик отвечает за управление выполнением на целевом объекте (запуск, остановка, точка останова и т. д.), а также за доступ к памяти и регистрам процессора.

Большинство отладчиков инструментов разработки обмениваются данными с целевым оборудованием через подключения OCD (отладка на микросхеме), например JTAG (IEEE 1149.1) и BDM (режим фоновой отладки). Отладчики также взаимодействуют с целевым оборудованием через подключения ICE (внутрисхемная эмуляция). Подключения OCD и ICE обеспечивают надежную передачу данных с минимальным влиянием на резидентное ПО на целевом объекте.

Как и в случае с ресурсами, используемыми на узле, исходный код для GUIX предоставляется в формате ASCII и требует около 30 МБ свободного пространства на жестком диске главного компьютера.

## <a name="target-considerations"></a>Замечания, касающиеся целевого оборудования

GUIX требует 5–80 КБ доступной только для чтения памяти на целевом объекте. Еще 5–10 КБ ОЗУ целевого объекта необходимо для стека потока GUIX и других глобальных структур данных.

Кроме того, GUIX требует использования таймера ThreadX и объекта мьютекса ThreadX. Эти средства используются для периодической обработки и защиты потоков в GUIX.

## <a name="product-distribution"></a>Дистрибутивы продукта

GUIX для ОСРВ Azure можно получить в нашем общедоступном репозитории исходного кода по адресу <https://github.com/azure-rtos/guix/>.

Ниже приведен список важных файлов, которые присутствуют в большинстве дистрибутивов продукта:

| Имя файла&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Описание   |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| gx_api.h        | Этот файл заголовка на C включает все системные равенства, структуры данных и прототипы служб. |
| gx_port.h       | Этот файл заголовка на C включает все определения и структуры данных, относящиеся к конкретным целевым объектам и инструментам разработки.                                                                                                                                         |
| gx.a (или gx.lib) | Это двоичная версия библиотеки GUIX на C. Обычно она создается путем компиляции и архивации предоставленных исходных файлов библиотеки GUIX, но эта библиотека может быть предоставлена в готовом виде в зависимости от целевого оборудования и типа лицензии. |
|

> [!IMPORTANT]
> *Для имен всех файлов используется нижний регистр, что упрощает преобразование команд для платформ разработки Linux (UNIX).*

## <a name="guix-installation"></a>Установка GUIX

GUIX устанавливается путем клонирования репозитория GitHub на локальный компьютер. Ниже приведен типичный синтаксис для создания клона репозитория GUIX на вашем компьютере:

```c
    git clone https://github.com/azure-rtos/guix
```

Или вы можете скачать копию репозитория с помощью соответствующей кнопки на главной странице GitHub.

Вы также можете найти инструкции по созданию библиотеки GUIX на главной странице онлайн-репозитория.

>[!NOTE]  
> *Программному обеспечению приложений требуется доступ к файлу библиотеки GUIX, который обычно имеет имя **gx.a** (или **gx.lib**), а также к включаемым файлам на C **gx_api.h** и **gx_port.h**. Это можно реализовать, задав соответствующий путь в инструментах разработки или скопировав такие файлы в область разработки приложения.*

## <a name="using-guix"></a>Использование GUIX

Использовать GUIX просто. В сущности, код приложения должен включать ***gx_api.h** _ во время компиляции и связь с библиотекой GUIX _*_gx.a_*_ (или _ *_gx.lib_*)*.

Для создания приложения GUIX нужно выполнить четыре простых шага.

| Шаги   | Описание    |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Шаг&nbsp;1. | Включите файл ***gx_api.h*** во все файлы приложения, которые используют службы или структуры данных GUIX.                                                               |
| Шаг&nbsp;2. | Инициализируйте систему GUIX, вызвав ***gx_system_initialize** _ из функции _ *_tx_application_define_** или потока приложения.                       |
| Шаг&nbsp;3. | Создайте экземпляр отображения, холст для отображения и корневое окно, а также любые другие необходимые окна или мини-приложения.                                 |
| Шаг&nbsp;4. | Выполните компиляцию исходного кода и свяжите его с библиотекой среды выполнения GUIX ***gx.a** _ (или _*_gx.lib_**). Полученный образ можно скачать на целевой объект и запустить. |

## <a name="troubleshooting"></a>Устранение неполадок

Каждый порт GUIX предоставляется с демонстрационным приложением, которое выполняется на определенном оборудовании для отображения. Со всеми версиями GUIX предоставляется одна базовая демонстрация. Рекомендуется первой запускать именно демонстрационную систему.

Если демонстрационная система не работает надлежащим образом, выполните следующие операции, чтобы определить проблему:

1. Определите, на каком этапе демонстрации возникает ошибка.

2. Увеличьте размер стека для потока GUIX, изменив константу времени компиляции **GX_THREAD_STACK_SIZE** и повторно выполнив компиляцию библиотеки GUIX.

3. Повторно скомпилируйте библиотеку GUIX с соответствующими параметрами отладки, которые приведены в разделе параметров конфигурации.

4. Изучите возвращаемое состояние от всех вызовов API.

5. Определите, присутствует ли внутренняя системная ошибка, задав точку останова в функции ***_gx_system_error_process***. Код ошибки и сведения о вызывающем объекте должны дать достаточно информации для определения проблемы.

6. Временно обойдите все недавние изменения, чтобы понять, как при этом меняется проблема. Такая информация будет полезной специалистам службы поддержки Майкрософт.

Выполните инструкции, приведенные в разделе "Что нам нужно", чтобы отправить информацию, собранную на этапе устранения неполадок.

## <a name="configuration-options"></a>Параметры конфигурации

При сборке библиотеки GUIX и приложения, использующего GUIX, доступно несколько параметров конфигурации. Такие параметры используются для настройки размера библиотеки и набора функций в соответствии с требованиями вашего приложения. Например, если ваше приложение работает только с одним потоком, использующим службы API GUIX, нужно определить флаг конфигурации **GX_DISABLE_MULTITHREAD_SUPPORT**, чтобы устранить издержки, связанные с защитой критически важных разделов кода от вытеснения несколькими потоками. Вы можете определить различные флаги конфигурации в исходном коде приложения, в командной строке или во включаемом файле **_gx_user.h_**.

При изменении флагов конфигурации для библиотеки GUIX вам потребуется повторно выполнить сборку библиотеки GUIX и модулей приложений, чтобы применить такие изменения конфигурации.

Полный список флагов конфигурации приведен в Приложении З "Флаги конфигурации GUIX во время сборки".

## <a name="guix-version-id"></a>Идентификатор версии GUIX

Сведения о текущей версии GUIX доступны во время выполнения как пользователю, так и ПО приложения. Программист может получить данные о версии GUIX, изучив файл ***gx_port.h** _. Кроме того, этот файл также включает историю версий для соответствующего порта. ПО приложения может получить данные о версии GUIX, обратившись к глобальной строке _ *_ _gx_version_id_* _ в файле _*_gx_port.h_**.

ПО приложения также может получить данные о выпуске из приведенных ниже констант, определенных в файле ***gx_api.h**.* Такие константы указывают текущий выпуск продукта по имени, а также основную и дополнительную версии продукта.

```C
#define __PRODUCT_GUIX__

#define __GUIX_MAJOR_VERSION__

#define __GUIX_MINOR_VERSION__
```