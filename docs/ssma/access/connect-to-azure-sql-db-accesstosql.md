---
description: Se connecter à Azure SQL Database (AccessToSQL)
title: Se connecter à Azure SQL Database (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connect to SQL Azure dialog box
ms.assetid: bf44b236-d9be-41ae-a5fd-bd73038e505f
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 57fadc47d241658b96f2d9a7087bcb174d72dcce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076310"
---
# <a name="connect-to-azure-sql-database-accesstosql"></a>Se connecter à Azure SQL Database (AccessToSQL)
Utilisez la boîte de dialogue se connecter au SQL Azure pour vous connecter à la base de données dans Azure SQL Database que vous souhaitez migrer.  
  
Pour accéder à cette boîte de dialogue, dans le menu **fichier** , sélectionnez **se connecter à SQL Azure**. Si vous vous êtes connecté précédemment, la commande se **reconnecte à SQL Azure.**  
  
## <a name="options"></a>Options  
**Nom de serveur**  
  
Sélectionnez ou entrez le nom du serveur pour la connexion à SQL Azure.  
  
**Sauvegarde de la base de données**  
  
Sélectionnez, entrez ou **recherchez** le nom de la base de données.  
  
> [!IMPORTANT]  
> SSMA pour Access ne prend pas en charge la connexion à la base de données Master dans SQL Azure.  
  
**Nom d'utilisateur**  
  
Entrez le nom d’utilisateur que SSMA utilisera pour se connecter à Azure SQL Database  
  
**Mot de passe**  
  
Entrez le mot de passe correspondant au nom d'utilisateur indiqué.  
  
**Encrypt**  
  
SSMA recommande la connexion chiffrée à SQL Azure.  
  
## <a name="create-database"></a>Créer une base de données  
Pour créer une base de données, procédez comme suit :  
  
1.  Cliquez sur le bouton Parcourir qui est présent dans la boîte de dialogue se connecter à SQL Azure  
  
2.  S’il n’y a aucune base de données, deux éléments de menu s’affichent  
  
    1.  **(aucune base de données trouvée)** qui est désactivée et grisée tout le temps  
  
    2.  **Créer une nouvelle base de données** qui est toujours activée, ce qui permet à l’utilisateur de créer une base de données. En cliquant sur cet élément de menu, la boîte de dialogue créer une base de données est présente avec le nom et la taille de la base de données.  
  
3.  Au moment de la création de la base de données, ces deux paramètres sont fournis comme entrée.  
  
    1.  **Nom de la base de données :** Entrez le nom de la base de données.  
  
    2.  **Taille de la base de données :** Sélectionnez la taille de base de données que vous devez créer dans SQL Azure compte.  
  
