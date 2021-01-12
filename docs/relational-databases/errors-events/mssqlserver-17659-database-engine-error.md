---
description: MSSQLSERVER_17659
title: MSSQLSERVER_17659
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 17659 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: b906310eb23bd1f9416cd792d95adfa79f72302c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099355"
---
# <a name="mssqlserver_17659"></a>MSSQLSERVER_17659
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|17659|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|DEMO_SYSCATUPDATE|
|Texte du message|L’ID de table système \%d a été mis à jour directement dans l’ID de base de données \%d et la cohérence du cache n’a peut-être pas été préservée. <br/> SQL Server doit être redémarré.|
||

## <a name="explanation"></a>Explication

Cette erreur indique qu’un objet système a été mis à jour directement. La mise à jour manuelle des tables système n’est pas prise en charge. Les tables système doivent être mises à jour par le moteur de base de données uniquement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quand [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] détecte les modifications apportées par l’utilisateur aux tables système, l’erreur 17659 est générée. Un événement qui ressemble à ce qui suit est consigné dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou dans le journal des applications de l’Observateur d’événements dans ce scénario.

> Nom du journal : Application  
Source : MSSQLServer  
ID d’événement : 17659  
Catégorie de la tâche : Serveur  
Niveau : Information  
Description : Avertissement : l’ID de table système \%d a été mis à jour directement dans l’ID de base de données %d et la cohérence du cache n’a peut-être pas été préservée. SQL Server doit être redémarré.

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