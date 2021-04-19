---
title: Глава 2. Установка и использование TraceX для ОСРВ Azure
description: В этой главе описываются различные вопросы, связанные с установкой, настройкой и использованием средства системного анализа TraceX для ОСРВ Azure.
author: philmea
ms.service: rtos
ms.topic: article
ms.date: 5/19/2020
ms.author: philmea
ms.openlocfilehash: 05d7fe3df38c7e8a3480c8ea0d4922a109de9ede
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104815768"
---
# <a name="chapter-2---installation-and-use-of-azure-rtos-tracex"></a>Глава 2. Установка и использование TraceX для ОСРВ Azure

В этой главе описываются различные вопросы, связанные с установкой, настройкой и использованием средства системного анализа TraceX для ОСРВ Azure. 

## <a name="product-distribution"></a>Распространение продукта

Приложение TraceX можно получить из [Microsoft App Store](https://microsoft.com/store/apps), выполнив поиск по строке TraceX или перейдя на [страницу TraceX](https://www.microsoft.com/p/azure-rtos-tracex/9nf1lfd5xxg3?activetab=pivot:overviewtab) по прямой ссылке. После этого выполните следующее.

1. На странице TraceX в App Store нажмите кнопку **Получить** или **Установить**, чтобы установить TraceX.

1. В браузере может появиться сообщение с предложением открыть Microsoft Store, как показано на рисунке ниже. В этом случае нажмите кнопку **Открыть**.
![Нажмите "Открыть", чтобы установить TraceX.](../guix/media/guix-studio/open-ms-store.png)

1. Когда установка завершится, нажмите кнопку **Запустить**. 

## <a name="using-tracex"></a>Использование TraceX

Чтобы начать работу с TraceX, достаточно просто открыть в TraceX файл трассировки. Запустите TraceX с помощью кнопки ***Пуск** _. Отобразится графический пользовательский интерфейс TraceX. Теперь вы можете использовать TraceX для просмотра существующего буфера трассировки на целевой системе в графическом виде. Для этого щелкните _ *_Файл -> Открыть,_** а затем введите двоичный файл трассировки.

>[!IMPORTANT]
>*Также можно дважды щелкнуть любой файл трассировки с расширением **trx,** , что автоматически запустит TraceX.*

![Снимок экрана: графический пользовательский интерфейс TraceX.](./media/user-guide/screen_shot_8.png)

**Рис. 1**

>[!IMPORTANT]
>*Изучите инструкции в **главе 5**, где описано создание буферов трассировки на целевой системе с помощью ThreadX.*

## <a name="tracex-examples"></a>Примеры для TraceX

При первом запуске или обновлении приложения TraceX вам будет предложено установить примеры файлов трассировки для TraceX и файл custom_events.trxc в выбранный пользователем каталог на локальном компьютере.

Завершив этот шаг установки, вы получите примеры файлов трассировки с расширением **trx** в подкаталоге **TraceFiles** выбранного для установки каталога. Эти предварительно созданные примеры помогут быстро освоиться с изучением в TraceX буферов трассировки, созданных системой ThreadX, которая работает вместе с основным приложением.

Один из примеров трассировки всегда присутствует в файле ***demo_threadx.trx** _. В этом примере файла трассировки демонстрируется выполнение стандартных операций ThreadX, которые описаны в главе 6 _"Руководство пользователя по ThreadX"*.

![Снимок экрана: диалоговое окно открытия файла в TraceX.](./media/user-guide/screen_shot_9.png)

**Рис. 2**