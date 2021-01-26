---
description: MSSQLSERVER_6522
title: MSSQLSERVER_6522
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 6522 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: f48534e104139ee7cbd4eb7602fce2a8e51eeb73
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597137"
---
# <a name="mssqlserver_6522"></a>MSSQLSERVER_6522
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|6522|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|SQLCLR_UDF_EXEC_FAILED|
|Texte du message|Une erreur .NET Framework s’est produite au cours de l’exécution de la routine ou de la fonction d’agrégation définie par l’utilisateur "%.*ls" : %ls.|
||

## <a name="explanation"></a>Explication

Examinez les scénarios suivants.

### <a name="scenario-1"></a>Scénario 1

Vous créez une routine de Common Language Runtime (CLR) qui fait référence à un assembly Microsoft .NET Framework. L’assembly .NET Framework n’est pas documenté dans [922672](/troubleshoot/sql/database-design/policy-untested-net-framework-assemblies). Ensuite, vous installez le .NET Framework 3.5 ou un correctif logiciel basé sur .NET Framework 2.0.

### <a name="scenario-2"></a>Scénario 2

Vous créez un assembly, puis vous l’inscrivez dans une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ensuite, vous installez une autre version de l’assembly dans le Global Assembly Cache (GAC).

Lorsque vous exécutez la routine CLR ou utilisez l’assembly de l’un de ces scénarios dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous recevez un message d’erreur semblable à ce qui suit :

> Serveur :  Message 6522, niveau 16, état 2, ligne 1  
Une erreur .NET Framework s’est produite au cours de l’exécution de la routine ou de la fonction d’agrégation définie par l’utilisateur 'getsid’ :
>
> System.IO.FileLoadException : Impossible de charger le fichier ou l’assembly 'System.DirectoryServices, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' ou une de ses dépendances. L’assembly dans le magasin hôte a une signature différente de celle de l’assembly dans le GAC. (Exception de HRESULT : 0x80131050)

## <a name="possible-cause"></a>Cause possible

Lorsque le CLR charge un assembly, il vérifie que le même assembly se trouve dans le GAC. Si le même assembly se trouve dans le GAC, le CLR vérifie que les ID de version de module (MVID) de ces assemblys correspondent. Si les MVID de ces assemblys ne correspondent pas, vous recevez le message d’erreur que la section [Explication](#explanation) mentionne.

Lorsqu’un assembly est recompilé, le MVID de l’assembly change. Par conséquent, si vous mettez à jour le .NET Framework, les assemblys .NET Framework ont des MVID différents, car ces assemblys sont recompilés. En outre, si vous mettez à jour votre propre assembly, l’assembly est recompilé. Par conséquent, l’assembly a également un MVID différent.

## <a name="user-action"></a>Action requise

### <a name="action-1"></a>Action 1

Pour contourner le scénario 1 dans la section [Explication](#explanation), vous devez mettre à jour manuellement les assemblys .NET Framework dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour ce faire, utilisez l’instruction `ALTER ASSEMBLY` pour pointer vers la nouvelle version de l’assembly .NET Framework dans le dossier suivant :

`%Windir%\Microsoft.NET\Framework\Version`

> [!NOTE]
> **Version** représente la version de .NET Framework que vous avez installée ou mise à jour.

### <a name="action-2"></a>Action 2

Pour contourner le scénario 2 de la section [Explication](#explanation), utilisez l’instruction `ALTER ASSEMBLY` pour mettre à jour l’assembly dans la base de données.

Si le problème persiste, supprimez l’assembly de la base de données, puis enregistrez la nouvelle version de l’assembly dans la base de données.

## <a name="more-information"></a>Informations complémentaires

Nous vous déconseillons d’utiliser des assemblys .NET Framework qui ne sont pas documentés dans [Stratégie de prise en charge des assemblys .NET Framework non testés dans l’environnement SQL Server hébergé par le CLR](/troubleshoot/sql/database-design/policy-untested-net-framework-assemblies). Elle répertorie les assemblys qui sont testés dans l’environnement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hébergé par le CLR.

### <a name="description-of-clr-routines"></a>Description des routines CLR

Les routines CLR incluent les objets suivants qui sont implémentés à l’aide de l’intégration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec le CLR .NET Framework :

- Fonctions scalaires définies par l'utilisateur (fonctions UDF scalaires)
- Fonctions table définies par l'utilisateur (TVF)
- Procédures définies par l'utilisateur (UDP)
- Déclencheurs définis par l’utilisateur
- Types de données définis par l'utilisateur
- Fonctions d'agrégation définies par l'utilisateur

### <a name="assemblies-to-update-after-you-install-the-net-framework-35"></a>Assemblys à mettre à jour après l’installation de .NET Framework 3.5

Après avoir installé .NET Framework 3.5, vous devez utiliser l’instruction ALTER ASSEMBLY pour mettre à jour les assemblys suivants :

- Accessibility.dll
- AspNetMMCExt.dll
- Cscompmgd.dll
- IEExecRemote.dll
- IEHost.dll
- IIEHost.dll
- Microsoft.Build.Conversion.dll
- Microsoft.Build.Engine.dll
- Microsoft.Build.Framework.dll
- Microsoft.Build.Tasks.dll 
- Microsoft.Build.Utilities.dll 
- Microsoft.CompactFramework.Build.Tasks.dll 
- Microsoft.JScript.dll 
- Microsoft.VisualBasic.Vsa.dll 
- Microsoft.Vsa.dll 
- Microsoft.Vsa.Vb.CodeDOMProcessor.dll 
- Microsoft_VsaVb.dll 
- Sysglobl.dll 
- System.Configuration.Install.dll 
- System.Design.dll 
- System.DirectoryServices.dll 
- System.DirectoryServices.Protocols.dll 
- System.Drawing.dll 
- System.Drawing.Design.dll 
- System.EnterpriseServices.dll 
- System.Management.dll 
- System.Messaging.dll 
- System.Runtime.Serialization.Formatters.Soap.dll 
- System.ServiceProcess.dll 
- System.Web.dll 
- System.Web.Mobile.dll 
- System.Web.RegularExpressions.dll 

Ces assemblys se trouvent dans le dossier suivant :

`%Windir%\Microsoft.NET\Framework\v2.0.50727`

### <a name="how-to-preserve-the-data-from-user-defined-data-types-after-you-drop-an-assembly"></a>Comment conserver les données des types de données définis par l’utilisateur après la suppression d’un assembly

Si vous supprimez un assembly qu’un type de données défini par l’utilisateur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise, vous pouvez utiliser l’une des méthodes suivantes pour conserver les données.

Supposons le scénario suivant :

- Vous créez un assembly dont le nom est *MyAssembly.dll*.
- L’assembly MyAssembly référence l’assembly `System.DirectoryServices.dll`.
- Vous avez un type de données défini par l’utilisateur dont le nom est *MyDateTime*.
- Le type de données *MyDateTime* utilise l’assembly *MyAssembly.dll*.
- Vous créez une table dont le nom est *MyTable*.
- La table *MyTable* contient des données du type de données *MyDateTime*.

#### <a name="method-1-use-the-bcpexe-utility"></a>Méthode 1 : Utiliser l’utilitaire bcp.exe

1. Utilisez l’utilitaire bcp.exe conjointement avec l’option -n pour copier les données de la table MyTable dans un fichier. Par exemple, exécutez la commande suivante à partir d’une invite de commandes :

    ```console
    bcp MyDatabase.dbo.MyTable out C:\MyFile.bcp -n -SSQLServerName -T
    ```

2. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio, procédez comme suit :
   1. Supprimez la table MyTable.
   2. Supprimez le type de données MyDateTime.
   3. Supprimez l’assembly `System.DirectoryServices.dll`.
   4. Supprimez l’assembly MyAssembly.
3. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio, procédez comme suit :

   1. Inscrire l’assembly `System.DirectoryServices.dll`.
   2. Inscrivez l’assembly MyAssembly.
   3. Créez le type de données MyDateTime.
   4. Créez une table qui a la même structure de table que la table MyTable.
4. Utilisez l’utilitaire bcp.exe conjointement avec l’option -n pour importer les données du fichier dans la table MyTable. Par exemple, exécutez la commande suivante à partir d’une invite de commandes :
  
    ```console
    bcp MyDatabase.dbo.MyTable in C:\MyFile.bcp -n -SSQLServerName -T
    ```

#### <a name="method-2-use-the-insert--select-statement"></a>Méthode 2 : Utilisez l’instruction INSERT... Instruction SELECT

Supposons que le type de données MyDateTime occupe 9 octets dans le stockage.

1. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio, créez une table qui contient une colonne du type de données `VARBINARY(9)` en exécutant l’instruction suivante :

    ```sql
    CREATE TABLE TempTable (c1 VARBINARY(9));
    ```

2. Exécutez l’exemple d’instruction INSERT... Instruction SELECT pour remplir la table tentant TempTable :

    ```sql
    INSERT INTO TempTable SELECT CAST(c1 as VARBINARY(9)) FROM MyTable;
    ```

3. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio, procédez comme suit :
   1. Supprimez la table MyTable.
   2. Supprimez le type de données MyDateTime.
   3. Supprimez l’assembly System.DirectoryServices.dll.
   4. Supprimez l’assembly MyAssembly.
4. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio, procédez comme suit :
   1. Inscrivez l’assembly System.DirectoryServices.dll.
   2. Inscrivez l’assembly MyAssembly.
   3. Créez le type de données MyDateTime.
   4. Créez une table qui a la même structure de table que la table MyTable.
5. Exécutez l’exemple d’instruction INSERT... Instruction SELECT pour remplir la table MyTable :

    ```sql
    INSERT INTO MyTable SELECT c1 FROM TempTable;
    ```

## <a name="references"></a>Références

- Pour plus d’informations sur la version de l’assembly, consultez la [Documentation de Visual Studio 2005 retirée](https://www.microsoft.com/download/details.aspx?id=55984).
- Pour plus d’informations sur la mise à jour d’un assembly, consultez [ALTER ASSEMBLY (Transact-SQL)](../../t-sql/statements/alter-assembly-transact-sql.md).
- Pour plus d’informations sur la suppression d’un assembly, consultez [DROP ASSEMBLY (Transact-SQL)](../../t-sql/statements/drop-assembly-transact-sql.md).
- Pour plus d’informations sur l’inscription d’un assembly dans une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [CREATE ASSEMBLY (Transact-SQL)](../../t-sql/statements/create-assembly-transact-sql.md).
- Pour plus d’informations sur l’utilitaire bcp.exe, consultez [https://msdn2.microsoft.com/library/ms162802.aspx](../../tools/bcp-utility.md).