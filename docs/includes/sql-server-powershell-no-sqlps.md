---
title: Utilisation de NOSQLPS pour SQL Server Agent avec PowerShell
description: Message expliquant comment utiliser la cmdlet SqlServer PowerShell au lieu de la cmdlet sqlps avec SQL Server Agent
ms.topic: include
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier
ms.openlocfilehash: bf161bd583f3d48dc389cea6b69f55c32e2cce59
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839402"
---
À partir de SQL Server 2019, vous pouvez désactiver SQLPS. Sur la première ligne d’une étape de travail de type PowerShell, vous pouvez ajouter `#NOSQLPS`, ce qui empêche SQL Agent de charger automatiquement le module SQLPS. À présent, la tâche de votre SQL Agent exécute la version de PowerShell installée sur l’ordinateur, puis vous pouvez utiliser n’importe quel autre module PowerShell de votre choix.

Pour utiliser le [**module SqlServer**](https://www.powershellgallery.com/packages/Sqlserver/21.1.18235) à l’étape de la tâche de votre SQL Agent, vous pouvez placer ce code sur les deux premières lignes de votre script.

```powershell
#NOSQLPS
Import-Module -Name SqlServer
```