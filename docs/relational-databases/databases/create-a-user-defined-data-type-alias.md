---
title: Créer un type de données d’alias défini par l’utilisateur | Microsoft Docs
description: Découvrez comment créer un alias de type de données défini par l’utilisateur dans SQL Server 2019 à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.userdefineddatatype.general.f1
- sql13.swb.new.datatype.properties.general.f1
helpviewer_keywords:
- alias data types [SQL Server], creating
ms.assetid: b1dd8413-0cd0-411b-a79b-1bb043ccc62d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bb3ec65ac712b64412896e011699cd57aa17c5be
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813579"
---
# <a name="create-a-user-defined-data-type-alias"></a>Créer un type de données d'alias défini par l'utilisateur
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Cette rubrique explique comment créer un nouvel alias de type de données défini par l'utilisateur dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour créer un alias de type de données défini par l'utilisateur, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Le nom d'un alias de type de données défini par l'utilisateur doit être conforme aux règles des identificateurs.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l’autorisation CREATE TYPE dans la base de données actuelle et l’autorisation ALTER sur *schema_name*. Si *schema_name* n’est pas spécifié, les règles de résolution de noms par défaut pour la détermination du schéma de l’utilisateur actuel s’appliquent.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-a-user-defined-data-type"></a>Pour créer un type de données défini par l'utilisateur  
  
1.  Dans l’Explorateur d’objets, développez successivement **Bases de données**, une base de données, **Programmabilité** et **Types**, cliquez avec le bouton droit sur **Types de données définis par l’utilisateur**, puis cliquez sur **Nouveau type de données défini par l’utilisateur**.  
  
     **Autoriser les valeurs NULL**  
     Spécifiez si le type de données défini par l'utilisateur accepte les valeurs NULL. L'acceptation de valeurs Null par un type de données défini par l'utilisateur existant n'est pas modifiable.  
  
     **Type de données**  
     Sélectionnez le type de données de base dans la zone de liste. La zone de liste affiche tous les types de données à l’exception des types de données **geography**, **geometry**, **hierarchyid**, **sysname**, **timestamp** et **xml** . Le type de données d'un type de données défini par l'utilisateur existant n'est pas modifiable.  
  
     **Par défaut**  
     (Facultatif) Sélectionnez une valeur par défaut pour la liaison à l’alias de type de données défini par l’utilisateur.  
  
     **Longueur/Précision**  
     Affiche la longueur ou la précision du type de données. **Longueur** s’applique aux types de données définis par l’utilisateur basés sur les caractères ; **Précision** s’applique uniquement aux types de données définis par l’utilisateur qui sont numériques. Le nom change en fonction du type de données sélectionné précédemment. Cette zone ne peut pas être modifiée si la longueur ou la précision du type de données sélectionné est fixe.  
  
     La longueur n’est pas affichée pour les types de données **nvarchar(max)** , **varchar(max)** ou **varbinary(max)** .  
  
     **Nom**  
     Si vous créez un nouvel alias de type de données défini par l'utilisateur, tapez un nom unique à utiliser dans la base de données pour représenter le type de données défini par l'utilisateur. Le nombre maximal de caractères doit correspondre au type de données système **sysname** . Le nom d'un alias de type de données défini par l'utilisateur existant n'est pas modifiable.  
  
     **Règle**  
     (Facultatif) Sélectionnez une règle pour la liaison à l'alias de type de données défini par l'utilisateur.  
  
     **Mettre à l'échelle**  
     Spécifie le nombre maximal de chiffres décimaux pouvant figurer à droite de la virgule décimale.  
  
     **Schéma**  
     Sélectionnez un schéma dans la liste de tous les schémas disponibles pour l'utilisateur actuel. La sélection par défaut est le schéma par défaut de l'utilisateur actuel.  
  
     **Stockage**  
     Affiche la taille de stockage maximale pour l'alias de type de données défini par l'utilisateur. Les tailles de stockage maximales varient en fonction de la précision.  
  
    |Precision|Taille de stockage maximale|  
    |-|-|  
    |1-9|5|  
    |10 - 19|9|  
    |20 - 28|13|  
    |29 - 38|17|  
  
     Pour les types de données **nchar** et **nvarchar** , la valeur de stockage équivaut toujours au double de la valeur contenue dans **Longueur**.  
  
     Le stockage n’est pas affiché pour les types de données **nvarchar(max)** , **varchar(max)** ou **varbinary(max)** .  
  
2.  Dans la boîte de dialogue **Nouveau type de données défini par l’utilisateur** , dans la zone **Schéma** , entrez le schéma qui doit détenir cet alias de type de données ou utilisez le bouton Parcourir pour sélectionner le schéma.  
  
3.  Dans la zone **Nom** , entrez le nom du nouvel alias de type de données.  
  
4.  Dans la zone **Type de données** , sélectionnez le type de données sur lequel le nouvel alias de type de données doit reposer.  
  
5.  Renseignez les zones **Longueur**, **Précision** et **Échelle** du type de données, si nécessaire.  
  
6.  Activez **Autoriser les valeurs NULL** si le nouvel alias de type de données peut autoriser les valeurs NULL.  
  
7.  Dans la zone **Liaison** , renseignez les zones **Par défaut** ou **Règle** pour lier une valeur par défaut ou une règle au nouvel alias de type de données. Vous ne pouvez pas créer de valeurs par défaut, ni de règles dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Utilisez [!INCLUDE[tsql](../../includes/tsql-md.md)]. Un exemple de code de création de valeurs par défaut et de règles est disponible dans l'Explorateur de modèles.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-create-a-user-defined-data-type-alias"></a>Pour créer un alias de type de données défini par l'utilisateur  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple crée un alias de type de données basé sur le type de données `varchar` fourni par le système. L'alias de type de données `ssn` est utilisé pour les colonnes comportant des numéros de sécurité sociale à 11 chiffres (999-99-9999). Cette colonne ne peut pas avoir la valeur NULL.  
  
```sql  
CREATE TYPE ssn  
FROM varchar(11) NOT NULL ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Identificateur de la base de données](../../relational-databases/databases/database-identifiers.md)   
 [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)  
  
  
