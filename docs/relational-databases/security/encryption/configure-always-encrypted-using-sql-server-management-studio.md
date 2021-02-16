---
title: Configurer Always Encrypted à l’aide de SSMS
description: Décrit les tâches de configuration et de gestion des bases de données Always Encrypted avec SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 10/31/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fc8018153ca9f3ee87db9d9e19b4a696537e9e38
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345416"
---
# <a name="configure-always-encrypted-using-sql-server-management-studio"></a>Configurer Always Encrypted à l’aide de SQL Server Management Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Cet article décrit les tâches de configuration d’Always Encrypted et de gestion des bases de données qui utilisent Always Encrypted avec [SSMS (SQL Server Management Studio)](../../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="security-considerations-when-using-ssms-to-configure-always-encrypted"></a>Considérations de sécurité sur l’utilisation de SSMS pour configurer Always Encrypted

Quand vous utilisez SSMS pour configurer Always Encrypted, SSMS gère à la fois les clés Always Encrypted et les données sensibles. Par conséquent, les clés et les données apparaissent en texte clair à l’intérieur du processus SSMS. Ainsi, il est important que vous exécutiez SSMS sur un ordinateur sécurisé. Si votre base de données est hébergée dans SQL Server, vérifiez que SSMS est exécuté sur un ordinateur autre que celui qui héberge votre instance de SQL Server. L’objectif principal d’Always Encrypted étant de garantir la sécurité des données sensibles chiffrées même si le système de base de données est compromis, l’exécution d’un script PowerShell qui traite des clés ou des données sensibles sur l’ordinateur SQL Server peut réduire ou annuler les avantages de la fonctionnalité. Pour obtenir des recommandations supplémentaires, consultez [Considérations en matière de sécurité pour la gestion des clés](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management).

SSMS ne prend pas en charge la séparation des rôles entre ceux qui gèrent la base de données (administrateurs de bases de données) et ceux qui gèrent les secrets de chiffrement et ont accès aux données en texte clair (administrateurs de sécurité et/ou administrateurs d’applications). Si votre organisation applique la séparation des rôles, vous devez utiliser PowerShell pour configurer Always Encrypted. Pour plus d’informations, consultez [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md) et [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md). 

## <a name="always-encrypted-tasks-using-ssms"></a>Tâches Always Encrypted à l’aide de SSMS

- [Provisionner des clés Always Encrypted à l’aide de SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md)
- [Effectuer une rotation des clés Always Encrypted avec SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md)
- [Configurer le chiffrement de colonne à l’aide de l’Assistant Always Encrypted](always-encrypted-wizard.md)
- [Configurer le chiffrement de colonne en utilisant Always Encrypted avec un package DAC](configure-always-encrypted-using-dacpac.md)
- [Interroger des colonnes en utilisant Always Encrypted avec SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Exporter et importer des bases de données avec Always Encrypted](always-encrypted-migrate-using-bacpac.md)
- [Sauvegarder et restaurer des bases de données avec Always Encrypted](always-encrypted-migrate-using-backup-restore.md)
- [Migrer des données à partir ou à destination de colonnes à l’aide d’Always Encrypted avec l’Assistant Importation et exportation SQL Server](always-encrypted-migrate-using-import-export-wizard.md)

## <a name="see-also"></a>Voir aussi
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)
- [Développer des applications avec Always Encrypted](always-encrypted-client-development.md)
