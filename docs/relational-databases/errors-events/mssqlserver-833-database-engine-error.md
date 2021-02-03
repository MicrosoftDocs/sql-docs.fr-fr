---
description: MSSQLSERVER_833
title: MSSQLSERVER_833 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 833 (Database Engine error)
ms.assetid: 14129cc4-be80-4772-9e3f-0e5da4d0696b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d562e3ffcde77c706d0cf56a92f188b895eeb457
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197879"
---
# <a name="mssqlserver_833"></a>MSSQLSERVER_833
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sql-asdbmi.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|833|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|BUF_LONG_IO|  
|Texte du message|SQL Server a rencontré %d occurrence(s) de requêtes d’E/S mettant plus de %d secondes à s’effectuer dans le fichier [%ls] de la base de données `[%ls] (%d)`.  Le descripteur de fichier du système d'exploitation est 0x%p.  Le décalage de la dernière E/S longue est : %#016I64x.|  
  
## <a name="explanation"></a>Explication  
Ce message indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a émis une demande de lecture ou d'écriture à partir du disque et que la demande a mis plus de 15 secondes à retourner un résultat. Cette erreur, signalée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], indique un problème avec le sous-système d’E/S. D’autres symptômes peuvent être associés à ce message : délai d’attente PAGEIOLATCH élevé, avertissements ou erreurs dans le journal des événements système ou indications de problèmes de latence de disque dans les compteurs du moniteur système. Supervisez [sys.dm_io_virtual_file_stats](../system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md) et choisissez le niveau de stockage et le nombre d’IOPS appropriés pour votre débit de stockage. 
  
### <a name="possible-causes"></a>Causes possibles  
Cette situation peut être due à des problèmes de performances du système d’exploitation, à des erreurs matérielles, à des erreurs de microprogramme, à des problèmes de pilote de périphérique ou à l’intervention d’un pilote de filtre dans le processus d’E/S ou le chemin de stockage de fichiers de base de données. SQL Server enregistre l’heure à laquelle il a initié une demande d’E/S et celle à laquelle l’E/S a été effectuée. Si la différence est supérieure ou égale à 15 secondes, cet état est détecté. Cela signifie également que SQL Server n’est pas la cause de la situation d’E/S différée décrite et signalée par ce message. Cet état est appelé « E/S bloquées ». La plupart des demandes de disque se produisent à la vitesse standard du disque, souvent appelée « délai de recherche du disque ». Le temps de recherche de la plupart des disques standard est de 10 millisecondes maximum. Par conséquent, 15 secondes représente un très long délai de renvoi du chemin d’E/S système à SQL Server. 
  
## <a name="user-action"></a>Action de l'utilisateur  
Essayez de résoudre le problème en recherchant dans le journal des événements système d'éventuels messages d'erreur liés au matériel. Examinez également les journaux spécifiques au matériel s'ils sont disponibles. Utilisez les méthodes et les techniques nécessaires pour déterminer la cause du retard dans le système d’exploitation, avec les pilotes ou avec le matériel d’E/S. La résolution de ce problème peut impliquer de mettre à jour tous les pilotes de périphérique et du microprogramme ou d’exécuter d’autres diagnostics associés au système de disque. 
  
Utilisez l'Analyseur de performances pour consulter les compteurs suivants :  
  
-   **Moyenne disque s/transfert**  
  
-   **Longueur moyenne de la file d’attente du disque**  
  
-   **Longueur actuelle de la file d’attente du disque**  
  
Par exemple, la durée **Moyenne disque s/transfert** sur un ordinateur exécutant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est généralement inférieure à 15 millisecondes. Si la valeur **Moyenne disque s/transfert** augmente, cela indique que le sous-système d’E/S ne parvient pas parfaitement à suivre la demande d’E/S.

Vous pouvez également utiliser des fonctionnalités telles que la [journalisation Storport ETW](/archive/blogs/ntdebugging/storport-etw-logging-to-measure-requests-made-to-a-disk-unit) pour mesurer la latence des demandes effectuées sur une unité de disque. Un autre kit de dépannage des E/S de disque du même type est disponible sous la forme d’un profil intégré de [l’Enregistreur de performance Windows](/windows-hardware/test/wpt/introduction-to-wpr).
  
> [!NOTE]  
> L'accès au disque peut être ralenti par un antivirus. Pour augmenter la vitesse d'accès, excluez des analyses antivirus actives les fichiers de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui sont spécifiés dans le message d'erreur. Vous pouvez utiliser [l’utilitaire de ligne de commande fltmc.exe](/windows-hardware/drivers/ifs/development-and-testing-tools#fltmcexe-control-program) pour interroger tous les pilotes de filtre installés sur le système et comprendre les fonctions qu’il effectue sur le chemin de stockage des fichiers de base de données. 
  
Pour plus d’informations sur les erreurs d’E/S, consultez [Concepts de base des E/S de Microsoft SQL Server, chapitre 2](/previous-versions/sql/sql-server-2005/administrator/cc917726(v=technet.10)) et l’article de la Base de connaissances disponible à l’adresse [https://support.microsoft.com/kb/897284/en-us](https://support.microsoft.com/kb/897284/en-us).  
