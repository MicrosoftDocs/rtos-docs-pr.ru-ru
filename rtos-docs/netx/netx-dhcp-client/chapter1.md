---
title: Глава 1. Введение в клиент ОСРВ Azure NetX DHCP
description: В NetX IP-адрес приложения является одним параметров, указываемых в вызове функции nx_ip_create.
author: philmea
ms.author: philmea
ms.date: 06/04/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 44eb764c84a15a1bc96cf94bcbc8f81be7b41eef
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104815256"
---
# <a name="chapter-1---introduction-to-azure-rtos-netx-dhcp-client"></a><span data-ttu-id="56b9a-103">Глава 1. Введение в клиент ОСРВ Azure NetX DHCP</span><span class="sxs-lookup"><span data-stu-id="56b9a-103">Chapter 1 - Introduction to Azure RTOS NetX DHCP Client</span></span>

<span data-ttu-id="56b9a-104">В NetX IP-адрес приложения является одним параметров, указываемых в вызове функции *nx_ip_create*.</span><span class="sxs-lookup"><span data-stu-id="56b9a-104">In NetX, the application’s IP address is one of the supplied parameters to the *nx_ip_create* service call.</span></span> <span data-ttu-id="56b9a-105">Предоставление IP-адреса не вызывает проблем, если этот IP-адрес известен приложению, то есть назначен статически или посредством конфигурации пользователя.</span><span class="sxs-lookup"><span data-stu-id="56b9a-105">Supplying the IP address poses no problem if the IP address is known to the application, either statically or through user configuration.</span></span> <span data-ttu-id="56b9a-106">Однако существуют ситуации, в которых приложение не знает своего IP-адреса или он не важен.</span><span class="sxs-lookup"><span data-stu-id="56b9a-106">However, there are some instances where the application doesn’t know or care what its IP address is.</span></span> <span data-ttu-id="56b9a-107">В таких ситуациях необходимо передать нулевой IP-адрес в функцию *nx_ip_create* и использовать протокол клиента ОСРВ Azure DHCP для динамического получения IP-адреса.</span><span class="sxs-lookup"><span data-stu-id="56b9a-107">In such situations, a zero IP address should be supplied to the *nx_ip_create* function and the Azure RTOS DHCP Client protocol should be used to dynamically obtain an IP address.</span></span>

## <a name="dynamic-ip-address-assignment"></a><span data-ttu-id="56b9a-108">Динамическое назначение IP-адресов</span><span class="sxs-lookup"><span data-stu-id="56b9a-108">Dynamic IP Address Assignment</span></span>

<span data-ttu-id="56b9a-109">Основной службой, используемой для получения динамического IP-адреса из сети, является протокол RARP.</span><span class="sxs-lookup"><span data-stu-id="56b9a-109">The basic service used to obtain a dynamic IP address from the network is the Reverse Address Resolution Protocol (RARP).</span></span> <span data-ttu-id="56b9a-110">Этот протокол аналогичен протоколу ARP, за исключением того, что он предназначен для получения IP-адреса для себя, а не для поиска MAC-адреса для другого сетевого узла.</span><span class="sxs-lookup"><span data-stu-id="56b9a-110">This protocol is similar to ARP, except it is designed to obtain an IP address for itself instead of finding the MAC address for another network node.</span></span> <span data-ttu-id="56b9a-111">Сообщение низкого уровня RARP транслируется по локальной сети, и сервер в этой сети обязан отправить ответ RARP, который содержит динамически выделенный IP-адрес.</span><span class="sxs-lookup"><span data-stu-id="56b9a-111">The low-level RARP message is broadcast on the local network and it is the responsibility of a server on the network to respond with an RARP response, which contains a dynamically allocated IP address.</span></span>

<span data-ttu-id="56b9a-112">Хотя протокол RARP предоставляет службу для динамического выделения IP-адресов, у него есть несколько недостатков.</span><span class="sxs-lookup"><span data-stu-id="56b9a-112">Although RARP provides a service for dynamic allocation of IP addresses, it has several shortcomings.</span></span> <span data-ttu-id="56b9a-113">Самый очевидный недостаток заключается в том, что протокол RARP обеспечивает динамическое выделение только IP-адреса.</span><span class="sxs-lookup"><span data-stu-id="56b9a-113">The most glaring deficiency is that RARP only provides dynamic allocation of the IP address.</span></span> <span data-ttu-id="56b9a-114">В большинстве случаев для того, чтобы устройство правильно работало в сети, требуется дополнительная информация.</span><span class="sxs-lookup"><span data-stu-id="56b9a-114">In most situations, more information is necessary in order for a device to properly participate on a network.</span></span> <span data-ttu-id="56b9a-115">В дополнение к IP-адресу большинству устройств требуется маска сети и IP-адрес шлюза.</span><span class="sxs-lookup"><span data-stu-id="56b9a-115">In addition to an IP address, most devices need the network mask and the gateway IP address.</span></span> <span data-ttu-id="56b9a-116">Может также потребоваться IP-адрес DNS-сервера и другие сведения о сети.</span><span class="sxs-lookup"><span data-stu-id="56b9a-116">The IP address of a DNS server and other network information may also be needed.</span></span> <span data-ttu-id="56b9a-117">Протокол RARP не может предоставить эти сведения.</span><span class="sxs-lookup"><span data-stu-id="56b9a-117">RARP does not have the ability to provide this information.</span></span>

## <a name="rarp-alternatives"></a><span data-ttu-id="56b9a-118">Альтернативы протоколу RARP</span><span class="sxs-lookup"><span data-stu-id="56b9a-118">RARP Alternatives</span></span>

<span data-ttu-id="56b9a-119">Чтобы преодолеть недостатки протокола RARP, исследователи разработали более полный механизм выделения IP-адресов, называемый протоколом начальной загрузки (BOOTP).</span><span class="sxs-lookup"><span data-stu-id="56b9a-119">In order to overcome the deficiencies of RARP, researchers developed a more comprehensive IP address allocation mechanism called the Bootstrap Protocol (BOOTP).</span></span> <span data-ttu-id="56b9a-120">Этот протокол может динамически выделять IP-адреса, а также предоставлять важные дополнительные сведения о сети.</span><span class="sxs-lookup"><span data-stu-id="56b9a-120">This protocol has the ability to dynamically allocate an IP address and also provide additional important network information.</span></span> <span data-ttu-id="56b9a-121">Тем не менее, недостаток протокола BOOTP заключается в том, что он разработан для статических сетевых конфигураций.</span><span class="sxs-lookup"><span data-stu-id="56b9a-121">However, BOOTP has the drawback of being designed for static network configurations.</span></span> <span data-ttu-id="56b9a-122">Он не позволяет быстро или автоматически назначать адреса.</span><span class="sxs-lookup"><span data-stu-id="56b9a-122">It does not allow for quick or automated address assignment.</span></span>

<span data-ttu-id="56b9a-123">Именно здесь может оказаться очень полезным протокол DHCP.</span><span class="sxs-lookup"><span data-stu-id="56b9a-123">This is where the Dynamic Host Configuration Protocol (DHCP) is extremely useful.</span></span> <span data-ttu-id="56b9a-124">Протокол DHCP предназначен расширить основные функции BOOTP, включая полностью автоматизированное выделение IP-серверов и полностью динамическое выделение IP-адресов путем "аренды" IP-адреса клиенту на указанный период времени.</span><span class="sxs-lookup"><span data-stu-id="56b9a-124">DHCP is designed to extend the basic functionality of BOOTP to include completely automated IP server allocation and completely dynamic IP address allocation through “leasing” an IP address to a client for a specified period of time.</span></span> <span data-ttu-id="56b9a-125">Протокол DHCP можно также настроить для статического выделения IP-адресов, как в протоколе BOOTP.</span><span class="sxs-lookup"><span data-stu-id="56b9a-125">DHCP can also be configured to allocate IP addresses in a static manner like BOOTP.</span></span>

## <a name="dhcp-messages"></a><span data-ttu-id="56b9a-126">Сообщения DHCP</span><span class="sxs-lookup"><span data-stu-id="56b9a-126">DHCP Messages</span></span>

<span data-ttu-id="56b9a-127">Хотя протокол DHCP значительно расширяет функции протокола BOOTP, в DHCP используется тот же формат сообщений, что и в BOOTP, и поддерживаются те же варианты поставщиков, что и в BOOTP.</span><span class="sxs-lookup"><span data-stu-id="56b9a-127">Although DHCP greatly enhances the functionality of BOOTP, DHCP uses the same message format as BOOTP and supports the same vendor options as BOOTP.</span></span> <span data-ttu-id="56b9a-128">Для выполнения функций в протокол DHCP добавлены семь новых параметров, относящихся к DHCP:</span><span class="sxs-lookup"><span data-stu-id="56b9a-128">In order to perform its function, DHCP introduces seven new DHCP-specific options, as follows:</span></span>

- <span data-ttu-id="56b9a-129">DISCOVER (1) (отправляется DHCP-клиентом);</span><span class="sxs-lookup"><span data-stu-id="56b9a-129">DISCOVER (1) (sent by DHCP Client)</span></span>

- <span data-ttu-id="56b9a-130">OFFER (2) (отправляется DHCP-сервером);</span><span class="sxs-lookup"><span data-stu-id="56b9a-130">OFFER (2) (sent by DHCP Server)</span></span>

- <span data-ttu-id="56b9a-131">DISCOVER (3) (отправляется DHCP-клиентом);</span><span class="sxs-lookup"><span data-stu-id="56b9a-131">REQUEST (3) (sent by DHCP Client)</span></span>

- <span data-ttu-id="56b9a-132">DECLINE (4) (отправляется DHCP-клиентом);</span><span class="sxs-lookup"><span data-stu-id="56b9a-132">DECLINE (4) (sent by DHCP Client)</span></span>

- <span data-ttu-id="56b9a-133">ACK (5) (отправляется DHCP-сервером);</span><span class="sxs-lookup"><span data-stu-id="56b9a-133">ACK (5) (sent by DHCP Server)</span></span>

- <span data-ttu-id="56b9a-134">NACK (6) (отправляется DHCP-сервером);</span><span class="sxs-lookup"><span data-stu-id="56b9a-134">NACK (6) (sent by DHCP Server)</span></span>

- <span data-ttu-id="56b9a-135">RELEASE (7) (отправляется DHCP-клиентом);</span><span class="sxs-lookup"><span data-stu-id="56b9a-135">RELEASE (7) (sent by DHCP Client)</span></span>

- <span data-ttu-id="56b9a-136">INFORM (8) (отправляется DHCP-клиентом);</span><span class="sxs-lookup"><span data-stu-id="56b9a-136">INFORM (8) (sent by DHCP Client)</span></span>

- <span data-ttu-id="56b9a-137">FORCERENEW (9) (отправляется DHCP-клиентом).</span><span class="sxs-lookup"><span data-stu-id="56b9a-137">FORCERENEW (9) (sent by DHCP Client)</span></span>

## <a name="dhcp-communication"></a><span data-ttu-id="56b9a-138">Взаимодействие по протоколу DHCP</span><span class="sxs-lookup"><span data-stu-id="56b9a-138">DHCP Communication</span></span>

<span data-ttu-id="56b9a-139">Протокол DHCP использует протокол UDP для отправки запросов и ответных значений полей.</span><span class="sxs-lookup"><span data-stu-id="56b9a-139">DHCP utilizes the UDP protocol to send requests and field responses.</span></span> <span data-ttu-id="56b9a-140">Перед получением IP-адреса выполняются отправка и получение сообщений UDP, в которых хранятся данные DHCP, с помощью широковещательного IP-адреса 255.255.255.255.</span><span class="sxs-lookup"><span data-stu-id="56b9a-140">Prior to having an IP address, UDP messages carrying the DHCP information are sent and received by utilizing the IP broadcast address of 255.255.255.255.</span></span>

## <a name="dhcp-client-state-machine"></a><span data-ttu-id="56b9a-141">Конечный автомат DHCP-клиента</span><span class="sxs-lookup"><span data-stu-id="56b9a-141">DHCP Client State Machine</span></span>

<span data-ttu-id="56b9a-142">DHCP-клиент реализуется как конечный автомат.</span><span class="sxs-lookup"><span data-stu-id="56b9a-142">The DHCP Client is implemented as a state machine.</span></span> <span data-ttu-id="56b9a-143">Конечный автомат обрабатывается внутренним потоком DHCP, который создается во время обработки вызова *nx_dhcp_create*.</span><span class="sxs-lookup"><span data-stu-id="56b9a-143">The state machine is processed by an internal DHCP thread that is created during *nx_dhcp_create* processing.</span></span> <span data-ttu-id="56b9a-144">Ниже приведены основные состояния DHCP-клиента.</span><span class="sxs-lookup"><span data-stu-id="56b9a-144">The main states of DHCP Client are as follows:</span></span>


- <span data-ttu-id="56b9a-145">**NX_DHCP_STATE_BOOT**: запуск с предыдущим IP-адресом.</span><span class="sxs-lookup"><span data-stu-id="56b9a-145">**NX_DHCP_STATE_BOOT** Starting with a previous IP address</span></span>

- <span data-ttu-id="56b9a-146">**NX_DHCP_STATE_INIT**: запуск без предыдущего IP-адреса.</span><span class="sxs-lookup"><span data-stu-id="56b9a-146">**NX_DHCP_STATE_INIT** Starting with no previous   IP address value</span></span>

- <span data-ttu-id="56b9a-147">**NX_DHCP_STATE_SELECTING**: ожидание ответа от любого DHCP-сервера.</span><span class="sxs-lookup"><span data-stu-id="56b9a-147">**NX_DHCP_STATE_SELECTING** Waiting for a response from any DHCP server</span></span>

- <span data-ttu-id="56b9a-148">**NX_DHCP_STATE_REQUESTING**: DHCP-сервер определен, отправлен запрос IP-адреса.</span><span class="sxs-lookup"><span data-stu-id="56b9a-148">**NX_DHCP_STATE_REQUESTING** DHCP Server identified, IP address request sent</span></span>

- <span data-ttu-id="56b9a-149">**NX_DHCP_STATE_BOUND**: установлена аренда IP-адреса DHCP.</span><span class="sxs-lookup"><span data-stu-id="56b9a-149">**NX_DHCP_STATE_BOUND** DHCP IP Address lease established</span></span>

- <span data-ttu-id="56b9a-150">**NX_DHCP_STATE_RENEWING**: пришло время продления аренды IP-адреса DHCP, запрошено продление.</span><span class="sxs-lookup"><span data-stu-id="56b9a-150">**NX_DHCP_STATE_RENEWING** DHCP IP Address lease renewal time elapsed, renewal requested</span></span>

- <span data-ttu-id="56b9a-151">**NX_DHCP_STATE_REBINDING**: пришло время повторной привязки для аренды IP-адреса DHCP, запрошено продление.</span><span class="sxs-lookup"><span data-stu-id="56b9a-151">**NX_DHCP_STATE_REBINDING** DHCP IP Address lease rebind time elapsed, renewal requested</span></span>

- <span data-ttu-id="56b9a-152">**NX_DHCP_STATE_FORCERENEW**: установлена аренда IP-адреса DHCP и выполнено принудительное продление сервером (в настоящее время не поддерживается) или приложением (посредством вызова nx_dhcp_force_renew).</span><span class="sxs-lookup"><span data-stu-id="56b9a-152">**NX_DHCP_STATE_FORCERENEW** DHCP IP Address lease established, force renewal by server (currently not supported) or by the application calling nx_dhcp_force_renew</span></span>

- <span data-ttu-id="56b9a-153">**NX_DHCP_STATE_ADDRESS_PROBING**: на IP-адрес DHCP отправлена проба ARP для обнаружения конфликта IP-адресов.</span><span class="sxs-lookup"><span data-stu-id="56b9a-153">**NX_DHCP_STATE_ADDRESS_PROBING** DHCP IP Address probing, send the ARP probe to detect IP address conflict.</span></span>

## <a name="dhcp-client-multiple-interface-support"></a><span data-ttu-id="56b9a-154">Поддержка нескольких интерфейсов для DHCP-клиента</span><span class="sxs-lookup"><span data-stu-id="56b9a-154">DHCP Client Multiple Interface Support</span></span>

<span data-ttu-id="56b9a-155">Ранее DHCP-клиент был реализован для запуска только на одном сетевом интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="56b9a-155">The DHCP Client was previously implemented to run on only a single network interface.</span></span> <span data-ttu-id="56b9a-156">По умолчанию DHCP-клиент запускался (и сейчас запускается) на основном интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="56b9a-156">The default behavior was (and still is) for the DHCP Client to run on the primary interface.</span></span> <span data-ttu-id="56b9a-157">Вызвав службу *nx_dhcp_set_interface_index*, приложение могло (и по-прежнему может) запустить DHCP на дополнительном сетевом интерфейсе, а не на основном интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="56b9a-157">By calling *nx_dhcp_set_interface_index*, the application could (and still can) run DHCP on a secondary network interface instead of the primary interface.</span></span>

<span data-ttu-id="56b9a-158">Теперь поддерживается параллельное выполнение протокола DHCP на нескольких интерфейсах.</span><span class="sxs-lookup"><span data-stu-id="56b9a-158">It now supports DHCP running on multiple interfaces in parallel.</span></span> <span data-ttu-id="56b9a-159">Дополнительные сведения о том, как запустить DHCP-клиент на нескольких физических интерфейсах одновременно, см. в разделе **Одновременный запуск DHCP-клиента на нескольких интерфейсах** главы 2.</span><span class="sxs-lookup"><span data-stu-id="56b9a-159">See **DHCP Client on Multiple Interfaces Simultaneously** in Chapter Two for specific details how to run DHCP Client on more than one physical interface simultaneously.</span></span>

## <a name="dhcp-user-request"></a><span data-ttu-id="56b9a-160">Запрос пользователя DHCP</span><span class="sxs-lookup"><span data-stu-id="56b9a-160">DHCP User Request</span></span>

<span data-ttu-id="56b9a-161">После того как DHCP-сервер предоставит адрес, для обработки DHCP-клиента могут запрашиваться дополнительные параметры (по одному) с помощью службы *nx_dhcp_user_option_request*.</span><span class="sxs-lookup"><span data-stu-id="56b9a-161">Once the DHCP server grants an IP address, the DHCP client processing can request additional parameters — one at a time — by using the *nx_dhcp_user_option_request* service.</span></span>

## <a name="dhcp-client-socket-queue"></a><span data-ttu-id="56b9a-162">Очередь сокета DHCP-клиента</span><span class="sxs-lookup"><span data-stu-id="56b9a-162">DHCP Client Socket Queue</span></span> 

<span data-ttu-id="56b9a-163">Ожидая ответа сервера, DHCP-клиент автоматически удаляет широковещательные пакеты от DHCP-серверов, предназначенные для других DHCP-клиентов, из очереди получения сокета.</span><span class="sxs-lookup"><span data-stu-id="56b9a-163">The DHCP Client automatically clears broadcast packets from DHCP Servers intended for other DHCP Clients from its socket receive queue while waiting for Server to respond to itself.</span></span> <span data-ttu-id="56b9a-164">Если этого не делать, то в нагруженной сети это может привести к удалению пакетов, предназначенных для клиента.</span><span class="sxs-lookup"><span data-stu-id="56b9a-164">In a busy network, not doing so could cause packets intended for the Client to be dropped.</span></span>

## <a name="dhcp-rfcs"></a><span data-ttu-id="56b9a-165">Документы RFC, которым соответствует протокол DHCP</span><span class="sxs-lookup"><span data-stu-id="56b9a-165">DHCP RFCs</span></span>

<span data-ttu-id="56b9a-166">Протокол NetX DHCP соответствует RFC 2132, RFC 2131 и связанным документам RFC.</span><span class="sxs-lookup"><span data-stu-id="56b9a-166">NetX DHCP is compliant with RFC2132, RFC2131, and related RFCs.</span></span>

