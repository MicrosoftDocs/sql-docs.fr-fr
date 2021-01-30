---
description: sp_addlinkedserver (Transact-SQL)
title: sp_addlinkedserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addlinkedserver_TSQL
- sp_addlinkedserver
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addlinkedserver
ms.assetid: fed3adb0-4c15-4a1a-8acd-1b184aff558f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c894bbf5bdb3e523901fc4f1200d146d0a569449
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171786"
---
# <a name="sp_addlinkedserver-transact-sql"></a>sp_addlinkedserver (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crée un serveur lié. Un serveur lié autorise l'accès à des sources de données OLE DB par l'intermédiaire de requêtes distribuées et hétérogènes. Une fois qu’un serveur lié a été créé à l’aide de **sp_addlinkedserver**, des requêtes distribuées peuvent être exécutées sur ce serveur. Si le serveur lié est défini comme une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les procédures stockées distantes peuvent être exécutées.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_addlinkedserver [ @server= ] 'server' [ , [ @srvproduct= ] 'product_name' ]   
     [ , [ @provider= ] 'provider_name' ]  
     [ , [ @datasrc= ] 'data_source' ]   
     [ , [ @location= ] 'location' ]   
     [ , [ @provstr= ] 'provider_string' ]   
     [ , [ @catalog= ] 'catalog' ]   
```  
  
## <a name="arguments"></a>Arguments  
[ @server =] *\' serveur \'*          
Nom du serveur lié à créer. *server* est de type **sysname** et n'a pas de valeur par défaut.  
  
[ @srvproduct =] *\' product_name \'*          
Nom de produit de la source de données OLE DB à ajouter comme serveur lié. *product_name* est de type **nvarchar (** 128 **)**, avec NULL comme valeur par défaut. Si **SQL Server**, *provider_name*, *data_source*, *location*, *provider_string* et *Catalog* n’ont pas besoin d’être spécifiés.  
  
[ @provider =] *\' provider_name \'*          
ID de programme unique (PROGID) du fournisseur OLE DB correspondant à la source de données. *provider_name* doit être unique pour le fournisseur de OLE DB spécifié installé sur l’ordinateur actuel. *provider_name* est de type **nvarchar (128)**, avec NULL comme valeur par défaut. Toutefois, si *provider_name* est omis, SQLNCLI est utilisé. 

> [!NOTE]
> L’utilisation de SQLNCLI redirige [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers la dernière version du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client. Le fournisseur OLE DB doit être inscrit dans le Registre avec le PROGID spécifié.

> [!IMPORTANT] 
> Le fournisseur Microsoft OLE DB pour SQL Server (SQLOLEDB) et le fournisseur SQL Server Native Client (SQLNCLI) précédents restent dépréciés, et nous vous déconseillons d’utiliser l’un ou l’autre dans de nouveaux développements. Au lieu de cela, utilisez le nouveau [Microsoft OLE DB Driver pour SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL), qui sera mis à jour avec les fonctionnalités serveur les plus récentes.
  
[ @datasrc =] *\' data_source \'*          
 Nom de la source de données, tel qu'il est interprété par le fournisseur OLE DB. *data_source* est **de type nvarchar (** 4000 **)**. *data_source* est passé comme DBPROP_INIT_DATASOURCE propriété pour initialiser le fournisseur de OLE DB.  
  
[ @location =] *\' emplacement \'*          
 Emplacement de la base de données, tel qu'il est interprété par le fournisseur OLE DB. *location* est de type **nvarchar (** 4000 **)**, avec NULL comme valeur par défaut. l' *emplacement* est passé en tant que propriété DBPROP_INIT_LOCATION pour initialiser le fournisseur OLE DB.  
  
[ @provstr =] *\' provider_string \'*          
 Chaîne de connexion spécifique au fournisseur OLE DB identifiant une source de données unique. *provider_string* est de type **nvarchar (** 4000 **)**, avec NULL comme valeur par défaut. *provstr* est passé à IDataInitialize ou défini en tant que propriété DBPROP_INIT_PROVIDERSTRING pour initialiser le fournisseur OLE DB.  
  
 Lorsque le serveur lié est créé sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur Native Client OLE DB, l’instance peut être spécifiée à l’aide du mot clé Server en tant que Server =*ServerName* \\ *InstanceName* pour spécifier une instance spécifique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *ServerName* est le nom de l’ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute, et *nom_instance* est le nom de l’instance spécifique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle l’utilisateur est connecté.  
  
> [!NOTE]
> Pour accéder à une base de données miroir, une chaîne de connexion doit contenir le nom de la base de données. Ce nom est nécessaire pour permettre au fournisseur d'accès aux données d'effectuer des tentatives de basculement. La base de données peut être spécifiée dans le paramètre **\@ provstr** ou **\@ Catalog** . Le cas échéant, la chaîne de connexion peut également fournir un nom de partenaire de basculement.  
  
[ @catalog =] *\' catalogue \'*       
 Catalogue à utiliser lors de l'établissement d'une connexion au fournisseur OLE DB. *Catalog* est de **type sysname**, avec NULL comme valeur par défaut. le *catalogue* est passé en tant que propriété DBPROP_INIT_CATALOG pour initialiser le fournisseur OLE DB. Lorsque le serveur lié est défini sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le catalogue fait référence à la base de données par défaut sur laquelle le serveur lié est mappé.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun.  
  
## <a name="remarks"></a>Notes  
 Le tableau suivant présente les façons dont un serveur lié peut être configuré pour des sources de données accessibles via OLE DB. Un serveur lié peut être configuré au moyen de plusieurs méthodes pour une même source de données ; il peut y avoir plusieurs lignes pour un type de source de données. Ce tableau indique également les valeurs de paramètres de **sp_addlinkedserver** à utiliser pour configurer le serveur lié.  
  
|Source de données OLE DB distante|Fournisseur OLE DB|product_name|provider_name|data_source|location|provider_string|catalogue|  
|-------------------------------|---------------------|-------------------|--------------------|------------------|--------------|----------------------|-------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Fournisseur OLE DB Native Client|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<sup>1</sup> (par défaut)||||||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Fournisseur OLE DB Native Client||**SQLNCLI**|Nom réseau de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (pour l'instance par défaut)|||Nom de base de données (facultatif)|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Fournisseur OLE DB Native Client||**SQLNCLI**|*nom du serveur* \\ *InstanceName* (pour une instance spécifique)|||Nom de base de données (facultatif)|  
|Oracle, version 8 et ultérieure|Fournisseur Oracle pour OLE DB|Quelconque|**OraOLEDB.Oracle**|Alias de la base de données Oracle||||  
|Access/Jet|Fournisseur Microsoft OLE DB pour Jet|Quelconque|**Microsoft.Jet.OLEDB.4.0**|Nom d'accès complet du fichier de base de données Jet||||  
|Source de données ODBC|Fournisseur Microsoft OLE DB pour ODBC|Quelconque|**MSDASQL**|Système DSN de la source de données ODBC||||  
|Source de données ODBC|Fournisseur [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB pour ODBC|Quelconque|**MSDASQL**|||Chaîne de connexion ODBC||  
|Système de fichiers|Fournisseur [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB pour le service d'indexation|Quelconque|**MSIDXS**|Nom du catalogue du Service d'indexation||||  
|Feuille de calcul [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel|Fournisseur [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB pour Jet|Quelconque|**Microsoft.Jet.OLEDB.4.0**|Chemin complet du fichier Excel||Excel 5,0||  
|Base de données IBM DB2|Fournisseur [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB pour DB2|Quelconque|**DB2OLEDB**|||Consultez [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB fournisseur pour la documentation DB2.|Nom de catalogue de base de données DB2|  
  
 <sup>1</sup> cette méthode de configuration d’un serveur lié force le nom du serveur lié à être le même que le nom réseau de l’instance distante de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Utilisez *data_source* pour spécifier le serveur.  
  
 <sup>2</sup> « any » indique que le nom du produit peut être n’importe quoi.  
  
 Le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client est le fournisseur utilisé avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si aucun nom de fournisseur n’est spécifié ou si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est spécifié comme nom de produit. Même si vous spécifiez l'ancien nom du fournisseur, SQLOLEDB, il est remplacé par SQLNCLI lorsqu'il est persistant pour le catalogue.  
  
 Les paramètres *data_source*, *location*, *provider_string* et *Catalog* identifient la ou les bases de données vers lesquelles pointe le serveur lié. Si un de ces paramètres a la valeur NULL, la propriété d'initialisation OLE DB correspondante n'est pas définie.  
  
 Dans un environnement ordonné en clusters, lorsque vous spécifiez des noms de fichiers qui pointent vers des sources de données OLE DB, utilisez le nom UNC (Universal Naming Convention) ou un lecteur partagé pour spécifier l'emplacement.  
  
 **sp_addlinkedserver** ne peut pas être exécutée dans une transaction définie par l’utilisateur.  
  
> [!IMPORTANT]
> Lorsqu’un serveur lié est créé à l’aide de **sp_addlinkedserver**, un auto-mappage par défaut est ajouté pour toutes les connexions locales. Pour les non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseurs, les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexions authentifiées peuvent être en mesure d’accéder au fournisseur sous le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte de service. Les administrateurs doivent envisager d'utiliser `sp_droplinkedsrvlogin <linkedserver_name>, NULL` pour supprimer le mappage global.  
  
## <a name="permissions"></a>Autorisations  
 L' `sp_addlinkedserver` instruction requiert l' `ALTER ANY LINKED SERVER` autorisation. (Le [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] La boîte de dialogue **nouveau serveur lié** est implémentée d’une manière qui requiert l’appartenance au `sysadmin` rôle serveur fixe.)  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-the-microsoft-sql-server-ole-db-provider"></a>R. Utilisation du fournisseur de OLE DB Microsoft SQL Server  
 L'exemple suivant crée un serveur lié nommé `SEATTLESales`. Le nom du produit est `SQL Server` ; aucun nom de fournisseur n'est utilisé.  
  
```sql  
USE master;  
GO  
EXEC sp_addlinkedserver   
   N'SEATTLESales',  
   N'SQL Server';  
GO  
```  

 L’exemple suivant crée un serveur lié `S1_instance1` sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote OLE DB.  

```sql  
EXEC sp_addlinkedserver     
   @server=N'S1_instance1',   
   @srvproduct=N'',  
   @provider=N'MSOLEDBSQL',   
   @datasrc=N'S1\instance1';  
```  

 L’exemple suivant crée un serveur lié `S1_instance1` sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client.  
 
> [!IMPORTANT] 
> Le fournisseur SQL Server Native Client OLE DB (SQLNCLI) reste déprécié, et il n’est pas recommandé de l’utiliser pour les nouveaux développements. Au lieu de cela, utilisez le nouveau [Microsoft OLE DB Driver pour SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL), qui sera mis à jour avec les fonctionnalités serveur les plus récentes.
  
```sql  
EXEC sp_addlinkedserver     
   @server=N'S1_instance1',   
   @srvproduct=N'',  
   @provider=N'SQLNCLI',   
   @datasrc=N'S1\instance1';  
```  
  
### <a name="b-using-the-microsoft-ole-db-provider-for-microsoft-access"></a>B. Utilisation du fournisseur Microsoft OLE DB pour Microsoft Access  
 Le fournisseur Microsoft.Jet.OLEDB.4.0 se connecte aux bases de données Microsoft Access qui utilisent le format 2002-2003. L'exemple suivant crée un serveur lié nommé `SEATTLE Mktg`.  
  
> [!NOTE]  
> Cet exemple suppose que l' [!INCLUDE[msCoName](../../includes/msconame-md.md)] accès et l’exemple de base de données **Northwind** sont installés et que la base de données **Northwind** réside dans C:\Msoffice\Access\Samples.  
  
```sql  
EXEC sp_addlinkedserver   
   @server = N'SEATTLE Mktg',   
   @provider = N'Microsoft.Jet.OLEDB.4.0',   
   @srvproduct = N'OLE DB Provider for Jet',  
   @datasrc = N'C:\MSOffice\Access\Samples\Northwind.mdb';  
GO  
```  
  
 Le fournisseur Microsoft.ACE.OLEDB.12.0 se connecte aux bases de données Microsoft Access qui utilisent le format 2007. L'exemple suivant crée un serveur lié nommé `SEATTLE Mktg`.  
  
> [!NOTE]  
> Cet exemple suppose que l' [!INCLUDE[msCoName](../../includes/msconame-md.md)] accès et l’exemple de base de données **Northwind** sont installés et que la base de données **Northwind** réside dans C:\Msoffice\Access\Samples.  
  
```sql  
EXEC sp_addlinkedserver   
   @server = N'SEATTLE Mktg',   
   @provider = N'Microsoft.ACE.OLEDB.12.0',   
   @srvproduct = N'OLE DB Provider for ACE',  
   @datasrc = N'C:\MSOffice\Access\Samples\Northwind.accdb';  
GO  
```  
  
### <a name="c-using-the-microsoft-ole-db-provider-for-odbc-with-the-data_source-parameter"></a>C. Utilisation du fournisseur Microsoft OLE DB pour ODBC avec le paramètre data_source  
 L’exemple suivant crée un serveur lié nommé `SEATTLE Payroll` qui utilise le [!INCLUDE[msCoName](../../includes/msconame-md.md)] fournisseur OLE DB pour ODBC ( `MSDASQL` ) et le paramètre *data_source* .  
  
> [!NOTE]  
> Le nom de la source de données ODBC spécifiée doit être défini comme DSN système dans le serveur avant d'utiliser le serveur lié.  
  
```sql  
EXEC sp_addlinkedserver   
   @server = N'SEATTLE Payroll',   
   @srvproduct = N'',  
   @provider = N'MSDASQL',   
   @datasrc = N'LocalServer';  
GO  
```  
  
### <a name="d-using-the-microsoft-ole-db-provider-for-excel-spreadsheet"></a>D. Utilisation du fournisseur Microsoft OLE DB pour une feuille de calcul Excel  
 Pour créer une définition de serveur lié à l’aide du [!INCLUDE[msCoName](../../includes/msconame-md.md)] fournisseur de OLE DB pour jet afin d’accéder à une feuille de calcul Excel au format 1997-2003, commencez par créer une plage nommée dans Excel en spécifiant les colonnes et les lignes de la feuille de calcul Excel à sélectionner. Le nom de la plage peut correspondre à un nom de table dans une requête distribuée.  
  
```sql  
EXEC sp_addlinkedserver 'ExcelSource',  
   'Jet 4.0',  
   'Microsoft.Jet.OLEDB.4.0',  
   'c:\MyData\DistExcl.xls',  
   NULL,  
   'Excel 5.0';  
GO  
```  
  
 Pour accéder aux données d'une feuille de calcul Excel, associez une plage de cellules à un nom. La requête suivante peut s'utiliser pour accéder à la plage nommée `SalesData` en tant que table en utilisant le serveur lié défini précédemment.  
  
```sql  
SELECT *  
   FROM ExcelSource...SalesData;  
GO  
```  
  
 Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute sous un compte de domaine qui peut accéder à un partage distant, il est possible d'utiliser un chemin UNC à la place d'un lecteur mappé.  
  
```sql  
EXEC sp_addlinkedserver 'ExcelShare',  
   'Jet 4.0',  
   'Microsoft.Jet.OLEDB.4.0',  
   '\\MyServer\MyShare\Spreadsheets\DistExcl.xls',  
   NULL,  
   'Excel 5.0';  
```  
  
 Pour vous connecter à une feuille de calcul Excel au format Excel 2007, utilisez le fournisseur ACE.  
  
```sql  
EXEC sp_addlinkedserver @server = N'ExcelDataSource',   
@srvproduct=N'ExcelData', @provider=N'Microsoft.ACE.OLEDB.12.0',   
@datasrc=N'C:\DataFolder\People.xlsx',  
@provstr=N'EXCEL 12.0' ;  
  
```  
  
### <a name="e-using-the-microsoft-ole-db-provider-for-jet-to-access-a-text-file"></a>E. Utilisation du fournisseur Microsoft OLE DB pour Jet pour accéder à un fichier texte  
 Le code exemple suivant crée un serveur lié pour accéder directement aux fichiers texte, sans lier les fichiers en tant que tables dans un fichier .mdb de Microsoft Access. Le fournisseur est `Microsoft.Jet.OLEDB.4.0` ; la chaîne de caractères du fournisseur est `Text`.  
  
 La source de données est le chemin d'accès complet au répertoire qui contient les fichiers texte. Un fichier schema.ini, qui décrit la structure des fichiers texte, doit se trouver dans le même répertoire que les fichiers texte. Pour plus d'informations sur la création d'un fichier Schema.ini, consultez la documentation du moteur de base de données Jet.  
  
```sql  
--Create a linked server.  
EXEC sp_addlinkedserver txtsrv, N'Jet 4.0',   
   N'Microsoft.Jet.OLEDB.4.0',  
   N'c:\data\distqry',  
   NULL,  
   N'Text';  
GO  
  
--Set up login mappings.  
EXEC sp_addlinkedsrvlogin txtsrv, FALSE, Admin, NULL;  
GO  
  
--List the tables in the linked server.  
EXEC sp_tables_ex txtsrv;  
GO  
  
--Query one of the tables: file1#txt  
--using a four-part name.   
SELECT *   
FROM txtsrv...[file1#txt];  
```  
  
### <a name="f-using-the-microsoft-ole-db-provider-for-db2"></a>F. Utilisation du fournisseur Microsoft OLE DB pour DB2  
 Le code exemple suivant crée un serveur lié nommé `DB2` qui utilise le `Microsoft OLE DB Provider for DB2`.  
  
```sql  
EXEC sp_addlinkedserver  
   @server=N'DB2',  
   @srvproduct=N'Microsoft OLE DB Provider for DB2',  
   @catalog=N'DB2',  
   @provider=N'DB2OLEDB',  
   @provstr=N'Initial Catalog=PUBS;  
       Data Source=DB2;  
       HostCCSID=1252;  
       Network Address=XYZ;  
       Network Port=50000;  
       Package Collection=admin;  
       Default Schema=admin;';  
```  
  
### <a name="g-add-a-sssdsfull-as-a-linked-server-for-use-with-distributed-queries-on-cloud-and-on-premises-databases"></a>G. Ajouter [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] en tant que serveur lié pour une utilisation avec des requêtes distribuées sur des bases de données Cloud et locales  
 Ajoutez [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] en tant que serveur lié, puis utilisez-le avec des requêtes distribuées qui couvrent les bases de données du cloud et locales. Il s’agit d’un composant pour les solutions hybrides de base de données couvrant des réseaux d’entreprise locaux et le Cloud Azure.  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] produit Box contient la fonctionnalité de requête distribuée, qui vous permet d’écrire des requêtes pour combiner des données provenant de sources de données locales et de données provenant de sources distantes (y compris des données provenant de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sources autres que des données) définies en tant que serveurs liés. Chaque [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (sauf le maître virtuel) peut être ajoutée en tant que serveur lié individuel, puis être utilisée directement dans vos applications de base de données comme toute autre base de données.  
  
 Les avantages de l'utilisation de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] sont la simplicité de gestion, la haute disponibilité, l'extensibilité, l'utilisation d'un modèle de développement familier et un modèle de données relationnelles. La configuration de votre application de base de données détermine le mode d'utilisation de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] dans le cloud. Déplacez toutes les données à la fois vers [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], ou déplacez progressivement certaines de vos données et conservez les autres localement. Pour une application de base de données hybride, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] peut désormais être ajoutée en tant que serveurs liés et l’application de base de données peut émettre des requêtes distribuées pour combiner des données provenant de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] sources de données et locales.  
  
 Voici un exemple simple expliquant comment se connecter à un [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] à l’aide de requêtes distribuées :  
  
```sql  
-- Configure the linked server  
-- Add one Azure SQL Database as Linked Server  
EXEC sp_addlinkedserver  
  @server='myLinkedServer', -- here you can specify the name of the linked server  
  @srvproduct='',       
  @provider='sqlncli', -- using SQL Server Native Client  
  @datasrc='myServer.database.windows.net',   -- add here your server name  
  @location='',  
  @provstr='',  
  @catalog='myDatabase'  -- add here your database name as initial catalog (you cannot connect to the master database)  

-- Add credentials and options to this linked server  
EXEC sp_addlinkedsrvlogin  
  @rmtsrvname = 'myLinkedServer',  
  @useself = 'false',  
  @rmtuser = 'myLogin',             -- add here your login on Azure DB  
  @rmtpassword = 'myPassword' -- add here your password on Azure DB  

EXEC sp_serveroption 'myLinkedServer', 'rpc out', true;  

-- Now you can use the linked server to execute 4-part queries  
-- You can create a new table in the Azure DB  
EXEC ('CREATE TABLE t1tutut2(col1 int not null CONSTRAINT PK_col1 PRIMARY KEY CLUSTERED (col1) )') at myLinkedServer  
-- Insert data from your local SQL Server  
EXEC ('INSERT INTO t1tutut2 VALUES(1),(2),(3)') at myLinkedServer  
  
-- Query the data using 4-part names  
SELECT * FROM myLinkedServer.myDatabase.dbo.myTable  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de requêtes distribuées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_dropserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropserver-transact-sql.md)   
 [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)   
 [sp_setnetname &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-setnetname-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
