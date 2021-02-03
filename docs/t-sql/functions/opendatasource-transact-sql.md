---
description: OPENDATASOURCE (Transact-SQL)
title: OPENDATASOURCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/26/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPENDATASOURCE
- OPENDATASOURCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data sources [SQL Server]
- OPENDATASOURCE function
- remote data access [SQL Server], OPENDATASOURCE
- ad hoc distributed queries
- OLE DB data sources [SQL Server]
- ad hoc connection information
ms.assetid: 5510b846-9cde-4687-8798-be9a273aad31
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 57b832a82329e3eb38d7d2d332d0eaa9c210c8be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189839"
---
# <a name="opendatasource-transact-sql"></a>OPENDATASOURCE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Fournit les informations de connexion appropriées pour un nom d'objet en quatre parties sans utiliser de nom de serveur lié.  

 ![Icône de lien](../../database-engine/configure-windows/media/topic-link.gif "Icône de lien") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
OPENDATASOURCE ( 'provider_name', 'init_string' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 '*provider_name*'  
 Nom enregistré comme PROGID du fournisseur OLE DB utilisé pour accéder à la source de données. *provider_name* est du type de données **char**, sans valeur par défaut.  

 > [!IMPORTANT]
 > Le fournisseur Microsoft OLE DB pour SQL Server (SQLOLEDB) et le fournisseur SQL Server Native Client (SQLNCLI) précédents restent dépréciés, et nous vous déconseillons d’utiliser l’un ou l’autre dans de nouveaux développements. Au lieu de cela, utilisez le nouveau [Microsoft OLE DB Driver pour SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL), qui sera mis à jour avec les fonctionnalités serveur les plus récentes.
 
 '*init_string*'  
 Chaîne de connexion transmise à l’interface IDataInitialize du fournisseur de destination. La syntaxe de la chaîne du fournisseur est basée sur des paires mot clé-valeur séparées par un point-virgule, par exemple : **'** _mot-clé1_=_valeur_ **;** _mot-clé2_=_valeur_ **'** .  
  
 Pour plus d'informations sur les paires mot clé-valeur prises en charge par le fournisseur, consultez le kit de développement logiciel (SDK) de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Data Access. Cette documentation définit la syntaxe de base. Le tableau suivant répertorie les mots clés utilisés fréquemment dans l’argument *init_string*.  
  
|Mot clé|Propriété OLE DB|Valeurs admises et description|  
|-------------|---------------------|----------------------------------|  
|source de données|DBPROP_INIT_DATASOURCE|Nom de la source de données à laquelle la connexion doit être établie. Ceci est interprété différemment selon les fournisseurs. Pour le fournisseur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB, cette propriété indique le nom du serveur. Pour le fournisseur Jet OLE DB, elle indique le chemin d'accès complet au fichier .mdb ou .xls.|  
|Emplacement|DBPROP_INIT_LOCATION|Emplacement de la base de données à laquelle la connexion doit être établie|  
|Extended Properties|DBPROP_INIT_PROVIDERSTRING|Chaîne de connexion spécifique au fournisseur|  
|Connect timeout|DBPROP_INIT_TIMEOUT|Délai d’expiration au bout duquel la tentative de connexion échoue|  
|ID d'utilisateur|DBPROP_AUTH_USERID|ID utilisateur à utiliser pour la connexion|  
|Mot de passe|DBPROP_AUTH_PASSWORD|Mot de passe à utiliser pour la connexion|  
|Catalogue|DBPROP_INIT_CATALOG|Nom du catalogue initial ou par défaut lors de la connexion à la source de données|  
|Sécurité intégrée|DBPROP_AUTH_INTEGRATED|SSPI pour spécifier l'authentification Windows|  
  
## <a name="remarks"></a>Remarques  
`OPENROWSET` hérite toujours du classement de l’instance, quel que soit le classement défini pour les colonnes.

`OPENDATASOURCE` peut être utilisé pour accéder à des données distantes à partir de sources de données OLE DB seulement si l’option de Registre DisallowAdhocAccess est explicitement définie sur 0 pour le fournisseur spécifié et que l’option de configuration avancée Ad Hoc Distributed Queries est activée. Lorsque ces options ne sont pas définies, le comportement par défaut n'autorise pas l'accès d'égal à égal.  
  
La fonction `OPENDATASOURCE` peut être utilisée aux mêmes endroits de la syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] qu’un nom de serveur lié. `OPENDATASOURCE` peut donc constituer la première des quatre parties d’un nom qui fait référence à un nom de table ou de vue (dans une instruction SELECT, INSERT, UPDATE ou DELETE) ou à une procédure stockée distante (dans une instruction EXECUTE). Lors de l’exécution de procédures stockées distantes, `OPENDATASOURCE` doit référencer une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. OPENDATASOURCE n'accepte pas de variables pour ses arguments.  
  
Comme la fonction `OPENROWSET`, `OPENDATASOURCE` doit référencer seulement des sources de données OLE DB qui ne sont pas sollicitées fréquemment. Définissez un serveur lié pour toutes les sources de données qui font l'objet de nombreux accès. Ni OPENDATASOURCE ni OPENROWSET n’offrent toutes les fonctionnalités des définitions de serveur lié, notamment la gestion de la sécurité et la possibilité de consulter des informations du catalogue. Toutes les informations de connexion, y compris les mots de passe, doivent être fournies à chaque appel de OPENDATASOURCE.  
  
> [!IMPORTANT]  
> L'authentification Windows est plus sûre que l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Veillez à utiliser l'authentification Windows chaque fois que c'est possible. `OPENDATASOURCE` ne doit pas être utilisé avec des mots de passe explicites dans la chaîne de connexion.  
  
Les exigences de connexion pour chaque fournisseur sont semblables à celles de ces paramètres lors de la création de serveurs liés. Des informations relatives à de nombreux fournisseurs courants sont présentées dans l’article [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).  
  
Tout appel à `OPENDATASOURCE`, `OPENQUERY` ou `OPENROWSET` dans la clause `FROM` est évalué séparément et indépendamment de tout appel à ces fonctions utilisé comme cible de la mise à jour, même si des arguments identiques sont fournis aux deux appels. En particulier, les conditions de filtre ou de jointure appliquées au résultat de l’un de ces appels n’ont aucun effet sur les résultats de l’autre.  
  
## <a name="permissions"></a>Autorisations  
 Tous les utilisateurs peuvent exécuter OPENDATASOURCE. Les autorisations utilisées pour la connexion au serveur distant sont déterminées à partir de la chaîne de connexion.  
  
## <a name="examples"></a>Exemples  

### <a name="a-using-opendatasource-with-select-and-the-sql-server-ole-db-driver"></a>R. Utilisation d’OPENDATASOURCE avec SELECT et OLE DB Driver pour SQL Server  
 L’exemple suivant utilise le pilote [Microsoft OLE DB Driver pour SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) pour accéder à la table `HumanResources.Department` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] sur le serveur distant `Seattle1`. Une instruction `SELECT` définit l’ensemble de lignes retourné. La chaîne de caractères du fournisseur contient les mots clés `Server` et `Trusted_Connection`. Ces mots clés sont reconnus par le pilote [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] OLE DB Driver.  
  
```sql  
SELECT GroupName, Name, DepartmentID  
FROM OPENDATASOURCE('MSOLEDBSQL', 'Server=Seattle1;Database=AdventureWorks2016;TrustServerCertificate=Yes;Trusted_Connection=Yes;').HumanResources.Department  
ORDER BY GroupName, Name;  
``` 

### <a name="b-using-opendatasource-with-select-and-the-sql-server-ole-db-provider"></a>B. Utilisation d’OPENDATASOURCE avec SELECT et le fournisseur SQL Server OLE DB  
L'exemple suivant crée une connexion appropriée à l'instance `Payroll` de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le serveur `London`, puis interroge la table `AdventureWorks2012.HumanResources.Employee`. 

> [!NOTE] 
> L’utilisation de SQLNCLI redirige [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers la version la plus récente du fournisseur SQL Server Native Client OLE DB. Le fournisseur OLE DB doit être inscrit dans le Registre avec le PROGID spécifié. 

> [!IMPORTANT]  
> Le fournisseur SQL Server Native Client OLE DB (SQLNCLI) reste déprécié, et il n’est pas recommandé de l’utiliser pour les nouveaux développements. Au lieu de cela, utilisez le nouveau [Microsoft OLE DB Driver pour SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL), qui sera mis à jour avec les fonctionnalités serveur les plus récentes.
  
```sql  
SELECT *  
FROM OPENDATASOURCE('SQLNCLI',  
    'Data Source=London\Payroll;Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Employee;  
```  

### <a name="c-using-the-microsoft-ole-db-provider-for-jet"></a>C. Utilisation du fournisseur Microsoft OLE DB pour Jet   
 L’exemple suivant crée une connexion ad hoc avec une feuille de calcul Excel au format de document Word 1997 - 2003.  
  
```sql  
SELECT * FROM OPENDATASOURCE('Microsoft.Jet.OLEDB.4.0',  
    'Data Source=C:\DataFolder\Documents\TestExcel.xls;Extended Properties=EXCEL 5.0')...[Sheet1$] ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)  
  
