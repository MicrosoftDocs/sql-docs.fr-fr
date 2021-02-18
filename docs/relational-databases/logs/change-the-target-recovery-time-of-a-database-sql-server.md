---
title: Modifier la durée de récupération cible d’une base de données
description: Découvrez comment définir ou modifier le temps de récupération cible d'une base de données SQL Server dans SQL Server à l’aide de SQL Server Management Studio ou Transact-SQL.
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
ms.assetid: e466419a-d8a4-48f7-8d97-13a903ad6b15
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: d5762a71ab0bd2425abb0ee59c997f1f30fb10b4
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489363"
---
# <a name="change-the-target-recovery-time-of-a-database-sql-server"></a>Modifier la durée de récupération cible d'une base de données (SQL Server)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment définir ou modifier le temps de récupération cible d’une base de données dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] en utilisant [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)]. Par défaut, le temps de récupération cible est de 60 secondes, et la base de données utilise des *points de contrôle indirects*. Le temps de récupération cible établit une limite supérieure sur le temps de récupération pour cette base de données.  
  
> [!NOTE]  
>  La limite supérieure spécifiée pour une base de données spécifique par son paramètre de temps de récupération cible peut être dépassée si une transaction longue entraîne des durées UNDO excessives.  
  
-   **Avant de commencer :**  [Limitations et restrictions](#Restrictions), [sécurité](#Security)  
  
-   **Pour changer le temps de récupération cible à l’aide de :**  [SQL Server Management Studio](#SSMSProcedure) ou de [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions 
  
> [!CAUTION]  
>  Une charge de travail transactionnelle en ligne sur une base de données configurée pour les points de contrôle indirects peut rencontrer une dégradation des performances. Les points de contrôle indirects permettent de s’assurer que le nombre de pages de modifications est inférieur à un certain seuil afin que la récupération de la base de données se termine dans le temps de récupération cible. L’option de configuration d’intervalle de récupération utilise le nombre de transactions pour déterminer le temps de récupération, contrairement aux points de contrôle indirects, qui utilisent le nombre de pages de modifications. Quand des points de contrôle indirects sont activés sur une base de données recevant un grand nombre d’opérations DML, l’enregistreur en arrière-plan peut commencer à vider de manière intense les mémoires tampons modifiées sur le disque afin de s’assurer que le délai nécessaire à la récupération se situe dans le temps de récupération cible défini sur la base de données. Cela peut entraîner une activité supplémentaire en termes d’E/S sur certains systèmes, ce qui peut contribuer à un goulot d’étranglement des performances si le sous-système du disque fonctionne au-delà du seuil d’E/S ou s’en rapproche.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'autorisation ALTER sur la base de données.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour modifier le temps de récupération cible**  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]et développez-la.  
  
2.  Développez le conteneur **Bases de données**, cliquez avec le bouton droit sur la base de données à modifier, puis cliquez sur la commande **Propriétés**.  
  
3.  Dans la boîte de dialogue **Propriétés de la base de données** , cliquez sur la page **Options** .  
  
4.  Dans le volet **Récupération**, dans le champ **Temps de récupération cible (secondes)** , spécifiez le nombre de secondes de votre choix pour définir la limite supérieure du temps de récupération de cette base de données.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour modifier le temps de récupération cible**  
  
1.  Connectez-vous à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où réside la base de données.  
  
2.  Utilisez l’instruction [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md) suivante, comme suit :  
  
     TARGET_RECOVERY_TIME **=** _target_recovery_time_ { SECONDS | MINUTES }  
  
     *target_recovery_time*  
     À partir de [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)], la valeur par défaut est de 1 minute. Lorsque la valeur est supérieure à 0 (valeur par défaut pour les versions antérieures), spécifie la limite supérieure du temps de récupération de la base de données spécifiée en cas de sinistre.  
  
     SECONDS  
     Indique que *target_recovery_time* correspond au nombre de secondes.  
  
     MINUTES  
     Indique que *target_recovery_time* correspond au nombre de minutes.  
  
     L'exemple suivant définit le temps de récupération cible de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] sur `60` secondes.  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET TARGET_RECOVERY_TIME = 60 SECONDS;  
    ```  
  
## <a name="see-also"></a>Voir aussi  
 [Points de contrôle de base de données &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md)   
 [ALTER DATABASE SET Options &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)  
  
  
