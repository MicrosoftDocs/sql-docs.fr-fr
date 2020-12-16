---
title: Autorisations ou Éléments sécurisables, page | Microsoft Docs
description: Utilisez la page Autorisations ou Éléments sécurisables pour afficher ou définir les autorisations pour les éléments sécurisables dans SQL Server.
ms.custom: ''
ms.date: 01/07/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.common.permissions.f1
- sql13.swb.SecurableAndEffectPermissions.f1
- sql13.swb.common.columnperm.f1
- sql13.swb.availabilitygroupproperties.permission.f1
- sql13.swb.SecurableAndEffectivePermission.f1
ms.assetid: b3bf077a-bec2-4161-ac0c-460586199906
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0bc202f99dfc1bcc067b3baf7f7606506f318d3d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479400"
---
# <a name="permissions-or-securables-page"></a>Page Autorisations ou Éléments sécurisables
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Utilisez la page **Autorisations** ou **Éléments sécurisables** pour afficher ou définir les autorisations pour les éléments sécurisables. Cette page peut être ouverte à partir de plusieurs emplacements. Le contenu de la page peut varier légèrement, selon la façon dont la page est ouverte et ce qu'elle contient. La grille supérieure de la page peut être remplie lorsque la page s'ouvre, ou elle peut être vide. Pour ajouter des éléments à la grille supérieure, cliquez sur **Rechercher**. Dans la grille supérieure, sélectionnez un élément, puis définissez les autorisations appropriées sous l'onglet **Autorisations explicites** . Pour afficher les autorisations agrégées, utilisez l'onglet **Autorisations effectives** .  
  
 Pour comprendre les combinaisons possibles d’éléments sécurisables et de principaux, consultez les liens relatifs à la syntaxe spécifique des éléments sécurisables dans la rubrique [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md). Pour plus d'informations, consultez [Securables](../../relational-databases/security/securables.md).  
  
## <a name="page-header"></a>En-tête de page  
 L'en-tête de la page **Autorisations** ou **Éléments sécurisables** varie en fonction de l'élément sécurisable ou du principal. Il affiche des informations pertinentes sur l'élément, telles que son nom.  
  
## <a name="upper-grid"></a>Grille supérieure  
 La grille supérieure contient un ou plusieurs éléments pour lesquels des autorisations peuvent être définies. Cette boîte de dialogue contient le bouton **Rechercher** , qui permet de sélectionner des objets ou des principaux à ajouter à la grille supérieure. Le nom de la grille peut afficher **Éléments sécurisables** ou un ou plusieurs types d'éléments sécurisables ou de principaux. Les colonnes affichées dans la grille supérieure varient selon le principal ou l'élément sécurisable.  
  
 **Nom**  
 Nom de chaque principal ou élément sécurisable ajouté à la grille.  
  
 **Type**  
 Décrit le type de chaque élément.  
  
## <a name="explicit-tab"></a>Onglet Autorisations explicites  
 L'onglet **Autorisations explicites** énumère les autorisations possibles pour les éléments sécurisables sélectionnés dans la grille supérieure. Pour configurer les autorisations, cochez ou décochez les cases **Accorder** (ou **Autoriser**), **Avec autorisation** et **Refuser** . Toutes les options ne sont pas disponibles pour toutes les autorisations explicites.  
  
 **autorisations**  
 Nom de l'autorisation.  
  
 **Fournisseur d'autorisations**  
 Principal ayant accordé l'autorisation.  
  
 **Octroyer**  
 Sélectionnez cette option pour octroyer cette autorisation à la connexion. Désactivez-la pour révoquer cette autorisation.  
  
 **Avec autorisation**  
 Reflète l'état de l'option WITH GRANT pour l'autorisation indiquée dans la liste. Cette zone est en lecture seule. Pour appliquer cette autorisation, utilisez l'instruction [GRANT](../../t-sql/statements/grant-transact-sql.md) .  
  
 **Deny**  
 Sélectionnez cette option pour refuser cette autorisation à la connexion. Désactivez-la pour révoquer cette autorisation.  
  
 **Autorisations au niveau des colonnes**  
 Pour les objets qui contiennent des colonnes (tels que les tables, les vues ou les fonctions table), le bouton **Autorisations au niveau des colonnes** ouvre la boîte de dialogue du **même nom** . Dans cette boîte de dialogue, vous pouvez définir les autorisations **Octroyer**, **Autoriser** ou **Refuser** sur des colonnes individuelles d'une table ou d'une vue. Cette option n'est pas disponible pour tous les types d'objets ou autorisations.  
  
## <a name="effective-tab"></a>Onglet Autorisations effectives  
 Les autorisations qu'un principal a mises en rapport avec un élément sécurisable peuvent provenir des autorisations définies pour plusieurs principaux différents. Par exemple, une connexion peut recevoir des autorisations individuellement, et également comme membre d'un groupe. L'onglet **Autorisations effectives** affiche le résultat de la combinaison d'autorisations explicites et des autorisations provenant d'appartenances de groupe ou de rôle. Les autorisations Octroyer sont agrégées. Une autorisation Refuser remplace toutes les autorisations Octroyer.  
  
 **autorisations**  
 Nom de l'autorisation.  
  
 **Colonne**  
 Noms des colonnes affectées par l'autorisation.  
  
## <a name="see-also"></a>Voir aussi  
 [Rôles au niveau de la base de données](../../relational-databases/security/authentication-access/database-level-roles.md)   
 [Centre de sécurité pour le moteur de base de données SQL Server et Azure SQL Database](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
