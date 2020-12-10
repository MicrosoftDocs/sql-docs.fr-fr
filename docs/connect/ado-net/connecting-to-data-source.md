---
title: Connexion à une source de données
description: En savoir plus sur les objets de connexion, utilisés pour se connecter aux sources de données dans ADO.NET. L'objet Connexion que vous utilisez dépend du type de la source de données.
ms.date: 11/13/2020
ms.assetid: 9abc3f92-1be3-4e1a-b360-762dc689650e
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 0fdf91c5a4da33b585f10939cd6a801a46b236b3
ms.sourcegitcommit: c127c0752e84cccd38a7e23ac74c0362a40f952e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96761517"
---
# <a name="connecting-to-a-data-source-in-adonet"></a>Connexion à une source de données dans ADO.NET

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Dans le Fournisseur de données Microsoft SqlClient, vous utilisez un objet **Connexion** pour vous connecter à une source de données spécifique en fournissant les informations d'authentification nécessaires dans une chaîne de connexion. L'objet **Connexion** que vous utilisez dépend du type de la source de données.

Le Fournisseur de données Microsoft SqlClient pour SQL Server comprend un type <xref:Microsoft.Data.SqlClient.SqlConnection> dérivé d’une classe <xref:System.Data.Common.DbConnection>.

## <a name="in-this-section"></a>Dans cette section  

[Établissement de la connexion](establishing-connection.md)\
Décrit comment utiliser un objet **Connexion** pour établir une connexion à une source de données.

[Événements de connexion](connection-events.md)\
Décrit comment utiliser un événement **InfoMessage** pour extraire des messages d'information d'une source de données.

## <a name="see-also"></a>Voir aussi

- [Chaînes de connexion](connection-strings.md)
- [Regroupement de connexions](connection-pooling.md)
- [Commandes et paramètres](commands-parameters.md)
- [DataAdapters et DataReaders](dataadapters-datareaders.md)
