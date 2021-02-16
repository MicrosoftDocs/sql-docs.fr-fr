---
title: Sécurité CLR stricte | Microsoft Docs
description: Apprenez à configurer la sécurité common language runtime (CLR) stricte dans SQL Server. Contrôlez l’interprétation des autorisations SAFE, EXTERNAL ACCESS et UNSAFE.
ms.custom: ''
ms.date: 06/20/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- clr strict security
- clr_strict_security_TSQL
- clr strict security
- strict_security_TSQL
helpviewer_keywords:
- assemblies [CLR integration], strick security
- clr strict security option
ms.assetid: ''
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 77d91e7c53b866cdaa6330d6ea728df2f4743037
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347021"
---
# <a name="clr-strict-security"></a>Sécurité CLR stricte   
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Contrôle l’interprétation de l’autorisation `SAFE`,`EXTERNAL ACCESS`, `UNSAFE` dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].   

|Valeur |Description | 
|----- |----- | 
|0 |Désactivée : fournie pour la compatibilité ascendante. La valeur `Disabled` n’est pas recommandée. | 
|1 |Activée : le [!INCLUDE[ssde-md](../../includes/ssde-md.md)] ignore les informations `PERMISSION_SET` sur les assemblys et les interprète toujours comme `UNSAFE`.  À compter de [!INCLUDE[sssql17](../../includes/sssql17-md.md)], la valeur par défaut est `Enabled`. | 

> [!WARNING]
>  CLR utilise la sécurité d’accès du code (CAS) dans le .NET Framework, qui n’est plus pris en charge comme limite de sécurité. Un assembly CLR créé avec `PERMISSION_SET = SAFE` peut être en mesure d’accéder à des ressources système externes, d’appeler du code non managé et d’acquérir des privilèges sysadmin. À compter de [!INCLUDE[sssql17](../../includes/sssql17-md.md)], une option de `sp_configure` appelée `clr strict security` est introduite pour renforcer la sécurité des assemblys CLR. `clr strict security` est activée par défaut et traite les assemblys `SAFE` et `EXTERNAL_ACCESS` comme s’ils étaient marqués `UNSAFE`. L’option `clr strict security` peut être désactivée pour assurer une compatibilité descendante, mais ceci n’est pas recommandé. Microsoft recommande que tous les assemblys soient signés par un certificat ou une clé asymétrique avec une connexion correspondante à laquelle a été accordée l’autorisation `UNSAFE ASSEMBLY` dans la base de données master. Les administrateurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent également ajouter des assemblys à une liste d’assemblys, que le moteur de base de données doit approuver. Pour plus d’informations, consultez [sys.sp_add_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-add-trusted-assembly-transact-sql.md).

## <a name="remarks"></a>Notes   

Quand elle est activée, l’option `PERMISSION_SET` dans les instructions `CREATE ASSEMBLY` et `ALTER ASSEMBLY` est ignorée au moment de l’exécution, mais les options `PERMISSION_SET` sont conservées dans les métadonnées. Ignorer cette option réduit les risques de rupture des instructions de code existantes.

`CLR strict security` est une `advanced option`.  

> [!IMPORTANT]
>  Une fois la sécurité stricte activée, le chargement des assemblys qui ne sont pas signés échoue. Vous devez modifier, ou bien supprimer et recréer, chaque assembly, de façon à ce qu’il soit signé avec un certificat ou une clé asymétrique qui a une connexion correspondante avec l’autorisation `UNSAFE ASSEMBLY` sur le serveur.

## <a name="permissions"></a>Autorisations 

### <a name="to-change-this-option"></a>Pour changer cette option  
Requiert l’autorisation `CONTROL SERVER` ou l’appartenance au rôle serveur fixe `sysadmin`.

### <a name="to-create-an-clr-assembly"></a>Pour créer un assembly CLR   
Les autorisations suivantes sont requises pour créer un assembly CLR quand `CLR strict security` est activée :

- L’utilisateur doit avoir l’autorisation `CREATE ASSEMBLY`.  
- Et une des conditions suivantes doit également être remplie :  
  - L’assembly est signé avec un certificat ou une clé asymétrique qui a une connexion correspondante avec l’autorisation `UNSAFE ASSEMBLY` sur le serveur. Signer l’assembly est recommandé.  
  - La base de données a la propriété `TRUSTWORTHY` définie sur `ON`, et elle est détenue par une connexion qui a l’autorisation `UNSAFE ASSEMBLY` sur le serveur. Cette option n’est pas recommandée.  

  
## <a name="see-also"></a>Voir aussi  
  
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [clr enabled (option de configuration de serveur)](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md)
