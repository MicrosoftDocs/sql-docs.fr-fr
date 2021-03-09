---
description: Exemple illustrant l’utilisation du fournisseur Azure Key Vault avec Always Encrypted
title: Exemple illustrant l’utilisation du fournisseur Azure Key Vault avec Always Encrypted | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: 48c9fbbe5afbc5115aa52dbb13d258e6595b6efc
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464859"
---
# <a name="example-demonstrating-use-of-azure-key-vault-provider-with-always-encrypted"></a>Exemple illustrant l’utilisation du fournisseur Azure Key Vault avec Always Encrypted

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

Cet exemple illustre l’utilisation du fournisseur Azure Key Vault lors de l’accès à des colonnes chiffrées.

## <a name="azurekeyvaultprovider-v20"></a>AzureKeyVaultProvider version 2.0 ou ultérieure

[!code-csharp [Azure Key Vault Provider 2.0 Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderExample_2_0.cs#1)]

## <a name="azurekeyvaultprovider-v1x"></a>AzureKeyVaultProvider version 1.x

[!code-csharp [Azure Key Vault Provider Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderExample.cs#1)]

> [!NOTE]
>
> - Pour utiliser la fonctionnalité Always Encrypted avec des enclaves sécurisées pour l’application .NET Standard, **Microsoft.Data.SqlClient** version 2.1.0 ou ultérieure est requis. La version de .NET Standard prise en charge est 2.0 ou une version ultérieure.
>
> - Pour utiliser la fonctionnalité Always Encrypted sur Linux et macOS, **Microsoft.Data.SqlClient** version 2.1.0 ou ultérieure est requis.

## <a name="see-also"></a>Voir aussi

- [Exemple illustrant l’utilisation du fournisseur Azure Key Vault avec Always Encrypted activé avec des enclaves sécurisées](azure-key-vault-enclave-example.md)
- [Tutoriel : Développer une application .NET en utilisant Always Encrypted avec enclaves sécurisées](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Utilisation d’Always Encrypted avec le Fournisseur de données Microsoft .NET pour SQL Server](sqlclient-support-always-encrypted.md)
