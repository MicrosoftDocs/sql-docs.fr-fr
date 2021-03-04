---
title: Compte de connexion SQL Server, boîte de dialogue (ODBC)
description: Le dialogue journalisé SQL Server peut s’afficher lorsqu’une application établit une connexion ODBC sans spécifier suffisamment d’informations pour se connecter à la base de données.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 716e95a477bdc517cde7fa0980a2a2b2c7b65eb8
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837276"
---
# <a name="sql-server-login-dialog-box-odbc"></a>Compte de connexion SQL Server, boîte de dialogue (ODBC)

Lorsque vous appelez une connexion ODBC sans spécifier suffisamment d’informations pour que le pilote ODBC puisse se connecter à un serveur SQL Server, il affiche la boîte de dialogue **Connexion à SQL Server**.

## <a name="options"></a>Options

### <a name="server"></a>Serveur

Nom d'une instance de SQL Server sur votre réseau. Tapez le nom serveur\instance dans la zone **Serveur**, ou sélectionnez-en un dans la liste. Vous pouvez si vous le souhaitez créer un alias de serveur sur l’ordinateur client avec le **Gestionnaire de configuration SQL Server** et taper ce nom dans la zone **Serveur**.

Vous pouvez entrer "(local)" lorsque vous utilisez le même ordinateur que SQL Server. Vous pouvez alors vous connecter à une instance locale de SQL Server, même s’il s’agit d’une version de SQL Server qui n’est pas en réseau.

Pour plus d’informations sur les noms de serveurs de différents types de réseaux, consultez la documentation d’installation de SQL Server dans la Documentation en ligne de SQL Server.

### <a name="authentication-mode"></a>Mode d'authentification

Sélectionnez l'un des modes d'authentification suivants :
- **SQL Server** avec ID de connexion et mot de passe
- Authentification **Windows intégrée** à l’aide du compte d’utilisateur actuellement connecté
- **Mot de passe Active Directory** avec ID de connexion et mot de passe
- Authentification **Active Directory intégrée** à l’aide du compte d’utilisateur actuellement connecté
- Authentification **interactive Active Directory** avec ID de connexion
- Authentification **Identités managées pour les ressources Azure** avec Identité managée
- Authentification du **principal de service Active Directory avec une application**  avec le principal de service Azure Active Directory

Consultez [Assistant Source de données - Écran 2](../../../connect/odbc/windows/dsn-wizard-2.md) pour plus d’informations sur les modes d’authentification.

### <a name="server-spn"></a>SPN du serveur

Si vous utilisez une connexion approuvée, vous pouvez spécifier un nom de principal de service (SPN) pour le serveur.

### <a name="login-id"></a>Nom d'accès

Spécifie l’ID de connexion SQL Server ou Azure Active Directory à utiliser pour la connexion si le **Mode d’authentification** est défini sur **SQL Server**, **Mot de passe Active Directory**, **Active Directory Interactif**, **Managed Service Identity** ou **Principal de service Active Directory**. Sinon, l’option **ID de connexion** est désactivée.

### <a name="password"></a>Mot de passe

Spécifie le mot de passe pour l’ID de connexion SQL Server ou Azure Active Directory utilisé pour la connexion si le **mode d’authentification** est défini sur **SQL Server** ou **Mot de passe Active Directory**. Sinon, l’option **Mot de passe** est désactivée.

### <a name="options"></a>Options

Affiche ou masque le groupe **Options**. Le bouton **Options** est activé si **Serveur** a une valeur.

### <a name="change-password"></a>Modification du mot de passe

Lorsque cette zone est sélectionnée, affiche les zones **Nouveau mot de passe** et **Confirmer le nouveau mot de passe**.

### <a name="new-password"></a>Nouveau mot de passe

Spécifie le nouveau mot de passe.

### <a name="confirm-new-password"></a>Confirmer le nouveau mot de passe

Spécifie une deuxième fois le nouveau mot de passe, à des fins de confirmation.

### <a name="database"></a>Base de données

Spécifie la base de données par défaut à utiliser sur la connexion. Ce paramètre remplace la base de données par défaut spécifiée pour le nom de connexion sur le serveur. Si aucune base de données n'est spécifiée, la connexion utilise la base de données par défaut spécifiée pour le nom de connexion sur le serveur.

### <a name="mirror-server"></a>Serveur miroir

Indique le nom du partenaire de basculement de la base de données à mettre en miroir.

### <a name="mirror-spn"></a>SPN miroir

Facultativement, vous pouvez spécifier un SPN pour le serveur miroir. Le SPN du serveur miroir est utilisé pour l'authentification mutuelle entre client et serveur.

### <a name="language"></a>Langage

Spécifie la langue nationale à utiliser pour les messages système SQL Server. La langue en question doit être installée sur l’ordinateur qui exécute SQL Server. Ce paramètre remplace la langue par défaut spécifiée pour le nom de connexion sur le serveur. Si aucune langue n'est spécifiée, la connexion utilise la langue par défaut spécifiée pour le nom de connexion sur le serveur.

### <a name="application-name"></a>Nom de l’application

(Facultatif) Spécifie le nom de l’application à stocker dans la colonne **program_name** dans la ligne de cette connexion dans **sys.sysprocesses**.

### <a name="workstation-id"></a>ID Station de travail

(Facultatif) Spécifie l’ID de station de travail à stocker dans la colonne **hostname** dans la ligne de cette connexion dans **sys.sysprocesses**.

### <a name="use-strong-encryption-for-data"></a>Utiliser le chiffrement renforcé pour les données

Lorsque cette option est sélectionnée, les données transmises via la connexion sont chiffrées. Les noms de connexion sont chiffrés par défaut, même si la case à cocher est désactivée.

### <a name="trust-server-certificate"></a>Faire confiance au certificat de serveur

Cette option s’applique uniquement si l’option **Utiliser le chiffrement renforcé pour les données** est activée, Si cette option est sélectionnée, le certificat du serveur n’aura pas le nom d’hôte de serveur correct et ne sera pas émis par une autorité de certification approuvée.

## <a name="see-also"></a>Voir aussi

[Microsoft ODBC Driver for SQL Server sur Windows](../../../connect/odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows.md)
