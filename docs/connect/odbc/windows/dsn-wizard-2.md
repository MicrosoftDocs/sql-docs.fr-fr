---
description: Assistant Source de données, écran 2 (Pilote ODBC pour SQL Server)
title: Assistant Source de données, écran 2 (ODBC Driver for SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
author: David-Engel
ms.author: v-jizho2
ms.openlocfilehash: 749ba8963234955e4893a2c5197bcfd4c26f2584
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159558"
---
# <a name="data-source-wizard-screen-2"></a>Assistant Source de données - Écran 2

Spécifiez la méthode d’authentification et configurez les entrées de client avancé Microsoft SQL Server ainsi que le nom de connexion et le mot de passe utilisés par ODBC Driver for SQL Server pour se connecter à SQL Server lors de la configuration de la source de données.

## <a name="options"></a>Options

### <a name="with-integrated-windows-authentication"></a>Avec l’authentification Windows intégrée

Spécifie que le pilote demande une connexion sécurisée (ou approuvée) à SQL Server. Si cette option est sélectionnée, SQL Server utilise la sécurité de connexion intégrée pour établir des connexions à l'aide de cette source de données, quel que soit le mode de sécurité de connexion en vigueur au niveau du serveur. Tout ID de connexion ou mot de passe fourni est ignoré. L’administrateur système SQL Server doit avoir associé votre nom de connexion Windows à un ID de connexion SQL Server (par exemple, avec SQL Server Management Studio).

Vous pouvez éventuellement spécifier un nom de principal du service (SPN) pour le serveur.

### <a name="with-active-directory-integrated-authentication"></a>Avec l’authentification intégrée Active Directory

Spécifie que le pilote s’authentifie auprès de SQL Server avec Azure Active Directory. Si cette option est sélectionnée, SQL Server utilise la sécurité de connexion intégrée Azure Active Directory pour établir une connexion avec cette source de données, quel que soit le mode de sécurité de connexion en vigueur au niveau du serveur.

### <a name="with-sql-server-authentication"></a>Avec l’authentification SQL Server

Spécifie que le pilote s’authentifie auprès de SQL Server avec un ID de connexion et un mot de passe.

### <a name="with-active-directory-password-authentication"></a>Avec l’authentification par mot de passe Active Directory

Spécifie que le pilote s’authentifie auprès de SQL Server avec un ID de connexion et un mot de passe Azure Active Directory.

### <a name="with-active-directory-interactive-authentication"></a>Avec l’authentification interactive Active Directory

Spécifie que le pilote s’authentifie auprès de SQL Server avec le mode interactif Azure Active Directory en fournissant un ID de connexion, Cette option déclenche la boîte de dialogue d’invite d’authentification Azure.

### <a name="with-managed-identity-authentication"></a>Avec l’authentification d’identités managées

Spécifie que le pilote s’authentifie auprès de SQL Server avec une identité managée.

### <a name="with-active-directory-service-principal-authentication"></a>Avec l’authentification par principal de service Active Directory

Spécifie que le pilote s’authentifie auprès de SQL Server en utilisant un principal de service Azure Active Directory.


### <a name="login-id"></a>Nom d'accès

Spécifie l’ID de connexion utilisé par le pilote lors de la connexion à SQL Server si l’option **Avec l’authentification SQL Server à l’aide d’un ID de connexion et d’un mot de passe entrés par l’utilisateur**, l’option **Avec l’authentification par mot de passe Active Directory à l’aide d’un ID de connexion et d’un mot de passe entrés par l’utilisateur** ou l’option **Avec l’authentification interactive Active Directory à l’aide d’un ID de connexion entré par l’utilisateur** est sélectionnée. Si l’option **Avec l’authentification d’identités managées** est sélectionnée, spécifiez l’ID de l’objet de l’identité managée ou laissez la case vide pour utiliser l’identité par défaut. Ce champ s'applique uniquement à la connexion établie pour déterminer les paramètres serveur par défaut et pas aux connexions établies ultérieurement à l'aide de la source de données, après sa création, sauf si l’authentification d’identités managées est utilisée.

### <a name="password"></a>Mot de passe

Spécifie le mot de passe utilisé par le pilote lors de la connexion à SQL Server si l’option **Avec l’authentification SQL Server à l’aide d’un ID de connexion et d’un mot de passe entrés par l’utilisateur** ou l’option **Avec l’authentification par mot de passe Active Directory à l’aide d’un ID de connexion et d’un mot de passe entrés par l’utilisateur** est sélectionnée. Ce champ s'applique uniquement à la connexion établie pour déterminer les paramètres serveur par défaut et pas aux connexions établies ultérieurement à l'aide de la nouvelle source de données.

Les zones **ID de connexion** et **Mot de passe** sont désactivées si l’option **Avec l’authentification Windows intégrée** ou l’option **Avec l’authentification intégrée Active Directory** est sélectionnée.

### <a name="next"></a>Suivant

Passe à l’écran suivant de l’Assistant.

### <a name="back"></a>Précédent

Revient à l’écran précédent de l’Assistant.

## <a name="next-steps"></a>Étapes suivantes

[Assistant Source de données, écran 1](../../../connect/odbc/windows/dsn-wizard-1.md)

[Assistant Source de données, écran 3](../../../connect/odbc/windows/dsn-wizard-3.md)

