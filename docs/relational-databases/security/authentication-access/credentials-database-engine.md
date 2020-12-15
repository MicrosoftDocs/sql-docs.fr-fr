---
title: Informations d’identification (moteur de base de données) | Microsoft Docs
description: Découvrez les informations d’identification dans SQL Server. Familiarisez-vous avec les informations d’authentification requises pour vous connecter à une ressource en dehors de SQL Server.
ms.custom: ''
ms.date: 06/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- principals [SQL Server], credentials
- schemas [SQL Server], credentials
- permissions [SQL Server], credentials
- groups [SQL Server], credentials
- ALTER ANY CREDENTIAL permission
- security [SQL Server], credentials
- authentication [SQL Server], credentials
- users [SQL Server], credentials
- credentials [SQL Server], about credentials
- credentials [SQL Server]
ms.assetid: c8df6022-e0b4-46b8-9670-3f86938d3177
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9bc63a0a3bf2482ea84f1a2f80febb5865892a6d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97406075"
---
# <a name="credentials-database-engine"></a>Informations d'identification (moteur de base de données)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Les informations d'identification correspondent à un enregistrement qui contient les informations d'authentification requises pour la connexion à une ressource en dehors de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Ces informations sont utilisées en interne par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La plupart des informations d'identification contiennent un nom d'utilisateur et un mot de passe Windows.  
  
 Les informations stockées dans des informations d'identification permettent à un utilisateur connecté à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] par le biais de l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] d'accéder à des ressources en dehors de l'instance du serveur. Lorsque la source externe est Windows, l'utilisateur est authentifié en tant qu'utilisateur Windows, tel que spécifié dans les informations d'identification. Les informations d'identification uniques ne peuvent être mappées qu’à une connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] unique. Et une connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne peut être mappée qu'à une seule information d'identification.  
  
 Pour les informations d’identification qui sont stockées dans la base de données MASTER et qui peuvent être utilisées dans l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md). Pour les informations d’identification qui sont utilisées par une base de données spécifique et qui sont portables avec cette même base de données, consultez [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-database-scoped-credential-transact-sql.md).  
  
 Les informations d'identification système sont créées automatiquement et sont associées à des points de terminaison spécifiques. Le nom de ces informations d'identification système commence par deux signes dièse (##).  
  
 Pour plus d’informations sur les informations d’identification, consultez les affichages catalogue [sys.credentials](../../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md) et [sys.database_scoped_credentials](../../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md).  
  
## <a name="related-content"></a>Contenu associé  
 [Créer des informations d’identification](../../../relational-databases/security/authentication-access/create-a-credential.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md)   
 [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-database-scoped-credential-transact-sql.md)  
 [Sécurisation de SQL Server](../../../relational-databases/security/securing-sql-server.md)  
  
  
