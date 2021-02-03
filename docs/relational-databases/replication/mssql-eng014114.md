---
description: MSSQL_ENG014114
title: MSSQL_ENG014114 | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG014114 error
ms.assetid: f5f04590-e1c6-40d8-ab2b-98c791a0fc44
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: e98d56ebf3f2439cc36fd0940f8680f3676793c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198832"
---
# <a name="mssql_eng014114"></a>MSSQL_ENG014114
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Détails du message  
  
|Attribut|Valeur|  
|-|-|  
|Nom du produit|SQL Server|  
|ID de l’événement|14114|  
|Source de l’événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|'%1!' n'est pas configuré en tant que distributeur.|  
  
## <a name="explanation"></a>Explication  
 Si le message d'erreur spécifie une instance particulière autre que 'null', l'instance spécifiée n'a pas été correctement configurée pour être reconnue comme serveur de distribution.  
  
 Si le message spécifie 'null' comme serveur de distribution, il n'y a pas d'entrée pour le serveur local dans la base de données **master** , ou bien l'entrée est incorrecte (peut-être parce que l'ordinateur a été renommé). La réplication requiert que tous les serveurs d'une topologie soient enregistrés avec le nom d'ordinateur et un nom d'instance facultatif (dans le cas d'une instance cluster, le nom du serveur virtuel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec le nom d'instance facultatif). Pour que la réplication fonctionne correctement, la valeur renvoyée par `SELECT @@SERVERNAME` pour chaque serveur de la topologie doit correspondre au nom d'ordinateur ou au nom de serveur virtuel avec le nom d'instance facultatif.  
  
 La réplication n'est pas prise en charge si vous avez inscrit une des instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par adresse IP ou par nom de domaine pleinement qualifié (FQDN). Si vous aviez une des instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inscrites par adresse IP ou par nom de domaine pleinement qualifié dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] quand vous avez configuré la réplication, cette erreur a pu se produire.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Si le message d'erreur spécifie une instance particulière, configurez le serveur comme serveur de distribution. Pour plus d'informations, voir [Configure Distribution](../../relational-databases/replication/configure-distribution.md).  
  
 Si le message d'erreur ne spécifie pas d'instance particulière ('null'), vérifiez que l'instance du serveur de distribution est inscrit correctement. Si le nom réseau de l'ordinateur et le nom de l'instance SQL Server diffèrent, effectuez une des actions suivantes :  
  
-   Ajoutez le nom de l'instance SQL Server comme nom réseau valide. Pour définir un autre nom réseau, une méthode possible consiste à l'ajouter au fichier hosts local. Le fichier hosts local est installé par défaut dans le répertoire WINDOWS\system32\drivers\etc ou WINNT\system32\drivers\etc. Pour plus d'informations, reportez-vous à la documentation Windows.  
  
     Si, par exemple, le nom d'ordinateur est comp1, l'adresse IP de l'ordinateur 10.193.17.129 et le nom de l'instance inst1/nominst, ajoutez l'entrée suivante au fichier des hôtes :  
  
     10.193.17.129 inst1  
  
-   Désactivez la distribution, inscrivez l'instance puis réactivez la distribution. Si la valeur de @@SERVERNAME n’est pas correcte pour une instance non-cluster, effectuez les étapes suivantes :  
  
    ```  
    sp_dropserver '<old_name>', 'droplogins'  
    go  
    sp_addserver '<new_name>', 'local'  
    go  
    ```  
  
     Après avoir exécuté la procédure stockée [sp_addserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md), vous devez redémarrer le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour que la modification apportée à @@SERVERNAME soit prise en compte.  
  
     Si la valeur de @@SERVERNAME n’est pas correcte pour une instance cluster, vous devez changer le nom à l’aide de l’administrateur de cluster. Pour plus d’informations, consultez [Instances de cluster de basculement Always On (SQL Server)](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
