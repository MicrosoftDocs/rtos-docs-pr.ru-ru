---
title: Глава 1. Введение в ThreadX (ОСРВ Azure)
description: В этой главе содержатся общие сведения о продукте ThreadX (ОСРВ Azure), способах его применения и преимуществах.
author: philmea
ms.author: philmea
ms.date: 05/19/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 536b2d59bf9f2cf15d320b91277f0efc7bf96097329f690b0849b2145c5a3abc
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2021
ms.locfileid: "116802071"
---
# <a name="chapter-1---introduction-to-azure-rtos-threadx"></a>Глава 1. Введение в ThreadX (ОСРВ Azure)

ThreadX (ОСРВ Azure) — это высокопроизводительное ядро реального времени, разработанное специально для встраиваемых приложений. В этой главе содержатся общие сведения о продукте, способах его применения и преимуществах.

## <a name="threadx-unique-features"></a>Уникальные возможности ThreadX

В отличие от ядер реального времени, предполагается, что ThreadX SMP обладает гибкостью благодаря возможности масштабирования от небольших микроконтроллерных приложений до систем на мощных процессорах CISC, RISC и DSP.

ThreadX масштабируется с учетом базовой архитектуры. Так как службы ThreadX реализованы как библиотека C, в образ времени выполнения переносятся только те службы, которые фактически используются приложением. Благодаря этому размер ThreadX полностью определяется приложением. Для большинства приложений размер образа с инструкциями ThreadX будет в диапазоне от 2 КБ до 15 КБ.

### <a name="picokerneltrade-architecture"></a>*Архитектура picokernel&trade;*

Вместо того, чтобы накладывать функции ядра слоями друг на друга, как в традиционных архитектурах *микроядер*, службы ThreadX подключаются непосредственно в ядро. Это приводит к максимально быстрому переключению контекста и максимально высокой производительности вызова к службе. Такую структуру без использования слоев мы назвали архитектурой *picokernel*.

### <a name="ansi-c-source-code"></a>Исходный код ANSI C

Решение ThreadX написано преимущественно на ANSI C. Для обеспечения соответствия между ядром и базовым целевым процессоров потребовался небольшой объем кода на языке ассемблера. Такая конструкция позволяет очень быстро перенести ThreadX на новое семейство процессоров — обычно за несколько недель.

### <a name="advanced-technology"></a>Передовая технология

Ниже перечислены основные особенности передовой технологии ThreadX:
- простая архитектура *picokernel*;
- автоматическое масштабирование (небольшой объем занимаемой памяти);
- детерминированная обработка;
- высокая производительность в реальном времени;
- планирование по приоритетам и координированное планирование;
- поддержка гибких приоритетов потоков;
- динамическое создание системных объектов;
- неограниченное количество системных объектов;
- оптимизированная обработка прерываний;
- планирование preemption-threshold™;
- наследование приоритетов;
- возможности event-chaining™;
- быстрые программные таймеры;
- управление памятью во время выполнения;
- мониторинг производительности во время выполнения;
- анализ стека во время выполнения;
- встроенная трассировка системы;
- обширная поддержка процессоров;
- обширная поддержка средств разработки;
- полная независимость от конвенций конца строки.

### <a name="not-a-black-box"></a>Отсутствие проблемы "черного ящика"

Большинство дистрибутивов ThreadX включают полный исходный код на C и ассемблере для определенных процессоров. Это устраняет проблему "черного ящика", которая характерна для многих коммерческих ядер. При работе с ThreadX разработчики приложений точно знают, что делает ядро. Нет никаких тайн!

Наличие исходного кода также позволяет изменять ядро с учетом конкретного приложения. Изменять ядро не рекомендуется, но если без этого нельзя обойтись — это вполне возможно.

Эти возможности особенно удобны для тех разработчиков, которые ранее работали с *собственными ядрами.* Они привыкли к тому, что исходный код известен и ядро можно изменить. ThreadX можно считать идеальным ядром для таких разработчиков.

### <a name="the-rtos-standard"></a>Стандарт ОСРВ

Благодаря универсальности, высокопроизводительной архитектуре *picokernel*, современным технологиям и демонстрируемой переносимости, решение ThreadX сегодня развернуто более чем на 2 миллиардах устройств. Это по сути делает ThreadX стандартом ОСРВ для глубоко встраиваемых приложений.

## <a name="safety-certifications"></a>Сертификация по безопасности

### <a name="tv-certification"></a>Сертификация TÜV

Решение ThreadX сертифицировано организацией SGS-TÜV Saar для использования в критических для безопасности систем в соответствии со стандартами IEC61508 и IEC-62304. Такая сертификация гарантирует, что ThreadX можно использовать при разработке программного обеспечения с особыми требованиями к высочайшим уровням защиты целостности в соответствии со стандартами IEC-61508 и IEC-62304 для "функциональной безопасности электрических, электронных и программируемых электронных систем, связанных с безопасностью". Компания SGS-TÜV Saar, основанная совместно немецкими компаниями SGS-Group и TÜV Saarland, стала ведущей аккредитованной и независимой компанией, выполняющей тестирование, аудит, проверку и сертификацию встраиваемого ПО для связанных с безопасностью систем. Соответствие промышленному стандарту безопасности IEC 61508 и всем его производным стандартам, в том числе IEC-62304, гарантирует функциональную безопасность электрических, электронных и программируемых электронных медицинских изделий, систем управления процессами, промышленного оборудования и систем управления железнодорожными перевозками с высоким уровнем защиты.

Компания SGS-TÜV Saar сертифицировала ThreadX для использования в критических для безопасности системах автомобилестроения в соответствии со стандартом ISO 26262. Более того, решение ThreadX сертифицировано согласно классификации Automotive Safety Integrity Level D (ASIL), которая соответствует высочайшему уровню сертификации ISO 26262.

Кроме того, компания SGS-TÜV Saar сертифицировала ThreadX для использования в критических для безопасности приложениях для железнодорожного сообщения в соответствии со стандартом EN 50128 вплоть до уровня SW-SIL 4.

![Сертификация TÜV](./media/overview-threadx/partener-logo-sgs-tuv-saar-2.png)

* IEC 61508 до уровня SIL 4

* IEC 62304 до уровня SW класса безопасности C

* ISO 26262 ASIL D

* EN 50128 SW-SIL 4

> [!NOTE]
> *Связавшись с нами, вы можете получить дополнительные сведения о версиях ThreadX, которые проходили сертификацию TÜV, а также копии доступных отчетов о тестах, сертификатов и других документов.*

### <a name="misra-c-compliant"></a>Соответствие MISRA C

MISRA C — это набор руководств по программированию для критически важных систем, использующих язык программирования C. Первоначальные рекомендации MISRA C главным образом были нацелены на приложения для автомобилестроения, но теперь MISRA C широко признается решением, применимым к любому критическому для безопасности приложению. ThreadX соответствует всем правилам уровней required (требуется) и mandatory (обязательно) стандартов MISRA-C:2004 и MISRA C:2012. Также ThreadX соответствует всем правилам уровня advisory (рекомендовано), кроме трех. Дополнительные сведения см. в документе ***ThreadX_MISRA_Compliance.pdf***.

### <a name="ul-certification"></a>Сертификация UL

Решение ThreadX сертифицировано организацией UL как соответствующее стандартам безопасности UL 60730-1 (приложение H), CSA E60730-1 (приложение H), IEC 60730-1 (приложение H), UL 60335-1 (приложение R), IEC 60335-1 (приложение R) и UL 1998 для программного обеспечения в программируемых компонентах. В дополнение к стандарту IEC/UL 60730-1, который в приложении H содержит требования к управлению с использованием программного обеспечения, стандарт IEC 60335-1 описывает в приложении R требования к программируемым электронным цепям. В IEC 60730 (приложение H) и IEC 60335-1 (приложение R) обсуждается безопасность программного и аппаратного обеспечения микроконтроллеров, используемых в разных устройствах, таких как стиральные, посудомоечные и сушильные машины, а также холодильные, морозильные и духовые шкафы.

![Сертификация UL](./media/overview-threadx/partener-logo-c-ru-us-2.png)

*UL/IEC 60730, UL/IEC 60335, UL 1998*

> [!NOTE]
> *Корпорация Майкрософт готова предоставить дополнительные сведения о версиях ThreadX, которые проходили сертификацию TÜV, а также копии доступных отчетов о тестах, сертификатов и других документов.*

### <a name="certification-pack"></a>Certification Pack

Certification Pack&trade; для ThreadX — это на 100 % завершенный и готовый к работе отраслевой автономный пакет, который предоставляет необходимое свидетельство соответствия ThreadX требованиям сертификации или подтверждения уровней надежности и критичности продуктов на основе ThreadX в авиационных, медицинских и промышленных системах. Поддерживаются следующие сертификаты: DO-178B, ED-12B, DO-278, FDA510(k), IEC62304, IEC-60601, ISO-14971, UL-1998, IEC-61508, CENELEC EN50128, BS50128 и 49CFR236. Обратитесь в корпорацию Майкрософт за дополнительными сведениями о пакете Certification Pack.

## <a name="embedded-applications"></a>Встраиваемые приложения

Встраиваемые приложения выполняются на микропроцессорах в таких продуктах, как устройства беспроводной связи, автомобильные двигатели, лазерные принтеры, медицинские устройства и многое другое. Важным отличием встраиваемых приложений является то, что программное и аппаратное обеспечение для них имеет конкретное назначение.

### <a name="real-time-software"></a>Программное обеспечение реального времени

Если к программному обеспечению приложения применяются ограничения по времени, оно называется программным обеспечением *реального времени*. Встроенные приложения практически всегда работают в реальном времени из-за особенностей их внутреннего взаимодействия с внешними событиями.

### <a name="multitasking"></a>Многозадачность

Как уже упоминалось, встраиваемые приложения имеют специальное назначение. Чтобы соответствовать ему, ПО должно выполнять различные *задачи*. Задачей называют частично независимую часть приложения, которая имеет конкретную цель. Кроме того, некоторые задачи являются более важными, чем другие. Одной из основных трудностей в реализации встраиваемого приложения является распределение ресурсов процессора между несколькими задачами приложения. Именно распределение возможностей обработки между конкурирующими задачами является основной целью ThreadX.

### <a name="tasks-vs-threads"></a>Задачи и потоки

Важной особенностью термина *задача* заключается в том, что он используется разными способами. Иногда задачей называют отдельно загружаемую программу. В других случаях подразумевается внутренний сегмент программы. Поэтому в современной теории операционных систем используется два термина, которые в определенной степени могут заменить термин "задача": *процесс* и *поток*. *Процесс* — это полностью независимая программа с собственным адресным пространством, тогда как *поток* — это частично независимый сегмент программы, который выполняется внутри процесса. Несколько потоков используют одно и то же адресное пространство процесса. Управление потоками требует минимальных дополнительных затрат.

Большинство встраиваемых приложений не имеет достаточно ресурсов памяти и производительности, чтобы поддерживать полноценную операционную систему, ориентированную на процессы. Кроме того, у небольших микропроцессоров отсутствует необходимая аппаратная архитектура для настоящей операционной системы, ориентированной на процессы. По этим причинам в ThreadX реализована модель потоков, которая очень эффективна и практична при использовании большинством встраиваемых приложений реального времени.

Чтобы избежать путаницы, в ThreadX не используется термин *задача*. Вместо него применяется более описательный и современный термин *поток*.

## <a name="threadx-benefits"></a>Преимущества ThreadX

Использование ThreadX предоставляет множество преимуществ для встраиваемых приложений. Разумеется, основным из них является механизм выделения времени обработки для потоков внедренных приложений.

### <a name="improved-responsiveness"></a>Повышенная скорость реагирования

До создания ядер реального времени, таких как ThreadX, большинство внедренных приложений выделяли время обработки в простом цикле управления, обычно в функции *main* программы на языке C. Этот подход по-прежнему используется для самых маленьких и простых приложений. Но по мере повышения сложности и размера он становится непрактичным, так как время реагирования на любое событие напрямую определяется самым долгим из возможных процессов, выполняемых в одном цикле управления. 

Ситуация ухудшается тем, что характеристики времени для приложения изменяются при любом внесении изменений в цикл управления. Это делает такое приложение нестабильным, усложняет его обслуживание и улучшение.

ThreadX обеспечивает быстрое и детерминированное время отклика на важные внешние события. Для этого в ThreadX применяется алгоритм планирования на основе приоритетов с вытеснением, который позволяет потоку с более высоким приоритетом вытеснять выполнение потока с более низким приоритетом. В результате время ответа в худшем случае приближается ко времени, требуемому для переключения контекста. Это выполняется детерминированно и невероятно быстро.

### <a name="software-maintenance"></a>Обслуживание программного обеспечения

Ядро ThreadX позволяет разработчикам приложений сосредоточиться на обеспечении определенных требований к потокам приложений и не беспокоиться об ухудшении времени работы других частей приложения. Кроме того, эта возможность значительно упрощает восстановление или усовершенствование приложения, использующего ThreadX.

### <a name="increased-throughput"></a>Повышение пропускной способности

Для устранения проблемы с временем ответа, характерной для цикла управления, можно применить дополнительные опросы. Это повышает скорость реагирования в приложении, но не гарантирует стабильное время отклика для самого худшего случая и никак не оптимизирует возможность последующего изменения приложения. Кроме того, в этом случае процессор выполняет еще больше ненужных вычислений, связанных с дополнительными опросами. Такая ненужная обработка снижает общую пропускную способность системы.

Интересно отметить, что многие разработчики предполагают, что ThreadX и другие многопоточные среды повышают общие издержки и негативно влияют на общую пропускную способность системы. Но в некоторых случаях многопоточность даже сокращает издержки, устраняя все избыточные опросы в средах цикла управления. Издержки, связанные с использованием многопоточных ядер, обычно зависят только от времени, необходимого для переключения контекста. Если время для переключения контекста меньше, чем для процесса опроса, ThreadX позволяет снизить общие издержки и повысить пропускную способность. Благодаря таким возможностям ThreadX будет подходящим вариантом для приложений любой сложности или размера.

### <a name="processor-isolation"></a>Изолированность от процессора

ThreadX предоставляет надежный, независимый от процессора интерфейс между приложением и базовым процессором. Это позволяет разработчикам сосредоточиться на работе приложения, не тратя значительное время на изучение особенностей оборудования.

### <a name="dividing-the-application"></a>Разделение приложения

В приложениях с единым циклом управления каждый разработчик должен знать все особенности поведения и требования приложения во время выполнения. Это обусловлено тем, что логика выделения времени обработки распределена по всему приложению. По мере увеличения размера или сложности приложения разработчикам будет все сложнее запомнить точные требования к работе процессора, действующие в приложении.

ThreadX освобождает разработчиков от решения проблем, связанных с выделением времени обработки, и позволяет сосредоточиться на конкретной части встраиваемого приложения. Кроме того, ThreadX помогает разделить приложение на четко определенные потоки. Такое разделение приложения само по себе значительно упрощает разработку.

### <a name="ease-of-use"></a>Удобство использования

Решение ThreadX разрабатывалось в первую очередь для разработчиков приложений. Архитектура и интерфейс вызова служб в ThreadX специально сделаны простыми для изучения. Это означает, что в ThreadX разработчики могут с легкостью использовать все современные возможности.

### <a name="improve-time-to-market"></a>Сокращение времени выхода на рынок

Все преимущества ThreadX ускоряют процесс разработки программного обеспечения. ThreadX отвечает за решение большинства проблем, связанных с процессором, и обеспечение соответствия самым распространенным сертификатам безопасности, позволяя исключить эти задачи из графика разработки. Все это приводит к ускорению выхода на рынок.

### <a name="protecting-the-software-investment"></a>Защита инвестиций в программное обеспечение

Благодаря продуманной архитектуре ThreadX легко переносится в среды с новыми процессорами и (или) средствами разработки. Наряду с тем, что ThreadX изолирует приложения от базовых процессоров, любые приложения ThreadX будут высокопереносимыми. Это гарантирует возможность миграции приложения и защиту инвестиций в исходную разработку.
