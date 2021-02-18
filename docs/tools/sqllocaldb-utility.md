---
title: Utilitaire SqlLocalDB
description: L’utilitaire SqlLocalDB est un outil en ligne de commande permettant aux utilisateurs et développeurs de créer et gérer une instance de SQL Server Express LocalDB.
ms.custom: seo-lt-2019
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- SqlLocalDB utility [SQL Server]
- local database runtime utility
- LocalDB, SqlLocalDB Utility
ms.assetid: d785cdb7-1ea0-4871-bde9-1ae7881190f5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 74a6f8fb42cccc907d2c99ce795f307c1f73330c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338563"
---
# <a name="sqllocaldb-utility"></a>Utilitaire SqlLocalDB
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  Utilisez l’utilitaire **SqlLocalDB** pour créer une instance de [!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssExpCurrent](../includes/ssexpcurrent-md.md)] **LocalDB**. L’utilitaire **SqlLocalDB** (SqlLocalDB.exe) est un outil en ligne de commande simple pour permettre aux utilisateurs et développeurs de créer et gérer une instance de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)] **LocalDB**. Pour plus d’informations sur l’utilisation de **LocalDB**, consultez [SQL Server 2016 Express LocalDB](../database-engine/configure-windows/sql-server-express-localdb.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
SqlLocalDB.exe   
{  
      [ create   | c ] \<instance-name>  \<instance-version> [-s ]  
    | [ delete   | d ] \<instance-name>  
    | [ start    | s ] \<instance-name>  
    | [ stop     | p ] \<instance-name>  [ -i ] [ -k ]  
    | [ share    | h ] [" <user_SID> " | " <user_account> " ] " \<private-name> " " \<shared-name> "  
    | [ unshare  | u ] " \<shared-name> "  
    | [ info     | i ] \<instance-name>  
    | [ versions | v ]  
    | [ trace    | t ] [ on | off ]  
    | [ help     | -? ]  
}  
```  
  
## <a name="arguments"></a>Arguments  
 [ **créer** | **c** ] *\<instance-name>* *\<instance-version>* [ **-s** ]  
 Crée une instance de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB**. **SqlLocalDB** utilise la version des binaires [!INCLUDE[ssExpress](../includes/ssexpress-md.md)] spécifiée par l'argument *\<instance-version>* . Le numéro de version est spécifié au format numérique avec au moins une décimale. Les numéros de version secondaire (Service Packs) sont facultatifs. Par exemple, les deux numéros de version suivants sont acceptables : 11.0 ou 11.0.1186. La version spécifiée doit être installée sur l'ordinateur. S'il n'est pas spécifié, le numéro de version a par défaut la version de l'utilitaire **SqlLocalDB** . L’ajout de **-s** permet de démarrer la nouvelle instance de **LocalDB**.  
  
 [ **share** | **h** ]  
 Partage l’instance privée spécifiée de **LocalDB** à l’aide du nom partagé spécifié. Si le SID ou le nom de compte de l'utilisateur est omis, il prend par défaut la valeur de l'utilisateur actuel.  
  
 [ **unshared** | **u** ]  
 Arrête le partage de l’instance partagée spécifiée de **LocalDB**.  
  
 [ **supprimer** | **d** ] *\<instance-name>*  
 Supprime l’instance spécifiée de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB**.  
  
 [**démarrer** | **s**] « *\<instance-name>*  »  
 Démarre l’instance spécifiée de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB**. En cas de réussite de l'opération, l'instruction retourne l'adresse du canal nommé de **LocalDB**.  
  
 [**Arrêter** | **p**] *\<instance-name>* [ **-i**] [ **-k**]  
 Arrête l’instance spécifiée de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB**. L’ajout de **-i** demande l’arrêt de l’instance avec l’option **NOWAIT**. L’ajout de **-k** met fin au processus de l’instance sans le contacter.  
  
 [ **info** | **i** ] [ *\<instance-name>* ]  
 Répertorie toutes les instances de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB** détenues par l’utilisateur actuel.  
  
 *\<instance-name>* retourne le nom, la version, l’état (En cours d’exécution ou Arrêté), la dernière heure de début de l’instance spécifiée de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB** et le nom du canal local de **LocalDB**.  
  
 [ **trace** | **t** ] **on** | **off**  
 **trace on** active le suivi des appels d’API de **SqlLocalDB** pour l’utilisateur actuel. **trace off** désactive le suivi.  
  
 **-?**  
 Retourne de brèves descriptions de chaque option de **SqlLocalDB** .  
  
## <a name="remarks"></a>Notes  
 L’argument *instance name* doit respecter les règles applicables aux identificateurs [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ou il doit être placé entre guillemets.  
  
 L’exécution de SqlLocalDB sans arguments retourne le texte d’aide.  
  
 Les opérations autres que le démarrage peuvent être exécutées sur une instance appartenant à l'utilisateur actuellement connecté. Une instance SQLLOCALDB, lorsqu’elle est partagée, peut uniquement être démarrée et arrêtée par le propriétaire de l’instance.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-an-instance-of-localdb"></a>R. Création d'une instance de LocalDB  
 L’exemple suivant crée une instance de [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]**LocalDB** nommée `DEPARTMENT` utilisant les binaires [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)] et démarre l’instance.  
  
```  
SqlLocalDB.exe create "DEPARTMENT" 12.0 -s  
```  
  
### <a name="b-working-with-a-shared-instance-of-localdb"></a>B. Utilisation d'une instance partagée de LocalDB  
 Ouvrez une invite de commandes en utilisant des autorisations d'administrateur.  
  
```  
SqlLocalDB.exe create "DeptLocalDB"  
SqlLocalDB.exe share "DeptLocalDB" "DeptSharedLocalDB"  
SqlLocalDB.exe start "DeptLocalDB"  
SqlLocalDB.exe info "DeptLocalDB"  
REM The previous statement outputs the Instance pipe name for the next step  
sqlcmd -S np:\\.\pipe\LOCALDB#<use your pipe name>\tsql\query  
CREATE LOGIN NewLogin WITH PASSWORD = 'Passw0rd!!@52';   
GO  
CREATE USER NewLogin;  
GO  
EXIT  
```  
  
 Exécutez le code suivant pour vous connecter à l'instance partagée de **LocalDB** à l'aide de la connexion `NewLogin` .  
  
```  
sqlcmd -S (localdb)\.\DeptSharedLocalDB -U NewLogin -P Passw0rd!!@52  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server 2016 Express LocalDB](../database-engine/configure-windows/sql-server-express-localdb.md)  
[Outil de gestion en ligne de commande : SqlLocalDB.exe](../relational-databases/express-localdb-instance-apis/command-line-management-tool-sqllocaldb-exe.md)  
