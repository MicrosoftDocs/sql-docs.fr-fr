---
description: Paramètres du projet (synchronisation) (DB2ToSQL)
title: Paramètres du projet (synchronisation) (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: a5629a72-8c17-46a4-bb4d-19d51a0b98a2
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 4d96e3e3caeb1c40a9f4529f8d2c0ea7e0b1f717
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072123"
---
# <a name="project-settingssynchronization-db2tosql"></a>Paramètres du projet (synchronisation) (DB2ToSQL)
La page synchronisation de la boîte de dialogue **paramètres du projet** contient des paramètres qui personnalisent la manière dont SSMA charge et actualise les objets de base de données, tels que les tables et les procédures stockées, dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Les options actions par défaut spécifient les paramètres par défaut pour l’actualisation des objets de la base de données DB2 et pour la synchronisation des objets avec la base de données SQL Server. Pour plus d’informations, consultez [Actualiser à partir de la base de données &#40;DB2ToSQL&#41;](../../ssma/db2/refresh-from-database-db2tosql.md).  
  
Vous pouvez accéder à deux pages de synchronisation différentes qui contiennent les mêmes paramètres :  
  
-   Pour spécifier les paramètres de tous les futurs projets SSMA, dans le menu **Outils** , cliquez sur **paramètres de projet par défaut**, puis sur **synchronisation** en bas du volet gauche.  
  
-   Pour spécifier les paramètres du projet actif, dans le menu **Outils** , cliquez sur **paramètres du projet**, puis sur **synchronisation** en bas du volet gauche.  
  
## <a name="miscellaneous-options"></a>Options diverses  
**Tentatives**  
Spécifie le nombre de tentatives que SSMA doit effectuer lors du chargement des objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les objets qui ne sont pas chargés [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans la tentative actuelle seront tentés à nouveau jusqu’à ce que SSMA atteigne le nombre maximal de tentatives dans le processus de synchronisation en cours. L’ensemble de valeurs par défaut est **2**  
  
## <a name="synchronization-for-db2-options"></a>Synchronisation pour les options DB2  
**Action sur la modification de l’objet local et distant**  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque la définition d’objet change dans SSMA et sur le serveur de base de données. L’ensemble de valeurs par défaut est **Refresh à partir de la base de données**.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA chargera les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **Ignorer**, SSMA n’effectue aucune action d’actualisation.  
  
**Action sur la modification de l’objet local**  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque l’objet change dans SSMA. L’ensemble de valeurs par défaut est **Skip**.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA chargera les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **Ignorer**, SSMA n’effectue aucune action d’actualisation.  
  
**Action sur la modification de l’objet distant**  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque les objets changent sur le serveur de base de données. L’ensemble de valeurs par défaut est **Refresh à partir de la base de données**.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA chargera les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **Ignorer**, SSMA n’effectue aucune action d’actualisation.  
  
**Action lorsque les métadonnées de l’objet local sont manquantes**  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque les métadonnées locales sont manquantes. L’ensemble de valeurs par défaut est **Refresh à partir de la base de données**.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA chargera les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **Ignorer**, SSMA n’effectue aucune action d’actualisation.  
  
## <a name="synchronization-for-sql-server-options"></a>Synchronisation pour les options de SQL Server  
**Action sur la modification de l’objet local et distant**  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque la définition d’objet change dans SSMA et sur le serveur de base de données. La valeur par défaut définie est **écrire dans la base de données**.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA chargera les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA met à jour les objets dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
  
-   Si vous sélectionnez **Ignorer**, SSMA n’effectue aucune action d’actualisation.  
  
**Action sur la modification de l’objet local**  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque l’objet change dans SSMA. La valeur par défaut définie est **écrire dans la base de données**.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA chargera les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA met à jour l’objet dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
  
-   Si vous sélectionnez **Ignorer**, SSMA n’effectue aucune action d’actualisation.  
  
**Action sur la modification de l’objet distant**  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque les objets changent sur le serveur de base de données.  L’ensemble de valeurs par défaut est **Refresh à partir de la base de données**.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA chargera les définitions de base de données dans les métadonnées lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA met à jour l’objet dans la base de données en fonction du contenu des métadonnées SSMA lorsque la condition est remplie.  
  
-   Si vous sélectionnez **Ignorer**, SSMA n’effectue aucune action d’actualisation.  
  
**Action lorsque les métadonnées de l’objet local sont manquantes**  
Spécifie le paramètre par défaut dans la boîte de dialogue synchronisation lorsque les métadonnées locales sont manquantes. L’ensemble de valeurs par défaut est **Refresh à partir de la base de données**.  
  
-   Si vous sélectionnez **Actualiser à partir de la base de données**, SSMA sélectionne l’option **Actualiser à partir de la base de données** lorsque la condition est remplie.  
  
-   Si vous sélectionnez **écrire dans la base de données**, SSMA supprime l’objet de la base de données lorsque la condition est remplie.  
  
-   Si vous sélectionnez **Ignorer**, SSMA n’effectue aucune action d’actualisation.  
  
