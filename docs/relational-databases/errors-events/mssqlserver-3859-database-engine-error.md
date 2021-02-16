---
description: MSSQLSERVER_3859
title: MSSQLSERVER_3859
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3859 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 7fff2944c0fdb3fc6d80326841b9950d3591f12e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348804"
---
# <a name="mssqlserver_3859"></a>MSSQLSERVER_3859
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|3859|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|DBCC_CHECKCAT_DIRECT_UPDATE|
|Texte du message|Avertissement : Le catalogue système a été mis à jour directement dans la base de données avec l’ID \%d, le plus récemment à %S_DATE.|
||

## <a name="explanation"></a>Explication

Cette erreur indique qu’un utilisateur a apporté des modifications dans les tables système. La mise à jour manuelle des tables système n’est pas prise en charge. Les tables système doivent être mises à jour par le moteur de base de données uniquement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] détecte les modifications apportées par l’utilisateur dans les tables système, l’erreur 3859 est générée dans les deux scénarios suivants :

- Scénario 1

    Un événement qui ressemble au suivant est journalisé dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou dans le journal des applications de l’Observateur d’événements quand vous démarrez une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contient une table système mise à jour manuellement :

    > Nom du journal : Application  
    Source : ID d’événement MSSQLSERVER : 3859  
    Catégorie de la tâche : Serveur  
    Niveau : Information  
    Description : Avertissement : Le catalogue système a été mis à jour directement dans la base de données avec l’ID \%d, le plus récemment à **date_heure**  

- Scénario 2  

    Le message d’avertissement suivant est retourné lorsque vous exécutez la commande `DBCC_CHECKDB` après la mise à jour manuelle d’une table système :

    > Résultats DBCC pour « **nom_base_de_données** ».  
    Msg 8992, Niveau 16, État 1, Ligne 1  
    Consultez le message du catalogue Msg 3859, État 1 : Avertissement : Le catalogue système a été mis à jour directement dans la base de données avec l’ID \%d, le plus récemment à **date_heure**.  
    CHECKDB a trouvé 0 erreur d’allocation et 0 erreur de cohérence dans la base de données « **nom_base_de_données** ».  
    Exécution de DBCC terminée. Si DBCC a affiché des messages d'erreur, contactez l'administrateur système.

## <a name="user-action"></a>Action requise

Pour résoudre ce problème, utilisez l’une des méthodes suivantes.

- Méthode 1

    Si vous disposez d’une sauvegarde saine de la base de données, restaurez la base de données à partir de cette sauvegarde.  
    > [!NOTE]
    > Cette méthode fonctionne uniquement s’il n’y a pas d’incohérences dans les métadonnées de la sauvegarde.  

- Méthode 2  

    Si vous ne pouvez pas restaurer la base de données à partir d’une sauvegarde, exportez les données et les objets vers une nouvelle base de données. Ensuite, transférez le contenu de la base de données mise à jour manuellement dans la nouvelle base de données. Remarque : Vous ne pouvez pas corriger les incohérences présentes dans les catalogues système à l’aide des options REPAIR des commandes DBCC CHECKDB. Par conséquent, étant donné qu’elle ne peut pas réparer les métadonnées endommagées, la commande ne fournit aucune recommandation concernant le niveau de réparation.

    > [!NOTE]
    > Vous pouvez afficher les données des tables système à l’aide des vues du catalogue système.

## <a name="more-information"></a>Informations complémentaires

Pour plus d’informations, consultez : [Tables de base système](../system-tables/system-base-tables.md).