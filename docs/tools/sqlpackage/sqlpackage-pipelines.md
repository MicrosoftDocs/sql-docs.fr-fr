---
title: SqlPackage dans les pipelines de développement
description: Découvrez comment résoudre les problèmes liés aux pipelines de développement de base de données avec SqlPackage.exe en vérifiant le numéro de build installé.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 11/4/2020
ms.openlocfilehash: 002d145328ca101fee467428e5b7c8b0ff1fdd95
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577857"
---
# <a name="sqlpackage-in-development-pipelines"></a>SqlPackage dans les pipelines de développement

**SqlPackage.exe** est un utilitaire de ligne de commande qui permet d’automatiser plusieurs tâches de développement de base de données. Il peut être intégré dans des pipelines CI/CD.

## <a name="managed-virtual-environments"></a>Environnements virtuels gérés

Les environnements virtuels utilisés pour les exécuteurs hébergés GitHub Actions et les images de machines virtuelles Azure Pipelines sont gérés dans le référentiel GitHub [virtual-environments](https://github.com/actions/virtual-environments).  SqlPackage est inclus dans l’environnement `windows-latest`. La mise à jour des images est effectuée dans les semaines qui suivent la publication d’une nouvelle version de SqlPackage.

## <a name="checking-the-sqlpackage-version"></a>Vérification de la version de SqlPackage

Lors du dépannage, il est important de connaître la version de SqlPackage utilisée.  Pour capturer cette information, vous pouvez ajouter une étape au pipeline afin d’exécuter SqlPackage avec le paramètre `/version`.  Des exemples sont donnés ci-dessous pour les environnements gérés Microsoft et GitHub. Les environnements auto-hébergés peuvent présenter des chemins d’installation différents pour le répertoire de travail.

### <a name="azure-pipelines"></a>Azure Pipelines

En tirant parti du mot clé [script](https://docs.microsoft.com/azure/devops/pipelines/yaml-schema#script) dans un pipeline Azure, il est possible d’ajouter une étape à un pipeline Azure Pipelines pour générer le numéro de version SqlPackage.

```yaml
- script: sqlpackage.exe /version
  workingDirectory: C:\Program Files\Microsoft SQL Server\150\DAC\bin\
  displayName: 'get sqlpackage version'
```

### <a name="github-actions"></a>GitHub Actions

En tirant parti du mot clé [run](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions) dans un workflow GitHub Actions, il est possible d’ajouter une étape à une action GitHub Actions pour générer le numéro de version SqlPackage.

```yaml
- name: get sqlpackage version
  working-directory: C:\Program Files\Microsoft SQL Server\150\DAC\bin\
  run: ./sqlpackage.exe /version
```

:::image type="content" source="media/sqlpackage-pipelines-github-action.png" alt-text="Sortie d’une action GitHub Actions indiquant le numéro de build 15.0.4897.1":::

## <a name="next-steps"></a>Étapes suivantes

- En savoir plus sur [sqlpackage](sqlpackage.md)
