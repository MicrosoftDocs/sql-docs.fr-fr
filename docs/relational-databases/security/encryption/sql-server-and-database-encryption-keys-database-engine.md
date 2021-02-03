---
title: SQL Server et clés de chiffrement de base de données
description: Découvrez la clé principale de service et la clé principale de base de données utilisées par le moteur de base de données SQL Server pour chiffrer et sécuriser les données.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- keys [SQL Server], database encryption
ms.assetid: 15c0a5e8-9177-484c-ae75-8c552dc0dac0
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: ac91abe799eb914d2a544d6f9fad598545a920c4
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251138"
---
# <a name="sql-server-and-database-encryption-keys-database-engine"></a>SQL Server et clés de chiffrement de base de données (moteur de base de données)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilise des clés de chiffrement pour mieux sécuriser les données, les informations d’identification et les informations de connexion qui sont stockées dans une base de données du serveur. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a deux types de clés : des clés *symétriques* et *asymétriques*. Les clés symétriques utilisent le même mot de passe pour chiffrer et déchiffrer des données. Les clés asymétriques utilisent un premier mot de passe pour chiffrer des données (appelé clé *publique* ) et un second mot de passe pour déchiffrer les données (appelé clé *privée* ).  
  
 Dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], les clés de chiffrement incluent une combinaison d'une clé publique, d'une clé privée et d'une clé symétrique, dont le but est de protéger les données sensibles. La clé symétrique est créée pendant l'initialisation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , lorsque vous démarrez pour la première fois l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Cette clé est utilisée par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour chiffrer les données sensibles stockées dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les clés publique et privée sont créées par le système d'exploitation et servent à protéger la clé symétrique. Une paire de clés privée et publique est créée pour chaque instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui stocke des données sensibles dans une base de données.  
  
## <a name="applications-for-sql-server-and-database-keys"></a>Applications pour les clés SQL Server et de base de données  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a deux applications principales pour les clés : une *clé principale de service* (SMK) générée sur et pour une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , et une *clé principale de base de données* (DMK) utilisée pour une base de données.

### <a name="service-master-key"></a>Clé principale du service
  
 La clé principale du service représente la racine de la hiérarchie de chiffrement SQL Server. La clé SMK est générée automatiquement la première fois que l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est démarrée, et elle est utilisée pour chiffrer un mot de passe de serveur lié, les informations d’identification et la clé principale de base de données dans chaque base de données. La clé SMK est chiffrée en utilisant la clé de l’ordinateur local à l’aide de l’API de protection des données Windows (DPAPI). L’interface DPAPI utilise une clé dérivée des informations d’identification Windows du compte de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et des informations d’identification de l’ordinateur. La clé principale de service peut être déchiffrée uniquement par le compte de service sous lequel elle a été créée ou par un principal qui a accès aux informations d'identification de l'ordinateur.

La clé principale du service ne peut être ouverte que par le compte de service Windows sous lequel elle a été créée ou par un principal ayant accès au nom du compte de service et à son mot de passe.

 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] utilise l’algorithme de chiffrement AES pour protéger la clé principale du service (SMK) et la clé principale de base de données (DMK). AES est un algorithme de chiffrement plus récent que 3DES, qui était utilisé dans les versions antérieures. Au terme de la mise à niveau d'une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)] vers [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] , les clés SMK et DMK doivent être régénérées pour mettre à niveau les clés principales vers AES. Pour plus d’informations sur la régénération de la clé SMK, consultez [ALTER SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-service-master-key-transact-sql.md) et [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-master-key-transact-sql.md).

### <a name="database-master-key"></a>Clé principale de base de données
  
 La clé principale de base de données est une clé symétrique qui permet de protéger les clés privées des certificats et des clés asymétriques présentes dans la base de données. Elle peut également être utilisée pour chiffrer des données, mais elle présente des limitations de longueur qui la rendent moins pratique pour les données que l'utilisation d'une clé symétrique. Pour activer le déchiffrement automatique de la clé principale de base de données, une copie de la clé est chiffrée au moyen de la clé SMK. Elle est stockée à la fois dans la base de données où elle est utilisée et dans la base de données système **master** .  
  
 La copie de la clé DMK stockée dans la base de données système **master** est mise à jour silencieusement chaque fois que la clé DMK est modifiée. Toutefois, vous pouvez modifier ce comportement par défaut à l’aide de l’option **DROP ENCRYPTION BY SERVICE MASTER KEY** de l’instruction **ALTER MASTER KEY** . Une DMK qui n’est pas chiffrée par la clé principale de service doit être ouverte à l’aide de l’instruction **OPEN MASTER KEY** et d’un mot de passe.  
  
## <a name="managing-sql-server-and-database-keys"></a>Gestion des clés SQL Server et de base de données  
 La gestion des clés de chiffrement consiste à créer de nouvelles clés de base de données, à créer une sauvegarde des clés de serveur et de base de données et à savoir quand et comment restaurer, supprimer et modifier les clés.  
  
 Pour gérer des clés symétriques, vous pouvez utiliser les outils inclus dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour effectuer les opérations suivantes :  
  
-   Sauvegarde d'une copie des clés de serveur et de base de données permettant de les utiliser pour récupérer une installation de serveur ou dans le cadre d'une migration planifiée.  
  
-   Restauration d'une clé précédemment enregistrée dans une base de données. Cela permet à une nouvelle instance de serveur d'accéder aux données existantes qu'elle n'a pas chiffrées à l'origine.  
  
-   Suppression des données chiffrées d'une base de données au cas improbable où l'accès à ces données chiffrées se révélerait impossible.  
  
-   Recréation de clés et nouveau chiffrement des données dans le cas improbable où la sécurité de la clé serait compromise. Comme meilleure pratique de sécurité, vous devez recréer les clés périodiquement (par exemple, tous les deux ou trois mois) pour protéger le serveur contre des attaques visant à déchiffrer les clés.  
  
-   Ajout ou suppression d'une instance de serveur à partir d'un déploiement évolutif de serveur dans lequel plusieurs serveurs partagent une base de données unique et la clé qui fournit un chiffrement réversible pour cette base de données.  
  
## <a name="important-security-information"></a>Informations de sécurité importantes  
 L'accès aux objets sécurisés par la clé principale du service requiert le compte de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui a permis de créer la clé ou le compte d'ordinateur (machine). Autrement dit, l'ordinateur est attaché au système dans lequel la clé a été créée. Vous pouvez modifier le compte de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]*ou* le compte d’ordinateur sans perdre l’accès à la clé. Toutefois, si vous modifiez les deux, vous perdrez l'accès à la clé principale de service. Si vous perdez l’accès à la clé principale de service sans un de ces deux éléments, vous ne pouvez pas déchiffrer les données et les objets chiffrés en utilisant la clé d’origine.  
  
 Les connexions sécurisées avec la clé principale de service ne peuvent pas être restaurées sans la clé principale de service.  
  
 L'accès à des objets et données sécurisés avec la clé principale de base de données requiert uniquement le mot de passe utilisé pour sécuriser la clé.  
  
> [!CAUTION]  
>  Si vous perdez tout accès aux clés décrites précédemment, vous perdez l'accès aux objets, connexions et données sécurisés par ces clés. Vous pouvez restaurer la clé principale de service, comme décrit dans les liens répertoriés ici, ou vous pouvez revenir au système de chiffrement d'origine pour récupérer l'accès. Il n'existe aucune « porte dérobée » pour récupérer l'accès.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Clé principale du service]()  
 Fournit une brève explication pour la clé principale de service et les meilleures pratiques associées.  
  
 [Gestion de clés extensible &#40;EKM&#41;](../../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
 Explique comment utiliser des systèmes de gestion de clés tiers avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="related-tasks"></a>Tâches associées  
 [Sauvegarder la clé principale du service](../../../relational-databases/security/encryption/back-up-the-service-master-key.md)  
  
 [Restaurer la clé principale du service](../../../relational-databases/security/encryption/restore-the-service-master-key.md)  
  
 [Créer une clé principale de base de données](../../../relational-databases/security/encryption/create-a-database-master-key.md)  
  
 [Sauvegarder une clé primaire de base de données](../../../relational-databases/security/encryption/back-up-a-database-master-key.md)  
  
 [Restaurer une clé principale de base de données](../../../relational-databases/security/encryption/restore-a-database-master-key.md)  
  
 [Créer des clés symétriques identiques sur deux serveurs](../../../relational-databases/security/encryption/create-identical-symmetric-keys-on-two-servers.md)  
  
 [Activer le chiffrement transparent des données à l’aide de la gestion de clés extensible (EKM)](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)  
  
 [Gestion de clés extensible à l’aide d’Azure Key Vault &#40;SQL Server&#41;](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
 [Chiffrer une colonne de données](../../../relational-databases/security/encryption/encrypt-a-column-of-data.md)  
  
## <a name="related-content"></a>Contenu associé  
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
 [ALTER SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-service-master-key-transact-sql.md)  
  
 [Restaurer une clé principale de base de données](../../../relational-databases/security/encryption/restore-a-database-master-key.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Sauvegarder et restaurer les clés de chiffrement Reporting Services](../../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)   
 [Supprimer et recréer des clés de chiffrement &#40;Gestionnaire de configuration de SSRS&#41;](../../../reporting-services/install-windows/ssrs-encryption-keys-delete-and-re-create-encryption-keys.md)   
 [Ajouter et supprimer des clés de chiffrement pour un déploiement évolutif &#40;Gestionnaire de configuration de SSRS&#41;](../../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md)   
 [Chiffrement transparent des données &#40;TDE&#41;](../../../relational-databases/security/encryption/transparent-data-encryption.md)  
  
