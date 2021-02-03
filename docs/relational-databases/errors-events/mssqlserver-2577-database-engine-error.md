---
description: MSSQLSERVER_2577
title: MSSQLSERVER_2577 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2577 (Database Engine error)
ms.assetid: f53256a2-2fb0-47fd-9ed9-c45389104145
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8c5656bbe837c18177206a3c97055a84f33d7c39
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209839"
---
# <a name="mssqlserver_2577"></a>MSSQLSERVER_2577
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|2577|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBCC_IAM_CHAIN_SEQUENCE_OUT_OF_ORDER|  
|Texte du message|Les numéros de séquence de chaînes sont désordonnés dans la chaîne IAM de l'ID d'objet O_ID, ID d'index I_ID, ID de partition PN_ID, ID d'unité d'allocation A_ID (type TYPE). La page P_ID1 avec le numéro de séquence SEQUENCE1 pointe vers la page P_ID2 avec le numéro de séquence SEQUENCE2.|  
  
## <a name="explanation"></a>Explication  
Chaque page IAM (Index Allocation Map) possède un numéro de séquence. Ce numéro de séquence correspond à la position de la page IAM dans la chaîne IAM. La règle est que les numéros de séquence augmentent d'une unité pour chaque page IAM. La page IAM *P_ID2* possède un numéro de séquence qui ne respecte pas cette règle.  
  
## <a name="user-action"></a>Action de l'utilisateur  
  
### <a name="look-for-hardware-failure"></a>Rechercher une défaillance matérielle  
Exécutez les diagnostics matériels et corrigez les éventuels problèmes rencontrés. Examinez également les journaux système et des applications de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, ainsi que le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour vérifier si l'erreur est due à une défaillance matérielle. Corrigez les éventuels problèmes matériels contenus dans les journaux.  
  
Si vous avez des problèmes persistants de données endommagées, tentez d'échanger votre ordinateur, vos contrôleurs et vos lecteurs de disque contre d'autres composants. Assurez-vous que votre système n'a pas une mémoire cache d'écriture active sur le contrôleur de disque. Si vous soupçonnez qu’il s’agit là de la source du problème, contactez votre fournisseur de matériel.  
  
Enfin, il peut s'avérer bénéfique d'utiliser un matériel totalement nouveau, avec reformatage des lecteurs de disque et réinstallation du système d'exploitation.  
  
### <a name="restore-from-backup"></a>Restaurer à partir de la sauvegarde  
Si le problème n'est pas matériel et si une restauration réputée en bon état est disponible, restaurez la base de données à partir de la sauvegarde.  
  
### <a name="run-dbcc-checkdb"></a>Exécuter DBCC CHECKDB  
Si aucune sauvegarde saine n'est disponible, exécutez DBCC CHECKDB sans clause REPAIR afin de déterminer l'étendue de l'altération. DBCC CHECKDB recommande une clause REPAIR à utiliser. Puis, exécutez DBCC CHECKDB avec la clause REPAIR adéquate afin de réparer les dommages.  
  
> [!CAUTION]  
> Si vous n'êtes pas sûr de l'effet de DBCC CHECKDB avec une clause REPAIR sur vos données, contactez l'assistance technique avant d'exécuter cette instruction.  
  
Si l'exécution de DBCC CHECKDB avec une des clauses REPAIR ne résout pas le problème, contactez l’assistance technique.  
  
### <a name="results-of-running-repair-options"></a>Résultats de l'exécution des options REPAIR  
L'exécution de REPAIR reconstruira la chaîne IAM. REPAIR commence par séparer la chaîne IAM existante en deux moitiés. La première moitié de la chaîne s’achèvera par la page IAM *P_ID1*. Le pointeur de page suivante de la page *P_ID1* sera défini avec les valeurs (0:0). La seconde moitié de la chaîne commencera par la page IAM *P_ID2*. Le pointeur de page précédente de la page *P_ID2* sera défini avec les valeurs (0:0).  
  
REPAIR assemblera alors les deux moitiés de la chaîne et régénérera les numéros de séquence de la chaîne IAM. Toutes les pages IAM qui ne peuvent pas être réparées seront désallouées.  
  
> [!CAUTION]  
> Cette réparation peut entraîner une perte de données.  
  
