---
description: Transactions - Groupes de disponibilité Always On et mise en miroir de bases de données
title: 'Transactions : groupes de disponibilité et mise en miroir de bases de données'
descripton: Learn about cross-database and distributed transaction support for SQL Server Always On availability groups and database mirroring.
ms.custom: seo-lt-2019
ms.date: 12/11/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- cross-database transactions [SQL Server]
- transactions [database mirroring]
- Availability Groups [SQL Server], interoperability
- troubleshooting [SQL Server], cross-database transactions
ms.assetid: 9f7ed895-ad65-43e3-ba08-00d7bff1456d
author: cawrites
ms.author: chadam
ms.openlocfilehash: f2295f8694a9573b0e9c541ea817e9e03e70f83e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171081"
---
# <a name="transactions---availability-groups-and-database-mirroring"></a>Transactions - Groupes de disponibilité Always On et mise en miroir de bases de données
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Cet article décrit la prise en charge des transactions entre bases de données et distribuées pour des groupes de disponibilité Always On et la mise en miroir de bases de données.  

## <a name="support-for-distributed-transactions"></a>Prise en charge des transactions distribuées

SQL Server 2017 prend en charge les transactions distribuées pour les bases de données des groupes de disponibilité. Cette prise en charge comprend les bases de données sur la même instance de SQL Server ou les bases de données sur différentes instances de SQL Server. Les transactions distribuées ne sont pas prises en charge pour les bases de données configurées pour la mise en miroir de bases de données.

> [!NOTE]
> [!INCLUDE[SQL Server 2016](../../../includes/sssql16-md.md)]Service Pack 2 et versions ultérieures fournissent une prise en charge complète des transactions distribuées dans les groupes de disponibilité. 
> 
> Dans les versions de [!INCLUDE[SQL Server 2016](../../../includes/sssql16-md.md)] antérieures au Service Pack 2, les transactions distribuées entre bases de données (autrement dit, les transactions qui utilisent des bases de données sur la même instance de SQL Server) impliquant une base de données dans un groupe de disponibilité ne sont pas prises en charge.

Pour configurer un groupe de disponibilité pour les transactions distribuées, consultez [Configurer de groupe de disponibilité pour les transactions distribuées](configure-availability-group-for-distributed-transactions.md).

Pour plus d’informations, consultez :

- [Guide d’Administration de DTC](/previous-versions/windows/desktop/ms681291(v=vs.85))
- [Guide du développeur de DTC](/previous-versions/windows/desktop/ms679938(v=vs.85))
- [Informations de référence pour les programmeurs de DTC](/previous-versions/windows/desktop/ms686108(v=vs.85))

## <a name="sql-server-2016-sp1-and-before-support-for-cross-database-transactions-within-the-same-sql-server-instance"></a>SQL Server 2016 SP1 et versions antérieures : Prise en charge des transactions entre bases de données dans la même instance de SQL Server  

Dans SQL Server 2016 SP1 et versions antérieures, les transactions entre bases de données dans la même instance de SQL Server ne sont pas prises en charge pour les groupes de disponibilité. Une même instance de SQL Server ne peut pas héberger deux bases de données d’une transaction entre bases de données si l’une ou les deux bases de données sont dans un groupe de disponibilité. Cette limitation s’applique aussi quand ces bases de données font partie du même groupe de disponibilité.  
  
En outre, les transactions de bases de données croisées ne sont non plus prises en charge pour la mise en miroir de bases de données.  
  
##  <a name="sql-server-2016-sp1-and-before-support-for-distributed-transactions"></a><a name="dtcsupport"></a> SQL Server 2016 SP1 et versions antérieures : Prise en charge des transactions distribuées  
Les transactions distribuées sont prises en charge avec les groupes de disponibilité quand les bases de données sont hébergées par différentes instances de SQL Server. Cela s’applique aussi aux transactions distribuées entre des instances de SQL Server et un autre serveur compatible DTC.  
 
MSDTC (Microsoft Distributed Transaction Coordinator ou DTC) est un service Windows qui fournit une infrastructure de transaction pour les systèmes distribués. MSDTC permet aux applications clientes d’intégrer plusieurs sources de données dans une transaction, laquelle est ensuite validée sur tous les serveurs inclus dans la transaction. Par exemple, vous pouvez utiliser MSDTC pour coordonner des transactions qui s’étendent sur plusieurs bases de données sur des serveurs différents.

SQL Server 2016 introduit la possibilité d’utiliser des transactions distribuées dans lesquelles une ou plusieurs bases de données de la transaction sont dans un groupe de disponibilité. Avant SQL Server 2016, les transactions distribuées n’étaient pas prises en charge pour les bases de données dans les groupes de disponibilité. SQL Server 2016 peut inscrire un gestionnaire de ressources par base de données. Cette nouvelle fonctionnalité permet aux transactions distribuées d’inclure des bases de données dans les groupes de disponibilité.
  
 Les conditions suivantes doivent être remplies :  
  
-   Les groupes de disponibilité doivent s’exécuter sur Windows Server 2012 R2 ou version supérieure. Pour Windows Server 2012 R2, vous devez installer la mise à jour KB3090973 disponible à l’adresse [https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973).  
  
-   Les groupes de disponibilité doivent être créés avec la commande **CREATE AVAILABILITY GROUP** et la clause **WITH DTC\_SUPPORT = PER_DB**. Actuellement, vous ne pouvez pas modifier un groupe de disponibilité.  

- Toutes les instances de SQL Server qui participent au groupe de disponibilité doivent avoir la version SQL Server 2016 ou ultérieure.
 
 ## <a name="non-support-for-distributed-transactions"></a>Absence de prise en charge des transactions distribuées
 Les cas spécifiques où les transactions distribuées ne sont pas prises en charge sont notamment les suivants :
 
 - Dans SQL Server 2016 SP1 et versions antérieures, quand plusieurs bases de données impliquées dans la transaction sont dans le même groupe de disponibilité.
 
 - Dans SQL Server 2016 SP1 et versions antérieures, quand au moins une base de données est dans un groupe de disponibilité et qu’une autre se trouve sur la même instance de SQL Server. 
 
 - Le groupe de disponibilité n’a pas été créé avec l’activation de la transaction distribuée.
 
 - Mise en miroir de bases de données.
 
 > [!IMPORTANT]
 > Déterminez le résultat par défaut approprié des transactions que DTC ne peut pas résoudre pour votre environnement.  Pour plus d’informations sur la façon de configurer le résultat par défaut, consultez [ (option de configuration de serveur)](../../../database-engine/configure-windows/in-doubt-xact-resolution-server-configuration-option.md).
  
## <a name="example-scenario-with-database-mirroring"></a>Exemple de scénario de mise en miroir de bases de données  
 L'exemple de mise en miroir de bases de données suivant illustre la manière dont une incohérence logique pourrait se produire. Dans cet exemple, une application utilise une transaction entre bases de données pour insérer deux lignes de données : une ligne est insérée dans une table dans une base de données mise en miroir, A, et l'autre ligne est insérée dans une table dans une autre base de données, B. La base de données A est mise en miroir en mode haute sécurité avec basculement automatique. Pendant la validation de la transaction, la base de données A devient indisponible et la session de mise en miroir bascule automatiquement vers le miroir de la base de données A.  
  
 Après le basculement, il se peut que la transaction entre bases de données soit validée correctement dans la base de données B mais pas dans la base de données basculée. Par exemple, quand le serveur principal d’origine de la base de données A n’a pas envoyé le journal pour la transaction entre bases de données au serveur miroir avant le basculement. Après le basculement, cette transaction n'existerait pas sur le nouveau serveur principal. Les bases de données A et B deviendraient incohérentes car les données insérées dans la base de donnes B demeureraient intactes, alors que celles insérées dans la base de données A auraient été perdues.  
  
 Un scénario semblable peut se produire en cas d’utilisation d'une transaction MS DTC. Par exemple, après un basculement, le nouveau principal contacte MS DTC. Mais MS DTC n'a aucune connaissance du nouveau serveur principal et interrompt toute transaction en « préparation pour validation », considérée comme validée dans d'autres bases de données.  
  
> [!NOTE]  
>  L’utilisation de la mise en miroir de bases de données ou de groupes de disponibilité avec DTC d’une façon non approuvée dans cet article n’est pas prise en charge.  Cela ne signifie pas que certains aspects du produit sans rapport avec DTC ne sont pas pris en charge, mais qu’aucun problème résultant de l’utilisation incorrecte des transactions distribuées n’est traité.  
  
## <a name="next-steps"></a>Étapes suivantes  
 [Groupes de disponibilité Always On : Interopérabilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)  
  
