---
title: Propriétés de la base de données (page Options) | Microsoft Docs
description: Découvrez comment utiliser l’onglet Options de la boîte de dialogue Propriétés de la base de données pour afficher ou modifier le classement, le mode de récupération et d’autres paramètres d’une base de données.
ms.custom: ''
ms.date: 08/28/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.options.f1
ms.assetid: a3447987-5507-4630-ac35-58821b72354d
author: stevestein
ms.author: sstein
ms.openlocfilehash: 87c3b10b95a37c74ea1595dca15341aeffe5c261
ms.sourcegitcommit: 4a813a0741502c56c0cd5ecaafafad2e857a9d7f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98031098"
---
# <a name="database-properties-options-page"></a>Propriétés de la base de données (page Options)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Utilisez cette page pour consulter ou modifier les options de la base de données sélectionnée. Pour plus d’informations sur les options disponibles dans cette page, consultez [ALTER DATABASE SET Options &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) et [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).  
  
## <a name="page-header"></a>En-tête de page  
 **Classement**  
 Spécifiez le classement de la base de données en le sélectionnant dans la liste. Pour plus d’informations, voir [Set or Change the Database Collation](../../relational-databases/collations/set-or-change-the-database-collation.md).  
  
 **Mode de récupération**  
 Spécifiez l’un des modèles suivants pour la récupération de la base de données : **Complet**, **Journalisé en bloc** ou **Simple**. Pour plus d’informations sur les modes de récupération, consultez [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md).  
  
 **Niveau de compatibilité**  
 Spécifiez la version la plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prise en charge par la base de données. Pour connaître les valeurs possibles, consultez [Niveau de compatibilité avec ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md). Quand une base de données SQL Server est mise à niveau, le niveau de compatibilité pour cette base de données est conservé si possible, ou remplacé par le niveau minimal pris en charge pour le nouveau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
 **Type de relation contenant-contenu**  
 Spécifiez aucun ou partiel pour indiquer s'il s'agit d'une base de données autonome. Pour plus d'informations sur les bases de données autonomes, consultez [Bases de données autonomes](../../relational-databases/databases/contained-databases.md). La propriété de serveur **Activer les bases de données autonomes** doit être définie sur **TRUE** pour qu'une base de données puisse être configurée comme étant autonome.  
  
> [!IMPORTANT]  
>  L'activation de bases de données autonomes partielle transfère le contrôle de l'accès à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aux propriétaires de la base de données. Pour plus d'informations, consultez [Meilleures pratiques de sécurité recommandées avec les bases de données autonomes](../../relational-databases/databases/security-best-practices-with-contained-databases.md).  
  
## <a name="automatic"></a>Automatique  
 **Fermeture automatique**  
 Spécifiez si la base de données doit être fermée proprement et doit libérer des ressources lorsque le dernier utilisateur ferme sa session. Les valeurs possibles sont **True** et **False**. Lorsque la valeur est **True**, la base de données est fermée proprement et ses ressources sont libérées à la fermeture de session du dernier utilisateur.  

 **Création automatique des statistiques incrémentielles**  
 Spécifiez si utiliser l'option incrémentielle lorsque les statistiques par partition sont créées. Pour plus d’informations sur les statistiques incrémentielles, consultez [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md).  
  
 **Création automatique des statistiques**  
 Spécifiez si la base de données doit automatiquement créer les statistiques d'optimisation manquantes. Les valeurs possibles sont **True** et **False**. Lorsque la valeur est **True**, toutes les statistiques manquantes requises par une requête pour l'optimisation sont créées automatiquement durant l'optimisation. Pour plus d’informations, consultez [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md).  
  
 **Réduction automatique**  
 Spécifiez si les fichiers de base de données peuvent faire l'objet d'une réduction de taille périodique. Les valeurs possibles sont **True** et **False**. Pour plus d’informations, voir [Shrink a Database](../../relational-databases/databases/shrink-a-database.md).  
  
 **Mise à jour automatique des statistiques**  
 Spécifiez si la base de données doit automatiquement mettre à jour les statistiques d'optimisation obsolètes. Les valeurs possibles sont **True** et **False**. Quand la valeur est **True**, toutes les statistiques obsolètes requises par une requête pour l’optimisation sont créées automatiquement durant l’optimisation. Pour plus d’informations, consultez [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md).  
  
 **Mise à jour automatique des statistiques de manière asynchrone**  
 Quand la valeur est **True**, les requêtes à l’origine d’une mise à jour automatique de statistiques obsolètes n’attendent pas la fin de la mise à jour des statistiques pour effectuer la compilation. Les requêtes suivantes utilisent les statistiques mises à jour une fois celles-ci disponibles.  
  
 Quand la valeur est **False**, les requêtes à l’origine d’une mise à jour automatique des statistiques obsolètes attendent de pouvoir utiliser les statistiques mises à jour dans le plan d’optimisation des requêtes.  
  
 L'attribution de la valeur **True** à cette option n'a aucun effet, sauf si l'option **Mise à jour automatique des statistiques** a également la valeur **True**.  

## <a name="azure"></a>Azure
Quand une connexion à Azure SQL Database a été établie, cette section contient des paramètres permettant de contrôler l’objectif de niveau de service. Standard S2 est l’objectif de niveau de service utilisé par défaut pour une nouvelle base de données.

  **Objectif de niveau de service actuel** : objectif de niveau de service spécifique à utiliser. Les valeurs disponibles dépendent de l’édition sélectionnée. Si l’objectif de niveau de service souhaité ne figure pas dans la liste, vous pouvez entrer manuellement une valeur.

  **Édition** : édition Azure SQL Database à utiliser, par exemple, De base ou Premium. Si l’édition qu’il vous faut n’est pas dans la liste, vous pouvez entrer manuellement une valeur (celle-ci doit correspondre à la valeur utilisée dans les API REST Azure).
  
  **Taille max.**  : taille maximale de la base de données. Si la taille souhaitée ne figure pas dans la liste, vous pouvez entrer manuellement une valeur. Ne renseignez pas ce paramètre si vous souhaitez utiliser la taille par défaut pour l’édition et l’objectif de niveau de service choisis.
  
## <a name="containment"></a>Containment  
 Dans une base de données autonome, certains paramètres généralement configurés au niveau serveur peuvent l’être au niveau de la base de données.  
  
 **LCID de la langue de texte intégral par défaut**  
 Spécifie une langue par défaut pour les colonnes de texte intégral indexées. L'analyse linguistique des données de texte intégral indexées dépend de la langue des données. La valeur par défaut de cette option est la langue du serveur. Pour connaître le langue correspondant au paramètre affiché, consultez [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md).  
  
 **Langue par défaut**  
 Spécifie la langue par défaut pour tous les nouveaux utilisateurs de base de données autonome, sauf indication contraire.  
  
 **Déclencheurs imbriqués activés**  
 Autorise des déclencheurs à en activer d'autres. Les déclencheurs peuvent compter jusqu'à 32 niveaux d'imbrication. Pour plus d’informations, consultez la section « Déclencheurs imbriqués » de [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
 **Transformer les mots parasites**  
 Supprimez un message d'erreur si des mots parasites, ou des mots vides, provoquent le retour de lignes nulles par une opération booléenne sur une requête de texte intégral. Pour plus d’informations, voir [transform noise words Server Configuration Option](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md).  
  
 **Année de coupure à deux chiffres**  
 Indique le numéro d'année le plus élevé qui peut être entré en tant qu'année à deux chiffres. L'année répertoriée et les 99 années précédentes peuvent être entrées sous forme d'années à deux chiffres. Toutes les autres années doivent être entrées sous forme d'années à quatre chiffres.  
  
 Par exemple, le paramètre par défaut 2049 indique qu'une date entrée sous la forme "14/3/49" sera interprétée comme le 14 mars 2049, tandis qu'une date entrée sous la forme "14/3/50" sera interprétée comme le 14 mars 1950. Pour plus d’informations, voir [Configurer l'option de configuration de serveur two digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).  
  
## <a name="cursor"></a>Curseur  
 **Fermer le curseur lors de l'activation de la validation**  
 Spécifiez si les curseurs doivent être fermés une fois que la transaction qui a ouvert le curseur est validée. Les valeurs possibles sont **True** et **False**. Lorsque la valeur est **True**, tous les curseurs ouverts lors de la validation ou de la restauration d'une transaction sont fermés. Lorsque la valeur est **False**, ces curseurs restent ouverts lorsqu'une transaction est validée. En revanche, lorsque la valeur est **False**, la restauration d'une transaction ferme tous les curseurs à l'exception des curseurs de type INSENSITIVE ou STATIC. Pour plus d’informations, consultez [SET CURSOR_CLOSE_ON_COMMIT &#40;Transact-SQL&#41;](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).  
  
 **Curseur par défaut**  
 Spécifiez le comportement du curseur par défaut. Lorsque la valeur est **True**, les déclarations de curseur ont pour valeur par défaut LOCAL. Lorsque la valeur est **False**, les curseurs [!INCLUDE[tsql](../../includes/tsql-md.md)] ont pour valeur par défaut GLOBAL.  
  
## <a name="database-scoped-configurations"></a>Configurations étendues à la base de données  
 SQL Server 2016 et Base de données SQL Azure comprennent plusieurs propriétés de configuration qui peuvent être étendues au niveau de la base de données. Pour plus d’informations sur tous ces paramètres, consultez [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).  
  
 **Estimation de la cardinalité héritée**  
 Spécifiez le modèle d’estimation de la cardinalité de l’optimiseur de requête pour la base de données primaire, quel que soit le niveau de compatibilité de la base de données. Cette propriété équivaut à l’ [indicateur de trace 9481](https://support.microsoft.com/kb/2801413).  
  
 **Estimation de la cardinalité héritée pour la base de données secondaire**  
 Spécifiez le modèle d’estimation de la cardinalité de l’optimiseur de requête pour les éventuelles bases de données secondaires, quel que soit le niveau de compatibilité de la base de données. Cette propriété équivaut à l’ [indicateur de trace 9481](https://support.microsoft.com/kb/2801413).  
  
 **Degré de parallélisme maximal**  
 Spécifiez le paramètre [MAXDOP](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) par défaut pour le serveur principal à utiliser pour les instructions.  
  
 **Degré de parallélisme maximal pour le serveur secondaire**  
 Spécifiez le paramètre [MAXDOP](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) par défaut pour les éventuels serveurs secondaires à utiliser pour les instructions.  
  
 **Détection de paramètres**  
 Active ou désactive la détection de paramètres sur la base de données primaire. Cette propriété est équivalente à l’ [indicateur de trace 4136](https://support.microsoft.com/kb/980653).  
  
 **Détection de paramètre pour la base de données secondaire**  
 Active ou désactive la détection de paramètres sur les éventuelles bases de données secondaires. Cette propriété est équivalente à l’ [indicateur de trace 4136](https://support.microsoft.com/kb/980653).  
  
 **Correctifs de l’optimiseur de requête**  
 Active ou désactive les correctifs logiciels d’optimisation de requête sur la base de données primaire, quel que soit le niveau de compatibilité de la base de données. Cette propriété est équivalente à l’ [indicateur de trace 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md). Pour plus d’informations, consultez [QUERY_OPTIMIZER_HOTFIXES](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md#qo_hotfixes).  
  
 **Correctifs logiciels de l’optimiseur de requête pour la base de données secondaire**  
 Active ou désactive les correctifs logiciels d’optimisation de requête sur les éventuelles bases de données secondaires, quel que soit le niveau de compatibilité de la base de données. Cette propriété est équivalente à l’ [indicateur de trace 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md). Pour plus d’informations, consultez [QUERY_OPTIMIZER_HOTFIXES](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md#qo_hotfixes).  
  
## <a name="filestream"></a>FILESTREAM  
 **Nom du répertoire FILESTREAM**  
 Spécifiez le nom de répertoire pour les données FILESTREAM associées à la base de données sélectionnée.  
  
 **Accès FILESTREAM non transactionnel**  
 Spécifiez l’une des options suivantes pour l’accès non transactionnel via le système de fichiers aux données FILESTREAM stockées dans les FileTables : **OFF**, **READ_ONLY** ou **FULL**. Si FILESTREAM n'est pas activé sur le serveur, cette valeur est définie sur OFF et est désactivée. Pour plus d’informations, consultez [FileTables &#40;SQL Server&#41;](../../relational-databases/blob/filetables-sql-server.md).  
  
## <a name="miscellaneous"></a>Divers  
**Autoriser l’isolement d’instantané**  
Active cette fonctionnalité.  

 **Valeur par défaut ANSI NULL**  
 Autorise les valeurs NULL pour tous les types de données définis par l’utilisateur ou les colonnes non explicitement définies comme **NOT NULL** lors d’une instruction **CREATE TABLE** ou **ALTER TABLE** (état par défaut). Pour plus d’informations, consultez [SET ANSI_NULL_DFLT_ON &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md) et [SET ANSI_NULL_DFLT_OFF &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-null-dflt-off-transact-sql.md).  
  
 **Valeurs ANSI NULL activées**  
 Spécifiez le comportement des opérateurs de comparaison Égal à (`=`) et Différent de (`<>`) lorsqu'ils sont utilisés avec des valeurs NULL. Les valeurs possibles sont **True** (activé) et **False** (désactivé). Lorsque la valeur est **True**, toutes les comparaisons effectuées avec une valeur NULL donnent comme résultat la valeur UNKNOWN (inconnu). Quand la valeur est **False**, les comparaisons de valeurs non UNICODE à une valeur NULL donnent comme résultat **True** si les deux valeurs sont NULL. Pour plus d’informations, consultez [SET ANSI_NULLS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-nulls-transact-sql.md).  
  
 **Remplissage ANSI activé**  
 Spécifiez si le remplissage ANSI est activé ou désactivé. Les valeurs autorisées sont **True** (activé) et **False** (désactivé). Pour plus d’informations, consultez [SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md).  
  
 **Avertissements ANSI activés**  
 Spécifiez le comportement compatible avec la norme ISO de plusieurs conditions d'erreur. Quand la valeur est **True**, un message d’avertissement est généré si des valeurs NULL apparaissent dans des fonctions d’agrégation (par exemple, SUM, AVG, MAX, MIN, STDEV, STDEVP, VAR, VARP ou COUNT). Lorsque la valeur est **False**, aucun avertissement n'est émis. Pour plus d’informations, consultez [SET ANSI_WARNINGS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-warnings-transact-sql.md).  
  
 **Annulation arithmétique activée**  
 Spécifiez si l'option d'annulation arithmétique est activée ou non pour la base de données. Les valeurs possibles sont **True** et **False**. Quand la valeur est **True**, un dépassement de capacité ou une division par zéro entraînent l’arrêt du traitement de la requête ou du lot. Si l'erreur se produit dans une transaction, cette dernière est restaurée. Lorsque la valeur est **False**, un message d'avertissement s'affiche, mais le traitement de la requête, du lot ou de la transaction se poursuit comme s'il n'y avait pas d'erreur. Pour plus d’informations, consultez [SET ARITHABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithabort-transact-sql.md).  
  
 **La concaténation de la valeur NULL donne NULL**  
 Spécifiez le comportement lorsque les valeurs NULL sont concaténées. Quand cette propriété a la valeur **True**, **string** + NULL retourne NULL. Lorsqu'elle a la valeur **False**, le résultat est **string**. Pour plus d’informations, consultez [SET CONCAT_NULL_YIELDS_NULL &#40;Transact-SQL&#41;](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).  
  
 **Chaînage des propriétés des bases de données croisées activé**  
 Cette valeur en lecture seule indique si le chaînage des propriétés des bases de données croisées a été activé. Quand la valeur est **True**, la base de données peut être la source ou la cible d'une chaîne des propriétés des bases de données croisées. Utilisez l'instruction ALTER DATABASE pour définir cette propriété.  
  
 **Optimisation des corrélations de dates activée**  
 Quand la valeur est **True**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserve les statistiques de corrélation entre deux tables quelconques de la base de données qui sont liées par une contrainte FOREIGN KEY et qui possèdent des colonnes **datetime** .  
  
 Si la valeur **False** est définie, les statistiques de corrélation ne sont pas conservées.  
 
 **Durabilité différée**  
 Active cette fonctionnalité. Pour plus d’informations, consultez [Contrôler la durabilité d’une transaction](../logs/control-transaction-durability.md).
 
 **Est un instantané Read Committed**  
 Active cette fonctionnalité.  
 
 **Abandon en cas d'arrondi numérique**  
 Spécifiez comment la base de données gère les erreurs d'arrondi. Les valeurs possibles sont **True** et **False**. Lorsque la valeur est **True**, une erreur est générée en cas de perte de précision dans une expression. Lorsque la valeur est **False**, les pertes de précision ne génèrent pas de messages d'erreur, et le résultat est arrondi selon la précision spécifiée pour la colonne ou la variable contenant le résultat. Pour plus d’informations, consultez [SET NUMERIC_ROUNDABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-numeric-roundabort-transact-sql.md).  
  
 **Paramétrage**  
 Si **SIMPLE** est spécifié, les requêtes sont paramétrables en fonction du comportement par défaut de la base de données. Si **FORCED** est spécifié, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] paramètre toutes les requêtes de la base de données.  
  
 **Identificateurs entre guillemets activés**  
 Spécifiez si les mots clés [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent servir d'identificateurs (nom d'objet ou de variable) s'ils sont entre guillemets. Les valeurs possibles sont **True** et **False**. Pour plus d’informations, consultez [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 **Déclencheurs récursifs activés**  
 Spécifiez si les déclencheurs peuvent être activés par d'autres déclencheurs. Les valeurs possibles sont **True** et **False**. Lorsque la valeur est **True**, le déclenchement récursif de déclencheurs est activé. Lorsque la valeur est **False**, seule la récursivité directe est désactivée. Pour désactiver la récurrence indirecte, affectez la valeur 0 à l'option de serveur déclencheurs imbriqués à l'aide de sp_configure. Pour plus d’informations, consultez [Créer des déclencheurs imbriqués](../../relational-databases/triggers/create-nested-triggers.md).  
  
 **Digne de confiance**  
 Quand elle affiche **True**, cette option en lecture seule indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autorise l’accès aux ressources extérieures à la base de données dans un contexte d’emprunt d’identité établi à l’intérieur de la base de données. Les contextes d'emprunt d'identité peuvent être établis dans la base de données à l'aide de l'instruction EXECUTE AS ou de l'une des clauses EXECUTE AS des modules de base de données.  
  
 Pour profiter de l'accès, le propriétaire de la base de données doit également bénéficier de l'autorisation AUTHENTICATE SERVER au niveau du serveur.  
  
 Cette propriété autorise également la création et l'exécution d'assemblys d'accès non sécurisés et externes dans la base de données. Il ne suffit pas d'affecter la valeur **True** à cette propriété ; le propriétaire de la base de données doit également avoir l'autorisation EXTERNAL ACCESS ASSEMBLY ou UNSAFE ASSEMBLY au niveau du serveur.  
  
 Dans toutes les bases de données utilisateur et système (à l’exception de **MSDB**), cette propriété a la valeur **False** par défaut. La valeur ne peut pas être modifiée pour les bases de données **model** et **tempdb** .  
  
 TRUSTWORTHY a la valeur **False** dès qu'une base de données est attachée au serveur.  
  
 La stratégie recommandée pour accéder aux ressources situées en dehors de la base de données dans un contexte d'emprunt d'identité consiste à utiliser les certificats et les signatures tels qu'ils sont apposés à l'option **Trustworthy** .  
  
 Pour définir cette propriété, utilisez l'instruction ALTER DATABASE.  
  
 **Format de stockage vardecimal activé**  
 Cette option est en lecture seule dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]et les versions ultérieures. Lorsque la valeur est **True**, cette base de données est activée pour le format de stockage vardecimal. Le format de stockage vardecimal ne peut pas être désactivé lorsque des tables de la base de données l'utilisent. Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures, toutes les bases de données sont activées pour le format de stockage vardecimal. Cette option utilise [sp_db_vardecimal_storage_format](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md).  
  
## <a name="recovery"></a>Récupération  
 **Vérification de page**  
 Spécifiez l'option utilisée pour détecter et signaler les transactions d'E/S incomplètes à cause d'erreurs d'E/S de disque. Les valeurs possibles sont **None**, **TornPageDetection** et **Checksum**. Pour plus d’informations, consultez [Gérer la table suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md).  
  
 **Temps de récupération cible (en secondes)**  
 Spécifie la durée maximale (en secondes) de la récupération de la base de données spécifiée en cas d'incident. Pour plus d’informations, consultez [Points de contrôle de base de données &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md).  

## <a name="service-broker"></a>Service Broker  
**Service Broker activé**  
Active ou désactive le Service Broker.  

**Priorité du service Broker respectée**  
Propriété de Service Broker en lecture seule.  

**Identificateur de Service Broker**  
Identificateur en lecture seule.  

## <a name="state"></a>State  
 **Base de données en lecture seule**  
 Spécifiez si la base de données est en lecture seule. Les valeurs possibles sont **True** et **False**. Lorsque la valeur est **True**, les utilisateurs peuvent uniquement lire les données de la base de données. Ils ne peuvent pas modifier les données ou les objets de la base de données. Toutefois, la base de données elle-même peut être supprimée à l’aide de l’instruction `DROP DATABASE`. La modification de la configuration de l'option **Base de données en lecture seule** n'est possible que si la base de données n'est pas en cours d'utilisation. La base de données master est l'exception à la règle mais seul l'administrateur système peut utiliser la base de données master pendant la configuration de cette option.  
  
 **État de la base de données**  
 Affiche l'état actuel de la base de données. Elle n’est pas modifiable. Pour plus d'informations sur l'option **État de la base de données**, consultez [Database States](../../relational-databases/databases/database-states.md).  

 **Chiffrement activé**  
 Quand la valeur est **True**, cette base de données est activée pour le chiffrement. Une clé de chiffrement de base de données est alors requise pour effectuer le chiffrement. Pour plus d’informations, consultez [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  
 
 **Restreindre l'accès**  
 Spécifiez les utilisateurs autorisés à accéder à la base de données. Les valeurs possibles sont les suivantes :  
  
-   **Multiple**  
  
     État standard d'une base de données de production qui permet à plusieurs utilisateurs d'accéder en même temps à la base de données.  
  
-   **Unique**  
  
     État utilisé pour les actions de maintenance qui permet à un seul utilisateur à la fois d'accéder à la base de données.  
  
-   **Restreint**  
  
     Seuls les membres des rôles db_owner, dbcreator ou sysadmin peuvent utiliser la base de données.  
  


## <a name="see-also"></a>Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)  
