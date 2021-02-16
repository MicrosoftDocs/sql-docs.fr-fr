---
title: Planifier des packages SSIS dans Azure à l’aide de SSMS | Microsoft Docs
description: Explique comment planifier des packages SSIS déployés dans Azure SQL Database à l’aide de la commande Planifier de SQL Server Management Studio (SSMS).
ms.date: 09/23/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 2a4460dadef71d61cb01ce67c27438cee2054857
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351980"
---
# <a name="schedule-the-execution-of-ssis-packages-deployed-in-azure-with-sql-server-management-studio-ssms"></a>Planifier l’exécution de packages SSIS déployés dans Azure avec SQL Server Management Studio (SSMS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Vous pouvez utiliser SQL Server Management Studio (SSMS) pour planifier le déploiement de packages SSIS dans Azure SQL Database. SQL Server local et SQL Managed Instance ont respectivement SQL Server Agent et l’agent Managed Instance comme planificateur de travaux SSIS de première classe. De son côté, SQL Database n’intègre pas de planificateur de travaux SSIS de première classe. La fonctionnalité SSMS décrite dans cet article présente une interface utilisateur bien connue qui s’apparente à celle de SQL Server Agent pour la planification des packages déployés dans SQL Database.

Si vous utilisez SQL Database pour héberger le catalogue SSIS, `SSISDB`, vous pouvez utiliser cette fonctionnalité de SSMS pour générer les pipelines, les activités et les déclencheurs Data Factory nécessaires à la planification de packages SSIS. Vous pouvez ensuite si vous le souhaitez modifier et étendre ces objets dans Data Factory.

Quand vous vous servez de SSMS pour planifier un package, SSIS crée automatiquement trois objets Data Factory et les nomme selon le nom du package sélectionné et de l’horodatage. Par exemple, si le nom du package SSIS est **MonPackage**, SSMS crée des objets Data Factory semblables aux suivants :

| Object | Name |
|---|---|
| Pipeline | **Pipeline_MonPackage_2018-05-08T09_00_00Z** |
| Activité Exécuter le package SSIS | **Activité_MonPackage_2018-05-08T09_00_00Z** |
| Déclencheur | **Déclencheur_MonPackage_2018-05-08T09_00_00Z** |
|||

## <a name="prerequisites"></a>Conditions préalables requises

La fonctionnalité décrite dans cet article nécessite SQL Server Management Studio version 17.7 ou supérieure. Pour obtenir la dernière version de SSMS, consultez [Télécharger SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="schedule-a-package-in-ssms"></a>Planifier un package dans SSMS

1. Dans SSMS, dans l’Explorateur d’objets SQL Server, sélectionnez successivement la base de données SSISDB, un dossier, un projet, puis un package. Cliquez avec le bouton droit sur le package, puis sélectionnez **Planifier**.

    ![Sélectionnez le package à planifier.](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image1-schedule.png)

2. La boîte de dialogue **Nouvelle planification** s’ouvre. Dans la page **Général** de la boîte de dialogue **Nouvelle planification**, fournissez un nom et une description pour le nouveau travail planifié.

    ![Page Général de la boîte de dialogue Nouvelle planification](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image2-new-schedule.png)

3. Dans la page **Package** de la boîte de dialogue **Nouvelle planification**, sélectionnez éventuellement des paramètres d’exécution et un environnement d’exécution.

    ![Page Package de la boîte de dialogue Nouvelle planification](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image3-new-schedule2.png)

4. Dans la page **Planification** de la boîte de dialogue **Nouvelle planification**, spécifiez les paramètres de planification tels que la fréquence, l’heure du jour et la durée.

    ![Page Planifier de la boîte de dialogue Nouvelle planification](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image4-new-schedule3.png)

5. Après avoir créé le travail dans la boîte de dialogue **Nouvelle planification**, une confirmation s’affiche pour vous rappeler les objets Data Factory que SSMS va créer. Si vous sélectionnez **Oui** dans la boîte de dialogue de confirmation, le nouveau pipeline Data Factory s’ouvre dans le portail Azure pour vous permettre de l’examiner et de le personnaliser.

    ![Confirmation de la nouvelle planification](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image5-confirmation.png)

6. Pour personnaliser le déclencheur de planification, sélectionnez **Nouveau/Modifier** dans le menu **Déclencheur**.

    ![Modifiez éventuellement le nouveau pipeline](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image6-edit.png)

    Le panneau **Modifier le déclencheur** s’ouvre pour vous permettre de personnaliser les options de planification.

    ![Modifier le déclencheur](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image7-edit2.png)

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur les autres méthodes de planification d’un package SSIS, consultez [Planifier l’exécution d’un package SSIS sur Azure](ssis-azure-schedule-packages.md).

Pour en savoir plus sur les pipelines, les activités et les déclencheurs Azure Data Factory, consultez les articles suivants :
-   [Pipelines et activités dans Azure Data Factory](/azure/data-factory/concepts-pipelines-activities)
-   [Exécution et déclencheurs de pipelines dans Azure Data Factory](/azure/data-factory/concepts-pipeline-execution-triggers)