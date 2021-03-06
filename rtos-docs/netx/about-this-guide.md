---
title: Сведения о руководстве пользователя по NetX в ОСРВ Azure
description: Это руководство содержит исчерпывающие сведения о высокопроизводительном сетевом стеке NetX в ОСРВ Azure, созданном корпорацией Майкрософт.
author: philmea
ms.author: philmea
ms.date: 05/19/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 7d77997e8c5bac598f382e1169a56727af09ab108f57c90cc6265df0691b5926
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2021
ms.locfileid: "116796418"
---
# <a name="about-the-azure-rtos-netx-user-guide"></a>Сведения о руководстве пользователя по NetX в ОСРВ Azure

Это руководство содержит исчерпывающие сведения о высокопроизводительном сетевом стеке NetX в ОСРВ Azure, созданном корпорацией Майкрософт.

Оно предназначено для разработчиков встроенного программного обеспечения реального времени, знакомых с основными принципами работы сетей, ThreadX в ОСРВ Azure и языком программирования C.

## <a name="organization"></a>План

[Глава 1](chapter1.md) — ознакомительные сведения о NetX для ОСРВ Azure.

[Глава 2](chapter2.md) — описание основных действий по установке и использованию NetX для ОСРВ Azure с приложением ThreadX.

[Глава 3](chapter3.md) — описание возможностей системы NetX для ОСРВ Azure и базовые сведения о сетевых стандартах TCP/IP.

[Глава 4](chapter4.md) — подробные сведения об интерфейсе между приложением и NetX для ОСРВ Azure.

[Глава 5](chapter5.md) — описание сетевых драйверов для NetX в ОСРВ Azure.

[Приложение А](appendix-a.md) — службы NetX для ОСРВ Azure.

[Приложение Б](appendix-b.md) — константы NetX для ОСРВ Azure.

[Приложение В](appendix-c.md) — типы данных NetX для ОСРВ Azure.

[Приложение Г](appendix-d.md) — API сокета, совместимого с BSD.

[Приложение Д](appendix-e.md) — таблица ASCII.

## <a name="azure-rtos-netx-data-types"></a>Типы данных NetX для ОСРВ Azure

Помимо пользовательских типов данных структур управления NetX для ОСРВ Azure, есть ряд специальных типов данных, которые используются в интерфейсах вызова служб NetX для ОСРВ Azure. Эти специальные типы данных сопоставляются непосредственно с типами данных базового компилятора C. Это необходимо для обеспечения переносимости кода между разными компиляторами C. Конкретная реализация наследуется от ThreadX и представлена в файле ***tx_port.h***, включенном в состав дистрибутива ThreadX.

Ниже приведен список типов данных вызова служб NetX для ОСРВ Azure и их значений.

| Типы данных | Описание  |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| **UINT**  | Простое целое число без знака. Этот тип должен поддерживать 32-разрядные данные без знака, но свободно сопоставляется с наиболее удобным типом данных без знака. |
| **ULONG** | Длинное целое число без знака. Этот тип должен поддерживать 32-разрядные данные без знака.                                                                      |
| **VOID**  | Почти всегда эквивалентен типу void компилятора.                                                                                 |
| **CHAR**  | Чаще всего это стандартный 8-разрядный символьный тип.                                                                                           |

В исходном коде NetX для ОСРВ Azure используются дополнительные типы данных. Они находятся в файлах ***tx_port.h** _ или _ *_nx_port.h_**.

## <a name="customer-support-center"></a>Центр поддержки клиентов

Если у вас возникли вопросы или требуется помощь при выполнении приведенных здесь шагов, отправьте запрос в службу поддержки на портале Azure. Чтобы мы могли более эффективно решить ваш запрос на поддержку, укажите в сообщении электронной почты следующие сведения.

1. Подробное описание проблемы, включая частоту возникновения и действия, позволяющие гарантированно воспроизвести эту проблему (если это возможно).

2. Подробное описание любых изменений в приложении и (или) NetX для ОСРВ Azure, предшествующих проблеме.

3. Содержимое строк **_tx_version_id** и **_nx_version_id** из файлов **_tx_port.h_ *_ и _* _nx_port.h_** вашего дистрибутива. Эти строки предоставят нам полезную информацию о вашей среде выполнения.

4. Содержимое ОЗУ для следующих переменных **ULONG**:

    **_tx_build_options**;

    **_nx_system_build_options1**

    **_nx_system_build_options2**

    **_nx_system_build_options3**

    **_nx_system_build_options4**

    **_nx_system_build_options5**.

    Эти переменные позволят нам узнать, как были созданы библиотеки ThreadX и NetX для ОСРВ Azure.

5. Буфер трассировки, записанный сразу после обнаружения проблемы. Для этого необходимо скомпилировать библиотеки ThreadX и NetX для ОСРВ Azure с параметром **TX_ENABLE_EVENT_TRACE** и вызвать службу **tx_trace_enable**, указав данные буфера трассировки. Дополнительные сведения см. в руководстве пользователя по ThreadX для ОСРВ Azure.
