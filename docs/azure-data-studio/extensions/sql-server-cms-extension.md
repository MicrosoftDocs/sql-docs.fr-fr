---
title: Extension de serveurs SQL Server Central Management
description: Découvrez comment installer et utiliser l’extension SQL Server des serveurs de gestion centralisée. Extension pour le regroupement de serveurs et l’application d’actions au groupe.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 06/06/2019
ms.openlocfilehash: 0d93943619d0825e5ba01b51a9c413950fffde37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048391"
---
# <a name="sql-server-central-management-servers-extension-preview"></a>Extension de serveurs de gestion centralisée SQL Server (préversion)

L’extension de serveurs de gestion centralisée permet aux utilisateurs de stocker une liste d'instances de SQL Server. Cette liste est organisée en un ou plusieurs groupes. Les actions prises en utilisant un groupe de serveurs de gestion centralisée s'appliquent à tous les serveurs du groupe.

Cette expérience est actuellement dans sa préversion initiale. Signalez les problèmes et les demandes de fonctionnalités [ici](https://github.com/microsoft/azuredatastudio/issues).

![Extension CMS](media/sql-server-cms-extension/cms-list.png)

## <a name="install-the-sql-server-central-management-servers-extension"></a>Installation de l’extension de serveurs SQL Server Central Management

1. Pour ouvrir le gestionnaire d’extensions et accéder aux extensions disponibles, sélectionnez l’icône d’extensions ou sélectionnez **Extensions** dans le menu **Affichage**.
2. Sélectionnez une extension disponible pour afficher ses détails.
3. Sélectionnez l’extension de votre choix (serveurs de gestion centralisée SQL Server) et **installez-la**.

### <a name="how-do-i-start-central-management-servers"></a>Comment démarrer les serveurs de gestion centralisée ?

 Les serveurs de gestion centralisée peuvent être affichés en cliquant sur l’icône Connexions (Ctrl/Cmd+G). La première fois que vous téléchargez l’extension, la vue des serveurs de gestion centralisée est réduite, et vous pouvez l’ouvrir en cliquant sur **Serveurs de gestion centralisée**

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur les serveurs de gestion centralisée, vous pouvez [consulter ceci.](../../ssms/register-servers/create-a-central-management-server-and-server-group.md)