---
title: Compte de connexion SQL Server, boîte de dialogue (OLE DB) | Microsoft Docs
description: Lorsque vous tentez de vous connecter sans spécifier suffisamment d’informations, OLE DB Driver pour SQL Server affiche la boîte de dialogue Connexion SQL Server.
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.author: v-beaziz
author: bazizi
ms.openlocfilehash: 9b5b0e7040bb2797729c9f759ea5a676a5fbc287
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837422"
---
# <a name="sql-server-login-dialog-box"></a>Boîte de dialogue de connexion SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

Lorsque vous tentez de vous connecter sans spécifier suffisamment d’informations, le pilote OLE DB affiche la boîte de dialogue **Connexion SQL Server**.

> [!NOTE]  
> Le comportement de l’invite de la boîte de dialogue de connexion de SQL Server est contrôlé par la propriété d’initialisation `DBPROP_INIT_PROMPT`. Pour plus d'informations, consultez les pages suivantes :
> - [Propriétés d’initialisation et d’autorisation](../ole-db-data-source-objects/initialization-and-authorization-properties.md)
> - [Guide du programmeur OLE DB](/previous-versions/windows/desktop/ms714342(v=vs.85))

![Capture d’écran de la boîte de dialogue de connexion SQL Server](../media/sql-server-login-dialog.png)

## <a name="options"></a>Options
|Option|Description|
|---   |---        |
|Serveur|Nom d'une instance de SQL Server sur votre réseau. Tapez le nom serveur\instance dans la zone **Serveur**, ou sélectionnez-en un dans la liste. Vous pouvez si vous le souhaitez créer un alias de serveur sur l’ordinateur client avec le **Gestionnaire de configuration SQL Server** et taper ce nom dans la zone **Serveur**. <br/><br/>Vous pouvez entrer "(local)" lorsque vous utilisez le même ordinateur que SQL Server. Vous pouvez alors vous connecter à une instance locale de SQL Server, même s’il s’agit d’une version de SQL Server qui n’est pas en réseau.<br/><br/>Pour plus d'informations sur les noms de serveurs de différents types de réseau, consultez [Installation de SQL Server](../../../database-engine/install-windows/install-sql-server.md).|
|Mode d'authentification|Vous pouvez sélectionner les options d’authentification suivantes dans la liste déroulante :<br/><ul><li>`Windows Authentication:` Authentification à SQL Server à l’aide des informations d’identification du compte Windows de l’utilisateur connecté.</li><li>`SQL Server Authentication:` Authentification à l’aide de l’ID de connexion et du mot de passe.</li><li>`Active Directory - Integrated:` Authentification intégrée avec une identité Azure Active Directory. Ce mode peut également être utilisé pour l’authentification Windows à SQL Server.</li><li>`Active Directory - Password:` Authentification par ID d’utilisateur et mot de passe avec une identité Azure Active Directory.</li><li>`Active Directory - Universal with MFA support:` Authentification interactive avec une identité Azure Active Directory. Ce mode prend en charge l’authentification multifacteur Azure Active Directory (MFA).</li><li>Authentification `Active Directory - Service Principal:` avec un principal de service Azure Active Directory. **L’identifiant de connexion** doit être défini sur l’ID de l’application (client). **Le mot de passe** doit être défini sur le secret de l’application (client).</li></ul>|
|SPN du serveur|Si vous utilisez une connexion approuvée, vous pouvez spécifier un nom de principal de service (SPN) pour le serveur.|
|Nom d'accès|Spécifie l’ID de connexion à utiliser pour la connexion. La zone de texte ID de connexion est activée uniquement si `Authentication Mode` est défini sur `SQL Server Authentication`, `Active Directory - Password`, `Active Directory - Universal with MFA support` ou `Active Directory - Service Principal`.|
|Mot de passe|Spécifie le mot de passe utilisé pour la connexion. La zone de texte Mot de passe est activée uniquement si `Authentication Mode` est défini sur `SQL Server Authentication`, `Active Directory - Password` ou `Active Directory - Service Principal`.|
|Options|Affiche ou masque le groupe **Options**. Le bouton **Options** est activé si **Serveur** a une valeur.|
|Modification du mot de passe|Lorsque cette option est activée, les zones de texte **Nouveau mot de passe** et **Confirmer le nouveau mot de passe** s’affichent.|
|Nouveau mot de passe|Spécifie le nouveau mot de passe.|
|Confirmer le nouveau mot de passe|Spécifie une deuxième fois le nouveau mot de passe, à des fins de confirmation.|
|Base de données|Sélectionnez ou entrez la base de données par défaut à utiliser sur la connexion. Ce paramètre remplace la base de données par défaut spécifiée pour le nom de connexion sur le serveur. Si aucune base de données n'est spécifiée, la connexion utilise la base de données par défaut spécifiée pour le nom de connexion sur le serveur.|
|Serveur miroir|Indique le nom du partenaire de basculement de la base de données à mettre en miroir.|
|SPN miroir|Facultativement, vous pouvez spécifier un SPN pour le serveur miroir. Le SPN du serveur miroir est utilisé pour l'authentification mutuelle entre client et serveur.|
|Langage|Spécifie la langue nationale à utiliser pour les messages système SQL Server. La langue en question doit être installée sur l’ordinateur qui exécute SQL Server. Ce paramètre remplace la langue par défaut spécifiée pour le nom de connexion sur le serveur. Si aucune langue n'est spécifiée, la connexion utilise la langue par défaut spécifiée pour le nom de connexion sur le serveur.|
|Nom de l’application|Spécifie le nom de l’application à stocker dans la colonne **program_name** dans la ligne de cette connexion dans **sys.sysprocesses**.|
|ID Station de travail|Spécifie l’ID de station de travail à stocker dans la colonne **hostname** dans la ligne de cette connexion dans **sys.sysprocesses**.|
|Utiliser le chiffrement renforcé pour les données|Lorsque cette option est activée, les données transmises via la connexion sont chiffrées.|
|Faire confiance au certificat de serveur|Lorsque cette option est activée, le certificat du serveur est validé. Le certificat du serveur doit avoir le nom d’hôte de serveur correct et être émis par une autorité de certification approuvée.|

> [!NOTE]  
> Lors de l’utilisation des modes `Windows Authentication` ou `SQL Server Authentication`, **Faire confiance au certificat de serveur** est pris en compte uniquement lorsque l’option **Utiliser le chiffrement renforcé pour les données** est activée.

## <a name="next-steps"></a>Étapes suivantes
- [S’authentifier auprès de Azure Active Directory](../features/using-azure-active-directory.md) à l’aide du pilote OLE DB.
- Définissez les informations de connexion à l’aide de [l’Universal Data Link (UDL)](data-link-pages.md).