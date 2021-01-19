---
title: Soft-NUMA (SQL Server) | Microsoft Docs
description: Apprenez-en davantage sur le NUMA logiciel dans SQL Server 2014 SP2 et les versions ultérieures. Découvrez comment utiliser le NUMA logiciel automatique et comment configurer manuellement SQL Server pour utiliser le NUMA logiciel.
ms.custom: ''
ms.date: 02/13/2018
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- NUMA
- soft-NUMA
helpviewer_keywords:
- NUMA
- non-uniform memory access
- soft-NUMA
ms.assetid: 1af22188-e08b-4c80-a27e-4ae6ed9ff969
author: markingmyname
ms.author: maghan
ms.openlocfilehash: df2d323f8978ea5ce9cdaf23c2acf177517ff1ff
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170931"
---
# <a name="soft-numa-sql-server"></a>Soft-NUMA (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Les processeurs modernes ont plusieurs cœurs par socket. Chaque socket est généralement représenté sous forme de nœud NUMA unique. Le moteur de base de données SQL Server partitionne plusieurs structures internes et threads de service de partitionnement par nœud NUMA.  Pour les processeurs contenant 10 noyaux par socket ou plus, l’utilisation du NUMA logiciel pour fractionner les nœuds NUMA matériels augmente généralement l’évolutivité et les performances. Avant [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2, le NUMA logiciel (soft-NUMA) nécessitait la modification du Registre pour ajouter un masque d’affinité de configuration des nœuds et était configuré au niveau de l’hôte plutôt que par instance. À partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 et [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], le soft-NUMA est configuré automatiquement au niveau de l’instance de base de données au démarrage du service [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
> [!NOTE]  
> Les processeurs ajoutés à chaud ne sont pas pris en charge par le soft-NUMA.  
  
## <a name="automatic-soft-numa"></a>Soft-NUMA automatique  
Avec [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], chaque fois que le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] détecte plus de huit cœurs physiques par socket ou nœud NUMA au démarrage, des nœuds soft-NUMA sont créés automatiquement par défaut. Les cœurs des processeurs multithreads ne sont pas différenciés lors du décompte des cœurs physiques dans un nœud.  Quand le nombre de cœurs physiques détectés est supérieur à huit par socket, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] crée des nœuds soft-NUMA contenant idéalement huit cœurs, ce chiffre pouvant varier de cinq à neuf cœurs logiques par nœud. La taille du nœud matériel peut être limitée par un masque d'affinité de l’UC. Le nombre de nœuds NUMA ne dépasse jamais le nombre maximal de nœuds NUMA pris en charge.  
  
Vous pouvez désactiver ou réactiver le soft-NUMA avec l’instruction [ALTER SERVER CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md) et l’argument `SET SOFTNUMA`. La modification de ce paramètre requiert le redémarrage du moteur de base de données pour s’appliquer.  
  
La figure ci-dessous montre le type d’informations concernant le soft-NUMA que vous voyez dans le journal des erreurs SQL Server quand [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] détecte des nœuds NUMA matériels avec plus de huit cœurs physiques par nœud ou socket.  


```
2016-11-14 13:39:43.17 Server      SQL Server detected 2 sockets with 12 cores per socket and 24 logical processors per socket, 48 total logical processors; using 48 logical processors based on SQL Server licensing. This is an informational message; no user action is required.     
2016-11-14 13:39:43.35 Server      Automatic soft-NUMA was enabled because SQL Server has detected hardware NUMA nodes with greater than 8 physical cores.     
2016-11-14 13:39:43.63 Server      Node configuration: node 0: CPU mask: 0x0000000000555555:0 Active CPU mask: 0x0000000000555555:0. This message provides a description of the NUMA configuration for this computer. This is an informational message only. No user action is required.    
2016-11-14 13:39:43.63 Server      Node configuration: node 1: CPU mask: 0x0000000000aaaaaa:0 Active CPU mask: 0x0000000000aaaaaa:0. This message provides a description of the NUMA configuration for this computer. This is an informational message only. No user action is required.    
2016-11-14 13:39:43.63 Server      Node configuration: node 2: CPU mask: 0x0000555555000000:0 Active CPU mask: 0x0000555555000000:0. This message provides a description of the NUMA configuration for this computer. This is an informational message only. No user action is required.     
2016-11-14 13:39:43.63 Server      Node configuration: node 3: CPU mask: 0x0000aaaaaa000000:0 Active CPU mask: 0x0000aaaaaa000000:0. This message provides a description of the NUMA configuration for this computer. This is an informational message only. No user action is required.   
```   

> [!NOTE]
> À compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2, utilisez l’indicateur de trace 8079 pour autoriser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à utiliser le NUMA logiciel automatique. À compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], ce comportement est contrôlé par le moteur. L’indicateur de trace 8079 n’a aucun effet. Pour plus d’informations, consultez [DBCC TRACEON - Indicateurs de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

## <a name="manual-soft-numa"></a>Soft-NUMA manuel  
Pour configurer manuellement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin d’utiliser le soft-NUMA, désactivez le soft-NUMA automatique et modifiez le Registre pour ajouter un masque d’affinité de configuration des nœuds. Lorsque cette méthode est utilisée, le masque soft-NUMA peut être établi comme entrée de Registre binaire, DWORD (hexadécimal ou décimal) ou QWORD (hexadécimal ou décimal). Pour configurer davantage que les 32 premiers processeurs, utilisez les valeurs de Registre QWORD ou BINARY (les valeurs QWORD ne peuvent pas être utilisées avant [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]). Après avoir modifié le Registre, vous devez redémarrer le [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour que la configuration du soft-NUMA prenne effet.  
  
> [!TIP]
> Les UC sont numérotées à partir de 0.  

> [!WARNING]
> [!INCLUDE[ssNoteRegistry](../../includes/ssnoteregistry-md.md)]  
  
Prenons l’exemple d’un ordinateur avec huit processeurs sans NUMA matériel. Trois nœuds soft-NUMA sont configurés.   
L'instance A du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est configurée pour utiliser les UC de 0 à 3. Une deuxième instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est installée et configurée pour utiliser les UC de 4 à 7. L'exemple peut être représenté visuellement de la façon suivante :  
  
 `CPUs          0  1  2  3  4  5  6  7`  
  
 `Soft-NUMA   <-N0--><-N1-><----N2---->`  
  
 `SQL Server  <instance A ><instance B>`  
  
 L’instance A, confrontée à des E/S importantes, a maintenant deux threads d’E/S et un thread d’écriture différée. L’instance B, qui exécute des opérations nécessitant des ressources de processeur importantes, a un seul thread d’E/S et un seul thread d’écriture différée. Vous pouvez affecter des quantités de mémoire différentes aux instances, mais contrairement au NUMA matériel, elles reçoivent toutes deux la mémoire du même bloc mémoire de système d’exploitation et il n’y a pas d’affinité entre la mémoire et le processeur.  
  
 Le thread d’écriture différée est lié à la vue du système d’exploitation SQL des nœuds de mémoire NUMA physiques. Par conséquent, quel que soit le nombre de nœuds NUMA physiques que le matériel présente, il correspond au nombre de thread d'écriture différée qui sont créés. Pour plus d’informations, consultez [How It Works: Soft NUMA, I/O Completion Thread, Lazy Writer Workers and Memory Nodes](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-soft-numa-i-o-completion-thread-lazy-writer-workers/ba-p/316044) (Fonctionnement : NUMA logiciel, thread d’achèvement d’E/S, threads de travail d’écriture différée et nœuds de mémoire).  
  
> [!NOTE]
> Les clés de Registre **NUMA logiciel** ne sont pas copiées quand vous mettez à niveau une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="set-the-cpu-affinity-mask"></a>Définir le masque d'affinité de l'UC  
 Exécutez l'instruction suivante sur l'instance A de façon à la configurer pour qu'elle utilise les UC 0, 1, 2 et 3 en définissant le masque d'affinité de l'UC :  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY CPU=0 TO 3;  
```  
  
Exécutez l'instruction suivante sur l'instance B de façon à la configurer pour qu'elle utilise les UC 4, 5, 6 et 7 en définissant le masque d'affinité de l'UC :  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY CPU=4 TO 7;  
```  
  
### <a name="map-soft-numa-nodes-to-cpus"></a>Mapper les nœuds soft-NUMA aux UC  
 À l'aide de l'Éditeur du Registre (regedit.exe), ajoutez les clés de Registre suivantes pour mapper le nœud soft-NUMA 0 aux processeurs 0 et 1, le nœud soft-NUMA 1 aux processeurs 2 et 3, et le nœud soft-NUMA 2 aux processeurs 4, 5, 6 et 7.  
  
> [!TIP]
> Pour spécifier les UC 60 à 63, utilisez une valeur QWORD de F000000000000000 ou une valeur BINARY de 1111000000000000000000000000000000000000000000000000000000000000.  
  
 Dans l’exemple suivant, supposons que vous avez un serveur DL580 G9 avec 18 cœurs par socket (et quatre sockets), chaque socket se trouvant dans son propre groupe de noyaux (« K-Group »). Vous pouvez créer une configuration soft-NUMA du type suivant : six cœurs par nœud, trois nœuds par groupe, quatre groupes.  
  
|Exemple pour un serveur [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] avec plusieurs « K-Groups »|Type|Nom de la valeur|Données de valeur|  
|-----------------------------------------------------------------------------------------------------------------|----------|----------------|----------------|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node0|DWORD|CPUMask|0x3F|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node0|DWORD|Groupe|0|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node1|DWORD|CPUMask|0x0fc0|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node1|DWORD|Groupe|0|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node2|DWORD|CPUMask|0x3f000|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node2|DWORD|Groupe|0|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node3|DWORD|CPUMask|0x3F|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node3|DWORD|Groupe|1|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node4|DWORD|CPUMask|0x0fc0|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node4|DWORD|Groupe|1|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node5|DWORD|CPUMask|0x3f000|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node5|DWORD|Groupe|1|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node6|DWORD|CPUMask|0x3F|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node6|DWORD|Groupe|2|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node7|DWORD|CPUMask|0x0fc0|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node7|DWORD|Groupe|2|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node8|DWORD|CPUMask|0x3f000|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node8|DWORD|Groupe|2|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node9|DWORD|CPUMask|0x3F|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node9|DWORD|Groupe|3|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node10|DWORD|CPUMask|0x0fc0|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node10|DWORD|Groupe|3|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node11|DWORD|CPUMask|0x3f000|  
|HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\NodeConfiguration\Node11|DWORD|Groupe|3|  
  
## <a name="metadata"></a>Métadonnées  
 Vous pouvez utiliser les vues de gestion dynamique (DMV) suivantes pour afficher l’état actuel et la configuration du soft-NUMA.  
  
-   [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) : affiche la valeur actuelle (0 ou 1) pour SOFTNUMA  
  
-   [sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) : Les colonnes *softnuma_configuration* et *softnuma_configuration_desc* affichent les valeurs de configuration actuelles.  
  
> [!NOTE]
> Même si vous pouvez afficher la valeur d’exécution du soft-NUMA automatique avec [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md), vous ne pouvez pas modifier sa valeur avec **sp_configure**. Vous devez utiliser l’instruction [ALTER SERVER CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md) avec l’argument `SET SOFTNUMA`.  
  
## <a name="see-also"></a>Voir aussi  
[Mapper les ports TCP/IP aux nœuds NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/map-tcp-ip-ports-to-numa-nodes-sql-server.md)    
[affinity mask (option de configuration de serveur)](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md)    
[ALTER SERVER CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md)     
[sys.dm_os_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-nodes-transact-sql.md)   
  
