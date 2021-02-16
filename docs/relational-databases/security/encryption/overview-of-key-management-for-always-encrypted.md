---
title: Vue d’ensemble de la gestion de clés pour Always Encrypted | Microsoft Docs
description: 'Découvrez comment gérer les deux types de clés de chiffrement utilisés par Always Encrypted pour protéger vos données dans SQL Server : clé de chiffrement de colonne et clé principale de colonne.'
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.prod_service: security, sql-database"
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 07a305b1-4110-42f0-b7aa-28a4e32e912a
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 234f6233e2f6b024e6ad74a82f403e3df7db9a05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340186"
---
# <a name="overview-of-key-management-for-always-encrypted"></a>Vue d’ensemble de la gestion de clés pour Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]


[Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) utilise deux types de clés de chiffrement pour protéger vos données : une clé pour chiffrer les données et une autre clé pour chiffrer la clé qui chiffre vos données. La clé de chiffrement de colonne chiffre vos données, tandis que la clé principale de colonne chiffre la clé de chiffrement de colonne. Cet article fournit une présentation détaillée de la gestion de ces clés de chiffrement.  

Quand il s’agit de gestion des clés et de clés Always Encrypted, il est important de comprendre la distinction entre les clés de chiffrement réelles et les objets de métadonnées qui *décrivent* les clés. Nous utilisons les termes **clé de chiffrement de colonne** et **clé principale de colonne** pour faire référence aux clés de chiffrement réelles, et nous utilisons les termes **métadonnées de clé de chiffrement de colonne** et **métadonnées de clé principale de colonne** pour faire référence aux *descriptions* des clés Always Encrypted dans la base de données.

- Les ***clés de chiffrement de colonne*** sont des clés de chiffrement de contenu utilisées pour chiffrer les données. Comme leur nom l’indique, les clés de chiffrement de colonne servent à chiffrer les données contenues dans les colonnes de base de données. Vous pouvez chiffrer une ou plusieurs colonnes avec la même clé de chiffrement de colonne, ou vous pouvez utiliser plusieurs clés de chiffrement de colonne en fonction des besoins de votre application. Les clés de chiffrement de colonne sont elles-mêmes chiffrées, et seules les valeurs chiffrées des clés de chiffrement de colonne sont stockées dans la base de données (dans le cadre des métadonnées de clé de chiffrement de colonne). Les métadonnées de clé de chiffrement de colonne sont stockées dans les affichages catalogue [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md) et [sys.column_encryption_key_values (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md) . Les clés de chiffrement de colonne utilisées avec l’algorithme AES-256 ont une longueur de 256 bits.


- Les ***clés principales de colonne*** sont des clés de protection de clé utilisées pour chiffrer les clés de chiffrement de colonne. Les clés principales de colonne doivent être stockées dans un magasin de clés approuvé, tel que le Magasin de certificats Windows, Azure Key Vault ou un module de sécurité matériel. La base de données contient uniquement des métadonnées sur les clés principales de colonne (le type de magasin de clés et l’emplacement). Les métadonnées de clé principale de colonne sont stockées dans l’affichage catalogue [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md) .  

Il est important de noter que les métadonnées de clé dans le système de base de données ne contiennent pas de clés principales de colonne en texte clair ni de clés de chiffrement de colonne en texte clair. La base de données contient uniquement des informations sur le type et l’emplacement des clés principales de colonne, et des valeurs chiffrées des clés de chiffrement de colonne. Cela signifie que les clés en texte clair ne sont jamais exposées au système de base de données. Ainsi, les données protégées à l’aide d’Always Encrypted sont sécurisées, même si le système de base de données est compromis. Pour vous assurer que le système de base de données ne peut pas accéder aux clés en texte clair, veillez à exécuter vos outils de gestion de clés sur un ordinateur différent de celui qui héberge votre base de données. Pour plus d’informations, consultez les [Considérations relatives à la sécurité pour la gestion des clés](#security-considerations-for-key-management) ci-dessous.

Étant donné que la base de données contient uniquement des données chiffrées (dans les colonnes protégées par Always Encrypted) et ne peut pas accéder aux clés en texte clair, elle ne peut pas déchiffrer les données. Cela signifie que l’interrogation de colonnes Always Encrypted retourne simplement des valeurs chiffrées. Ainsi, les applications clientes qui doivent chiffrer ou déchiffrer des données protégées doivent pouvoir accéder à la clé principale de colonne et aux clés de chiffrement de colonne associées. Pour plus de détails, consultez [Développer des applications à l’aide d’Always Encrypted](always-encrypted-client-development.md).



## <a name="key-management-tasks"></a>Tâches de gestion de clés

Le processus de gestion de clés implique les tâches principales suivantes :

- **Mise en service des clés** : création des clés physiques dans un magasin de clés approuvé (par exemple dans le Magasin de certificats Windows, Azure Key Vault ou un module de sécurité matériel), chiffrement de clés de chiffrement de colonne avec des clés principales de colonne, et création de métadonnées pour les deux types de clés dans la base de données.

- **Permutation des clés** : remplacement périodique d’une clé existante par une nouvelle clé. Vous devrez peut-être permuter une clé si elle a été compromise, ou pour vous conformer aux stratégies ou aux réglementations de conformité de votre organisation qui régissent la permutation des clés de chiffrement. 


## <a name="key-management-roles"></a><a name="KeyManagementRoles"></a> Rôles de gestion de clés

Deux rôles d’utilisateurs distincts gèrent les clés Always Encrypted, les administrateurs de sécurité et les administrateurs de base de données :

- **Administrateur de sécurité** : génère des clés de chiffrement de colonne et des clés principales de colonne, et gère les magasins de clés contenant les clés principales de colonne. Pour effectuer ces tâches, un administrateur de sécurité doit pouvoir accéder aux clés et au magasin de clés, mais il n’a pas besoin de l’accès à la base de données.
- **Administrateur de base de données** : gère les métadonnées relatives aux clés dans la base de données. Pour effectuer les tâches de gestion de clés, un administrateur de base de données doit pouvoir gérer les métadonnées de clés dans la base de données, mais il n’a pas besoin de l’accès aux clés ou au magasin de clés contenant les clés principales de colonne.

Si l’on considère les rôles ci-dessus, il existe deux façons d’effectuer des tâches de gestion de clés pour Always Encrypted : *avec séparation des rôles* et *sans séparation des rôles*. En fonction des besoins de votre organisation, vous pouvez sélectionner le processus de gestion de clés qui correspond le mieux à vos besoins.

## <a name="managing-keys-with-role-separation"></a>Gestion des clés avec séparation des rôles
Quand les clés Always Encrypted sont gérées avec séparation des rôles, différentes personnes au sein d’une organisation assument les rôles d’administrateur de sécurité et d’administrateur de base de données. Un processus de gestion des clés avec séparation des rôles garantit que les administrateurs de base de données n’ont pas accès aux clés ou aux magasins de clés contenant les clés, et que les administrateurs de sécurité n’ont pas accès à la base de données contenant des données sensibles. La gestion des clés avec séparation des rôles est recommandée si votre objectif est de garantir que les administrateurs de base de données de votre organisation ne peuvent pas accéder aux données sensibles. 

**Remarque :** Les administrateurs de sécurité génèrent et utilisent des clés en texte en clair. Ils ne doivent donc jamais effectuer leurs tâches sur les mêmes ordinateurs hébergeant un système de base de données ou sur des ordinateurs qui sont accessibles des par des DBA ou toute autre personne pouvant être un adversaire potentiel. 

## <a name="managing-keys-without-role-separation"></a>Gestion des clés sans séparation des rôles
Quand les clés Always Encrypted sont gérées sans séparation des rôles, une seule personne peut assumer les rôles d’administrateur de base de données et d’administrateur de sécurité. Cette personne doit donc pouvoir accéder aux clés, aux magasins de clés et aux métadonnées de clés, et les gérer. La gestion des clés sans séparation des rôles est recommandée pour les organisations qui utilisent le modèle DevOps, ou si la base de données est hébergée dans le cloud et que le principal objectif est de restreindre l’accès des administrateurs du cloud (mais pas des administrateurs de base de données) aux données sensibles.



## <a name="tools-for-managing-always-encrypted-keys"></a>Outils de gestion des clés Always Encrypted

Vous pouvez gérer les clés Always Encrypted à l’aide de [SQL Server Management Studio (SSMS)](../../../ssms/sql-server-management-studio-ssms.md) et [PowerShell](../../../powershell/sql-server-powershell.md):

- **SQL Server Management Studio (SSMS)**  : fournit des boîtes de dialogue et des Assistants qui combinent des tâches impliquant l’accès au magasin de clés et l’accès à la base de données. SSMS ne prend pas en charge la séparation des rôles, mais il simplifie la configuration des clés. Pour plus d’informations sur la gestion des clés à l’aide de SSMS, consultez :
    - [Provisionner des clés Always Encrypted à l’aide de SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md)
    - [Effectuer une rotation des clés Always Encrypted avec SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md)

- **SQL Server PowerShell** : inclut des applets de commande pour la gestion des clés Always Encrypted avec et sans séparation des rôles. Pour plus d'informations, consultez les pages suivantes :
    - [Configurer des clés Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell.md)
    - [Permuter des clés Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md)


## <a name="security-considerations-for-key-management"></a>Considérations relatives à la sécurité pour la gestion des clés

L’objectif principal d’Always Encrypted consiste à garantir la sécurité des données sensibles stockées dans une base de données, même si le système de base de données ou son environnement d’hébergement est compromis. Voici quelques exemples d’attaques de sécurité où Always Encrypted peut vous aider à éviter les fuites de données sensibles :

- Interrogation des colonnes de données sensibles par un utilisateur de base de données à privilèges élevés malveillant (par exemple un administrateur de base de données).
- Analyse la mémoire d’un processus SQL Server ou des fichiers de vidage de processus SQL Server par un administrateur malveillant sur un ordinateur hébergeant une instance de SQL Server.
- Interrogation d’une base de données de clients, examen des fichiers de vidage de SQL Server ou examen de la mémoire d’un ordinateur hébergeant des données sur les clients dans le cloud par un opérateur de centre de données malveillant.
- Exécution de logiciels malveillants sur un ordinateur hébergeant la base de données.

Pour qu’Always Encrypted soit efficace contre ces types d’attaques, votre processus de gestion de clés doit garantir que les clés principales de colonne et les clés de chiffrement de colonne, ainsi que les informations d’identification pour l’accès à un magasin de clé contenant les clés principales de colonne, ne sont jamais révélées à un intrus potentiel. Voici quelques recommandations à suivre :

- Ne générez jamais de clés principales de colonne ou de clés de chiffrement de colonne sur un ordinateur qui héberge votre base de données. Au lieu de cela, générez les clés sur un ordinateur distinct dédié à la gestion de clés ou hébergeant des applications qui auront besoin d’un accès aux clés. Cela signifie que **vous ne devez jamais exécuter les outils utilisés pour générer les clés sur l’ordinateur qui héberge votre base de données** car, si un intrus accède à un ordinateur utilisé pour provisionner ou gérer vos clés Always Encrypted, il risque d’obtenir vos clés, même si elles apparaissent uniquement dans la mémoire de l’outil pendant une courte durée.
- Pour vous assurer que votre processus de gestion de clés ne révèle pas par inadvertance les clés principales de colonne ou les clés de chiffrement de colonne, vous devez impérativement identifier les adversaires potentiels et les menaces de sécurité avant de définir et d’implémenter un processus de gestion de clés. Par exemple, si votre objectif est de vous assurer que les administrateurs de base de données n’ont pas accès aux données sensibles, un administrateur de base de données ne doit pas être responsable de la génération des clés. En revanche, un administrateur de base de données *peut* gérer les métadonnées de clés dans la base de données, puisqu’elles ne contiennent pas les clés en clair.

## <a name="next-steps"></a>Étapes suivantes
- [Configurer le chiffrement de colonne à l’aide de l’Assistant Always Encrypted](always-encrypted-wizard.md)
- [Créer et stocker des clés principales de colonne pour Always Encrypted](create-and-store-column-master-keys-always-encrypted.md)
- [Provisionner des clés Always Encrypted à l’aide de SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md)
- [Provisionner des clés Always Encrypted en utilisant PowerShell](configure-always-encrypted-keys-using-powershell.md)

## <a name="see-also"></a>Voir aussi
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Didacticiel pour l’Assistant Always Encrypted (Azure Key Vault)](/azure/azure-sql/database/always-encrypted-azure-key-vault-configure)
- [Didacticiel pour l’Assistant Always Encrypted (Magasin de certificats Windows)](/azure/azure-sql/database/always-encrypted-certificate-store-configure)