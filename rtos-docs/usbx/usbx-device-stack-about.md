---
title: Руководство пользователя по стеку устройств USBX в ОСРВ Azure
description: В этом руководстве приведены полные сведения о USBX для ОСРВ Azure, высокопроизводительном базовом программном обеспечении для USB от корпорации Майкрософт.
author: philmea
ms.author: philmea
ms.date: 5/19/2020
ms.service: rtos
ms.topic: article
ms.openlocfilehash: c8e9360c8b72adbc41f840a48e333668c489399e
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104814339"
---
# <a name="azure-rtos-usbx-device-stack-user-guide"></a>Руководство пользователя по стеку устройств USBX в ОСРВ Azure

В этом руководстве приведены полные сведения о USBX для ОСРВ Azure, высокопроизводительном базовом программном обеспечении для USB от корпорации Майкрософт.

Оно предназначено для разработчиков внедренного программного обеспечения, работающего в реальном времени. Такие разработчики должны быть знакомы со стандартными функциями операционной системы, работающими в реальном времени, спецификациями USB и языком программирования C.

Технические сведения о USB см. в спецификациях USB и классов USB, которые можно скачать по адресу https://www.USB.org/developers

## <a name="organization"></a>План

- [**Глава 1**](usbx-device-stack-1.md) содержит введение в USBX для ОСРВ Azure.

- [**Глава 2**](usbx-device-stack-2.md) содержит основные шаги по установке и использованию USBX для ОСРВ Azure с приложением ThreadX.

- [**Глава 3**](usbx-device-stack-3.md) описывает функциональные компоненты стека устройств USBX для ОСРВ Azure.

- [**Глава 4**](usbx-device-stack-4.md) описывает службы стека устройств USBX для ОСРВ Azure.

- [**Глава 5**](usbx-device-stack-5.md) описывает все классы устройств USBX для ОСРВ Azure, в том числе их API.

## <a name="customer-support-center"></a>Центр поддержки клиентов

Отправьте запрос в службу поддержки на портале Azure, если у вас возникли вопросы или требуется помощь при выполнении приведенных здесь шагов. Предоставьте нам следующие сведения в сообщении электронной почты, чтобы мы могли более эффективно решить ваш запрос в службу поддержки:

1. Подробное описание проблемы, включая частоту возникновения и возможность гарантированного воспроизведения.
2. Подробное описание любых изменений в приложении и (или) ThreadX для ОСРВ Azure, предшествующих проблеме.
3. Содержимое строки **_tx_version_id** в файле **_tx_port.h_** вашего дистрибутива. Эта строка предоставит нам полезную информацию о среде выполнения.
4. Содержимое в ОЗУ переменной *_tx_build_options* типа **ULONG**. Эта переменная предоставит нам сведения о том, как была создана библиотека ThreadX для ОСРВ Azure.