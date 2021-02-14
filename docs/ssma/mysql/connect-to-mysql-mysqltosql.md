---
description: Se connecter à MySQL (MySQLToSQL)
title: Se connecter à MySQL (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 94099d01-ab19-4d58-a172-340c86b4a0f3
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 33017262982effedd9fa85ea26818fdb7b2ae8ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068952"
---
# <a name="connect-to-mysql-mysqltosql"></a>Se connecter à MySQL (MySQLToSQL)
Utilisez la boîte de dialogue **se connecter à MySQL** pour vous connecter à la base de données MySQL que vous souhaitez migrer.  
  
Pour accéder à cette boîte de dialogue, dans le menu **fichier** , sélectionnez **se connecter à MySQL**. Si vous vous êtes connecté précédemment, la commande se **reconnecte à MySQL**.  
  
## <a name="options"></a>Options  
**Fournisseur**  
  
Le fournisseur MySQL disponible est le pilote MySQL ODBC 5,1 (approuvé).  
  
**Mode**  
  
Le mode par défaut est le mode standard. En mode standard, vous entrez ou sélectionnez des valeurs pour MySQL, le nom du serveur, le port du serveur, le nom d’utilisateur et le mot de passe.  
  
**Nom du serveur**  
  
Entrez le nom du serveur MySQL. Il s’agit d’une option de mode standard.  
  
**Port du serveur**  
  
Entrez le port du serveur. Le port du serveur par défaut est 3306. Il s’agit d’une option de mode standard.  
  
**Nom d'utilisateur**  
  
Entrez le nom d’utilisateur que SSMA utilisera pour se connecter à la base de données MySQL.  
  
**Mot de passe**  
  
Entrez le mot de passe correspondant au nom d'utilisateur indiqué.  
  
**SSL**  
  
Si vous souhaitez vous connecter en toute sécurité à MySQL, utilisez SSL (Secure Socket Layer) en activant la case à cocher **SSL** .  
  
**Configurer**  
  
Il fournit une option permettant de configurer la connexion à MySQL via SSL (Secure Socket Layer).  
  
> [!NOTE]  
> Pour activer la **configuration**, SSL doit avoir la valeur **true**.  
  
Lorsque vous cliquez sur le bouton « configurer », une boîte de dialogue s’affiche. Pour utiliser le chiffrement lors de la connexion à la base de données MySQL, le chemin d’accès aux trois fichiers de certificat suivants présents dans la boîte de dialogue doit être défini [Privacy Enhanced Mail les certificats (PEM)] :  
  
-   **Autorité de certification SSL :** Spécifie le chemin d’accès à un fichier avec une liste d’autorités de certification SSL de confiance.  
  
-   **Certificat SSL :** Spécifie le nom du fichier de certificat SSL à utiliser pour établir une connexion sécurisée.  
  
-   **clé SSL :** Spécifie le nom du fichier de clé SSL à utiliser pour établir une connexion sécurisée.  
  
> [!NOTE]  
> -   Le bouton **OK** est activé lorsque les informations requises ont été fournies. Si l’un des chemins d’accès aux fichiers n’est pas valide, le bouton « OK » reste désactivé.  
> -   Le bouton **Annuler** ferme la boîte de dialogue et **désactive** l’option SSL dans le formulaire de connexion principal.  
  
