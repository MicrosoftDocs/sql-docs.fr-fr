---
title: Configurer les transactions distribuées pour un groupe de disponibilité
description: 'Décrit comment configurer les transactions distribuées pour des bases de données membres d’un groupe de disponibilité Always On. '
ms.custom: seodec18
ms.date: 02/06/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- cross-database transactions [SQL Server]
- transactions [database mirroring]
- Availability Groups [SQL Server], interoperability
- troubleshooting [SQL Server], cross-database transactions
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: 93876b488d5e265bca363d213c04cb0611c185f4
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642175"
---
# <a name="configure-distributed-transactions-for-an-always-on-availability-group"></a>Configurer les transactions distribuées pour un groupe de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[SQL2017](../../../includes/sssqlv14-md.md)] prend en charge toutes les transactions distribuées incluant des bases de données dans un groupe de disponibilité. Cet article explique comment configurer un groupe de disponibilité pour les transactions distribuées.  

Afin de garantir des transactions distribuées, le groupe de disponibilité doit être configuré pour inscrire les bases de données en tant que gestionnaires de ressources de transactions distribuées.  

>[!NOTE]
>[!INCLUDE[SQL2016](../../../includes/sssql15-md.md)]Service Pack 2 et versions ultérieures fournissent une prise en charge complète des transactions distribuées dans les groupes de disponibilité. Dans les versions de [!INCLUDE[SQL2016](../../../includes/sssql15-md.md)] antérieures au Service Pack 2, les transactions distribuées entre bases de données (autrement dit, les transactions qui utilisent des bases de données sur la même instance de SQL Server) impliquant une base de données dans un groupe de disponibilité ne sont pas prises en charge. [!INCLUDE[SQL2017](../../../includes/sssqlv14-md.md)] n’a pas cette limitation. 
>
>Dans [!INCLUDE[SQL2016](../../../includes/sssql15-md.md)], les étapes de configuration sont les mêmes que dans [!INCLUDE[SQL2017](../../../includes/sssqlv14-md.md)].

Dans une transaction distribuée, les applications clientes utilisent Microsoft Distributed Transaction Coordinator (MS DTC ou DTC) afin de garantir la cohérence transactionnelle entre plusieurs sources de données. DTC est un service disponible sur les systèmes d’exploitation Windows Server pris en charge. Pour une transaction distribuée, DTC est le *coordinateur de la transaction*. Normalement, une instance de SQL Server est le *gestionnaire de ressources*. Quand une base de données est dans un groupe de disponibilité, chaque base de données doit être son propre gestionnaire de ressources. 

[!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] n’empêche pas les transactions distribuées pour des bases de données dans un groupe de disponibilité, même quand le groupe de disponibilité n’est pas configuré pour les transactions distribuées. Cependant, quand un groupe de disponibilité n’est pas configuré pour les transactions distribuées, le basculement peut échouer dans certaines situations. En particulier, l’instance de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] du nouveau réplica principal ne peut parfois pas obtenir le résultat de la transaction auprès du DTC. Pour permettre à l’instance de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] d’obtenir le résultat des transactions incertaines auprès du DTC après un basculement, configurez le groupe de disponibilité pour les transactions distribuées. 

DTC n’est pas impliqué dans le traitement d’un groupe de disponibilité sauf si une base de données est également membre d’un cluster de basculement. Dans un groupe de disponibilité, la cohérence entre les réplicas est gérée par la logique du groupe de disponibilité : Le réplica principal n’effectue pas la validation et n’envoie pas d’accusé de réception de la validation à l’appelant tant que le réplica secondaire n’a pas confirmé qu’il a rendu les enregistrements de journal persistants dans un stockage durable. C’est alors seulement que le réplica principal déclare la transaction terminée. En mode asynchrone, nous n’attendons pas que le réplica secondaire envoie un accusé de réception, et il existe explicitement un risque de perte d’une petite quantité de données.

## <a name="prerequisites"></a>Prérequis

Avant de configurer un groupe de disponibilité pour prendre en charge les transactions distribuées, vous devez respecter les prérequis suivants :

* Toutes les instances de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] qui participent à la transaction distribuée doivent être [!INCLUDE[SQL2016](../../../includes/sssql15-md.md)] ou ultérieur.

* Les groupes de disponibilité doivent s’exécuter sur Windows Server 2016 ou Windows Server 2012 R2. Pour Windows Server 2012 R2, vous devez installer la mise à jour KB3090973 disponible à l’adresse [https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973).  

## <a name="create-an-availability-group-for-distributed-transactions"></a>Créer un groupe de disponibilité pour les transactions distribuées

Configurez un groupe de disponibilité pour prendre en charge les transactions distribuées. Définissez le groupe de disponibilité pour permettre à chaque base de données de s’inscrire comme gestionnaire de ressources. Cet article explique comment configurer un groupe de disponibilité de sorte que chaque base de données puisse être un gestionnaire de ressources dans DTC.



Vous pouvez créer un groupe de disponibilité pour les transactions distribuées sur [!INCLUDE[SQL2016](../../../includes/sssql15-md.md)] ou ultérieur. Pour créer un groupe de disponibilité pour des transactions distribuées, incluez `DTC_SUPPORT = PER_DB` dans la définition du groupe de disponibilité. Le script suivant crée un groupe de disponibilité pour des transactions distribuées. 

```sql
CREATE AVAILABILITY GROUP MyAG
   WITH (
      DTC_SUPPORT = PER_DB  
      )
   FOR DATABASE DB1, DB2
   REPLICA ON
      'Server1' WITH (
         ENDPOINT_URL = 'TCP://SERVER1.corp.com:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         ),
      'Server2' WITH (
         ENDPOINT_URL = 'TCP://SERVER2.corp.com:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         )
```

>[!NOTE]
>Le script précédent est un simple exemple de groupe de disponibilité et n’est pas conçu pour un quelconque environnement de production spécifique. 

## <a name="alter-an-availability-group-for-distributed-transactions"></a>Modifier un groupe de disponibilité pour les transactions distribuées

Vous pouvez modifier un groupe de disponibilité pour les transactions distribuées sur [!INCLUDE[SQL2017](../../../includes/sssqlv14-md.md)] ou ultérieur. Pour modifier un groupe de disponibilité pour les transactions distribuées, incluez `DTC_SUPPORT = PER_DB` dans le script `ALTER AVAILABILITY GROUP`. L’exemple de script change le groupe de disponibilité pour qu’il prenne en charge les transactions distribuées. 

```sql
ALTER AVAILABILITY GROUP MyaAG
   SET (
      DTC_SUPPORT = PER_DB  
      );
```

>[!NOTE]
>Depuis [!INCLUDE[SQL2016](../../../includes/sssql15-md.md)] Service Pack 2, vous pouvez modifier un groupe de disponibilité pour les transactions distribuées. Pour les versions de [!INCLUDE[SQL2016](../../../includes/sssql15-md.md)] antérieures au Service Pack 2, vous devez supprimer et recréer le groupe de disponibilité avec le paramètre `DTC_SUPPORT = PER_DB`. 

Pour désactiver les transactions distribuées, utilisez la commande Transact-SQL suivante :

```sql
ALTER AVAILABILITY GROUP MyaAG
   SET (
      DTC_SUPPORT = NONE  
      );
```

## <a name="distributed-transactions---technical-concepts"></a><a name="distTran"/>Transactions distribuées - concepts techniques

Une transaction distribuée s’étend sur deux bases de données ou plus. En tant que gestionnaire des transactions, DTC coordonne la transaction entre les instances de SQL Server et d’autres sources de données. Chaque instance du moteur de base de données [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] peut fonctionner comme gestionnaire de ressources. Quand un groupe de disponibilité est configuré avec `DTC_SUPPORT = PER_DB`, les bases de données peuvent fonctionner comme gestionnaires de ressources. Pour plus d'informations, consultez la documentation MS DTC.

Une transaction avec deux bases de données ou plus dans une même instance du moteur de base de données est en réalité une transaction distribuée. Cette instance gère la transaction distribuée de manière interne ; elle apparaît comme une transaction locale pour l'utilisateur. [!INCLUDE[SQL2017](../../../includes/sssqlv14-md.md)] promeut toutes les transactions entre bases de données vers DTC quand les bases de données sont dans un groupe de disponibilité configuré avec `DTC_SUPPORT = PER_DB`, même au sein d’une seule instance de SQL Server. 

Dans une application, une transaction distribuée est gérée de manière comparable à une transaction locale. À la fin de la transaction, l'application requiert que la transaction soit validée ou restaurée. La validation d'une transaction distribuée doit être gérée de façon particulière par le gestionnaire de transaction pour minimiser les risques qu'une défaillance du réseau entraîne la validation de la transaction par certains gestionnaires de ressources, alors qu'elle sera restaurée par d'autres. Pour cela, le processus de validation est géré en deux phases, une phase de préparation et une phase de validation, d’où son nom de « validation en deux phases ».

- **Phase de préparation**
   
   Lorsque le gestionnaire de transactions reçoit une requête de validation, il envoie une commande de préparation à tous les gestionnaires de ressources concernés par la transaction. Chaque gestionnaire de ressources effectue alors toutes les opérations nécessaires à l'enregistrement de la transaction, et tous les tampons contenant les images du journal de la transaction sont vidés sur le disque. Lorsque chaque gestionnaire de ressources a terminé la phase de préparation, il retourne un message de succès ou d'échec au gestionnaire de transactions.

- **Phase de validation**
   
   Si le gestionnaire de transactions reçoit des messages de préparation réussie de tous les gestionnaires de ressources, il envoie une commande de validation à chacun d'entre eux. Les gestionnaires de ressources peuvent alors effectuer la validation. Si tous les gestionnaires de ressources signalent le succès de la validation, le gestionnaire de transactions envoie alors une notification de succès à l'application. Si l'un des gestionnaires de ressources indique un échec de la préparation, le gestionnaire de transactions envoie une commande de restauration à chaque gestionnaire de ressources et notifie l'échec de la validation à l'application.

### <a name="detailed-steps"></a>Procédure détaillée

La liste suivante explique comment l’application travaille avec DTC pour effectuer des transactions distribuées.

1. L’instance de SQL Server s’inscrit dans la transaction DTC. Ceci peut se produire quand il existe plusieurs gestionnaires de ressources dans la transaction, ou si le client demande qu’une transaction soit promue en transaction DTC.
2. Le client effectue un certain travail dans l’instance de SQL Server sous la transaction DTC.
3. Le client émet une validation ou un abandon pour la transaction DTC.
    - Si le client émet un abandon, la transaction est abandonnée immédiatement.
    - Si le client émet une validation, DTC commence le protocole de validation en deux phases en demandant à tous les gestionnaires de ressources de la transaction de préparer la transaction.
4. DTC informe tous les gestionnaires de ressources de valider la transaction une fois qu’ils ont tous accusé réception de la phase de préparation. Si quoi que ce soit empêche la réussite de l’accusé de réception, DTC abandonne la transaction. 

### <a name="effects-of-configuring-an-availability-group-for-distributed-transactions"></a>Effets de la configuration d’un groupe de disponibilité pour les transactions distribuées

Chaque entité participant à une transaction distribuée est appelée un gestionnaire de ressources. Voici des exemples de gestionnaires de ressources :

* Une instance de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]. 
* Une base de données dans un groupe de disponibilité qui a été configuré pour les transactions distribuées.
* Le service DTC, qui peut aussi être un gestionnaire de transactions.
* Autres sources de données. 

Pour pouvoir participer à des transactions distribuées, une instance de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] s’inscrit auprès d’un DTC. Normalement, l’instance de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] s’inscrit auprès de DTC sur le serveur local. Chaque instance de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] crée un gestionnaire de ressources avec un identificateur de gestionnaire de ressources unique (RMID, Resource Manager Identifier) et l’inscrit auprès de DTC. Dans la configuration par défaut, toutes les bases de données sur une instance de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] utilisent le même RMID. 

Quand une base de données est dans un groupe de disponibilité, la copie en lecture-écriture de la base de données, ou réplica principal, peut être déplacée dans une autre instance de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]. Pour prendre en charge les transactions distribuées pendant ce déplacement, chaque base de données doit agir comme un gestionnaire de ressources distinct et doit avoir un RMID unique. Quand un groupe de disponibilité a `DTC_SUPPORT = PER_DB`, [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] crée un gestionnaire de ressources pour chaque base de données et s’inscrit auprès de DTC en utilisant un RMID unique. Dans cette configuration, la base de données est un gestionnaire de ressources pour les transactions DTC.

Pour plus d’informations sur les transactions distribuées dans [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)], consultez [Transactions distribuées](#distTran).

## <a name="manage-unresolved-transactions"></a>Gérer les transactions non résolues

Le résultat des transactions actives qui est produit pendant le changement du RMID ne peut pas être récupéré après un basculement. La raison en est que le RMID utilisé par SQL Server pour s’inscrire et le RMID utilisé par SQL Server pour la récupération sont différents. Le changement de RMID peut se produire dans les cas suivants :

* Modification de `DTC_SUPPORT` pour un groupe de disponibilité. 
* Ajout ou suppression d’une base de données dans un groupe de disponibilité. 
* Suppression d’un groupe de disponibilité.

Dans les cas précédents, si le réplica principal bascule sur une nouvelle instance de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)], l’instance tente de contacter DTC pour identifier le résultat de la transaction. DTC ne peut pas retourner le résultat, car le RMID utilisé par la base de données pour obtenir le résultat des transactions incertaines pendant la récupération n’a pas été inscrit auparavant. Par conséquent, la base de données passe à l’état SUSPECT.

Le nouveau journal des erreurs de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] a une entrée similaire à l’exemple suivant :

```
Microsoft Distributed Transaction Coordinator (MS DTC) 
failed to reenlist citing that the database RMID does 
not match the RMID [xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx] 
associated with the transaction.  Please manually resolve
the transaction.
    
SQL Server detected a DTC/KTM in-doubt transaction with UOW 
{yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy}.Please resolve it 
following the guideline for Troubleshooting DTC Transactions.
```

L’exemple précédent montre que DTC n’a pas pu réinscrire la base de données à partir du nouveau réplica principal dans la transaction qui a été créée après le basculement. L’instance de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] ne peut pas déterminer le résultat de la transaction distribuée et elle marque donc la base de données comme étant suspecte. La transaction est marquée en tant qu’unité de travail et elle est référencée par un GUID. Pour pouvoir récupérer la base de données, validez ou annulez la transaction manuellement. 

>[!WARNING]
>Quand vous validez ou que vous annulez manuellement une transaction, cela peut affecter une application. Vérifiez que l’action de validation ou d’annulation est cohérente avec les spécifications de votre application. 

Exécutez seulement un des scripts suivants :

   * Pour valider la transaction, modifiez et exécutez le script suivant : remplacez le `yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy` par l’unité de travail de la transaction incertaine provenant du message d’erreur précédent puis exécutez :

   ```sql
   KILL 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy' WITH COMMIT
   ```

   * Pour annuler la transaction, modifiez et exécutez le script suivant : remplacez le `yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy` par l’unité de travail de la transaction incertaine provenant du message d’erreur précédent puis exécutez :

   ```sql
   KILL 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy' WITH ROLLBACK
   ```

Après avoir validé ou annulé la transaction, vous pouvez utiliser `ALTER DATABASE` pour placer la base de données en ligne. Modifiez et exécutez le script suivant - utilisez le nom de la base de données pour le nom de la base de données suspecte :

   ```sql
   ALTER DATABASE [DB1] SET ONLINE
   ```

Pour plus d’informations sur la résolution des transactions incertaines, consultez [Résoudre les transactions manuellement](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754134(v=ws.10)).

## <a name="next-steps"></a>Étapes suivantes  

[Transactions distribuées](/dotnet/framework/data/adonet/distributed-transactions)

[Groupes de disponibilité Always On : Interopérabilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)  
  
[Transactions : groupes de disponibilité AlwaysOn et mise en miroir de bases de données](transactions-always-on-availability-and-database-mirroring.md)  

[Prise en charge des transactions XA](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753563(v=ws.10))

[Fonctionnement : Session/SPID (-2) pour les transactions DTC](/archive/blogs/bobsql/how-it-works-sessionspid-2-for-dtc-transactions)