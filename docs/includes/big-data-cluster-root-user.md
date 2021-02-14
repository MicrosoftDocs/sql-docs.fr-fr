---
author: MikeRayMSFT
ms.prod: sql
ms.technology: big-data-cluster
ms.topic: include
ms.date: 06/22/2020
ms.author: mikeray
ms.openlocfilehash: 599b4072c5d03c8a4753c7c00bc7edc14e0f3b05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038923"
---
À partir de SQL Server 2019 CU5, lorsque vous déployez un nouveau cluster avec l’authentification de base, tous les points de terminaison, dont la passerelle, utilisent `AZDATA_USERNAME` et `AZDATA_PASSWORD`. Les points de terminaison sur les clusters mis à niveau vers la CU5 continuent à utiliser `root` comme nom d’utilisateur pour se connecter au point de terminaison de la passerelle. Cette modification ne s’applique pas aux déploiements utilisant l’authentification Active Directory. Voir [Informations d’identification pour l’accès aux services via le point de terminaison de passerelle](../big-data-cluster/release-notes-big-data-cluster.md#credentials-for-accessing-services-through-gateway-endpoint) dans les notes de publication.