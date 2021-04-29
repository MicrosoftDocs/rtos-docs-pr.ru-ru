---
title: Приложение D. Интерфейс API совместимого с BSD сокета в NetX Duo для ОСРВ Azure
description: Сведения об интерфейсе API совместимого с BSD сокета для IPv4 и IPv6.
author: philmea
ms.author: philmea
ms.date: 05/19/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 82439184d9facb6ddcc08ce81bc51182d7f34429
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104814875"
---
# <a name="appendix-d---azure-rtos-netx-duo-bsd-compatible-socket-api"></a><span data-ttu-id="41e70-103">Приложение D. Интерфейс API совместимого с BSD сокета в NetX Duo для ОСРВ Azure</span><span class="sxs-lookup"><span data-stu-id="41e70-103">Appendix D - Azure RTOS NetX Duo BSD-Compatible Socket API</span></span>

## <a name="bsd-compatible-socket-api"></a><span data-ttu-id="41e70-104">API совместимого с BSD сокета</span><span class="sxs-lookup"><span data-stu-id="41e70-104">BSD-Compatible Socket API</span></span> 
<span data-ttu-id="41e70-105">Интерфейс API совместимого с BSD сокета поддерживает подмножество вызовов API для сокетов BSD (с некоторыми ограничениями), автоматически используя примитивы NetX Duo&reg;.</span><span class="sxs-lookup"><span data-stu-id="41e70-105">The BSD-Compatible Socket API supports a subset of the BSD Sockets API calls (with some limitations) by utilizing NetX Duo&reg; primitives underneath.</span></span> <span data-ttu-id="41e70-106">Поддерживаются протоколы и сетевая адресация IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="41e70-106">Both IPv6 and IPv4 protocols and network addressing are supported.</span></span> <span data-ttu-id="41e70-107">Этот уровень API совместимых с BSD сокетов обычно работает с той же скоростью, что и стандартные реализации BSD, или немного большей, так как этот интерфейс API использует внутренние примитивы NetX Duo и обходит ненужную проверку ошибок NetX.</span><span class="sxs-lookup"><span data-stu-id="41e70-107">This BSD-Compatible Sockets API layer should perform as fast or slightly faster than typical BSD implementations because this API utilizes internal NetX Duo primitives and bypasses unnecessary NetX error checking.</span></span>  

<span data-ttu-id="41e70-108">Настраиваемые параметры позволяют ведущему приложению определять максимальное число сокетов, максимальный размер окна TCP и глубину очереди прослушивания.</span><span class="sxs-lookup"><span data-stu-id="41e70-108">Configurable options allow the host application to define the maximum number of sockets, TCP maximum window size, and depth of listen queue.</span></span>

<span data-ttu-id="41e70-109">Из-за ограничений производительности и архитектуры этот совместимый с BSD API сокетов поддерживает не все вызовы сокетов BSD.</span><span class="sxs-lookup"><span data-stu-id="41e70-109">Due to performance and architecture constraints, this BSD-Compatible Sockets API does not support all BSD Sockets calls.</span></span> <span data-ttu-id="41e70-110">Кроме того, для служб BSD доступны не все параметры BSD, в частности действуют следующие ограничения.</span><span class="sxs-lookup"><span data-stu-id="41e70-110">In addition, not all BSD options are available for the BSD services, specifically the following:</span></span>

  - <span data-ttu-id="41e70-111">Вызов \***select** _ работает только с fd_set \_readfds, и в нем не поддерживаются другие аргументы, например writefds или exceptfds.</span><span class="sxs-lookup"><span data-stu-id="41e70-111">\***select** _ call works with only fd_set \_readfds, other arguments in this call e.g., writefds, exceptfds are not supported.</span></span>
  - <span data-ttu-id="41e70-112">Аргумент int flags не поддерживается для вызовов ***send** _, _*_recv_*_, _*_sendto_\*_ и _ \*_recvfrom_\*\*.</span><span class="sxs-lookup"><span data-stu-id="41e70-112">The "int flags" argument is not supported for ***send** _, _*_recv_*_, _*_sendto,_*_ and _ *_recvfrom_** calls.</span></span> 
  - <span data-ttu-id="41e70-113">Совместимый с BSD API сокетов поддерживает только ограниченный набор вызовов для сокетов BSD.</span><span class="sxs-lookup"><span data-stu-id="41e70-113">The BSD-Compatible Socket API supports only limited set of BSD Sockets calls.</span></span>

<span data-ttu-id="41e70-114">Исходный код отличается простотой и размещен всего в двух файлах: ***nxd_bsd.c** _ и _*_nxd_bsd.h_\*_.</span><span class="sxs-lookup"><span data-stu-id="41e70-114">The source code is designed for simplicity and is comprised of only two files, ***nxd_bsd.c** _ and _*_nxd_bsd.h_\*_.</span></span> <span data-ttu-id="41e70-115">Для установки необходимо добавить эти два файла в проект сборки (но не в библиотеку NetX) и создать ведущее приложение, которое будет использовать вызовы службы сокетов BSD.</span><span class="sxs-lookup"><span data-stu-id="41e70-115">Installation requires adding these two files to the build project (not the NetX library) and creating the host application which will use BSD Socket service calls.</span></span> <span data-ttu-id="41e70-116">Файл _ *_nxd_bsd.h_*\* также нужно включить в исходный код приложения.</span><span class="sxs-lookup"><span data-stu-id="41e70-116">The _ *_nxd_bsd.h_*\* file must also be included in your application source.</span></span> <span data-ttu-id="41e70-117">Демонстрационные примеры файлов для приложений на основе IPv4 и IPv6 включены в свободно распространяемый дистрибутив NetX Duo.</span><span class="sxs-lookup"><span data-stu-id="41e70-117">Sample demo files for both IPv4 and IPv6  based applications are included with the distribution which is freely available with NetX Duo.</span></span> <span data-ttu-id="41e70-118">Дополнительные сведения см. в файлах справки и сведений, которые входят в пакет интерфейса API совместимого с BSD сокета.</span><span class="sxs-lookup"><span data-stu-id="41e70-118">Further details are available in the help and Readme files bundled with the BSDCompatible Socket API package.</span></span>

<span data-ttu-id="41e70-119">Совместимый с BSD API сокетов поддерживает следующие вызовы API.</span><span class="sxs-lookup"><span data-stu-id="41e70-119">The BSD-Compatible Sockets API supports the following BSD Sockets API calls:</span></span>

```c
INT     bsd_initialize (NX_IP *default_ip, NX_PACKET_POOL *default_pool, CHAR
        *bsd_memory_not_used);
```
```c
INT     getpeername( INT sockID, struct sockaddr *remoteAddress, INT
        *addressLength);
```
```c
INT     getsockname( INT sockID, struct sockaddr *localAddress, INT *addressLength);
```
```c
INT     recvfrom(INT sockID, CHAR *buffer, INT buffersize, INT flags,struct sockaddr
        *fromAddr, INT *fromAddrLen);
```
```c        
INT     recv(INT sockID, VOID *rcvBuffer, INT bufferLength, INT flags);
```
```c
INT     sendto(INT sockID, CHAR *msg, INT msgLength, INT flags, struct sockaddr
        *destAddr, INT destAddrLen);
```
```c        
INT     send(INT sockID, const CHAR *msg, INT msgLength, INT flags);
```
```c
INT     accept(INT sockID, struct sockaddr *ClientAddress, INT *addressLength);
```
```c
INT     listen(INT sockID, INT backlog);
```
```c
INT     bind (INT sockID, struct sockaddr *localAddress, INT addressLength);
```
```c
INT     connect(INT sockID, struct sockaddr *remoteAddress, INT addressLength);
```
```c
INT     socket( INT protocolFamily, INT type, INT protocol);
```
```c
INT     soc_close ( INT sockID);
```
```c
INT     select(INT nfds, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct
        timeval *timeout);
```
```c
VOID    FD_SET(INT fd, fd_set *fdset);
```
```c
VOID    FD_CLR(INT fd, fd_set *fdset);
```
```c
INT     FD_ISSET(INT fd, fd_set *fdset);
```
```c
VOID    FD_ZERO(fd_set *fdset);
```