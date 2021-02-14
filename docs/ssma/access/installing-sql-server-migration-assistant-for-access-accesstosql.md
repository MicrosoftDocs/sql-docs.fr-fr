---
title: Installation de Assistant Migration SQL Server pour Access (AccessToSQL) | Microsoft Docs
description: En savoir plus sur les conditions préalables à l’installation de Assistant Migration SQL Server (SSMA) pour l’accès et sur l’installation, la licence, la mise à niveau et la désinstallation.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- installing SSMA
- instructions, installation
- instructions, upgrade
- licensing SSMA
- prerequisites for installing SSMA
- procedure, installation
- procedure, licensing
- procedure, upgrading
- removing SSMA
- Setup
- uninstalling SSMA
- upgrading SSMA
ms.assetid: dd50eebd-75df-4e0d-8c4d-88b511aae4c7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 3e34f70a41816dcf9a6009f8b38feb016f409a3a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078940"
---
# <a name="installing-sql-server-migration-assistant-for-access-accesstosql"></a>Installation de Assistant Migration SQL Server pour Access (AccessToSQL)

[!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Assistant Migration (SSMA) pour Access est installé à l’aide d’un assistant basé sur Windows Installer. Cette rubrique fournit des informations sur les conditions préalables à l’installation, un lien vers la dernière version de SSMA et des instructions pour l’installation, la gestion des licences, la désinstallation et la mise à niveau de SSMA.

## <a name="prerequisites"></a>Prérequis

Avant d’installer SSMA, assurez-vous que votre système répond aux exigences suivantes :

- Windows 7 ou une version ultérieure, ou Windows Server 2008 ou une version ultérieure.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 ou une version ultérieure.
- La [!INCLUDE[msCoName](../../includes/msconame_md.md)] version de .NET Framework 4.7.2 ou une version ultérieure. Le .NET Framework est disponible dans [Microsoft .net Guide](/dotnet/framework/).
- Accès et autorisations suffisantes sur l’ordinateur qui héberge l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] dans laquelle vous allez migrer des données et des objets de base de données.
- Microsoft Data Access Object (DAO) Provider version 12,0 ou 14,0. Vous pouvez installer le fournisseur DAO à partir de Microsoft Office produit 2010/2007 ou le télécharger à partir du site Web de Microsoft.
- 4 Go de RAM (recommandé).

## <a name="installing-ssma"></a>Installation de SSMA

SSMA est téléchargeable sur le Web. Pour télécharger la dernière version, consultez la [page de téléchargement de Assistant Migration SQL Server](https://aka.ms/ssmaforaccess).

> [!IMPORTANT]
> Désinstallez toutes les versions antérieures de SSMA pour l’accès avant d’installer la nouvelle version.

Pour installer SSMA :
  
1. Double-cliquez sur **SSMAforAccess_ *n*. msi**, où *n* est le numéro de Build.
2. Sur la page d'accueil, cliquez sur **Suivant**.

   Si la configuration requise n’est pas installée, un message s’affiche pour indiquer que vous devez d’abord installer les composants requis. Assurez-vous que vous avez installé tous les composants requis, puis réexécutez le programme d’installation.

3. Lisez le contrat de licence End-User ; Si vous acceptez, sélectionnez **J’accepte le contrat**, puis cliquez sur **suivant**.
4. Dans la page **choisir le type d’installation** , cliquez sur par **défaut**.
5. Sur la page **prêt pour l’installation** , vous pouvez activer ou désactiver la télémétrie et les vérifications automatiques des mises à jour à chaque démarrage de l’outil. Cliquez sur **Installer** pour commencer l'installation.
  
L’emplacement d’installation par défaut est `C:\Program Files\Microsoft SQL Server Migration Assistant for Access`.

## <a name="uninstalling-ssma-for-access"></a>Désinstallation de SSMA pour Access

Désinstallez SSMA en utilisant **Ajout/suppression de programmes** dans le panneau de configuration. N’oubliez pas que la désinstallation du programme ne supprime pas les fichiers de projet ou les fichiers journaux SSMA.

Pour désinstaller SSMA :

1. Cliquez sur **Démarrer**, sur **Panneau de configuration**, puis sur **Ajout/Suppression de programmes**.
2. Sélectionnez **Assistant Migration Microsoft SQL Server pour l’accès**, puis cliquez sur **supprimer**.

## <a name="upgrading-to-a-later-version"></a>Mise à niveau vers une version ultérieure

Si vous souhaitez effectuer une mise à niveau vers une version ultérieure de SSMA pour Access, vous devez d’abord désinstaller SSMA pour accéder à, puis installer la version plus récente. Suivez les instructions de la section désinstallation de SSMA for Access pour terminer ce processus.

Si vous ouvrez un projet créé dans une version antérieure de SSMA pour l’accès, SSMA peut vous demander si vous souhaitez convertir le projet vers la version la plus récente. Cliquez sur **Oui** pour travailler avec le projet dans la version plus récente de SSMA.

## <a name="see-also"></a>Voir également

- [Préparation des bases de données Access pour la migration](preparing-access-databases-for-migration-accesstosql.md)
- [Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)
- [Liaison des applications Access à SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md)