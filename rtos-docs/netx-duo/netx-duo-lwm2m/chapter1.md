---
title: Глава 1. Введение в LWM2M-клиент в NetX Duo для ОСРВ Azure
description: В этой главе приведены основные сведения об клиенте протокола LWM2M в NetX Duo для ОСРВ Azure.
author: v-condav
ms.author: v-condav
ms.date: 01/22/2021
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 12f13c154668b3cadfae0924e59b55631dc27424
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104814691"
---
# <a name="chapter-1--introduction-to-lwm2m-client"></a>Глава 1. Введение в LWM2M-клиент

Протокол LWM2M-клиента в NetX Duo для ОСРВ Azure реализует клиентскую часть для стандарта LWM2M. (LWM2M 1.0-20170208A)

## <a name="netx-lwm2m-client-requirements"></a>Требования к LWM2M-клиенту в NetX

Для правильной работы библиотеки времени выполнения LWM2M-клиента требуется, чтобы экземпляр NetX IP уже был создан. Пакет LWM2M-клиента в NetX не имеет дополнительных требований.

## <a name="netx-lwm2m-client-rfcs"></a>Стандарты RFC для LWM2M-клиента в NetX

LWM2M-клиент совместим со стандартом OMA-TS-LightweightM2M-V1\_0-20170208-A и следующими RFC, связанными с протоколом CoAP.

* RFC 7252 "Протокол ограниченного применения (CoAP)".

* RFC 7641 Наблюдение за ресурсами в протоколе ограниченного применения (CoAP)".

* RFC 6690 "Формат ссылок для ограниченных сред RESTful (CoRE)".

Дополнительные сведения см. на странице [OMA-TS-LightweightM2M-V1\_0-2017208-A](http://www.openmobilealliance.org/release/LightweightM2M/V1_0-20170208-A/OMA-TS-LightweightM2M-V1_0-20170208-A.pdf).