---
title: Planifier des packages SSIS sur Linux avec cron
description: Cet article explique comment planifier des packages SQL Server Integration Services (SSIS) sur Linux avec le service cron.
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 75ac9be9d128f76157cee054674ecfe29de06063
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346433"
---
# <a name="schedule-sql-server-integration-services-package-execution-on-linux-with-cron"></a>Planifier l’exécution du package SQL Server Integration Services sur Linux avec cron

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Lorsque vous exécutez SQL Server Integration Services (SSIS) et SQL Server sous Windows, vous pouvez automatiser l’exécution des packages SSIS à l’aide de SQL Server Agent. Toutefois, lorsque vous exécutez SQL Server et SSIS sur Linux, l’utilitaire SQL Server Agent n’est pas disponible pour planifier des tâches sur Linux. Au lieu de cela, vous utilisez le service cron, qui est largement utilisé sur les plateformes Linux pour automatiser l’exécution des packages.

Cet article fournit des exemples qui expliquent comment automatiser l’exécution de packages SSIS. Les exemples sont écrits pour s’exécuter sur Red Hat Enterprise. Le code est similaire pour les autres distributions Linux, telles qu’Ubuntu.

## <a name="prerequisites"></a>Conditions préalables requises

Avant d’utiliser le service cron pour exécuter des tâches, vérifiez s’il s’exécute sur votre ordinateur.

Pour vérifier l’état du service cron, utilisez la commande suivante : `systemctl status crond.service`.

Si le service n’est pas actif (autrement dit, s’il n’est pas en cours d’exécution), contactez votre administrateur pour installer et configurer le service cron correctement.

## <a name="create-jobs"></a>Créer des tâches

Une tâche cron est une tâche que vous pouvez configurer pour qu’elle s’exécute régulièrement à un intervalle spécifié. La tâche peut être aussi simple qu’une commande que vous devez normalement saisir directement dans la console ou exécuter en tant que script d’interpréteur de commandes.

Pour faciliter la gestion et la maintenance, nous vous recommandons de placer les commandes d’exécution des packages dans un script qui contient un nom descriptif.

Voici un exemple de script d’interpréteur de commandes simple pour l’exécution d’un package. Il ne contient qu’une seule commande, mais vous pouvez en ajouter d’autres en fonction des besoins.

```bash
# A simple shell script that contains a simple package execution command
# Script name: SSISpackageName.daily

/opt/ssis/bin/dtexec /F yourSSISpackageName.dtsx >> $HOME/tmp/out 2>&1
```

## <a name="schedule-jobs-with-the-cron-service"></a>Planifier des tâches avec le service cron

Une fois que vous avez défini vos tâches, vous pouvez les planifier pour qu’elles s’exécutent automatiquement à l’aide du service cron.

Pour ajouter votre tâche pour que cron s’exécute, ajoutez la dans le fichier crontab. Pour ouvrir le fichier crontab dans un éditeur où vous pouvez ajouter ou mettre à jour la tâche, utilisez la commande suivante : `crontab -e`.

Pour planifier l’exécution de la tâche décrite précédemment tous les jours à 2h10, ajoutez la ligne suivante au fichier crontab :

```
# run <SSIS package name> at 2:10 AM every day
10 2 \* \* \* $/HOME/SSIS/jobs/SSISpackageName.daily
```

Enregistrez le fichier crontab, puis quittez l’éditeur.

Pour comprendre le format de l’exemple de commande, consultez les informations contenues dans la section suivante.
 
## <a name="format-of-a-crontab-file"></a>Format d’un fichier crontab

L’image suivante décrit le format de la ligne de tâche ajoutée au fichier crontab.

![Description du format pour l’entrée dans le fichier crontab](media/sql-server-linux-schedule-ssis-packages/ssis-linux-cron-job-definition.png)

Pour obtenir une description plus détaillée du format de fichier crontab, utilisez la commande suivante : `man 5 crontab`.

Voici un exemple partiel de la sortie qui permet d’expliquer l’exemple de cet article :

![Description partielle détaillée du format crontab](media/sql-server-linux-schedule-ssis-packages/ssis-linux-cron-crontab-format.png)

## <a name="related-content-about-ssis-on-linux"></a>Contenu connexe relatif à SSIS sur Linux
-   [Extraire, transformer et charger des données sur Linux avec SSIS](sql-server-linux-migrate-ssis.md)
-   [Installer SQL Server Integration Services (SSIS) sur Linux](sql-server-linux-setup-ssis.md)
-   [Configurer SQL Server Integration Services sur Linux avec ssis-conf](sql-server-linux-configure-ssis.md)
-   [Limitations et problèmes connus pour SSIS sur Linux](sql-server-linux-ssis-known-issues.md)
