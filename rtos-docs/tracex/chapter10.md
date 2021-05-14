---
title: Глава 10. Пользовательские события
description: В этой главе приводится описание создания определяемых пользователем событий, а также настраиваемых значков и полей сведений для таких событий.
author: philmea
ms.service: rtos
ms.topic: article
ms.date: 5/19/2020
ms.author: philmea
ms.openlocfilehash: 635c2d79922de9d5649bab841ae946cac862056c
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104815791"
---
# <a name="chapter-10---customer-user-events"></a>Глава 10. Пользовательские события

В этой главе приводится описание создания определяемых пользователем событий, а также настраиваемых значков и полей сведений для таких событий. Эта глава состоит из следующих разделов. 

## <a name="inserting-user-defined-events"></a>Вставка определяемых пользователем событий

ThreadX позволяет разработчикам регистрировать собственные события и указывать еще более точную и полезную информацию, которую можно просматривать в TraceX в графическом виде. Номера определяемых пользователем событий входят в диапазон от

**TX_TRACE_USER_EVENT_START** (4096) до **TX_TRACE_USER_EVENT_END** (65535) включительно. Размещение событий в буфере трассировки выполняется с помощью службы ***tx_trace_user_event_insert***, определенной в главе 5. В следующем примере приведены вызовы службы для вставки двух определяемых пользователем событий в текущий буфер трассировки в целевом объекте. Это события 4096 и 4098.

```c
tx_trace_user_event_insert(4096, 1, 2, 3, 4);
tx_trace_user_event_insert(4098,0x100,0x200,0x300,0x400);
```

## <a name="default-display-of-user-defined-events"></a>Отображение определяемых пользователем событий по умолчанию

![Значок определяемого пользователем события](./media/user-guide/tx-events/image0.png)

По умолчанию TraceX отображает все пользовательские события с заданным по умолчанию значком, как описано в главе 6. На рис. 28 показан заданный по умолчанию значок для определяемых пользователем событий 452 и 453, которые были помещены в буфер событий с помощью предыдущих примеров использования службы ***tx_trace_user_event_insert***.

![Снимок экрана: отображение определяемых пользователем событий по умолчанию](./media/user-guide/10.1.png)
**РИС. 28**

Для определяемых пользователем событий также доступны подробные сведения. На рис. 28 показаны подробные сведения о событии 452 с номером события 4096 и указаны четыре поля сведений.

![Снимок экрана: отображение определяемых пользователем событий по умолчанию](./media/user-guide/10.2.png)
**РИС. 29**

## <a name="defining-custom-user-defined-event-icons"></a>Определение настраиваемых значков для определяемых пользователем событий

TraceX предоставляет возможность создания настраиваемых значков для определяемых пользователем событий и настраиваемых меток полей сведений. Она реализуется путем добавления спецификаций значков событий в файл конфигурации ***tracex_custom.trxc** _. Этот файл находится в подкаталоге _ *_CustomEvents_** определяемого пользователем каталога установки TraceX. По умолчанию это каталог C:\Azure_RTOS\TraceX. На рис. 30 показан пример пути к каталогу.

![Снимок экрана: пример пути к каталогу](./media/user-guide/custom_events_folder.png)
**РИС. 30**

Файл конфигурации пользовательского события ***tracex_custom.trxc*** — это простой текстовый файл в формате ASCII, содержащий ноль определений пользовательских событий или более. Формат файла выглядит следующим образом.

```c
//Comments
**Start **
[custom event definition(s)] **End **
```

Каждая строка между метками Start и End используется для определения одного пользовательского события. В TraceX доступна версия шаблона этого файла без определенных пользовательских событий (между метками Start и End ничего не указано). Определение пользовательского события имеет следующий формат.

**number, name, abbreviation, top_color, bottom_color, label1, label2, label2, label4**

где:

- number определяет номер определяемого пользователем события в диапазоне от 4096 до 65535 включительно.</th>
- name определяет логическое имя для определяемого пользователем события.</td>
- abbreviation определяет двухбуквенное сокращенное обозначение определяемого пользователем события.</td>
- top_color определяет значение RGB для верхней половины значка, которое представляет собой трехзначное число в круглых скобках. Далее приведены некоторые стандартные определения RGB.
  - BLACK = (0,0,0)       
  - WHITE = (255,255,255)
  - RED = (255,0,0)     
  - GREEN = (0,255,0)     
  - BLUE = (0,0,255)     
  - YELLOW = (255,255,0)   
  - CYAN = (0,255,255)   
  - MAGENTA = (255,0,255)   
  Используя спецификацию RBG, пользователь может применять широкий спектр цветов при создании настраиваемых значков. Дополнительные сведения об определениях цветов RBG см. по адресу https://en.wikipedia.org/wiki/RGB#Digital_representations.
- botton_color определяет значение RGB для нижней половины значка, которое представляет собой трехзначное число в круглых скобках.
- label1 — метка для поля ***info_field_1** _, как указано в вызове службы _ *_tx_trace_user_event_insert_**.
- label2 — метка для поля ***info_field_2** _, как указано в вызове службы _ *_tx_trace_user_event_insert_**.
- label3 — метка для поля ***info_field_3** _, как указано в вызове службы _ *_tx_trace_user_event_insert_**.
- label4 — метка для поля ***info_field_4** _, как указано в вызове службы _ *_tx_trace_user_event_insert_**.

Примеры определений для каждого из двух определяемых пользователем событий, используемых в этой главе, показаны на рис. 10.4. Первое определение предназначено для события 4096 в строке 5 файла ***tracex_custom.trxc** _. Это определение задает пользовательскому событию 4096 имя _*First_User_Event**, присваивает двухбуквенное сокращение **FE**, выделяет верхнюю часть значка красным цветом, выделяет нижнюю часть значка зеленым цветом и задает полям сведений имена **First_Info1**, **First_Info2**, **First_Info3** и **First_Info4**. Определяемое пользователем событие 4098 определяется аналогичным образом в строке 6 файла **_tracex_custom.trxc_**.

![Снимок экрана: примеры определений для определяемых пользователем событий](./media/user-guide/10.4.png)
**РИС. 31**

Так как TraceX считывает файл ***tracex_custom.trxc** _ во время инициализации, для вступления в силу определений настраиваемых значков необходимо выйти из TraceX и затем перезапустить ее. На рис. 32 показано отображение определяемых пользователем событий 452 и 453 с настраиваемыми значками, определенными в файле _*_tracex_custom.trxc_**.

![Снимок экрана: TraceX с отображением определяемых пользователем событий с настраиваемыми значками](./media/user-guide/10.5.png)
**РИС. 32**

Дополнительные сведения в определении пользовательского события отображаются при двойном щелчке события, наведении на него курсора или нажатии кнопки текущего события. На рис. 33 показан выбор события 452 двойным щелчком мыши. Имя события и поля сведений соответствуют примеру определения, которое было добавлено в файл ***tracex_custom.trxc***.

![Снимок экрана: выбор события двойным щелчком мыши](./media/user-guide/10.6.png)
**РИС. 33**