---
description: Support de la haute disponibilité et de la récupération d’urgence pour les Pilotes Microsoft pour PHP pour SQL Server | Microsoft Docs
title: Prise en charge de la haute disponibilité et de la récupération d’urgence pour les pilotes Microsoft pour PHP pour SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 07/31/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 73a80821-d345-4fea-b076-f4aabeb4af3e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4a5c5747dad97cc54859cb9b40b966cec66970fc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058398"
---
# <a name="support-for-high-availability-disaster-recovery"></a>Prise en charge des fonctionnalités de récupération d’urgence/haute disponibilité
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Cette rubrique aborde la prise en charge par [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] (ajoutée à la version 3.0) de la haute disponibilité et de la récupération d’urgence.

À compter de la version 3.0 des pilotes Microsoft pour PHP pour SQL Server, il est possible de spécifier l’écouteur d’un groupe de disponibilité de récupération d’urgence haute disponibilité ou d’une instance de cluster de basculement en tant que serveur dans la chaîne de connexion.

La propriété de connexion **MultiSubnetFailover** indique que l’application est déployée dans un groupe de disponibilité ou une instance de cluster de basculement et que le pilote tente de se connecter à la base de données sur l’instance SQL Server principale en essayant toutes les adresses IP. Spécifiez toujours **MultiSubnetFailover=True** lorsque vous vous connectez à un écouteur de groupe de disponibilité SQL Server ou à une instance de cluster de basculement SQL Server. Si l’application est connectée à une base de données AlwaysOn en basculement, la connexion d’origine sera interrompue. L’application devra établir une nouvelle connexion pour continuer de fonctionner après le basculement.

Pour plus d’informations sur groupes de disponibilité AlwaysOn, consultez la page Docs [Haute disponibilité et récupération d’urgence](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md).

## <a name="transparent-network-ip-resolution-tnir"></a>Résolution d’adresses IP réseau transparente

La résolution d’adresses IP réseau transparente (TNIR) est une révision de la fonctionnalité **MultiSubnetFailover** existante. Elle affecte la séquence de connexion du pilote lorsque la première adresse IP résolue du nom d’hôte ne répond pas et qu’il existe plusieurs adresses IP associées au nom d’hôte. L’option de connexion correspondante est **TransparentNetworkIPResolution**. Avec **MultiSubnetFailover**, elle offre les quatre séquences de connexion suivantes : 

- TNIR activé et **MultiSubnetFailover** désactivé : Une adresse IP est tentée, suivie de toutes les adresses IP en parallèle.
- TNIR activé et **MultiSubnetFailover** activé : Toutes les adresses IP sont tentées en parallèle.
- TNIR désactivé et **MultiSubnetFailover** désactivé : Toutes les adresses IP sont tentées l’une après l’autre.
- TNIR désactivé et **MultiSubnetFailover** activé : Toutes les adresses IP sont tentées en parallèle.

TNIR est activé par défaut et **MultiSubnetFailover** désactivé par défaut.

Voici un exemple d’activation de TNIR et de **MultiSubnetFailover** avec le pilote PDO_SQLSRV :

```
<?php
$serverName = "yourservername";
$username = "yourusername";
$password = "yourpassword";
$connectionString = "sqlsrv:Server=$serverName; TransparentNetworkIPResolution=Enabled; MultiSubnetFailover=yes";
try {
    $conn = new PDO($connectionString, $username, $password, array(PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION));
    // your code 
    // more of your code
    // when done, close the connection
    unset($conn);
} catch(PDOException $e) {
    print_r($e->errorInfo);
}
?>
```

## <a name="upgrading-to-use-multi-subnet-clusters-from-database-mirroring"></a>Mise à niveau pour utiliser des clusters de sous-réseaux multiples à partir de la mise en miroir de bases de données  
Une erreur de connexion se produit si les mots clés de connexion **MultiSubnetFailover** et **Failover_Partner** sont présents dans la chaîne de connexion. Une erreur se produit également si **MultiSubnetFailover** est utilisé et que SQL Server retourne une réponse de partenaire de basculement indiquant qu’il fait partie d’une paire de mise en miroir de bases de données.  
  
Si vous mettez à niveau une application PHP qui utilise actuellement la mise en miroir de bases de données vers un scénario de sous-réseaux multiples, supprimez la propriété de connexion **Failover_Partner**, remplacez-la par **MultiSubnetFailover** avec la valeur **True** et remplacez le nom du serveur dans la chaîne de connexion par un écouteur de groupe de disponibilité. Si une chaîne de connexion utilise **Failover_Partner** et **MultiSubnetFailover=true**, le pilote génère une erreur. Toutefois, si une chaîne de connexion utilise **Failover_Partner** et **MultiSubnetFailover=false** (ou **ApplicationIntent=ReadWrite**), l’application utilise la mise en miroir de bases de données.  
  
Le pilote retournera une erreur si la mise en miroir de bases de données est utilisée sur la base de données principale au sein du groupe de disponibilité, et si **MultiSubnetFailover=true** est utilisé dans la chaîne de connexion qui établit une connexion à une base de données principale au lieu d’un écouteur de groupe de disponibilité.  

[!INCLUDE[specify-application-intent_read-only-routing](~/includes/paragraph-content/specify-application-intent-read-only-routing.md)]


## <a name="see-also"></a>Voir aussi  
[Connexion au serveur](../../connect/php/connecting-to-the-server.md)  
