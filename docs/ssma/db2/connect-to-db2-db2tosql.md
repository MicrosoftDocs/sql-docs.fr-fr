---
description: Se connecter à DB2 (DB2ToSQL)
title: Se connecter à DB2 (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 9d485fd0-ab5d-402a-a59a-e9982a61b7de
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 24ffef4775339baf182b01062f2a06136a04abf1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081570"
---
# <a name="connect-to-db2-db2tosql"></a>Se connecter à DB2 (DB2ToSQL)
Utilisez la boîte de dialogue **se connecter à DB2** pour vous connecter à la base de données DB2 que vous souhaitez migrer.  
  
Pour accéder à cette boîte de dialogue, dans le menu **fichier** , sélectionnez **se connecter à DB2**. Si vous vous êtes connecté précédemment, la commande se **reconnecte à DB2**.  
  
## <a name="options"></a>Options  
**Fournisseur**  
Sélectionnez le fournisseur d’accès aux données pour votre connexion à la base de données DB2. Les fournisseurs disponibles sont le fournisseur client DB2 et le fournisseur OLE DB. La valeur par défaut est fournisseur de client DB2.  
  
**Mode**  
Sélectionnez standard, TNSNAME ou mode de chaîne de connexion.  
  
-   En mode standard, vous entrez ou sélectionnez des valeurs pour le fournisseur, le nom du serveur, le port du serveur, le SID DB2, le nom d’utilisateur et le mot de passe.  
  
-   En mode TNSNAME, vous entrez l’identificateur de connexion (TNS alias) de la base de données DB2, le nom d’utilisateur et le mot de passe.  
  
-   En mode chaîne de connexion, vous fournissez une chaîne de connexion.  
  
    > [!IMPORTANT]  
    > Nous vous déconseillons d’utiliser le mode de chaîne de connexion, car le texte peut inclure des mots de passe, et il est envoyé en texte clair.  
  
La valeur par défaut est le mode standard.  
  
**Nom du serveur**  
Entrez le nom du serveur DB2. Le nom du serveur par défaut est le même que le nom de l’ordinateur. Il s’agit d’une option de mode standard.  
  
**Port du serveur**  
Si vous utilisez un numéro de port autre que 1521 (par défaut) pour les connexions à DB2, entrez le numéro de port. Il s’agit d’une option de mode standard.  
  
**Identificateur de connexion**  
Entrez l’identificateur de connexion DB2. Il s’agit de l’alias de la base de données tel que défini dans le fichier tnsnames. ora local.  
  
Il s’agit d’une option de mode TNSNAME.  
  
**SID DB2**  
Entrez le SID pour la base de données. Le SID est un identificateur qui distingue la base de données DB2 sur un ordinateur. Le SID par défaut d’une base de données est les huit premiers caractères du nom de la base de données.  
  
Il s’agit d’une option de mode standard.  
  
**Nom d'utilisateur**  
Entrez le nom d’utilisateur que SSMA utilisera pour se connecter à la base de données DB2.  
  
**Mot de passe**  
Entrez le mot de passe correspondant au nom d'utilisateur indiqué.  
  
**Chaîne de connexion**  
> [!IMPORTANT]  
> Nous vous déconseillons d’utiliser le mode de chaîne de connexion, car le texte peut inclure des mots de passe, et il est envoyé en texte clair.  
  
Si vous utilisez le mode de chaîne de connexion, entrez la chaîne de connexion complète pour la connexion à DB2.  
  
Les chaînes de connexion se composent de paires nom de paramètre/valeur.  
  
-   Pour OLE DB d’informations sur les chaînes de connexion, consultez [fournisseur OLE DB Microsoft pour DB2](../../ado/guide/appendixes/microsoft-ole-db-provider-for-oracle.md) article sur MSDN Library.  
  
Pour les chaînes de connexion SSMA, incluez toujours le paramètre provider. En outre, assurez-vous d’inclure le paramètre de port lorsque vous vous connectez à DB2.  
