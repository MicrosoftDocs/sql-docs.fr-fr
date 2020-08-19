---
description: MSSQLSERVER_7984
title: MSSQLSERVER_7984 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 7984 (Database Engine error)
ms.assetid: e3192f56-e4e2-41da-b132-65f1e7540b1a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f2b0b2938bc276105c9177e94aece38cb3c2de32
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88428591"
---
# <a name="mssqlserver_7984"></a>MSSQLSERVER_7984
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|7984|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBCC2_PRE_CHECKS_BAD_PAGE_TYPE|  
|Texte du message|Pré-vérifications de table système : ID d’objet O_ID. La page P_ID possède un type de page PAGETYPE. Vérifiez l'instruction qui s'est arrêtée en raison d'une erreur irréparable.|  
  
## <a name="explanation"></a>Explication  
Une page avec un type autre que DATA_PAGE a été trouvée dans le niveau de données de l'objet spécifié. Cette erreur est générée au cours de la première phase des vérifications de la commande DBCC CHECKDB. Au cours de cette phase, DBCC CHECKDB effectue des vérifications primitives sur les pages de données des tables de base système critiques.  
  
> [!NOTE]  
> Si des erreurs sont détectées dans les tables système, les erreurs ne peuvent pas être réparées ; par conséquent, la commande DBCC CHECKDB se termine immédiatement.  
  
## <a name="user-action"></a>Action de l'utilisateur  
  
### <a name="look-for-hardware-failure"></a>Rechercher une défaillance matérielle  
Exécutez les diagnostics matériels et corrigez les éventuels problèmes rencontrés. Examinez également les journaux système et des applications de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, ainsi que le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour vérifier si l'erreur est due à une défaillance matérielle. Corrigez les éventuels problèmes matériels contenus dans les journaux.  
  
Si vous avez des problèmes persistants de données endommagées, tentez d'échanger votre ordinateur, vos contrôleurs et vos lecteurs de disque contre d'autres composants. Assurez-vous que votre système n'a pas une mémoire cache d'écriture active sur le contrôleur de disque. Si vous soupçonnez qu’il s’agit là de la source du problème, contactez votre fournisseur de matériel.  
  
Enfin, il peut s'avérer bénéfique d'utiliser un matériel totalement nouveau, avec reformatage des lecteurs de disque et réinstallation du système d'exploitation.  
  
### <a name="restore-from-backup"></a>Restaurer à partir de la sauvegarde  
Si le problème n'est pas matériel et si une restauration réputée en bon état est disponible, restaurez la base de données à partir de la sauvegarde.  
  
### <a name="run-dbcc-checkdb"></a>Exécuter DBCC CHECKDB  
Non applicable. Cette erreur ne peut pas être réparée automatiquement. Si vous ne pouvez pas restaurer la base de données à partir d'une sauvegarde, contactez le service de support technique de [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
