---
description: Exemple illustrant l’utilisation du fournisseur Azure Key Vault avec Always Encrypted
title: Exemple illustrant l’utilisation du fournisseur Azure Key Vault avec Always Encrypted | Microsoft Docs
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
ms.openlocfilehash: 0cb62f9c1e32c71e982d74eb5fc868fb64020b76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042809"
---
# <a name="example-demonstrating-use-of-azure-key-vault-provider-with-always-encrypted"></a>Exemple illustrant l’utilisation du fournisseur Azure Key Vault avec Always Encrypted

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

Cet exemple illustre l’utilisation du fournisseur Azure Key Vault lors de l’accès à des colonnes chiffrées.

[!code-csharp [AKVProvider Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderExample.cs#1)]

> [!NOTE]
> - Pour utiliser la fonctionnalité Always Encrypted avec des enclaves sécurisées pour l’application .NET Standard, **Microsoft.Data.SqlClient** version 2.1.0 ou ultérieure est requis. La version de .NET Standard prise en charge est 2.0 ou une version ultérieure. 
>
> - Pour utiliser la fonctionnalité Always Encrypted sur Linux et macOS, **Microsoft.Data.SqlClient** version 2.1.0 ou ultérieure est requis.

## <a name="see-also"></a>Voir aussi

- [Exemple illustrant l’utilisation du fournisseur Azure Key Vault avec Always Encrypted activé avec des Enclaves sécurisées](azure-key-vault-enclave-example.md)
- [Tutoriel : Développer une application .NET en utilisant Always Encrypted avec enclaves sécurisées](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Utilisation d’Always Encrypted avec le Fournisseur de données Microsoft .NET pour SQL Server](sqlclient-support-always-encrypted.md)
