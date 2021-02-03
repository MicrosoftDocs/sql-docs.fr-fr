---
description: MSSQLSERVER_9004
title: MSSQLSERVER_9004 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 9004 (Database Engine error)
ms.assetid: b528bb49-340c-4a81-9c8d-cefce6562f16
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: b4706982db7c2c121af0658142344c0558321b52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161963"
---
# <a name="mssqlserver_9004"></a>MSSQLSERVER_9004
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|9004|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|LOG_CORRUPT|  
|Texte du message|Une erreur s'est produite lors du traitement du journal de la base de données '%.*ls'.  If possible, restore from backup. Si aucune sauvegarde n'est disponible, vous devrez peut-être recréer le journal.|  
  
## <a name="explanation"></a>Explication  
Une erreur s'est produite lors du traitement du journal, au cours d'une annulation, d'une récupération ou d'une réplication. Cela peut indiquer une erreur détectée par le système d'exploitation ou une erreur de cohérence interne détectée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] effectue des vérifications logiques sur la cohérence du contenu du journal des transactions lorsqu’il le lit et le traite. Tous les aspects de l’en-tête de journal, des blocs de journal et des enregistrements de journal ne sont pas vérifiés. Le numéro d’état fournit plus d’informations sur le type d’échec :

 - **État 1** L’en-tête du fichier journal du fichier journal virtuel (VLF) a été endommagé.  Si un en-tête de fichier journal endommagé est trouvé dans le cadre du démarrage de la base de données au démarrage du service, vous ne verrez peut-être que l’erreur 9004 dans ERRORLOG. L’en-tête du fichier journal est la première partie de chaque fichier journal virtuel à l’intérieur d’un journal des transactions. L’en-tête du fichier journal n’est pas le même que l’en-tête de fichier unique, ou les 8 premiers ko, du fichier journal. Si l’en-tête de fichier du fichier journal est endommagé, vous pouvez obtenir le message 5172, comme dans le cas d’un en-tête de fichier de base de données endommagé.
 - **États 2 et 3** Un bloc de journal n’était pas valide lors de l’exécution d’une opération RESTORE.
 - **États 4 à 12** Il s’agit des différentes vérifications des blocs de journal lors du traitement des enregistrements de journal. Cela inclut la parité, le secteur et d’autres vérifications logiques sur la cohérence du journal des transactions

Dans la plupart des cas, cette erreur se produit uniquement dans ERRORLOG ou le journal des événements d’application Windows, avec EventID = 9004, car l’opération de traitement du journal n’est pas basée sur une commande utilisateur directe (telle que la récupération exécutée au démarrage du moteur SQL Server). Dans ces situations, l’erreur 9004 est souvent détectée avec l’erreur 3414. Toutefois, certaines requêtes, comme ALTER DATABASE, peuvent nécessiter un traitement du journal et, par conséquent, voir ces erreurs. Étant donné que l’erreur a une Severity=21, la session utilisateur est déconnectée.

## <a name="cause"></a>Cause
L’erreur 9004 est une erreur générale indiquant que le contenu du journal des transactions est endommagé. La raison pour laquelle le journal devient incohérent est semblable à tout problème de corruption de base de données détecté par le moteur SQL Server. Pour déterminer la cause de l’endommagement du journal, vous devez suivre les mêmes techniques que pour l’endommagement de la base de données, notamment une analyse du matériel, du système de fichiers et des problèmes d’E/S possibles. Notez que DBCC CHECKDB ne vérifie pas le journal des transactions dans le cadre de ses opérations et ne peut pas détecter les erreurs d’endommagement du journal. L’erreur 9004 est déclenchée par le moteur SQL Server lui-même.

## <a name="user-action"></a>Action de l'utilisateur  
L'une des actions ci-dessous corrigera cette erreur :  
  
-   **Restaurer à partir d’une sauvegarde** :  Restaurez à partir d’une sauvegarde correcte connue pour résoudre ce problème. Il est possible que, si la partie journal d’une sauvegarde de base de données ou de fichier journal contient des contenus endommagés, vous rencontriez une erreur 9004 lors de RESTORE. Dans ce cas, le journal des transactions de la sauvegarde est endommagé.
  
-   **Régénérez le journal** :  Si vous ne pouvez pas effectuer une restauration à partir d’une sauvegarde, vous pourrez peut-être mettre la base de données en ligne en reconstruisant le journal des transactions. Vous devez soigneusement comprendre les ramifications de la reconstruction du journal des transactions. Cela comprend *la perte possible de la cohérence transactionnelle dans votre base de données*. Pour plus d’informations sur la régénération du journal des transactions, consultez [Résolution des erreurs en mode urgence dans la base de données](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md#resolving-errors-in-database-emergency-mode).
  
-   **Examinez les journaux pour les problèmes système** : Vérifiez également les journaux des événements et les journaux des erreurs du système qui peuvent être à l'origine du problème.  
  
