---
description: Affichages catalogue liées à la sécurité (Transact-SQL)
title: Affichages catalogue de sécurité (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/27/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- cryptography [SQL Server], catalog views
- encryption [SQL Server], catalog views
- catalog views [SQL Server], security
- security catalog views [SQL Server]
ms.assetid: 4d5cf1bf-09a7-4ee0-9dbb-5c584750fc67
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 26faef48c73b41bb96eedc921fb2f8f91673e746
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200899"
---
# <a name="security-catalog-views-transact-sql"></a>Affichages catalogue liées à la sécurité (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Les informations relatives à la sécurité sont exposées dans des affichages catalogue qui sont optimisés pour fournir un maximum de performances et de fonctionnalités. Lorsque cela est possible, utilisez les affichages catalogue suivants pour accéder aux métadonnées du catalogue.  
  
## <a name="database-level-views"></a>Affichages de niveau base de données   
  
:::row:::
    :::column:::
        [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.master_key_passwords](../../relational-databases/system-catalog-views/sys-master-key-passwords-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.database_role_members](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.user_token](../../relational-databases/system-catalog-views/sys-user-token-transact-sql.md)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="server-level-views"></a>Affichages de niveau serveur  

:::row:::
    :::column:::
        [sys.credentials](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.login_token](../../relational-databases/system-catalog-views/sys-login-token-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.securable_classes](../../relational-databases/system-catalog-views/sys-securable-classes-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.sql_logins](../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.system_components_surface_area_configuration](../../relational-databases/system-catalog-views/sys-system-components-surface-area-configuration-transact-sql.md)
    :::column-end:::
:::row-end:::

&nbsp;
  
## <a name="encryption-views"></a>Affichages relatifs au chiffrement  
  
:::row:::
    :::column:::
        [sys.asymmetric_keys](../../relational-databases/system-catalog-views/sys-asymmetric-keys-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.cryptographic_providers](../../relational-databases/system-catalog-views/sys-cryptographic-providers-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.certificates](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.key_encryptions](../../relational-databases/system-catalog-views/sys-key-encryptions-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.column_encryption_key_values](../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.openkeys](../../relational-databases/system-catalog-views/sys-openkeys-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.column_encryption_keys](../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.security_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.column_master_keys](../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.security_predicates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.crypt_properties](../../relational-databases/system-catalog-views/sys-crypt-properties-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.symmetric_keys](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server-audit-views"></a>Affichages d'audit SQL Server  
  
:::row:::
    :::column:::
        [sys.server_audits](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.server_file_audits](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.server_audit_specifications](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.server_audit_specifications_details](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.database_audit_specifications](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.database_audit_specification_details](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Centre de sécurité pour le moteur de base de données SQL Server et Azure SQL Database](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)   
 [Fonctions et vues de gestion dynamique relatives à la sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  
