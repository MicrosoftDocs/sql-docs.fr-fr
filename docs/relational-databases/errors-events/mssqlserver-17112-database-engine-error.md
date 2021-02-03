---
description: MSSQLSERVER_17112
title: MSSQLSERVER_17112
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17112 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 29f6dd1b508a5833c465ef5af013e69de8d44077
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196796"
---
# <a name="mssqlserver_17112"></a>MSSQLSERVER_17112
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|17112|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|INIT_INVCOMMAND|
|Texte du message|Une option de démarrage a non valide a été fournie, soit à partir du Registre, soit depuis l’invite de commandes. Corrigez ou supprimez cette option.|
||

## <a name="explanation"></a>Explication

Cette erreur indique qu’une [option de démarrage du service moteur de base de données](../../database-engine/configure-windows/database-engine-service-startup-options.md) non valide a été spécifiée. Quand une option de démarrage n’est pas correctement spécifiée, SQL Server ne peut pas démarrer ou risque de ne pas s’exécuter comme prévu. L’erreur 17112 est également déclenchée.

Dans certains cas, l’instance peut démarrer mais, quand vous examinez le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les paramètres de démarrage semblent incorrects :

> \<Datetime> Server Paramètres de démarrage du Registre :  
\<Datetime> Server -d D:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\DATA\master.mdf  
\<Datetime> Server -e D:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\LOG\ERRORLOG  
\<Datetime> Server -l D:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\DATA\mastlog.ldf  
\<Datetime> Server -T1118 -g512

Notez que les deux derniers paramètres de démarrage se trouvent sur la même ligne.

Vous pouvez également remarquer dans certains cas que l’ajout des paramètres de démarrage nécessaires n’avait pas l’effet escompté sur le comportement du serveur.

## <a name="possible-causes"></a>Causes possibles

Vous rencontrez ces problèmes pour les raisons suivantes :

- Utilisation de paramètres de démarrage qui ne sont pas présents dans la liste valide des paramètres de démarrage
- Spécification des paramètres de démarrage sans les délimiteurs appropriés [;]
- Copie et collage des paramètres de démarrage d’éditeurs de texte qui ont introduit des caractères spéciaux invisibles, [par exemple, un espace avant le -T]
- Utilisation d’une casse incorrecte pour le paramètre de démarrage

## <a name="user-action"></a>Action requise

Utilisez l’outil Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour fournir et valider les paramètres de démarrage spécifiés pour l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vérifiez que chacun des paramètres de démarrage est délimité correctement et qu’aucun caractère spécial n’est présent.

## <a name="more-information"></a>Informations complémentaires

Pour plus d’informations sur ce sujet, consultez les rubriques ci-dessous :

- [Options de démarrage du service moteur de base de données](../../database-engine/configure-windows/database-engine-service-startup-options.md)
- [Services SCM - Configurer les options de démarrage du serveur](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md)