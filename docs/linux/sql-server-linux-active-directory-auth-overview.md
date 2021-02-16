---
title: Authentification Active Directory pour SQL Server sur Linux
titleSuffix: SQL Server
description: Cet article fournit une vue d’ensemble de l'authentification Active Directory pour SQL Server sur Linux.
ms.date: 04/01/2019
author: Dylan-MSFT
ms.author: dygray
ms.reviewer: vanto
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
helpviewer_keywords:
- Linux, AAD authentication
ms.openlocfilehash: da2de7b66923ce82813fd8863556ac08b422bded
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350951"
---
# <a name="active-directory-authentication-for-sql-server-on-linux"></a>Authentification Active Directory pour SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article fournit une vue d’ensemble de l'authentification Active Directory (AD) pour [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Linux. L'authentification AD est également connue sous le nom d'authentification intégrée dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

## <a name="ad-authentication-overview"></a>Vue d’ensemble de l’authentification AD

L'authentification AD permet aux clients joints par domaine sur Windows ou Linux de s'authentifier auprès de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en utilisant leurs informations d'identification de domaine et le protocole Kerberos.

L'authentification AD présente les avantages suivants par rapport à l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] :

- Les utilisateurs s'identifient via une authentification unique, sans qu’aucun mot de passe ne leur soit demandé.
- En créant des connexions pour les groupes AD, vous pouvez gérer l'accès et les autorisations dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en utilisant des appartenances aux groupes AD.  
- Chaque utilisateur dispose d’une identité unique au sein de votre organisation et vous n'avez donc pas besoin de garder la trace des connexions [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui correspondent à chaque personne.   
- AD vous permet d'appliquer une stratégie de mot de passe centralisée au sein de votre entreprise.

## <a name="configuration-steps"></a>Configuration

Pour utiliser l'authentification Active Directory, vous devez avoir un contrôleur de domaine AD (Windows) sur votre réseau.

Les détails sur la configuration de l'authentification AD sont fournis dans le tutoriel, [Tutoriel : Utiliser l’authentification Active Directory avec SQL Server sur Linux](sql-server-linux-active-directory-authentication.md). La liste suivante fournit un résumé avec un lien vers chaque section du tutoriel :

1. [Joindre un hôte SQL Server à un domaine Active Directory](sql-server-linux-active-directory-join-domain.md).
1. [Créer un utilisateur AD pour SQL Server et définir ServicePrincipalName](sql-server-linux-active-directory-authentication.md#createuser).
1. [Configurer le keytab du service SQL Server](sql-server-linux-active-directory-authentication.md#configurekeytab).
1. [Sécuriser le fichier keytab](sql-server-linux-active-directory-authentication.md#configurekeytab).
1. [Configurer SQL Server pour utiliser le fichier keytab pour l’authentification Kerberos](sql-server-linux-active-directory-authentication.md#configurekeytab).
1. [Créer des connexions SQL Server basées sur AD dans Transact-SQL](sql-server-linux-active-directory-authentication.md#createsqllogins).
1. [Se connecter à SQL Server à l’aide de l’authentification AD](sql-server-linux-active-directory-authentication.md#connect).

## <a name="known-issues"></a>Problèmes connus

- À ce stade, la seule méthode d’authentification prise en charge pour le point de terminaison de mise en miroir de bases de données est CERTIFICATE. La méthode d'authentification WINDOWS sera activée dans une prochaine version.
- SQL Server sur Linux ne prend pas en charge le protocole NTLM pour les connexions à distance. La connexion locale peut fonctionner avec NTLM.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur le mode d’implémentation de l’authentification Active Directory pour SQL Server sur Linux, consultez [Tutoriel : Utiliser l’authentification Active Directory avec SQL Server sur Linux](sql-server-linux-active-directory-authentication.md).
