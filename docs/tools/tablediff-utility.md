---
title: tablediff (utilitaire)
description: Utilisez l’utilitaire tablediff pour comparer les données dans deux tables afin d’identifier une non-convergence et résoudre des problèmes de non-convergence dans une topologie de réplication.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- comparing data
- tablediff utility
- tables [SQL Server replication]
- table comparisons [SQL Server]
- command prompt utilities [SQL Server], tablediff
- troubleshooting [SQL Server replication], non-convergence
- non-convergence [SQL Server]
ms.assetid: 3c3cb865-7a4d-4d66-98f2-5935e28929fc
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 9054ebafe95fdfc1ab07e868f1d8e9233aee1795
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335928"
---
# <a name="tablediff-utility"></a>tablediff (utilitaire)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  L’utilitaire **tablediff** sert à comparer les données dans deux tables et à identifier une non-convergence. Il est particulièrement utile pour résoudre des problèmes de non-convergence dans une topologie de réplication. Cet utilitaire peut être employé à partir de l'invite de commandes ou dans un fichier de commandes pour effectuer les tâches suivantes :  
  
-   Une comparaison ligne par ligne entre une table source dans une instance de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] agissant comme serveur de publication de réplication et la table de destination dans une ou plusieurs instances de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] agissant comme abonnés de réplication.  
  
-   Comparaison rapide se limitant à la comparaison du nombre de lignes et du schéma.  
  
-   Comparaisons au niveau des colonnes.  
  
-   Génération d'un script [!INCLUDE[tsql](../includes/tsql-md.md)] pour corriger les différences sur le serveur de destination afin de mettre en convergence les tables source et de destination.  
  
-   Consignation des résultats dans un fichier de sortie ou dans une table de la base de données de destination.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
tablediff   
[ -? ] |   
{  
        -sourceserver source_server_name[\instance_name]  
        -sourcedatabase source_database  
        -sourcetable source_table_name   
    [ -sourceschema source_schema_name ]  
    [ -sourcepassword source_password ]  
    [ -sourceuser source_login ]  
    [ -sourcelocked ]  
        -destinationserver destination_server_name[\instance_name]  
        -destinationdatabase subscription_database   
        -destinationtable destination_table   
    [ -destinationschema destination_schema_name ]  
    [ -destinationpassword destination_password ]  
    [ -destinationuser destination_login ]  
    [ -destinationlocked ]  
    [ -b large_object_bytes ]   
    [ -bf number_of_statements ]   
    [ -c ]   
    [ -dt ]   
    [ -et table_name ]   
    [ -f [ file_name ] ]   
    [ -o output_file_name ]   
    [ -q ]   
    [ -rc number_of_retries ]   
    [ -ri retry_interval ]   
    [ -strict ]  
    [ -t connection_timeouts ]   
}  
```  
  
## <a name="arguments"></a>Arguments  
 [ **-?** ]  
 Retour de la liste des paramètres pris en charge.  
  
 **-sourceserver** _source_server_name_[ **\\** _instance\_name_]  
 Nom du serveur source. Spécifiez *source_server_name* pour l’instance par défaut de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Spécifiez _source_server_name_ **\\** _instance_name_ pour une instance nommée de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 **-sourcedatabase** _source_database_  
 Nom de la base de données source.  
  
 **-sourcetable** _source_table_name_  
 Nom de la table source en cours de vérification.  
  
 **-sourceschema** _source_schema_name_  
 Propriétaire du schéma de la table source. Par défaut, le propriétaire de la table est supposé être dbo.  
  
 **-sourcepassword** _source_password_  
 Mot de passe de la connexion utilisée pour la connexion au serveur source avec l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  Lorsque cela est possible, fournissez les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
 **-sourceuser** _source_login_  
 Connexion employée pour établir la connexion au serveur source avec l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Si *source_login* n’est pas fourni, l’authentification Windows est employée lors de la connexion au serveur source. [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)]  
  
 **-sourcelocked**  
 Durant la comparaison, la table source est verrouillée à l'aide des indicateurs de table TABLOCK et HOLDLOCK.  
  
 **-destinationserver** _destination_server_name_[ **\\** _instance_name_]  
 Nom du serveur de destination. Spécifiez *destination_server_name* pour l’instance par défaut de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Spécifiez _destination_server_name_ **\\** _instance_name_ pour une instance nommée de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 **-destinationdatabase** _subscription_database_  
 Nom de la base de données de destination.  
  
 **-destinationtable** _destination_table_  
 Nom de la table de destination.  
  
 **-destinationschema** _destination_schema_name_  
 Le propriétaire du schéma de la table de destination. Par défaut, le propriétaire de la table est supposé être dbo.  
  
 **-destinationpassword** _destination_password_  
 Mot de passe de la connexion utilisée pour établir une connexion au serveur de destination avec l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  Lorsque cela est possible, fournissez les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
 **-destinationuser** _destination_login_  
 Connexion employée pour établir la connexion au serveur de destination avec l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Si *destination_login* n’est pas fourni, l’authentification Windows est employée lors de la connexion au serveur. [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)]  
  
 **-destinationlocked**  
 Durant la comparaison, la table de destination est verrouillée à l'aide des indicateurs de table TABLOCK et HOLDLOCK.  
  
 **-b** _large_object_bytes_  
 Nombre d’octets à comparer pour des colonnes de types de données d’objet volumineuses, notamment **text**, **ntext**, **image**, **varchar(max)** , **nvarchar(max)** et **varbinary(max)** . *large_object_bytes* a comme valeur par défaut la taille de la colonne. Toutes les données au-dessus de *large_object_bytes* ne sont pas comparées.  
  
 **-bf**  _number_of_statements_  
 Nombre d’instructions [!INCLUDE[tsql](../includes/tsql-md.md)] à écrire dans le fichier de script [!INCLUDE[tsql](../includes/tsql-md.md)] actuel lorsque l’option **-f** est utilisée. Quand le nombre d’instructions [!INCLUDE[tsql](../includes/tsql-md.md)] dépasse *number_of_statements*, un fichier de script [!INCLUDE[tsql](../includes/tsql-md.md)] est créé.  
  
 **-c**  
 Compare les différences au niveau des colonnes.  
  
 **-dt**  
 Supprime la table de résultats spécifiée par *table_name* si la table existe déjà.  
  
 **-et** _table_name_  
 Spécifie le nom de la table de résultats à créer. Si cette table existe déjà, **-DT** doit être utilisé, sinon l’opération échoue.  
  
 **-f** [ *file_name* ]  
 Génère un script [!INCLUDE[tsql](../includes/tsql-md.md)] pour mettre la table sur le serveur de destination en convergence avec la table sur le serveur source. Vous pouvez éventuellement spécifier un nom et un chemin d'accès pour le fichier de script [!INCLUDE[tsql](../includes/tsql-md.md)] généré. Si *file_name* n’est pas spécifié, le fichier de script [!INCLUDE[tsql](../includes/tsql-md.md)] est créé dans le répertoire où l’utilitaire est exécuté.  
  
 **-o** _output_file_name_  
 Nom complet et chemin d'accès du fichier de sortie.  
  
 **-q**  
 Comparaison rapide se limitant à la comparaison du nombre de lignes et du schéma.  
  
 **-rc** _number_of_retries_  
 Nombre de fois où l'utilitaire retente une opération qui a échoué.  
  
 **-ri**  _retry_interval_  
 Intervalle, en secondes, d'attente entre les tentatives.  
  
 **-strict**  
 Les schémas source et de destination sont comparés de manière stricte.  
  
 **-t** _connection_timeouts_  
 Définit le délai d'attente de connexion, en secondes, pour les connexions au serveur source et au serveur de destination.  
  
## <a name="return-value"></a>Valeur de retour  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Succès|  
|**1**|Erreur critique|  
|**2**|Tables différentes|  
  
## <a name="remarks"></a>Notes  
 L’utilitaire **tablediff** ne peut pas être utilisé avec des serveurs non [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 Les tables comprenant des colonnes de types de données **sql_variant** ne sont pas prises en charge.  
  
 Par défaut, l’utilitaire **tablediff** prend en charge les mappages de types de données suivants entre les colonnes source et de destination.  
  
|Type de données sources|Type de données de destination|  
|----------------------|---------------------------|  
|**tinyint**|**smallint**, **int** ou **bigint**|  
|**smallint**|**int** ou **bigint**|  
|**int**|**bigint**|  
|**timestamp**|**varbinary**|  
|**varchar(max)**|**text**|  
|**nvarchar(max)**|**ntext**|  
|**varbinary(max)**|**image**|  
|**text**|**varchar(max)**|  
|**ntext**|**nvarchar(max)**|  
|**image**|**varbinary(max)**|  
  
 Utilisez l’option **-strict** pour interdire ces mappages et effectuer une validation stricte.  
  
 La table source de la comparaison doit contenir au moins une colonne de clé primaire, d'identité ou ROWGUID. Quand vous utilisez l’option **-strict** , la table de destination doit également contenir une colonne de clé primaire, d’identité ou ROWGUID.  
  
 Le script [!INCLUDE[tsql](../includes/tsql-md.md)] généré pour faire converger la table de destination n'inclut pas les types de données suivants :  
  
-   **varchar(max)**  
  
-   **nvarchar(max)**  
  
-   **varbinary(max)**  
  
-   **timestamp**  
  
-   **xml**  
  
-   **text**  
  
-   **ntext**  
  
-   **image**  
  
## <a name="permissions"></a>Autorisations  
 Pour comparer les tables, vous avez besoin des autorisations SELECT ALL sur les objets de table comparés.  
  
 Pour utiliser l’option **-et** , vous devez être membre du rôle de base de données fixe db_owner, ou au moins disposer de l’autorisation CREATE TABLE dans la base de données d’abonnement et de l’autorisation ALTER sur le schéma du propriétaire de destination sur le serveur de destination.  
  
 Pour utiliser l’option **-dt** , vous devez être membre du rôle de base de données fixe db_owner ou disposer au moins de l’autorisation ALTER sur le schéma du propriétaire de destination sur le serveur de destination.  
  
 Pour utiliser l’option **-o** ou **-f** , vous devez disposer d’autorisations d’écriture sur l’emplacement du répertoire de fichiers spécifié.  
  
## <a name="see-also"></a>Voir aussi  
 [Comparer des tables répliquées pour identifier les différences &#40;programmation de réplication&#41;](../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md)  
  
  
