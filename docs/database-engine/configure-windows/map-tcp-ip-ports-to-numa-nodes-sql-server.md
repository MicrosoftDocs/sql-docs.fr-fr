---
title: Mapper les ports TCP/IP aux nœuds NUMA (SQL Server) | Microsoft Docs
description: Apprenez à utiliser le gestionnaire de configuration SQL Server pour mapper les ports TCP/IP aux nœuds d'accès à la mémoire non uniforme (NUMA). Voir Comment créer un bitmap d'identification des nœuds.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- NUMA
- memory [SQL Server], NUMA
- affinity NUMA
- ports [SQL Server], working with NUMA
- CPU [SQL Server], NUMA support
- NUMA, How to map a port to a NUMA node
- NUMA affinity
- TCP/IP [SQL Server], NUMA support
- non-uniform memory access
ms.assetid: 07727642-0266-4cbc-8c55-3c367e4458ca
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 055821c4d005c52ff20b79967fca35ac2994ff9f
ms.sourcegitcommit: 99f61724de5edf6640efd99916d464172eb23f92
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87362616"
---
# <a name="map-tcp-ip-ports-to-numa-nodes-sql-server"></a>Mapper les ports TCP/IP aux nœuds NUMA (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment mapper des ports TCP/IP à des nœuds NUMA (Non-Uniform Memory Access) à l'aide du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Au démarrage, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] écrit les informations relatives aux nœuds dans le journal des erreurs.  
  
 Pour déterminer quel numéro de nœud utiliser, consultez les informations sur les nœuds dans le journal des erreurs ou dans la vue **sys.dm_os_schedulers** . Pour définir l'adresse et le port TCP/IP d'un ou de plusieurs nœuds, ajoutez une bitmap d'identification de nœud (masque d'affinité) entre crochets après le numéro de port. Les nœuds peuvent être spécifiés au format décimal ou hexadécimal. Pour créer la bitmap, numérotez d'abord les nœuds de droite à gauche en commençant par zéro (par exemple, 76543210). Créez une représentation binaire de la liste des nœuds en attribuant la valeur 1 aux nœuds que vous souhaitez utiliser et la valeur 0 aux nœuds que vous ne souhaitez pas utiliser. Par exemple, pour utiliser les nœuds NUMA 0, 2 et 5, spécifiez 00100101.  
  
```text
NUMA node number                            76543210
Mask for 0, 2, and 5 counting from right    00100101
```
  
 Convertissez la représentation binaire (00100101) au format décimal `[37]`ou hexadécimal `[0x25]`. Pour écouter tous les nœuds, n'indiquez aucun identificateur de nœud.  
  
 Si un port est mappé à plusieurs nœuds NUMA, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attribue les connexions aux nœuds les unes après les autres sans chercher à équilibrer la charge entre les nœuds.  
  
> [!NOTE]  
>  Pour activer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour écouter sur plusieurs ports TCP pour chaque adresse IP, consultez [Configurer le moteur de base de données de manière à écouter sur plusieurs ports TCP](../../database-engine/configure-windows/configure-the-database-engine-to-listen-on-multiple-tcp-ports.md).  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Utilisation du Gestionnaire de configuration SQL Server  
  
#### <a name="to-map-a-tcpip-port-to-a-numa-node"></a>Pour mapper un port TCP/IP à un nœud NUMA  
  
1.  Dans le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Gestionnaire de configuration **, développez Configuration du réseau SQL Server**, puis cliquez sur**Protocoles pour** *\<instance name>* .  
  
2.  Dans le volet de détails, double-cliquez sur **TCP/IP**.  
  
3.  Sous l'onglet **Adresses IP** , dans la zone **Port TCP** de la section correspondant à l'adresse IP à configurer, ajoutez l'identificateur du nœud NUMA entre crochets, après le numéro de port. Par exemple, pour le port TCP 1500 et les nœuds 0, 2 et 5, utilisez **1500[37]** ou **1500[0x25]** .  
  
## <a name="see-also"></a>Voir aussi  
 [Soft-NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md)  
  
  
