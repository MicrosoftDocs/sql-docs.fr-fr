---
description: Assistant Source de données, écran 4 (ODBC Driver pour SQL Server)
title: Assistant Source de données, écran 4 (ODBC Driver for SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 09/27/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6d1f72323f3db7db1f0f9c0084ef3da9de034490
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88462181"
---
# <a name="data-source-wizard-screen-4"></a>Assistant Source de données, écran 4

Spécifiez la langue à utiliser pour les messages SQL Server et la traduction de jeu de caractères ; indiquez si ODBC Driver for SQL Server doit utiliser des paramètres régionaux. Vous pouvez également contrôler l'enregistrement des paramètres des requêtes longues et des statistiques du pilote.

## <a name="options"></a>Options

### <a name="change-the-language-of-sql-server-system-messages-to"></a>Modifier la langue des messages systèmes de SQL Server

Chaque instance de SQL Server peut avoir plusieurs jeux de messages système, chacun dans une langue différente (par exemple, anglais, espagnol, français, etc.). Si une source de données est définie contre un serveur qui possède plusieurs jeux de messages systèmes, vous pouvez spécifier la langue que vous voulez utiliser pour vos messages systèmes. Dans la liste, cliquez sur la langue. Cette option n’est pas disponible s’il n’y a qu’une seule langue installée sur SQL Server.

### <a name="use-strong-encryption-for-data"></a>Utiliser le chiffrement renforcé pour les données

Si l'option est sélectionnée, les données transférées via les connexions établies à l'aide de ce DSN seront chiffrées. Les noms de connexion sont chiffrés par défaut, même si la case à cocher est désactivée.

### <a name="trust-server-certificate"></a>Faire confiance au certificat de serveur

Cette option s’applique uniquement si l’option **Utiliser le chiffrement renforcé pour les données** est activée, Si cette option est sélectionnée, le certificat du serveur n’aura pas le nom d’hôte de serveur correct et ne sera pas émis par une autorité de certification approuvée. 

### <a name="perform-translation-for-character-data"></a>Traduire les données de type caractère

Lorsque cette case est cochée, ODBC Driver for SQL Server convertit les chaînes ANSI envoyées entre l’ordinateur client et SQL Server avec Unicode. Le pilote ODBC fait parfois la conversion entre la page de codes SQL Server et Unicode sur l’ordinateur client. Il faut pour cela que la page de codes utilisée par SQL Server soit l’une des pages de codes disponibles sur l’ordinateur client.

Lorsque cette case à cocher est désactivée, aucune traduction de caractères étendus des chaînes de caractères ANSI n'est réalisée lorsqu'ils sont transférés entre l'application cliente et le serveur. Si l’ordinateur client utilise une page de codes ANSI (ACP) différente de celle de SQL Server, les caractères étendus présents dans les chaînes de caractères ANSI risquent d’être mal interprétés. Si l’ordinateur client utilise la même page de codes pour son ACP que SQL Server, les caractères étendus sont interprétés correctement.

### <a name="use-regional-settings-when-outputting-currency-numbers-dates-and-times"></a>Utiliser les paramètres régionaux lors de la copie de devises, de nombres, de dates et d'heures

Spécifie que le pilote utilise les paramètres régionaux de l'ordinateur client pour mettre en forme les devises, les nombres, les dates et les heures dans les chaînes de sortie de caractères. Le pilote utilise le paramètre régional par défaut pour le compte de connexion Windows de l'utilisateur qui se connecte via la source de données. Sélectionnez cette option pour les applications qui affichent uniquement les données, pas pour les applications qui traitent les données.

### <a name="save-long-running-queries-to-the-log-file"></a>Enregistrer les requêtes à long terme dans le fichier journal

Spécifie que le pilote enregistre toutes les requêtes qui dépassent la valeur **Temps des requêtes longues**. Les requêtes longues sont enregistrées dans le fichier spécifié. Pour spécifier un fichier journal, tapez le chemin d’accès complet et le nom de fichier dans la zone ou cliquez sur **Parcourir** pour sélectionner un fichier journal en naviguant parmi les répertoires de fichiers existants.

### <a name="long-query-time-milliseconds"></a>Durée de requête longue (millisecondes)

Spécifie une valeur de seuil, en millisecondes, pour l'enregistrement de requête longue. Toute requête qui prend plus de temps que cette durée en millisecondes pour s'exécuter est enregistrée.

### <a name="log-odbc-driver-statistics-to-the-log-file"></a>Enregistrer les statistiques de pilote ODBC dans le fichier journal

Spécifie que les statistiques sont enregistrées. Les statistiques sont enregistrées dans le fichier spécifié. Pour spécifier un fichier journal, tapez le chemin d’accès complet et le nom de fichier dans la zone ou cliquez sur **Parcourir** pour sélectionner un fichier journal en naviguant parmi les répertoires de fichiers existants.

Le journal de statistiques est un fichier délimité par des tabulations qui peut être analysé dans Microsoft Excel ou toute autre application qui prend en charge les fichiers délimités par des tabulations.

### <a name="connect-retry-count"></a>Nombre de nouvelles tentatives de connexion

Spécifie le nombre de nouvelles tentatives de connexion après un échec.

### <a name="connect-retry-interval-seconds"></a>Intervalle avant nouvelle tentative de connexion (en secondes)

Spécifie le nombre de secondes entre les nouvelles tentatives de connexion. Pour plus d’informations sur ce fonctionnement et sur les options **Nombre de nouvelles tentatives de connexion**, consultez [Résilience de connexion dans Windows ODBC Driver](../../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md).

### <a name="back"></a>Précédent

Cliquez sur ce bouton pour revenir à la page précédente de l’Assistant.

### <a name="finish"></a>Finish

Si les informations spécifiées sur cet écran sont complètes, vous pouvez cliquer sur **Terminer**. Le nom de source de données est créé à partir de tous les attributs spécifiés sur différents écrans de l’Assistant, dont celui-ci ; il vous est offert la possibilité de le tester.

## <a name="next-steps"></a>Étapes suivantes

[Assistant Source de données, écran 3](../../../connect/odbc/windows/dsn-wizard-3.md)
