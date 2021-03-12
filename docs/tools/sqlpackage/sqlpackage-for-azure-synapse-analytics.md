---
title: SqlPackage pour Azure Synapse Analytics
description: Conseils pour l’utilisation de SqlPackage dans les scénarios Azure Synapse Analytics
ms.custom: tools|sos
ms.date: 03/10/2021
ms.prod: sql
ms.reviewer: llali; sstein
ms.prod_service: sql-tools
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.openlocfilehash: 661230edf4deea3d62ceef7d8b400cdb951330a5
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622905"
---
# <a name="sqlpackage-for-azure-synapse-analytics"></a>SqlPackage pour Azure Synapse Analytics

Cet article présente les fonctionnalités de SqlPackage.exe axées sur les bases de données Azure Synapse Analytics.

## <a name="extract"></a>Extract
Pour [extraire](sqlpackage-extract.md) un schéma vers Stockage Blob Azure, les propriétés suivantes sont obligatoires :
- /p:AzureStorageBlobEndpoint
- /p:AzureStorageContainer
- /p:AzureStorageKey

L’accès à l’extraction est autorisé via une clé de compte de stockage.  Il existe un paramètre supplémentaire facultatif qui permet de définir le chemin racine de stockage dans le conteneur :
- /p:AzureStorageRootPath

Sans cette propriété, le chemin par défaut est `servername/databasename/timestamp/`.

## <a name="publish"></a>Publish
Pour [publier](sqlpackage-publish.md) un schéma à partir d’un dacpac dans Stockage Blob Azure, les propriétés suivantes sont obligatoires :
- /p:AzureStorageBlobEndpoint
- /p:AzureStorageContainer
- /p:AzureStorageRootPath
- /p:AzureStorageKey or /p:AzureSharedAccessSignatureToken

L’accès à la publication peut être autorisé via une clé de compte de stockage ou un jeton de signature d’accès partagé (SAS).

## <a name="next-steps"></a>Étapes suivantes
- En savoir plus sur [Extract](sqlpackage-extract.md)
- En savoir plus sur [Publish](sqlpackage-publish.md)
- En savoir plus sur le [Stockage Blob Azure](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)
- En savoir plus sur les [clés de compte de stockage Azure](https://docs.microsoft.com/azure/storage/common/storage-account-keys-manage)