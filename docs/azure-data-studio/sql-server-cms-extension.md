---
title: Extension de serveurs SQL Server Central Management
description: Découvrez comment installer et utiliser l’extension Serveurs de gestion centralisée SQL Server (préversion), qui permet de regrouper des serveurs et d’appliquer des actions au groupe.
ms.reviewer: alayu, maghan, sstein
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.custom: seodec18
ms.date: 06/06/2019
ms.openlocfilehash: 3024a9c61fb51063b50f8fde769e7bdbb5608fbf
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88766048"
---
# <a name="sql-server-central-management-servers-extension-preview"></a>Extension de serveurs SQL Server Central Management (préversion)

L’extension de serveurs de gestion centralisée permet aux utilisateurs de stocker une liste d'instances de SQL Server. Cette liste est organisée en un ou plusieurs groupes. Les actions prises en utilisant un groupe de serveurs de gestion centralisée s'appliquent à tous les serveurs du groupe.

Cette expérience est actuellement dans sa préversion initiale. Signalez les problèmes et les demandes de fonctionnalités [ici](https://github.com/microsoft/azuredatastudio/issues).

![Extension CMS](media/sql-server-cms-extension/cms-list.png)

## <a name="install-the-sql-server-central-management-servers-extension"></a>Installation de l’extension de serveurs SQL Server Central Management

1. Pour ouvrir le gestionnaire d’extensions et accéder aux extensions disponibles, sélectionnez l’icône d’extensions ou sélectionnez **Extensions** dans le menu **Affichage**.
2. Sélectionnez une extension disponible pour afficher ses détails.
1. Sélectionnez l’extension de votre choix (serveurs de gestion centralisée SQL Server) et **installez-la**.

### <a name="how-do-i-start-central-management-servers"></a>Comment démarrer les serveurs de gestion centralisée ?
 Les serveurs de gestion centralisée peuvent être affichés en cliquant sur l’icône Connexions (Ctrl/Cmd+G). La première fois que vous téléchargez l’extension, la vue des serveurs de gestion centralisée est réduite, et vous pouvez l’ouvrir en cliquant sur **Serveurs de gestion centralisée**

## <a name="next-steps"></a>Étapes suivantes
Pour en savoir plus sur les serveurs de gestion centralisée, vous pouvez [consulter ceci.](../ssms/register-servers/create-a-central-management-server-and-server-group.md)