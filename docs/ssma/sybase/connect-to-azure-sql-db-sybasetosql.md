---
description: Se connecter à Azure SQL Database (SybaseToSQL)
title: Se connecter à Azure SQL Database (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 96538007-1099-40c8-9902-edd07c5620ee
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 93d1901cd5701cbdea537b947aa93de64fe59661
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056714"
---
# <a name="connect-to-azure-sql-database--sybasetosql"></a>Se connecter à Azure SQL Database (SybaseToSQL)
Utilisez la boîte de dialogue se connecter au Azure SQL Database pour vous connecter à la base de données Azure SQL Database que vous souhaitez migrer.  
  
Pour accéder à cette boîte de dialogue, dans le menu **fichier** , sélectionnez **se connecter à Azure SQL Database**. Si vous vous êtes connecté précédemment, la commande se **reconnecte à Azure SQL Database.**  
  
## <a name="options"></a>Options  
**Nom de serveur**  
  
Sélectionnez ou entrez le nom du serveur pour la connexion à Azure SQL Database.  
  
**Sauvegarde de la base de données**  
  
Sélectionnez, entrez ou **recherchez** le nom de la base de données.  
  
> [!IMPORTANT]  
> SSMA pour Sybase ne prend pas en charge la connexion à la base de données Master dans Azure SQL Database.  
  
**Nom d'utilisateur**  
  
Entrez le nom d’utilisateur que SSMA utilisera pour se connecter à la base de données Azure SQL Database  
  
**Mot de passe**  
  
Entrez le mot de passe correspondant au nom d'utilisateur indiqué.  
  
**Encrypt**  
  
SSMA recommande la connexion chiffrée à Azure SQL Database.  
  
## <a name="create-azure-database"></a>Créer une base de données Azure  
S’il n’existe aucune base de données dans le compte Azure SQL Database, vous pouvez créer la toute première base de données.  
  
Pour créer une base de données pour la première fois, procédez comme suit :  
  
1.  Cliquez sur le bouton Parcourir qui est présent dans la boîte de dialogue se connecter à Azure SQL Database  
  
2.  S’il n’y a aucune base de données, les deux éléments de menu suivants s’affichent.  
  
    1.  **(aucune base de données trouvée)** qui est désactivée et grisée tout le temps  
  
    2.  **Créer une nouvelle base de données** qui n’est activée que si aucune base de données n’est présente sur Azure SQL Database compte. Lorsque vous cliquez sur cet élément de menu, la boîte de dialogue créer une base de données Azure est présente avec le nom et la taille de la base de données.  
  
3.  Au moment de la création de la base de données, les deux paramètres suivants sont fournis comme entrée :  
  
    1.  **Nom de la base de données :** Entrez le nom de la base de données.  
  
    2.  **Taille de la base de données :** Sélectionnez la taille de base de données que vous devez créer dans Azure SQL Database compte.  
  
