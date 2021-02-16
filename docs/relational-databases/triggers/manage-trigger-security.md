---
description: Gérer la sécurité des déclencheurs
title: Gérer la sécurité des déclencheurs | Microsoft Docs
ms.custom: ''
ms.date: 06/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- triggers [SQL Server], security
ms.assetid: e94720a8-a3a2-4364-b0a3-bbe86e3ce4d5
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 931ba1487b8212c6c088b044df5e0b9fa30b5ef1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347594"
---
# <a name="manage-trigger-security"></a>Gérer la sécurité des déclencheurs

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Par défaut, les déclencheurs DML et DDL s'exécutent dans le contexte de l'utilisateur ayant appelé le déclencheur. L'appelant d'un déclencheur correspond à l'utilisateur exécutant l'instruction qui provoque l'activation du déclencheur. Par exemple, si l’utilisatrice **Mary** exécute une instruction DELETE provoquant l’activation d’un déclencheur DML intitulé **DML_trigMary** , le code inclus dans **DML_trigMary** s’exécute dans le contexte des autorisations utilisateur associées à **Mary**. Ce comportement par défaut peut malheureusement être exploité par des utilisateurs mal intentionnés voulant introduire du code dangereux dans une base de données ou une instance de serveur. Prenons pour exemple le déclencheur DDL suivant créé par l’utilisateur **JohnDoe** :  

```sql
CREATE TRIGGER DDL_trigJohnDoe
ON DATABASE
FOR ALTER_TABLE
AS
SET NOCOUNT ON;

BEGIN TRY
  EXEC(N'
    USE [master];
    GRANT CONTROL SERVER TO [JohnDoe];
');
END TRY
BEGIN CATCH
  DECLARE @DoNothing INT;
END CATCH;
GO
```

Ce déclencheur signifie que dès qu’un utilisateur disposant de l’autorisation d’exécuter une instruction `GRANT CONTROL SERVER`, comme un membre du rôle serveur fixe **sysadmin**, exécute une instruction `ALTER TABLE`, **JohnDoe** se voit octroyer l’autorisation `CONTROL SERVER`. En d'autres termes, même si **JohnDoe** ne peut pas s’accorder lui-même l’autorisation `CONTROL SERVER`, il a activé le code du déclencheur qui lui accorde cette autorisation pour qu’il s’exécute sous des privilèges promus. Aussi bien les déclencheurs DML que les déclencheurs DDL permettent ce type de menace de sécurité.  
  
## <a name="trigger-security-best-practices"></a>Méthodes conseillées pour la sécurité liée aux déclencheurs  
 Nous vous proposons les mesures suivantes pour éviter que du code de déclencheur s'exécute sous des privilèges promus :  
  
::: moniker range=">=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current"

-   Prenez connaissance des déclencheurs DML et DDL existant dans la base de données et sur l’instance du serveur en interrogeant les vues de catalogue [sys.triggers](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) et [sys.server_triggers](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md) . La requête suivante renvoie tous les déclencheurs DML de la base de données actuelle, tous les déclencheurs DDL au niveau de la base de données actuelle et tous les déclencheurs DDL de niveau serveur inclus dans l'instance du serveur :  
  
    ```sql
    SELECT type, name, parent_class_desc FROM sys.triggers
    UNION ALL
    SELECT type, name, parent_class_desc FROM sys.server_triggers;
    ```  

   > [!NOTE]
   > Seul **sys.triggers** est disponible pour [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], sauf si vous utilisez [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)].

::: moniker-end

::: moniker range="=azuresqldb-current"

-   Prenez connaissance des déclencheurs DML et DDL existant dans la base de données en interrogeant la vue de catalogue [sys.triggers](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md). La requête suivante retourne tous les déclencheurs DDL de niveau base de données et DML dans la base de données actuelle :  
  
    ```sql
    SELECT type, name, parent_class_desc FROM sys.triggers;
    ```  
  
::: moniker-end

-   Utilisez [DISABLE TRIGGER](../../t-sql/statements/disable-trigger-transact-sql.md) afin de désactiver les déclencheurs pouvant mettre en péril l’intégrité de la base de données ou du serveur si les déclencheurs s’exécutent sous des privilèges promus. L'instruction suivante désactive tous les déclencheurs DDL de niveau base de données dans la base de données actuelle :  
  
    ```sql
    DISABLE TRIGGER ALL ON DATABASE;
    ```  
  
     L'instruction suivante désactive tous les déclencheurs DDL de niveau serveur sur l'instance du serveur :  
  
    ```sql
    DISABLE TRIGGER ALL ON ALL SERVER;
    ```  
  
     Enfin, cette instruction désactive tous les déclencheurs DML de la base de données actuelle :  
  
    ```sql
    DECLARE @schema_name sysname, @trigger_name sysname, @object_name sysname;
    DECLARE @sql nvarchar(max);
    DECLARE trig_cur CURSOR FORWARD_ONLY READ_ONLY FOR
        SELECT SCHEMA_NAME(schema_id) AS schema_name,
            name AS trigger_name,
            OBJECT_NAME(parent_object_id) AS object_name
        FROM sys.objects WHERE type IN ('TR', 'TA');

    OPEN trig_cur;
    FETCH NEXT FROM trig_cur INTO @schema_name, @trigger_name, @object_name;
  
    WHILE @@FETCH_STATUS = 0
    BEGIN
        SELECT @sql = N'DISABLE TRIGGER ' + QUOTENAME(@schema_name) + N'.'
            + QUOTENAME(@trigger_name)
            + N' ON ' + QUOTENAME(@schema_name) + N'.'
            + QUOTENAME(@object_name) + N'; ';
        EXEC (@sql);
        FETCH NEXT FROM trig_cur INTO @schema_name, @trigger_name, @object_name;
    END;
    GO

    -- Verify triggers are disabled. Should return an empty result set.
    SELECT * FROM sys.triggers WHERE is_disabled = 0;
    GO

    CLOSE trig_cur;
    DEALLOCATE trig_cur;
    ```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [Déclencheurs DML](../../relational-databases/triggers/dml-triggers.md)   
 [Déclencheurs DDL](../../relational-databases/triggers/ddl-triggers.md)  
 [Déclencheurs de connexion](../../relational-databases/triggers/logon-triggers.md)  
  
