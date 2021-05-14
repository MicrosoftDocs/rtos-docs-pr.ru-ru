---
title: Глава 2. Установка и использование ОСРВ Azure NetX Crypto
description: В этой главе описываются различные проблемы, связанные с установкой, настройкой и использованием компонента NetX Crypto.
author: philmea
ms.author: philmea
ms.date: 05/19/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 1616667c5efd73229ed69bcd4e5de5f80e5826f9
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104815264"
---
# <a name="chapter-2---installation-and-use-of-azure-rtos-netx-crypto"></a>Глава 2. Установка и использование ОСРВ Azure NetX Crypto

В этой главе описываются различные проблемы, связанные с установкой, настройкой и использованием компонента ОСРВ Azure NetX Crypto.

## <a name="product-distribution"></a>Распространение продукта

Компонент ОСРВ Azure NetX Crypto доступен по адресу [https://github.com/azure-rtos/netxduo](https://github.com/azure-rtos/netxduo). Он содержит файлы исходного кода, включаемые файлы и PDF-файл, содержащий этот документ:

- **nx_crypto.h**: файл заголовка общедоступного API модуля NetX Crypto;
- **nx_crypto_*.c/h**: файлы исходного кода C/H для NetX Crypto;
- **nx_port.h**: файл заголовка C, содержащий все определения и структуры данных для инструментов разработки и целевых платформ;
- **NetX_Crypto_User_Guide.pdf**: PDF-файл с описанием модуля NetX Crypto.

## <a name="netx-crypto-installation"></a>Установка NetX Crypto

Все ранее упомянутые дистрибутивы доступны в каталоге **crypto_libraries**, который находится на корневом уровне репозитория NetX Duo.

Чтобы использовать NetX Crypto, весь дистрибутив, упомянутый ранее, должен быть скопирован в каталог, где установлен экземпляр NetX. Например, если экземпляр NetX установлен в каталоге \threadx\arm7\NetX, то каталоги nx_crypto *.* должны быть скопированы в \threadx\arm7\NetXCrypto.

Чтобы использовать NetX Crypto в изолированном режиме, необходимо скопировать в проект приложения весь дистрибутив, упомянутый выше. Например, следует скопировать в проект приложения каталог **crypto_libraries** или создать проект библиотеки с каталогом **crypto_libraries**, а затем связать ее с проектом приложения. 

## <a name="using-netx-crypto"></a>Использование NetX Crypto

Использовать NetX Crypto просто. Проще говоря, код приложения должен включать в себя *nx_crypto.h*.  После добавления файла *nx_crypto.h* код приложения сможет выполнять вызовы функций NetX Crypto, описанных далее в этом руководстве.

## <a name="configuration-options"></a>Параметры конфигурации

Существует ряд параметров конфигурации для сборки NetX Crypto. Ниже приведен список всех параметров с подробным описанием каждого из них.

- **NX_CRYPTO_MAX_RSA_MODULUS_SIZE**: если этот параметр определен, он задает максимальный ожидаемый размер модуля RSA в битах. Значение по умолчанию — 4096 для 4096-разрядных модулей. Другие возможные значения: 3072, 2048 или 1024 (не рекомендуется).
- **NX_CRYPTO_FIPS**: если этот параметр определен, он включает дополнительные функции безопасности, необходимые для соответствия стандартам FIPS. Этот параметр не включается для сборки, не соответствующей требованиям FIPS.
- **NX_CRYPTO_STANDALONE_ENABLE**: если этот параметр определен, он позволяет использовать NetX Crypto в изолированном режиме (без ОСРВ Azure). По умолчанию этот символ не определен.