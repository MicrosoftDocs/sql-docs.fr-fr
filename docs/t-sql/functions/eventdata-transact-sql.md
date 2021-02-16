---
description: EVENTDATA (Transact-SQL)
title: EVENTDATA (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EVENTDATA
- fn_event_data
- EVENTDATA_TSQL
- fn_event_data_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- server instance event data [SQL Server]
- event notifications [SQL Server], event status
- events [SQL Server], status information
- EVENTDATA function
- status information [SQL Server], events
- DDL triggers, returning event data
ms.assetid: 03a80e63-6f37-4b49-bf13-dc35cfe46c44
author: cawrites
ms.author: chadam
ms.openlocfilehash: a9815f7cc0cdcb86ffc716b53c9cbe579aca9824
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350883"
---
# <a name="eventdata-transact-sql"></a>EVENTDATA (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Cette fonction retourne des informations sur les événements de serveur et de base de données. Lorsqu’une notification d’événement est déclenchée et que le Service Broker spécifié reçoit les résultats, `EVENTDATA` est appelé. Un déclencheur DDL ou d’ouverture de session prend également en charge l’utilisation interne de `EVENTDATA`.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
EVENTDATA( )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Notes  
`EVENTDATA` ne retourne des données que si elles sont référencées directement au sein d’un déclencheur DDL ou d’ouverture de session. `EVENTDATA` retourne la valeur Null si d’autres routines l’appellent, même si un déclencheur DDL ou d’ouverture de session appelle ces routines.
  
Les données retournées par `EVENTDATA` ne sont pas valides après une transaction qui :

+ a appelé `EVENTDATA` explicitement ;
+ a appelé `EVENTDATA` implicitement ;
+ est en validation ;
+ est restaurée.  
  
> [!CAUTION]  
>  `EVENTDATA` retourne des données XML qui sont envoyées au client au format Unicode, qui utilise 2 octets pour chaque caractère. Le code XML retourné par `EVENTDATA` peut représenter ces points de code Unicode :  
>   
>  `0x0009`  
>   
>  `0x000A`  
>   
>  `0x000D`  
>   
>  `>= 0x0020 && <= 0xD7FF`  
>   
>  `>= 0xE000 && <= 0xFFFD`  
>   
>  XML ne peut ni exprimer ni autoriser certains caractères susceptibles d’apparaître dans les identificateurs et les données [!INCLUDE[tsql](../../includes/tsql-md.md)]. Les caractères ou données ayant des points de code non représentés dans la liste précédente sont mappés sur un point d'interrogation (?).  
  
Les mots de passe ne s’affichent pas pendant l’exécution d’instructions `CREATE LOGIN` ou `ALTER LOGIN` de façon à préserver la sécurité de connexion.  
  
## <a name="schemas-returned"></a>Schémas retournés  
EVENTDATA renvoie une valeur dont le type de données est **xml**. Par défaut, la définition de schéma de tous les événements s’installe dans ce répertoire : [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]Tools\Binn\schemas\sqlserver\2006\11\events\events.xsd.  
  
La page web [Schémas XML Microsoft SQL Server](https://go.microsoft.com/fwlink/?LinkID=31850) comporte également le schéma d’événement.  
  
Pour extraire le schéma pour un événement particulier, recherchez dans le schéma le type complexe `EVENT_INSTANCE_<event_type>`. Par exemple, pour extraire le schéma de l’événement `EVENT_INSTANCE_DROP_TABLE`, recherchez `DROP_TABLE` dans le schéma.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-querying-event-data-in-a-ddl-trigger"></a>R. Interrogation des données d'événement dans un déclencheur DDL  
Cet exemple crée un déclencheur DDL qui empêche la création de nouvelles tables de base de données. La requête XQuery effectuée sur les données XML générées par `EVENTDATA` a pour effet de capturer l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qui active le déclencheur. Voir [Informations de référence sur le langage XQuery &#40;SQL Server&#41;](../../xquery/xquery-language-reference-sql-server.md) pour plus d’informations.  
  
> [!NOTE]  
>  Lorsque vous utilisez **Résultats dans des grilles** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour interroger l’élément `<TSQLCommand>`, les sauts de ligne n’apparaissent pas dans le texte de la commande. Utilisez plutôt l’option **Résultats dans du texte**.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE TRIGGER safety   
ON DATABASE   
FOR CREATE_TABLE   
AS   
    PRINT 'CREATE TABLE Issued.'  
    SELECT EVENTDATA().value  
        ('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]','nvarchar(max)')  
   RAISERROR ('New tables cannot be created in this database.', 16, 1)   
   ROLLBACK  
;  
GO  
--Test the trigger.  
CREATE TABLE NewTable (Column1 INT);  
GO  
--Drop the trigger.  
DROP TRIGGER safety  
ON DATABASE;  
GO  
```  
  
> [!NOTE]  
>  Pour retourner des données d’événement, utilisez la méthode XQuery **value()** plutôt que la méthode **query()**. La méthode **query()** renvoie des instances XML et CR/LF (retour chariot/saut de ligne) à séquence d’échappement perluète dans la sortie, tandis que la méthode **value()** retourne des instances CR/LF invisibles dans la sortie.  
  
### <a name="b-creating-a-log-table-with-event-data-in-a-ddl-trigger"></a>B. Création d'une table de journal avec données d'événements dans un déclencheur DDL  
Cet exemple crée une table pour stocker des informations sur tous les événements de niveau base de données, et la remplit avec un déclencheur DDL. La requête XQuery effectuée sur les données XML générées par `EVENTDATA` a pour effet de capturer le type d’événement et l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
```sql 
USE AdventureWorks2012;  
GO  
CREATE TABLE ddl_log (PostTime DATETIME, DB_User NVARCHAR(100), Event NVARCHAR(100), TSQL NVARCHAR(2000));  
GO  
CREATE TRIGGER log   
ON DATABASE   
FOR DDL_DATABASE_LEVEL_EVENTS   
AS  
DECLARE @data XML  
SET @data = EVENTDATA()  
INSERT ddl_log   
   (PostTime, DB_User, Event, TSQL)   
   VALUES   
   (GETDATE(),   
   CONVERT(NVARCHAR(100), CURRENT_USER),   
   @data.value('(/EVENT_INSTANCE/EventType)[1]', 'NVARCHAR(100)'),   
   @data.value('(/EVENT_INSTANCE/TSQLCommand)[1]', 'NVARCHAR(2000)') ) ;  
GO  
--Test the trigger.  
CREATE TABLE TestTable (a INT);  
DROP TABLE TestTable ;  
GO  
SELECT * FROM ddl_log ;  
GO  
--Drop the trigger.  
DROP TRIGGER log  
ON DATABASE;  
GO  
--Drop table ddl_log.  
DROP TABLE ddl_log;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Utiliser la fonction EVENTDATA](../../relational-databases/triggers/use-the-eventdata-function.md)   
 [Déclencheurs DDL](../../relational-databases/triggers/ddl-triggers.md)   
 [Notifications d'événements](../../relational-databases/service-broker/event-notifications.md)   
 [Déclencheurs de connexion](../../relational-databases/triggers/logon-triggers.md)  
  
  
