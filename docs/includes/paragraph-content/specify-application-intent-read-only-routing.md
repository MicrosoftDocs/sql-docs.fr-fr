---
title: Fichier Include
description: Fichier Include
services: sql-database
author: MightyPen
ms.service: sql-database
ms.topic: include
ms.date: 04/05/2018
ms.author: genemi
ms.custom: include file
ms.openlocfilehash: 7086a69625b6ba365e5efb8ce3422799442cb05d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058387"
---
## <a name="specifying-application-intent"></a>Spécification de l'intention d'application

Le mot clé **ApplicationIntent** peut être spécifié dans votre chaîne de connexion. Les valeurs assignables sont **ReadWrite** ou **ReadOnly**. La valeur par défaut est **ReadWrite**.

Quand **ApplicationIntent=ReadOnly**, le client demande une charge de travail en lecture lors de la connexion. Le serveur applique l’intention au moment de la connexion et pendant une instruction de base de données **USE**.

Le mot clé **ApplicationIntent** ne fonctionne pas avec les bases de données en lecture seule héritées.  


#### <a name="targets-of-readonly"></a>Cibles de ReadOnly

Lorsqu’une connexion choisit **ReadOnly**, elle est affectée à l’une des configurations spéciales suivantes qui peuvent exister pour la base de données :

- [Always On](~/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)
    - Une base de données peut autoriser ou interdire les charges de travail en lecture sur la base de données AlwaysOn ciblée. Ce choix est contrôlé à l’aide de la clause **ALLOW_CONNECTIONS** des instructions Transact-SQL **PRIMARY_ROLE** et **SECONDARY_ROLE**.

- [Géoréplication](/azure/sql-database/sql-database-geo-replication-overview)

- [Scale-out en lecture](/azure/sql-database/sql-database-read-scale-out)

Si aucune cible spéciale n’est disponible, la base de données normale est utilisée pour la lecture.

&nbsp;

Le mot clé **ApplicationIntent** active le *routage en lecture seule*.


## <a name="read-only-routing"></a>Routage en lecture seule

Le routage en lecture seule est une fonctionnalité qui permet de garantir la disponibilité d'un réplica de base de données en lecture seule. Pour activer le routage en lecture seule, tous les éléments suivants s’appliquent :

- Vous devez vous connecter à un écouteur du groupe de disponibilité Always On.

- Dans la chaîne de connexion, le mot clé **ApplicationIntent** doit avoir la valeur **ReadOnly**.

- Le groupe de disponibilité doit être configuré par l'administrateur de base de données pour activer le routage en lecture seule.

Il est possible que plusieurs connexions utilisant toutes le routage en lecture seule ne se connectent pas toutes au même réplica en lecture seule. Les modifications apportées à la synchronisation de base de données ou à la configuration du routage du serveur peuvent entraîner des connexions clientes à différents réplicas en lecture seule. Vous pouvez vous assurer que toutes les demandes de lecture seule se connectent au même réplica en lecture seule. Pour vous en assurer, ne passez *pas* un écouteur de groupe de disponibilité au mot clé de chaîne de connexion de **Serveur**. Au lieu de cela, spécifiez le nom de l'instance en lecture seule.

Le routage en lecture seule peut prendre plus de temps que la connexion au serveur principal. Le délai plus long est dû au fait que le routage en lecture seule se connecte d’abord à la base de données principale, puis recherche la meilleure base de données secondaire lisible disponible. En raison de ces multiples étapes, vous devez augmenter le délai d’expiration de la connexion à au moins 30 secondes.