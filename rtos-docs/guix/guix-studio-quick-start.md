---
title: Краткое руководство по началу работы с GUIX Studio для ОСРВ Azure
description: В этом руководстве приводятся общие сведения об использовании приложения GUIX Studio для ОСРВ Azure, которое представляет собой среду быстрой разработки пользовательского интерфейса на основе Microsoft Windows, специально разработанную для библиотеки среды выполнения GUIX Studio для ОСРВ Azure от корпорации Майкрософт.
author: philmea
ms.author: philmea
ms.date: 7/20/2020
ms.service: rtos
ms.topic: article
ms.openlocfilehash: 9ab4dfb2edd8990692ee3dc134207f43e4c757538dbc738f6f406bf40864bfb3
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2021
ms.locfileid: "116785430"
---
# <a name="azure-rtos-guix-studio-quick-start-guide"></a>Краткое руководство по началу работы с GUIX Studio для ОСРВ Azure

В этом учебнике приводится краткое описание использования GUIX Studio для ОСРВ Azure. GUIX Studio — это приложение для разработки пользовательского интерфейса на основе Windows, предназначенное для использования с библиотекой среды выполнения GUIX Studio в ОСРВ Azure от корпорации Майкрософт. 

Его целевой аудиторией являются разработчики встраиваемого ПО реального времени, использующие ОСРВ ThreadX и библиотеку времени выполнения для пользовательского интерфейса GUIX для ОСРВ Azure. Разработчик должен быть знаком со стандартными концепциями ThreadX для ОСРВ Azure и GUIX для ОСРВ Azure.

## <a name="summary"></a>Сводка

В состав GUIX Studio для ОСРВ Azure входит все необходимое для создания, сборки и запуска собственного графического интерфейса. Если вы только начинаете работу с GUIX Studio, воспользуйтесь ознакомительным пакетом, который позволяет создать и запустить собственный проект GUIX в качестве автономного классического приложения Windows в целях тестирования и оценки. Поскольку GUIX предназначен для использования практически в любом внедренном целевом объекте, поддерживающим графические выходные данные, работу, которую вы выполняете, и проекты, которые вы создаете на настольном компьютере, всегда можно скомпилировать и запустить на внедренном целевом объекте, не изменяя программное обеспечение приложения.
Установщик GUIX Studio размещает в системе разработки несколько компонентов. Они приведены ниже.

- Приложение GUIX Studio.
- Несколько примеров проектов GUIX.
- Все графические ресурсы и шрифты, используемые в примерах проектов.
- Файлы решений и файлы проектов для разработки на настольных ПК Windows с помощью IDE Microsoft Visual Studio.
- Предварительно созданные библиотеки GUIX и ThreadX для Win32, позволяющие создавать и запускать собственные приложения на компьютере.
- Файлы заголовков API GUIX и ThreadX.

## <a name="prerequisites"></a>Предварительные требования

Установщик GUIX Studio для ОСРВ Azure содержит несколько простых примеров проектов, поэтому, чтобы научиться использовать приложение GUIX Studio, рекомендуется начать с изменения, сборки и выполнения этих примеров. Для сборки и запуска примеров на ПК Windows потребуется компилятор Microsoft Visual Studio. Эти средства можно скачать из следующего расположения:

https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs#DownloadFamilies_4

Если у вас не установлены инструменты разработчика Майкрософт, вы можете запустить и использовать приложение GUIX Studio для создания собственного проекта интерфейса и изучения созданного исходного кода. Однако вы не сможете создать и запустить проект в виде автономного приложения.

## <a name="running-the-examples"></a>Выполнение примеров

После запуска установщика GUIX Studio вы увидите несколько примеров проектов Studio и файлов сборки, которые включены в установленное содержимое. Чтобы убедиться в том, что средства для настольного ПК установлены и работают правильно, рекомендуется начать с создания и запуска каждого из представленных примеров как есть. Мы будем называть каталог установки \<root>. В этом случае следует использовать обозреватель файлов и перейти в каталог \<root>/GUIX_Studio_6.x/examples. В этом каталоге вы увидите несколько простых примеров программ, таких как demo_guix_calculator, demo_guix_car_infotainment, demo_guix__home_automation, demo_guix_widget_types и другие.

## <a name="building-an-example"></a>Создание примера

Подкаталог с именем *build* можно найти в каждой папке с примером. Он содержит предварительно настроенные проекты для каждой поддерживаемой цепочки инструментов. Например, можно перейти к \<root> /GUIX_Studio_6.x/examples/thermometer/build/vs_2019 и найти предварительно настроенный файл решения и файл проекта Microsoft Visual Studio, готовые для загрузки и запуска в интегрированной среде разработки Visual Studio. Если вы хотите использовать другую цепочку инструментов, см. сведения в разделе [azure_rtos_support](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request#create-a-support-request).

Рекомендуется запустить интегрированную среду разработки Microsoft Visual C++ и открыть хотя бы один из этих примеров. Нажмите клавишу \<F7>, чтобы создать пример проекта, и нажмите клавишу \<F5>, чтобы запустить программу после ее успешного построения. Теперь вы должны увидеть пользовательский интерфейс GUIX, работающий в окне Microsoft Windows.

## <a name="designing-and-running-your-own-user-interface"></a>Проектирование и запуск собственного пользовательского интерфейса

Это краткое руководство не заменяет руководство пользователя GUIX Studio или руководство пользователя GUIX, но здесь содержатся достаточные сведения для начала работы. Однако для получения более подробной информации рекомендуется обратиться к руководству пользователя GUIX Studio.

Существует два способа создания и изменения собственного пользовательского интерфейса. Вы можете изучить руководство по программированию библиотеки GUIX и использовать API GUIX непосредственно в программном обеспечении приложения для полной реализации проекта. Чаще всего вы будете использовать приложение GUIX Studio для выполнения большей части работы по проектированию и созданию макета для элементов, отображаемых на экране, а затем завершите обработку событий и реализуете другую логику приложения, необходимую для реального функционирования пользовательского интерфейса.

Каждый из предоставленных примеров был создан с помощью приложения конструирования интерфейса GUIX Studio. После запуска установщика GUIX Studio на рабочем столе должен появиться значок для GUIX Studio 6.x.x.x.  Запустите GUIX Studio и откройте проект с именем "demo_guix_widget_types\guix_widget_types.gxp". Демонстрационный проект *widget_types* — это пример проекта, демонстрирующий несколько вариантов наиболее распространенных типов мини-приложений GUIX.

В открытом проекте щелкните "+", чтобы открыть узел дерева с именем "Primary" в представлении проекта в левом верхнем углу интегрированной среды разработки, и щелкните папку верхнего уровня с именем "Menu_Screen". Ваш проект должен отобразиться, как показано ниже:

![Снимок экрана: Studio с открытым проектом.](./media/guix-studio/qs_project_open.png)

## <a name="guix-studio-views"></a>Представления GUIX Studio

Интегрированная среда разработки GUIX Studio состоит из нескольких ***представлений***. Каждое представление предназначено для упрощения навигации по проекту и внесения изменений в проект.

### <a name="project-view"></a>представление проекта

Левое верхнее представление называется представлением проекта. В нем отображаются все физические дисплеи, включенные в проект (большинство проектов содержат только один дисплей), а также экраны и дочерние мини-приложения, предназначенные для выполнения на этом дисплее.

### <a name="properties-view"></a>Представление свойств

Под представлением проекта находится представление свойств. Как следует из его имени, в этом представлении можно изменять мини-приложения путем изменения различных свойств, связанных с ними.

### <a name="target-view"></a>Целевое представление

Центральная область отображения называется целевым представлением. Это представление является WYSIWYG-дисплеем пользовательского интерфейса. Так как библиотека GUIX отображается в целевом представлении, оно является точным представлением того, как будет выглядеть проект при выполнении во внедренном целевом объекте. Щелкайте другие мини-приложения в представлении проекта или в центральном целевом представлении, чтобы увидеть, как значения, отображаемые в представлении свойств, меняются в соответствии со свойствами выбираемого мини-приложения.

### <a name="resource-view"></a>Ресурсы

Наконец, в правой части находится представление ресурсов. В нем можно выбирать, добавлять, удалять и изменять цвета, шрифты, пиксельные карты и строки, включаемые в проект.

## <a name="modifying-the-example"></a>Изменение примера

GUIX Studio является интуитивно понятным приложением. Чтобы переместить одно из мини-приложений, показанных выше, просто щелкните его в целевом представлении и перетащите в новое место. Чтобы изменить цвета мини-приложения, щелкните нужное мини-приложение и измените цвета, отображаемые в представлении свойств. Чтобы изменить шрифт, используемый мини-приложением для отображения текста, просто щелкните нужный шрифт в представлении ресурсов и перетащите его в нужное целевое мини-приложение. Наводите указатель мыши на кнопки панели инструментов, чтобы увидеть подсказки для операций, выполняемых каждой кнопкой.

Экспериментируйте и вносите в пример небольшие изменения. Например, можно перетащить мини-приложение в новое место, изменить цвет фона окна или размер кнопки. Не рекомендуется удалять мини-приложения из примера, пока вы не научитесь более уверенно работать с GUIX, так как для удаления мини-приложений может потребоваться внести изменения в исходный код приложения.

## <a name="running-the-application-within-studio"></a>Запуск приложения в Studio

Используйте команду "Запустить приложение" в меню "Правка" (или кнопку "Запустить приложение" на панели кнопок), чтобы сразу же запустить приложение в новом окне. Этот способ не вызывает пользовательские функции рисования и другой код приложения, но позволяет быстро перемещаться по структуре пользовательского интерфейса и получать общее представление о виде и работе приложения, включая навигацию с одного экрана на следующий.

## <a name="generating-source-files"></a>Создание исходных фалов

После внесения изменений необходимо вызвать команды меню GUIX Studio для создания новых исходных файлов проекта. Затем можно перестроить пример программы, чтобы увидеть изменения в действии. Для создания исходных файлов используйте команды меню "Проект"|"Создать файлы ресурсов" и "Проект"|"Создать файлы спецификаций" (для выполнения этих команд можно также щелкнуть правой кнопкой мыши на экране в представлении проекта).

При создании этих исходных файлов должно отображаться сообщение, информирующее о том, что исходные файлы, связанные с проектом, были обновлены. Если вы не видите это сообщение, убедитесь, что у вас есть разрешения на запись в каталог, в котором находится проект. Теперь приложение GUIX Studio можно закрыть. Если вы внесли изменения в проект, GUIX Studio выведет запрос на их сохранение. Сохраните изменения. Вы будете использовать эти примеры и экспериментировать с ними в ходе обучения работе с GUIX Studio.

### <a name="building-and-running-the-application"></a>Сборка и запуск приложения

Теперь, когда приложение GUIX Studio создало выходные файлы проекта, можно их можно скомпилировать и связать для создания автономного исполняемого файла Win32. Кроме того, чтобы включить любые пользовательские функции рисования или обработки событий, определенные в приложении, необходимо скомпилировать и связать выходные файлы, созданные GUIX Studio, с собственным программным обеспечением приложения. В качестве примера мы будем использовать цепочку инструментов Microsoft Visual C++, но именно эта же процедура используется при сборке и запуске для предполагаемого целевого объекта.

- Запустите интегрированную среду разработки MSVC и откройте файл решения \<root>/GUIX_Studio_5.x/examples/demo_guix_widget_types/build/vs_2019/guix_widget_types.sln.

- Нажмите клавишу \<F7>, чтобы перестроить решение.
- Нажмите клавишу \<F5>, чтобы запустить программу.
 
Вы должны увидеть запущенную программу с изменениями, внесенными в Studio.

### <a name="learning-more"></a>Дополнительные сведения

**Руководство пользователя GUIX Studio** доступно [здесь](https://docs.microsoft.com/azure/rtos/guix/about-guix-studio). Руководство пользователя GUIX Studio — это более подробный документ по использованию GUIX Studio.

Кроме того, **руководство пользователя GUIX** можно найти [здесь](https://docs.microsoft.com/azure/rtos/guix/about-guix).  Оно содержит более подробные сведения дробные сведения о том, как работают внутренние механизмы при выполнении приложения GUIX. Чтобы полностью использовать возможности GUIX Studio и библиотеки времени выполнения GUIX, необходимо изучить оба руководства.

## <a name="customer-support-center"></a>Центр поддержки клиентов

Если у вас возникли вопросы или требуется помощь при выполнении приведенных здесь шагов, отправьте запрос в службу поддержки на портале Azure. Предоставьте нам следующие сведения в сообщении электронной почты, чтобы мы могли более эффективно решить ваш запрос, в службу поддержки:

- Подробное описание проблемы, включая частоту возникновения и возможность гарантированного воспроизведения.
- Вложите файл трассировки, содержащий проблему.
- Используемая версия ОСРВ Azure GUIX Studio (отображается в левом верхнем углу экрана).
- Укажите используемую версию GUIX для ОСРВ Azure, включая переменные **_gx_version_idstring** и **_gx_build_options**.
