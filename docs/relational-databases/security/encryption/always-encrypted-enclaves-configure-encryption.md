---
description: Configurer le chiffrement de colonne sur place en utilisant Always Encrypted avec enclaves sécurisées
title: Configurer le chiffrement de colonne sur place en utilisant Always Encrypted avec enclaves sécurisées | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 67b74e36ff5b2872e619a4b26fabdd05428e6a16
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534838"
---
# <a name="configure-column-encryption-in-place-using-always-encrypted-with-secure-enclaves"></a>Configurer le chiffrement de colonne sur place en utilisant Always Encrypted avec enclaves sécurisées 
[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md) prend en charge les opérations de chiffrement sur les colonnes de base de données sur place, à l’intérieur d’une enclave sécurisée dans le [!INCLUDE[ssde-md](../../../includes/ssde-md.md)]. Le chiffrement sur place élimine la nécessité de déplacer les données pour ces opérations en dehors de la base de données, ce qui rend les opérations de chiffrement plus rapides et plus fiables. 

> [!NOTE]
> En dépit des avantages en matière de performances du chiffrement sur place, les opérations de chiffrement sur des grandes tables peuvent prendre beaucoup de temps et consommer des ressources importantes, ce qui peut affecter et dégrader les performances et la disponibilité de vos applications.

Le chiffrement sur place permet aussi de déclencher des opérations de chiffrement avec l’instruction [ALTER TABLE ALTER COLUMN (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md), ce qui n’est pas possible sans une enclave.

## <a name="prerequisites"></a>Prérequis
Les opérations de chiffrement prises en charge et les exigences pour la ou les clés de chiffrement de colonne utilisées pour les opérations sont les suivantes :
- Chiffrement d’une colonne de texte en clair. La clé de chiffrement de colonne utilisée pour chiffrer la colonne doit être activée pour les enclaves.
- Rechiffrement d’une colonne chiffrée avec un nouveau type de chiffrement et/ou une nouvelle clé de chiffrement de colonne. La clé de chiffrement de colonne actuelle et la nouvelle clé de chiffrement de colonne (si elle est différente de la clé actuelle) doivent être activées pour les enclaves.
- Déchiffrement d’une colonne chiffrée : la clé de chiffrement de colonne protégeant la colonne doit être activée pour les enclaves.

Pour plus d’informations sur la façon de garantir que vos clés de chiffrement de colonne sont activées pour les enclaves, consultez [Gérer les clés pour Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-manage-keys.md).

Vous devez également vérifier que votre environnement répond aux [prérequis généraux pour l’exécution d’instructions utilisant des enclaves sécurisées](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves).

Un utilisateur ou une application déclenchant des opérations de chiffrement doit avoir les autorisations nécessaires pour apporter des modifications de schéma à la table contenant les colonnes impactées et pour accéder aux clés principales de colonne impliquées dans les opérations, de même que les métadonnées de clé pertinentes dans la base de données.

Vous pouvez déclencher le chiffrement sur place seulement avec [ALTER TABLE ALTER COLUMN (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md) depuis SQL Server Management Studio ou depuis votre application personnalisée. Consultez [Configurer le chiffrement de colonne sur place avec Transact-SQL](always-encrypted-enclaves-configure-encryption-tsql.md).

> [!NOTE]
> Actuellement, l’[Assistant Always Encrypted](always-encrypted-wizard.md) et l’applet de commande [Set-SqlColumnEncryption](/powershell/module/sqlserver/set-sqlcolumnencryption) ne prennent pas en charge le chiffrement sur place et téléchargent toujours les données pour les opérations de chiffrement, même si votre configuration répond aux exigences ci-dessus. 

## <a name="next-steps"></a>Étapes suivantes
- [Configurer le chiffrement de colonne sur place avec Transact-SQL](always-encrypted-enclaves-configure-encryption-tsql.md)
- [Créer et utiliser des index sur des colonnes en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-create-use-indexes.md)
- [Développer des applications en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>Voir aussi  
- [Résoudre les problèmes courants concernant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-troubleshooting.md)