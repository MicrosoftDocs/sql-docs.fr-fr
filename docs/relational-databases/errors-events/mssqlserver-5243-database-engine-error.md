---
description: MSSQLSERVER_5243
title: MSSQLSERVER_5243 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5243 (Database Engine error)
ms.assetid: e04a1934-e57d-420e-ac79-97071745824e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 6cd72c5e595537fd4afa3d33581321b022d55077
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198106"
---
# <a name="mssqlserver_5243"></a>MSSQLSERVER_5243
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|5243|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique||  
|Texte du message|Une incohérence a été détectée durant une opération interne. Contactez le support technique. Numéro de référence %ld.|  
  
## <a name="explanation"></a>Explication  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a détecté une incohérence structurelle dans une structure de moteur de stockage en mémoire.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Recherchez une défaillance matérielle. Exécutez les diagnostics matériels et corrigez les éventuels problèmes rencontrés. Examinez également les journaux système et des applications Windows, ainsi que le journal des erreurs de SQL Server, pour vérifier si l'erreur est due à une défaillance matérielle. Corrigez les éventuels problèmes matériels contenus dans les journaux.

Si vous avez des problèmes persistants de données endommagées, tentez d'échanger votre ordinateur, vos contrôleurs et vos lecteurs de disque contre d'autres composants. Assurez-vous que la mise en cache des écritures n’est pas activée sur le contrôleur de disque de votre système. Si vous soupçonnez que c’est la cause du problème, contactez votre fournisseur de matériel.

Enfin, il peut s'avérer bénéfique d'utiliser un matériel totalement nouveau, avec reformatage des lecteurs de disque et réinstallation du système d'exploitation.

Restaurer à partir d’une sauvegarde : s’il ne s’agit pas d’un problème de matériel et qu’une sauvegarde propre est disponible, restaurez la base de données à partir de cette sauvegarde.

Exécuter DBCC CHECKDB : si aucune sauvegarde propre n’est disponible, exécutez DBCC CHECKDB sans clause REPAIR pour déterminer l’étendue de l’altération. DBCC CHECKDB recommande une clause REPAIR à utiliser. Puis, exécutez DBCC CHECKDB avec la clause REPAIR adéquate afin de réparer les dommages.

> **balise d’alerte non prise en charge !** 
> **balise tr non prise en charge !** 
> **balise tr non prise en charge !**

Si l'exécution de DBCC CHECKDB avec une des clauses REPAIR ne résout pas le problème, contactez l’assistance technique.
  
## <a name="see-also"></a>Voir aussi  
[DBCC CHECKDB &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[Groupes de fichiers et fichiers de base de données](~/relational-databases/databases/database-files-and-filegroups.md)  
  
