---
description: Exemple illustrant l’utilisation du fournisseur Azure Key Vault avec Always Encrypted activé avec des enclaves sécurisées
title: Exemple illustrant l’utilisation du fournisseur Azure Key Vault avec Always Encrypted activé avec des enclaves sécurisées | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2020
ms.reviewer: v-kaywon
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 1feed9699d925c47ae7d38ab72db5b366ad00ba8
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771539"
---
# <a name="example-demonstrating-use-of-azure-key-vault-provider-with-always-encrypted-enabled-with-secure-enclaves"></a>Exemple illustrant l’utilisation du fournisseur Azure Key Vault avec Always Encrypted activé avec des enclaves sécurisées

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

Cet exemple illustre l’utilisation du fournisseur Azure Key Vault lors de l’accès à des colonnes chiffrées.

[!code-csharp [Azure Key Vault Provider with Enclave Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderWithEnclaveProviderExample.cs#1)]

> [!NOTE]
> - Pour utiliser Always Encrypted avec des enclaves sécurisées pour l’application .NET Standard, **Microsoft.Data.SqlClient** version 2.1.0 ou ultérieure est requis. La version de .NET Standard prise en charge est 2.1 ou une version ultérieure. 
>
> - Pour utiliser Always Encrypted avec des enclaves sécurisées sur Linux et macOS, **Microsoft.Data.SqlClient** version 2.1.0 ou ultérieure est requis.

## <a name="see-also"></a>Voir aussi

- [Exemple illustrant l’utilisation du fournisseur Azure Key Vault avec Always Encrypted](azure-key-vault-example.md)
- [Tutoriel : Développer une application .NET en utilisant Always Encrypted avec enclaves sécurisées](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Utilisation d’Always Encrypted avec le Fournisseur de données Microsoft .NET pour SQL Server](sqlclient-support-always-encrypted.md)
