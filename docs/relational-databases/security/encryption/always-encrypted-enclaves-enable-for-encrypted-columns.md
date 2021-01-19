---
description: Activer Always Encrypted avec enclaves sécurisées pour les colonnes chiffrées existantes
title: Activer Always Encrypted avec enclaves sécurisées pour les colonnes chiffrées existantes | Microsoft Docs
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
ms.openlocfilehash: 03a57949624897b2cd2e0552892803c21a408b52
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534808"
---
# <a name="enable-always-encrypted-with-secure-enclaves-for-existing-encrypted-columns"></a>Activer Always Encrypted avec enclaves sécurisées pour les colonnes chiffrées existantes 
[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Cet article explique comment déverrouiller les fonctionnalités d’Always Encrypted avec enclaves sécurisées et comment activer les calculs d’enclave pour les colonnes chiffrées existantes.  

Si des colonnes existantes sont chiffrées avec des clés qui ne sont pas activées pour les enclaves, vous pouvez les chiffrer avec des clés activées pour les enclaves. Ceci vous permet d’utiliser une enclave sécurisée dans les requêtes effectuées sur vos colonnes.

Vous pouvez activer les calculs d’enclave pour les colonnes chiffrées existantes de différentes façons, en fonction des éléments suivants :

- **Étendue/granularité :** voulez-vous activer la fonctionnalité d’enclave pour un sous-ensemble de colonnes, ou pour toutes les colonnes protégées par une clé principale de colonne donnée ?
- **Taille des données :** quelle est la taille des tables contenant les colonnes dont vous souhaitez qu’elles prennent en charge des enclaves ?
- Voulez-vous également modifier le type de chiffrement pour vos colonnes ? N’oubliez pas que seul le chiffrement aléatoire prend en charge les calculs complexes (critères spéciaux, les opérateurs de comparaison). Si votre colonne est chiffrée avec un chiffrement déterministe, vous devez également la rechiffrer avec un chiffrement aléatoire pour déverrouiller les calculs complexes.

La section suivante décrit les trois approches permettant d’activer les enclaves pour des colonnes existantes.

## <a name="method-1-rotate-the-column-master-key-to-replace-it-with-an-enclave-enabled-column-master-key"></a>Méthode 1 : Effectuer la rotation de la clé principale de colonne pour la remplacer par une clé principale de colonne activée pour les enclaves
Le remplacement d’une clé principale de colonne existante (qui n’est pas activée pour les enclaves) par une nouvelle clé principale de colonne qui est activée pour les enclaves rend toutes les clés de chiffrement de colonne (associées à la clé principale de colonne) également activées pour les enclaves.

- Avantages :
  - N’implique pas le nouveau chiffrement des données et est donc généralement l’approche la plus rapide. Il s’agit d’une approche recommandée pour les colonnes contenant de grandes quantités de données, qui sont déjà activées pour les calculs complexes et qui utilisent un chiffrement déterministe.
  - Peut activer la fonctionnalité d’enclave pour plusieurs colonnes à l’échelle. Le remplacement de la clé principale de colonne par la clé principale de colonne activée pour les enclaves rend activées pour les enclaves toutes les clés de chiffrement de colonne et toutes les colonnes chiffrées associées à la clé principale de colonne d’origine.
  
- Inconvénients :
  - Ne prend pas en charge le changement du type de chiffrement de déterministe en aléatoire. Bien qu’il déverrouille le chiffrement sur place pour les colonnes chiffrées avec un chiffrement déterministe, il n’active pas les calculs complexes. Vous devez rechiffrer les colonnes avec un chiffrement aléatoire.
  - Ne vous permet pas de convertir sélectivement certaines des colonnes associées avec une clé principale de colonne donnée.
  - Introduit un surcroît de travail pour la gestion des clés. Vous devez créer une clé principale de colonne et la rendre disponible pour les applications qui interrogent les colonnes impactées.

Pour plus d’informations sur la rotation d’une clé principale de colonne, consultez [Effectuer la rotation des clés activées pour les enclaves](always-encrypted-enclaves-rotate-keys.md).

## <a name="method-2-rotate-the-column-master-key-and-re-encrypt-columns-using-randomized-encryption-in-place"></a>Méthode 2 : Effectuer une rotation de la clé principale de colonne et rechiffrer les colonnes avec un chiffrement aléatoire sur place
Cette méthode implique l’exécution de la méthode 1 au titre de première étape, puis le rechiffrement des colonnes. Les colonnes commencent par utiliser un chiffrement déterministe, puis elles sont rechiffrées avec un chiffrement aléatoire pour déverrouiller les requêtes avancées.

- Avantages :
  - Rechiffre les données sur place. Il s’agit d’une méthode recommandée si vous devez activer les requêtes avancées pour les colonnes chiffrées qui utilisent actuellement un chiffrement déterministe et qui contiennent de grandes quantités de données. L’étape 1 (la rotation de la clé principale de colonne) déverrouille le chiffrement sur place pour les colonnes avec un chiffrement déterministe, et l’étape 2 (rechiffrement des colonnes) peut être effectuée sur place.
  - Peut activer la fonctionnalité d’enclave pour plusieurs colonnes à l’échelle.
  
- Inconvénients :
  - Ne vous permet pas de convertir sélectivement certaines des colonnes associées avec une clé principale de colonne donnée.
  - Elle introduit un surcroît de travail pour la gestion des clés. Vous devez créer une clé principale de colonne et la rendre disponible pour les applications qui interrogent les colonnes impactées.

Pour plus d’informations sur la façon d’effectuer la rotation d’une clé principale de colonne et de rechiffrer une colonne sur place pour effectuer la rotation d’une clé de chiffrement de colonne, consultez [Effectuer la rotation de clés activées pour les enclaves](always-encrypted-enclaves-rotate-keys.md).

## <a name="method-3-re-encrypt-a-selected-column-with-an-enclave-enabled-column-encryption-key-on-the-client-side"></a>Méthode 3 : Rechiffrer une colonne sélectionnée avec une clé de chiffrement de colonne activée pour les enclaves du côté client
Cette méthode implique le rechiffrement d’une colonne avec une clé de chiffrement de colonne activée pour les enclaves, afin d’activer les requêtes avancées avec un chiffrement aléatoire. Comme la clé de chiffrement de colonne actuelle n’est pas activée pour les enclaves, vous ne pouvez pas rechiffrer la colonne sur place. Utilisez l’Assistant Always Encrypted ou l’applet de commande Set-SqlColumnEncryption pour rechiffrer la colonne en dehors de la base de données.

- Avantages :
  - Vous permet d’activer sélectivement les fonctionnalités des enclaves (chiffrement sur place et requêtes avancées, si vous rechiffrez la colonne avec un chiffrement aléatoire) pour une colonne ou un petit sous-ensemble de colonnes.
  - Elle peut activer les calculs complexes pour les colonnes en une seule étape.
  - Elle ne nécessite pas la création d’une clé principale de colonne et a donc un impact moindre sur les applications.
  
- Inconvénients :
  - Pour rechiffrer les données, l’outil les déplace en dehors de la base de données, ce qui peut prendre beaucoup de temps et est sujet aux erreurs réseau.

Pour plus d’informations sur la façon d’effectuer la rotation d’un chiffrement de colonne via un outil côté client, consultez [Effectuer la rotation de clés Always Encrypted avec SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md) et [Effectuer la rotation de clés Always Encrypted avec PowerShell](rotate-always-encrypted-keys-using-powershell.md).

## <a name="next-steps"></a>Étapes suivantes
- [Exécuter des instructions Transact-SQL à l’aide d’enclaves sécurisées](always-encrypted-enclaves-query-columns.md)
- [Développer des applications en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-client-development.md)
