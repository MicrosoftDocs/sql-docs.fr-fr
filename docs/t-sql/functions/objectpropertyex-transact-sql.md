---
description: OBJECTPROPERTYEX (Transact-SQL)
title: OBJECTPROPERTYEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- OBJECTPROPERTYEX
- OBJECTPROPERTYEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- displaying schema-scoped object information
- viewing schema-scoped object information
- OBJECTPROPERTYEX function
- schema-scoped objects [SQL Server]
- objects [SQL Server], schema-scoped
ms.assetid: be36b3e3-3309-4332-bfb5-c7e9cf8dc8bd
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 371e5230707c4b82680213689f8e4cd4ddc45986
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170291"
---
# <a name="objectpropertyex-transact-sql"></a>OBJECTPROPERTYEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne des informations concernant les objets étendus aux schémas dans la base de données actuelle. Pour obtenir la liste de ces objets, consultez [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md). OBJECTPROPERTYEX ne peut pas être utilisé pour des objets qui ne sont pas étendus aux schémas, tels que les déclencheurs DDL (Data Definition Language) et les notifications d'événements.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
OBJECTPROPERTYEX ( id , property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *id*  
 Expression représentant l'ID de l'objet dans la base de données active. *id* est de type **int** et est considéré comme un objet étendu aux schémas dans le contexte de la base de données active.  
  
 *property*  
 Expression contenant les informations à retourner pour l’objet spécifié par id. Le type de retour est **sql_variant**. Le tableau qui suit indique le type de données de base pour chaque valeur de propriété.  
  
> [!NOTE]  
>  Sauf indication contraire, la valeur NULL est retournée lorsque *property* n’est pas un nom de propriété valide, lorsque *id* n’est pas un ID d’objet valide, lorsque *id* est un type d’objet qui n’est pas pris en charge pour la propriété *property* spécifiée ou lorsque l’appelant n’est pas autorisé à consulter les métadonnées de l’objet.  
  
|Nom de la propriété|Type d'objet|Description et valeurs retournées|  
|-------------------|-----------------|-------------------------------------|  
|BaseType|Tout objet étendu aux schémas|Identifie le type de base de l'objet. Lorsque l'objet spécifié est un SYNONYM, le type de base de l'objet sous-jacent est retourné.<br /><br /> Nonnull = Type d'objet<br /><br /> Type de données de base : **char(2)**|  
|CnstIsClustKey|Contrainte|Contrainte PRIMARY KEY avec un index cluster.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|CnstIsColumn|Contrainte|Contrainte CHECK, DEFAULT ou FOREIGN KEY sur une seule colonne.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|CnstIsDeleteCascade|Contrainte|Contrainte FOREIGN KEY avec l'option ON DELETE CASCADE.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|CnstIsDisabled|Contrainte|Contrainte désactivée.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|CnstIsNonclustKey|Contrainte|Contrainte PRIMARY KEY avec un index non cluster.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|CnstIsNotRepl|Contrainte|La contrainte est définie avec les mots clés NOT FOR REPLICATION.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|CnstIsNotTrusted|Contrainte|La contrainte a été activée sans vérification des lignes existantes. Il se peut donc qu'elle ne s'applique pas à toutes les lignes.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|CnstIsUpdateCascade|Contrainte|Contrainte FOREIGN KEY avec l'option ON UPDATE CASCADE.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsAfterTrigger|Déclencheur|Déclencheur AFTER.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsAnsiNullsOn|Fonction [!INCLUDE[tsql](../../includes/tsql-md.md)], procédure [!INCLUDE[tsql](../../includes/tsql-md.md)], déclencheur [!INCLUDE[tsql](../../includes/tsql-md.md)], vue|Définition des valeurs ANSI NULL lors de la création.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsDeleteTrigger|Déclencheur|Déclencheur DELETE.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsFirstDeleteTrigger|Déclencheur|Premier déclencheur exécuté lorsqu'une instruction DELETE est exécutée sur la table.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsFirstInsertTrigger|Déclencheur|Premier déclencheur exécuté lorsqu'une instruction INSERT est exécutée sur la table.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsFirstUpdateTrigger|Déclencheur|Premier déclencheur exécuté lorsqu'une instruction UPDATE est exécutée sur la table.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsInsertTrigger|Déclencheur|Déclencheur INSERT.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsInsteadOfTrigger|Déclencheur|Déclencheur INSTEAD OF.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsLastDeleteTrigger|Déclencheur|Dernier déclencheur activé lorsqu'une instruction DELETE est exécutée sur la table.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsLastInsertTrigger|Déclencheur|Dernier déclencheur activé lorsqu'une instruction INSERT est exécutée sur la table.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsLastUpdateTrigger|Déclencheur|Dernier déclencheur activé lorsqu'une instruction UPDATE est exécutée sur la table.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsQuotedIdentOn|Fonction [!INCLUDE[tsql](../../includes/tsql-md.md)], procédure [!INCLUDE[tsql](../../includes/tsql-md.md)], déclencheur [!INCLUDE[tsql](../../includes/tsql-md.md)], vue|Définition de QUOTED_IDENTIFIER lors de la création.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsStartup|Procédure|Procédure de démarrage.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsTriggerDisabled|Déclencheur|Déclencheur désactivé.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsTriggerNotForRepl|Déclencheur|Déclencheur défini comme NOT FOR REPLICATION.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsUpdateTrigger|Déclencheur|Déclencheur UPDATE.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|ExecIsWithNativeCompilation|Procédure [!INCLUDE[tsql](../../includes/tsql-md.md)]|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Procédure compilée en mode natif.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|HasAfterTrigger|Table, vue|La table ou la vue comporte un déclencheur AFTER.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|HasDeleteTrigger|Table, vue|La table ou la vue comporte un déclencheur DELETE.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|HasInsertTrigger|Table, vue|La table ou la vue comporte un déclencheur INSERT.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|HasInsteadOfTrigger|Table, vue|La table ou la vue comporte un déclencheur INSTEAD OF.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|HasUpdateTrigger|Table, vue|La table ou la vue comporte un déclencheur UPDATE.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsAnsiNullsOn|Fonction [!INCLUDE[tsql](../../includes/tsql-md.md)], procédure [!INCLUDE[tsql](../../includes/tsql-md.md)], table, déclencheur [!INCLUDE[tsql](../../includes/tsql-md.md)], vue|Spécifie que l'option ANSI NULLS de la table a la valeur ON. Toutes les comparaisons avec une valeur Null produisent la valeur UNKNOWN. Ce paramètre s'applique à l'ensemble des expressions dans la définition de la table, y compris les contraintes et les colonnes calculées, aussi longtemps que la table existe.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsCheckCnst|Tout objet étendu aux schémas|Contrainte CHECK.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsConstraint|Tout objet étendu aux schémas|Contrainte.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsDefault|Tout objet étendu aux schémas|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Valeur par défaut associée.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsDefaultCnst|Tout objet étendu aux schémas|Contrainte DEFAULT.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsDeterministic|Fonctions scalaires et fonctions table, vue|Propriété de déterminisme de la fonction ou de la vue.<br /><br /> 1 = Déterministe<br /><br /> 0 = Non déterministe<br /><br /> Type de données de base : **int**|  
|IsEncrypted|Fonction [!INCLUDE[tsql](../../includes/tsql-md.md)], procédure [!INCLUDE[tsql](../../includes/tsql-md.md)], table, déclencheur [!INCLUDE[tsql](../../includes/tsql-md.md)], vue|Indique que le texte d'origine provenant de l'instruction du module a été converti dans un format d'obfuscation. La sortie générée par l'obfuscation n'est pas visible directement dans les affichages catalogue de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Les utilisateurs n’ayant pas accès aux tables système ou aux fichiers de base de données ne peuvent pas récupérer le texte obscurci. Le texte est cependant à la disposition des utilisateurs qui peuvent accéder aux tables système via le [port DAC](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md) ou accéder directement aux fichiers de base de données. Les utilisateurs qui peuvent associer un débogueur au processus serveur peuvent également récupérer la procédure d'origine de la mémoire au moment de l'exécution.<br /><br /> 1 = Chiffrée<br /><br /> 0 = Non chiffrée<br /><br /> Type de données de base : **int**|  
|IsExecuted|Tout objet étendu aux schémas|Spécifie quel objet peut être exécuté (vue, procédure, fonction ou déclencheur).<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsExtendedProc|Tout objet étendu aux schémas|Procédure étendue.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsForeignKey|Tout objet étendu aux schémas|Contrainte FOREIGN KEY.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsIndexed|Table, vue|Table ou vue avec un index.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsIndexable|Table, vue|Table ou vue dans laquelle un index peut être créé.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsInlineFunction|Fonction|Fonction Inline.<br /><br /> 1 = Fonction Inline<br /><br /> 0 = Fonction non Inline<br /><br /> Type de données de base : **int**|  
|IsMSShipped|Tout objet étendu aux schémas|Objet créé durant l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsPrecise|Colonne calculée, fonction, type défini par l'utilisateur, vue|Indique si l'objet contient un calcul imprécis, tel que des opérations en virgule flottante.<br /><br /> 1 = Précis<br /><br /> 0 = Imprécis<br /><br /> Type de données de base : **int**|  
|IsPrimaryKey|Tout objet étendu aux schémas|Contrainte PRIMARY KEY.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsProcedure|Tout objet étendu aux schémas|Procédure.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsQuotedIdentOn|Contrainte CHECK, définition DEFAULT, fonction [!INCLUDE[tsql](../../includes/tsql-md.md)], procédure [!INCLUDE[tsql](../../includes/tsql-md.md)], table, déclencheur [!INCLUDE[tsql](../../includes/tsql-md.md)], vue|Spécifie que le paramètre d'identificateur entre guillemets de l'objet a la valeur ON. Les guillemets doubles délimitent les identificateurs dans toutes les expressions impliquées dans la définition de l'objet.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsQueue|Tout objet étendu aux schémas|File d'attente Service Broker<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsReplProc|Tout objet étendu aux schémas|Procédure de réplication.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsRule|Tout objet étendu aux schémas|Règle liée.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsScalarFunction|Fonction|Fonction scalaire.<br /><br /> 1 = Fonction scalaire<br /><br /> 0 = Fonction non scalaire<br /><br /> Type de données de base : **int**|  
|IsSchemaBound|Fonction, Procédure, vue|Fonction ou vue liée à un schéma, créée à l'aide de SCHEMABINDING.<br /><br /> 1 = Fonction liée à un schéma<br /><br /> 0 = Fonction non liée à un schéma<br /><br /> Type de données de base : **int**|  
|IsSystemTable|Table de charge de travail|Table système.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsSystemVerified|Colonne calculée, fonction, type défini par l'utilisateur, vue|Les propriétés de précision et de déterminisme de l'objet peuvent être vérifiées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsTable|Table de charge de travail|Table.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsTableFunction|Fonction|Fonction table.<br /><br /> 1 = Fonction table<br /><br /> 0 = Fonction non-table<br /><br /> Type de données de base : **int**|  
|IsTrigger|Tout objet étendu aux schémas|Déclencheur.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsUniqueCnst|Tout objet étendu aux schémas|Contrainte UNIQUE.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsUserTable|Table de charge de travail|Table définie par l'utilisateur.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|IsView|Vue|Vue.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|OwnerId|Tout objet étendu aux schémas|Propriétaire de l'objet.<br /><br /> **Remarque :** Le propriétaire du schéma n’est pas nécessairement le propriétaire de l’objet. Par exemple, les objets enfants (ceux où *parent_object_id* est non-NULL) retournent toujours le même ID de propriétaire que leur parent.<br /><br /> Nonnull = ID d'utilisateur de base de données du propriétaire de l'objet.<br /><br /> NULL = Type d'objet non pris en charge, ou ID d'objet non valide.<br /><br /> Type de données de base : **int**|  
|SchemaId|Tout objet étendu aux schémas|ID du schéma associé à l'objet.<br /><br /> Nonnull = ID de schéma de l'objet.<br /><br /> Type de données de base : **int**|  
|SystemDataAccess|Fonction, vue|L'objet accède aux données système, aux catalogues système ou aux tables système virtuelles, dans l'instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 0 = Aucun(e)<br /><br /> 1 = Lecture<br /><br /> Type de données de base : **int**|  
|TableDeleteTrigger|Table de charge de travail|La table comporte un déclencheur DELETE.<br /><br /> >1 = ID du premier déclencheur du type spécifié.<br /><br /> Type de données de base : **int**|  
|TableDeleteTriggerCount|Table de charge de travail|La table comporte le nombre de déclencheurs DELETE spécifié.<br /><br /> Nonnull = Nombre de déclencheurs DELETE<br /><br /> Type de données de base : **int**|  
|TableFullTextMergeStatus|Table de charge de travail|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Indique s'il s'agit d'une table qui a un index de recherche en texte intégral qui est en cours de fusion.<br /><br /> 0 = La table n'a pas d'index de recherche en texte intégral ou l'index de recherche en texte intégral n'est pas en cours de fusion.<br /><br /> 1 = L'index de recherche en texte intégral est en cours de fusion.|  
|TableFullTextBackgroundUpdateIndexOn|Table de charge de travail|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> L'index de mise à jour d'arrière-plan en texte intégral (suivi des modifications automatiques) est activé pour la table.<br /><br /> 1 = TRUE<br /><br /> 0 = FALSE<br /><br /> Type de données de base : **int**|  
|TableFulltextCatalogId|Table de charge de travail|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> ID du catalogue de texte intégral dans lequel résident les données d'indexation de texte intégral de la table.<br /><br /> Différent de zéro = ID de catalogue de texte intégral associé à l'index unique qui identifie les lignes dans une table indexée en texte intégral.<br /><br /> 0 = Table sans index de recherche en texte intégral.<br /><br /> Type de données de base : **int**|  
|TableFullTextChangeTrackingOn|Table de charge de travail|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Le suivi des modifications de texte intégral est activé pour la table.<br /><br /> 1 = TRUE<br /><br /> 0 = FALSE<br /><br /> Type de données de base : **int**|  
|TableFulltextDocsProcessed|Table de charge de travail|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Nombre de lignes traitées depuis le démarrage de l'indexation de texte intégral. Dans une table en cours d'indexation pour une recherche en texte intégral, toutes les colonnes d'une ligne sont considérées comme faisant partie d'un même document à indexer.<br /><br /> 0 = Aucune analyse ou indexation de texte intégral active n'est terminée.<br /><br /> > 0 = une des valeurs suivantes (A ou B) : A) nombre de documents traités par les opérations d’insertion ou de mise à jour depuis le démarrage de l’alimentation du suivi des modifications complet, incrémentiel ou manuel ; (B) nombre de lignes traitées par les opérations d’insertion ou de mise à jour depuis l’activation de l’alimentation du suivi des modifications avec index de mise à jour en arrière-plan, depuis la modification du schéma d’index de recherche en texte intégral, depuis la reconstruction du catalogue de recherche en texte intégral ou depuis le redémarrage de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], et ainsi de suite.<br /><br /> NULL = La table n'a pas d'index de recherche en texte intégral.<br /><br /> Type de données de base : **int**<br /><br /> **Remarque** Cette propriété ne contrôle pas et ne compte pas les lignes supprimées.|  
|TableFulltextFailCount|Table de charge de travail|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Nombre de lignes que la recherche en texte intégral n'a pas indexées.<br /><br /> 0 = Le remplissage est terminé.<br /><br /> > 0 = une des valeurs suivantes (A ou B) : A) nombre de documents qui n’ont pas été indexés depuis le démarrage de l’alimentation du suivi des modifications complet, incrémentiel ou manuel ; B) pour le suivi des modifications avec index de mise à jour en arrière-plan, nombre de lignes qui n’ont pas été indexées depuis le démarrage ou le redémarrage de l’alimentation. Ceci peut être provoqué par une modification du schéma, une reconstruction du catalogue, un redémarrage du serveur, etc.<br /><br /> NULL = Table sans index de recherche en texte intégral.<br /><br /> Type de données de base : **int**|  
|TableFulltextItemCount|Table de charge de travail|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Nonnull = Nombre de lignes qui ont été correctement indexées en texte intégral.<br /><br /> NULL = La table n'a pas d'index de recherche en texte intégral.<br /><br /> Type de données de base : **int**|  
|TableFulltextKeyColumn|Table de charge de travail|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> ID de la colonne associée à l'index de colonne unique qui fait partie de la définition d'un index de recherche en texte intégral et d'un index sémantique.<br /><br /> 0 = Table sans index de recherche en texte intégral.<br /><br /> Type de données de base : **int**|  
|TableFulltextPendingChanges|Table de charge de travail|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Nombre d'entrées de suivi des modifications en attente de traitement.<br /><br /> 0 = Le suivi des modifications n'est pas activé.<br /><br /> NULL = La table n'a pas d'index de recherche en texte intégral.<br /><br /> Type de données de base : **int**|  
|TableFulltextPopulateStatus|Table de charge de travail|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> 0 = Inactif.<br /><br /> 1 = Remplissage complet en cours.<br /><br /> 2 = Remplissage incrémentiel en cours.<br /><br /> 3 = Propagation des changements suivis en cours.<br /><br /> 4 = Création de l'index de mise à jour d'arrière-plan en cours, par exemple le suivi des modifications automatiques.<br /><br /> 5 = Indexation de texte intégral accélérée ou suspendue.<br /><br /> 6 = Une erreur s’est produite. Examinez le journal d’analyse pour plus d’informations. Pour plus d’informations, consultez la section **Résolution des erreurs dans une alimentation de texte intégral (Analyse)** dans [Alimenter des index de recherche en texte intégral](../../relational-databases/search/populate-full-text-indexes.md).<br /><br /> Type de données de base : **int**|  
|TableFullTextSemanticExtraction|Table de charge de travail|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> La table est activée pour l'indexation sémantique.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasActiveFulltextIndex|Table de charge de travail|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> La table dispose d'un index de recherche en texte intégral actif.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasCheckCnst|Table de charge de travail|La table comporte une contrainte CHECK.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasClustIndex|Table de charge de travail|La table comporte un index cluster.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasDefaultCnst|Table de charge de travail|La table comporte une contrainte DEFAULT.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasDeleteTrigger|Table de charge de travail|La table comporte un déclencheur DELETE.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasForeignKey|Table de charge de travail|La table comporte une contrainte FOREIGN KEY.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasForeignRef|Table de charge de travail|La table est référencée par une contrainte FOREIGN KEY.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasIdentity|Table de charge de travail|La table comporte une colonne d'identité.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasIndex|Table de charge de travail|La table comporte un index de type non défini.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasInsertTrigger|Table de charge de travail|L'objet comporte un déclencheur INSERT.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasNonclustIndex|Table de charge de travail|La table comporte un index non cluster.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasPrimaryKey|Table de charge de travail|La table comporte une clé primaire.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasRowGuidCol|Table de charge de travail|La table comporte un ROWGUIDCOL pour une colonne **uniqueidentifier**.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasTextImage|Table de charge de travail|La table comporte une colonne **text**, **ntext** ou **image**.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasTimestamp|Table de charge de travail|La table comporte une colonne **timestamp**.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasUniqueCnst|Table de charge de travail|La table comporte une contrainte UNIQUE.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasUpdateTrigger|Table de charge de travail|La table comporte un déclencheur UPDATE.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableHasVarDecimalStorageFormat|Table de charge de travail|La table est activée pour le format de stockage **vardecimal**.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux|  
|TableInsertTrigger|Table de charge de travail|La table comporte un déclencheur INSERT.<br /><br /> >1 = ID du premier déclencheur du type spécifié.<br /><br /> Type de données de base : **int**|  
|TableInsertTriggerCount|Table de charge de travail|La table comporte le nombre de déclencheurs INSERT spécifié.<br /><br /> >0 = Nombre de déclencheurs INSERT.<br /><br /> Type de données de base : **int**|  
|TableIsFake|Table de charge de travail|La table n'est pas réelle. Elle est matérialisée en interne à la demande par le [!INCLUDE[ssDE](../../includes/ssde-md.md)].<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableIsLockedOnBulkLoad|Table de charge de travail|La table est verrouillée en raison d’un travail **bcp** ou BULK INSERT.<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**|  
|TableIsMemoryOptimized|Table de charge de travail|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> La table est optimisée en mémoire<br /><br /> 1 = Vrai<br /><br /> 0 = Faux<br /><br /> Type de données de base : **int**<br /><br /> Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).|  
|TableIsPinned|Table de charge de travail|La table est épinglée pour être conservée dans le cache de données.<br /><br /> 0 = Faux<br /><br /> Cette fonctionnalité n'est pas prise en charge par [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures.|  
|TableTextInRowLimit|Table de charge de travail|Cette table comporte un jeu d'options text in row.<br /><br /> > 0 = Nombre maximal d’octets autorisé pour text in row.<br /><br /> 0 = l'option text in row n'est pas définie.<br /><br /> Type de données de base : **int**|  
|TableUpdateTrigger|Table de charge de travail|La table comporte un déclencheur UPDATE.<br /><br /> > 1 = ID du premier déclencheur du type spécifié.<br /><br /> Type de données de base : **int**|  
|TableUpdateTriggerCount|Table de charge de travail|La table comporte le nombre spécifié de déclencheurs UPDATE.<br /><br /> > 0 = Nombre de déclencheurs UPDATE.<br /><br /> Type de données de base : **int**|  
|UserDataAccess|Fonction, vue|Indique que l'objet a accès aux données utilisateur, aux tables utilisateur, dans l'instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = Lecture<br /><br /> 0 = Aucun(e)<br /><br /> Type de données de base : **int**|  
|TableHasColumnSet|Table de charge de travail|La table comporte un jeu de colonnes.<br /><br /> 0 = Faux<br /><br /> 1 = Vrai<br /><br /> Pour plus d’informations, consultez [Utiliser des jeux de colonnes](../../relational-databases/tables/use-column-sets.md).|  
|Cardinalité|Table (système ou définie par l'utilisateur), vue ou index|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Nombre de lignes dans l'objet spécifié.|  
|TableTemporalType|Table de charge de travail|**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et versions ultérieures.<br /><br /> Spécifie le type de table.<br /><br /> 0 = table non temporelle<br /><br /> 1 = table historique de la table à système par version<br /><br /> 2 = table temporelle à système par version|  
  
## <a name="return-types"></a>Types de retour  
 **sql_variant**  
  
## <a name="exceptions"></a>Exceptions  
 Retourne la valeur NULL en cas d'erreur ou si un appelant n'est pas autorisé à afficher l'objet.  
  
 Un utilisateur peut voir uniquement les métadonnées des éléments sécurisables qui lui appartiennent ou pour lesquels il dispose d'une autorisation. Cela signifie que les fonctions intégrées générant des métadonnées, telles que OBJECTPROPERTYEX, peuvent retourner la valeur NULL si l'utilisateur ne dispose d'aucune autorisation sur l'objet. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Remarques  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] considère que *object_id* se situe dans le contexte de la base de données active. Une requête référençant un *object_id* dans une autre base de données retourne NULL ou des résultats incorrects. Par exemple, dans la requête qui suit, le contexte de la base de données active est la base de données MASTER. [!INCLUDE[ssDE](../../includes/ssde-md.md)] va tenter de renvoyer la valeur de propriété de l’*object_id* spécifié dans cette base de données et non pas dans la base de données spécifiée dans la requête. La requête retourne des résultats incorrects car la vue `vEmployee` ne se trouve pas dans la base de données MASTER.  
  
```sql  
USE master;  
GO  
SELECT OBJECTPROPERTYEX(OBJECT_ID(N'AdventureWorks2012.HumanResources.vEmployee'), 'IsView');  
GO  
```  
  
 OBJECTPROPERTYEX(*view_id*, 'IsIndexable') peut utiliser un volume important de ressources système, car l’évaluation de la propriété IsIndexable nécessite l’analyse de la définition de la vue, sa normalisation et son optimisation partielle. Bien que la propriété IsIndexable identifie les tables ou les vues qui peuvent être indexées, la création réelle de l'index peut malgré tout échouer si certaines conditions de clé d'index ne sont pas remplies. Pour plus d’informations, consultez [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
 OBJECTPROPERTYEX (*table_id*, 'TableHasActiveFulltextIndex') retourne la valeur 1 (True) lorsqu’au moins une colonne d’une table est ajoutée pour l’indexation. L'indexation de texte intégral est activée au niveau du remplissage dès l'ajout de la première colonne à indexer.  
  
 Des restrictions sur la visibilité des métadonnées sont appliquées à le jeu de résultats. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-finding-the-base-type-of-an-object"></a>R. Recherche du type de base d'un objet  
 L'exemple qui suit crée un SYNONYM `MyEmployeeTable` pour la table `Employee` de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], puis renvoie le type de base du SYNONYM.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE SYNONYM MyEmployeeTable FOR HumanResources.Employee;  
GO  
SELECT OBJECTPROPERTYEX ( object_id(N'MyEmployeeTable'), N'BaseType')AS [Base Type];  
GO  
```  
  
 Le jeu de résultats montre que le type de base de l'objet sous-jacent, la table `Employee`, est une table utilisateur.  
  
 ```
Base Type 
--------  
U
```  
  
### <a name="b-returning-a-property-value"></a>B. Renvoi d'une valeur de propriété  
 L'exemple suivant renvoie le nombre de déclencheurs UPDATE sur la table spécifiée.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT OBJECTPROPERTYEX(OBJECT_ID(N'HumanResources.Employee'), N'TABLEUPDATETRIGGERCOUNT');  
GO  
```  
  
### <a name="c-finding-tables-that-have-a-foreign-key-constraint"></a>C. Recherche de tables comportant une contrainte FOREIGN KEY  
 L'exemple suivant utilise la propriété `TableHasForeignKey` pour retourner toutes les tables comportant une contrainte FOREIGN KEY.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT name, object_id, schema_id, type_desc  
FROM sys.objects   
WHERE OBJECTPROPERTYEX(object_id, N'TableHasForeignKey') = 1  
ORDER BY name;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-finding-the-base-type-of-an-object"></a>D. Recherche du type de base d’un objet  
 L’exemple suivant retourne le type de base de l’objet `dbo.DimReseller`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT OBJECTPROPERTYEX ( object_id(N'dbo.DimReseller'), N'BaseType')AS BaseType;  
```  
  
 Le jeu de résultats montre que le type de base de l'objet sous-jacent, la table `dbo.DimReseller`, est une table utilisateur.  
  
```  
BaseType   
--------   
U   
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE SYNONYM &#40;Transact-SQL&#41;](../../t-sql/statements/create-synonym-transact-sql.md)   
 [Fonctions de métadonnées &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md)   
 [OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md)   
 [OBJECT_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/object-name-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md)  
  
  

