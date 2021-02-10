---
description: Paramètres du projet (Synchronisation) (MySQLToSQL)
title: Paramètres du projet (synchronisation) (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 42061ff7-e6e7-497b-a0d9-440b9cf5986c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 3f778b2bd361fab5ac1210005076692c813bb4b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017309"
---
# <a name="project-settings-synchronization-mysqltosql"></a>Paramètres du projet (Synchronisation) (MySQLToSQL)
Les **paramètres du projet** de synchronisation vous permettent de configurer le mode de synchronisation des objets de base de données MySQL avec SQL Server objets de base de données.  
  
Les actions par défaut spécifient les paramètres par défaut pour l’actualisation des objets de la base de données MySQL et pour la synchronisation des objets avec la base de données SQL Server. Pour plus d’informations, consultez [Actualiser à partir de la base de données &#40;MySQLToSQL&#41;](../../ssma/mysql/refresh-from-database-mysqltosql.md)  
  
Vous pouvez accéder à deux pages de synchronisation différentes qui contiennent les mêmes paramètres :  
  
-   Pour spécifier les paramètres de tous les futurs projets SSMA, dans le menu **Outils** , cliquez sur **paramètres DefaultProject**, puis sur **synchronisation** en bas du volet gauche.  
  
-   Pour spécifier les paramètres du projet actif, dans le menu **Outils** , cliquez sur **paramètres du projet**, puis sur **synchronisation** en bas du volet gauche.  
  
## <a name="options"></a>Options  
  
##### <a name="misc"></a>Divers  
  
##### <a name="attempts"></a>Tentatives  
Fournit les informations relatives au nombre d’objets Pass à charger dans SQL Server. Le chargement d’objets dans SQL Server est généralement effectué en plusieurs passes. Les objets dont le chargement a échoué lors de la première passe, tels que les clés étrangères, peuvent être chargés dans la passe suivante.  
  
La valeur par défaut est 2.  
  
## <a name="synchronization-for-mysql"></a>Synchronisation pour MySQL  
  
##### <a name="action-on-local-and-remote-object-change"></a>Action sur la modification de l’objet local et distant  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque la définition d’objet change dans SSMA et sur le serveur de base de données.  
  
-   Si vous sélectionnez actualiser à partir de la base de données, SSMA chargera les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez Ignorer, SSMA n’effectue aucune action d’actualisation.  
  
##### <a name="action-on-local-object-change"></a>Action sur la modification de l’objet local  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque l’objet change dans SSMA.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA chargera les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **Ignorer**, SSMA n’effectue aucune action d’actualisation.  
  
##### <a name="action-on-remote-object-change"></a>Action sur la modification de l’objet distant  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque les objets changent sur le serveur de base de données.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA chargera les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **Ignorer**, SSMA n’effectue aucune action d’actualisation.  
  
##### <a name="action-when-local-object-metadata-is-missing"></a>Action lorsque les métadonnées de l’objet local sont manquantes  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque les métadonnées locales sont manquantes.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA chargera les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **Ignorer**, SSMA n’effectue aucune action d’actualisation  
  
## <a name="synchronization-for-sql-server"></a>Synchronisation pour SQL Server  
  
##### <a name="action-on-local-and-remote-object-change"></a>Action sur la modification de l’objet local et distant  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque la définition d’objet change dans SSMA et sur le serveur de base de données.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA chargera les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA met à jour les objets dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
  
-   Si vous sélectionnez **Ignorer**, SSMA n’effectue aucune action d’actualisation.  
  
##### <a name="action-on-local-object-change"></a>Action sur la modification de l’objet local  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque l’objet change dans SSMA.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA chargera les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA met à jour l’objet dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
  
-   Si vous sélectionnez **Ignorer**, SSMA n’effectue aucune action d’actualisation.  
  
##### <a name="action-on-remote-object-change"></a>Action sur la modification de l’objet distant  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque les objets changent sur le serveur de base de données.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA chargera les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA met à jour l’objet dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
  
-   Si vous sélectionnez **Ignorer**, SSMA n’effectue aucune action d’actualisation.  
  
##### <a name="action-when-local-object-metadata-is-missing"></a>Action lorsque les métadonnées de l’objet local sont manquantes  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque les métadonnées locales sont manquantes.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA sélectionne l’option Actualiser à partir de la base de données lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA supprime l’objet de la base de données lorsque la condition est remplie.  
  
-   Si vous sélectionnez **Ignorer**, SSMA n’effectue aucune action d’actualisation.  
  
