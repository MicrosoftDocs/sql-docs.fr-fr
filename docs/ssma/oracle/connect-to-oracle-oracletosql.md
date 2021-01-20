---
title: Se connecter à Oracle (OracleToSQL) | Microsoft Docs
description: Découvrez comment vous connecter à une base de données Oracle pour commencer la migration à l’aide de SSMA pour Oracle. Utilisez la boîte de dialogue Connexion à Oracle.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 23a48cb6-ff30-49bb-b4a7-603ebcab336f
ms.author: alexiva
ms.openlocfilehash: 8e13b9ea8bcd264d1668cf0979dc2956bd11bef7
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596718"
---
# <a name="connect-to-oracle-oracletosql"></a>Se connecter à Oracle (OracleToSQL)

Utilisez la boîte de dialogue **connexion à Oracle** pour vous connecter à la base de données Oracle que vous souhaitez migrer.

Pour accéder à cette boîte de dialogue, dans le menu **fichier** , sélectionnez **se connecter à Oracle**. Si vous vous êtes connecté précédemment, la commande se **reconnecte à Oracle**.

## <a name="options"></a>Options

**Fournisseur**  
Sélectionnez le fournisseur d’accès aux données pour votre connexion à la base de données Oracle. Les fournisseurs disponibles sont le fournisseur client Oracle et le fournisseur OLE DB. La valeur par défaut est fournisseur Oracle client.

**Mode**  
Sélectionnez standard, TNSNAME ou mode de chaîne de connexion.

- En mode standard, vous entrez ou sélectionnez des valeurs pour le fournisseur, le nom du serveur, le port du serveur, le SID Oracle, le nom d’utilisateur et le mot de passe.
- En mode TNSNAME, vous entrez l’identificateur de connexion (TNS alias) de la base de données Oracle, le nom d’utilisateur et le mot de passe.
- En mode chaîne de connexion, vous fournissez une chaîne de connexion.

  > [!IMPORTANT]
  > Nous vous déconseillons d’utiliser le mode de chaîne de connexion, car le texte peut inclure des mots de passe, et il est envoyé en texte clair.

La valeur par défaut est le mode standard.

**Nom du serveur**  
Entrez le nom du serveur Oracle. Le nom du serveur par défaut est le même que le nom de l’ordinateur. Il s’agit d’une option de mode standard.

**Port du serveur**  
Si vous utilisez un numéro de port autre que 1521 (par défaut) pour les connexions à Oracle, entrez le numéro de port. Il s’agit d’une option de mode standard.

**Identificateur de connexion**  
Entrez l’identificateur Oracle Connect. Il s’agit de l’alias de la base de données tel que défini dans le fichier tnsnames. ora local.

Il s’agit d’une option de mode TNSNAME.

**SID Oracle**  
Entrez le SID pour la base de données. Le SID est un identificateur qui distingue la base de données Oracle sur un ordinateur. Le SID par défaut d’une base de données est les huit premiers caractères du nom de la base de données.

Il s’agit d’une option de mode standard.

**Nom d'utilisateur**  
Entrez le nom d’utilisateur que SSMA utilisera pour se connecter à la base de données Oracle.

**Mot de passe**  
Entrez le mot de passe correspondant au nom d'utilisateur indiqué.

**Chaîne de connexion**  
> [!IMPORTANT]
> Nous vous déconseillons d’utiliser le mode de chaîne de connexion, car le texte peut inclure des mots de passe, et il est envoyé en texte clair.

Si vous utilisez le mode de chaîne de connexion, entrez la chaîne de connexion complète pour la connexion à Oracle.

Les chaînes de connexion se composent de paires nom de paramètre/valeur.

- Pour OLE DB d’informations sur les chaînes de connexion, consultez [fournisseur Microsoft OLE DB pour Oracle](../../ado/guide/appendixes/microsoft-ole-db-provider-for-oracle.md) article sur MSDN Library.

Pour les chaînes de connexion SSMA, incluez toujours le paramètre provider. En outre, assurez-vous d’inclure le paramètre port quand vous vous connectez à Oracle.

## <a name="next-steps"></a>Étapes suivantes

L’étape suivante du processus de migration consiste à [se connecter à SQL Server](connect-to-sql-server-oracletosql.md).