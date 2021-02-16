---
title: Bien démarrer avec SSIS Scale Out sur un seul ordinateur| Microsoft Docs
description: Cet article vous explique tout ce que vous devez savoir pour bien démarrer avec SSIS Scale Out sur un seul ordinateur.
ms.custom: performance
ms.date: 12/13/2017
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.reviewer: maghan
ms.openlocfilehash: 1cf9077a9b500c9e5052b48aab13d325809bb297
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347380"
---
# <a name="get-started-with-integration-services-ssis-scale-out-on-a-single-computer"></a>Bien démarrer avec Integration Services (SSIS) Scale Out sur un seul ordinateur

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Cette section explique comment configurer Integration Services Scale Out dans un environnement à un seul ordinateur avec les paramètres par défaut.

## <a name="1-install-sql-server-features"></a>1. Installer les fonctionnalités SQL Server
Dans l’Assistant Installation de SQL Server, dans la page **Sélection de fonctionnalités**, sélectionnez les éléments suivants :
-   Services Moteur de base de données
-   Integration Services
    -   Scale Out Master
    -   Scale Out Worker

![Première moitié de la liste de sélection de fonctionnalités](media/feature-select-onebox1.PNG)

![Deuxième moitié de la liste de sélection de fonctionnalités](media/feature-select-onebox2.PNG)

Dans la page **Configuration du serveur**, cliquez sur **Suivant** pour accepter les comptes de service et types de démarrage par défaut.

Dans la page **Configuration du moteur de base de données**, sélectionnez **Mode mixte**, puis cliquez sur **Ajouter l’utilisateur actuel**. 

![Configuration du moteur](media/engine-config.PNG)

Dans les pages **Configuration d’Integration Services Scale Out - Nœud Master** et **Configuration d’Integration Services Scale Out - Nœud Worker**, cliquez sur **Suivant** pour accepter les paramètres par défaut pour le port et les certificats.

Terminez l’Assistant Installation de SQL Server.

## <a name="2-install-sql-server-management-studio"></a>2. Installer SQL Server Management Studio

Téléchargez et installez [SSMS (SQL Server Management Studio)](../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="3-enable-scale-out"></a>3. Activer Scale Out
Ouvrez SSMS et connectez-vous à l’instance locale de SQL Server.
Dans l’Explorateur d’objets, cliquez avec le bouton droit sur **Catalogues Integration Services**, puis sélectionnez **Créer un catalogue**.

Dans la boîte de dialogue **Créer un catalogue**, l’option **Activer ce serveur comme SSIS Scale Out Master** est sélectionnée par défaut.

## <a name="4-enable-a-scale-out-worker"></a>4. Activer un Scale Out Worker
Dans SSMS, cliquez avec le bouton droit sur **SSISDB**, puis sélectionnez **Gérer Scale-out**. 

![Gérer Scale-out](media/manage-scale-out.PNG)

L’application Integration Services Scale Out Manager s’ouvre. Pour plus d’informations, consultez [Scale Out Manager](integration-services-ssis-scale-out-manager.md).

Pour activer un Scale Out Worker, passez au **Gestionnaire de workers**, puis sélectionnez le Worker à activer. Les Workers sont désactivés par défaut. Cliquez sur **Activer le Worker** pour activer le Worker sélectionné.

## <a name="5-run-packages-in-scale-out"></a>5. Exécuter des packages dans Scale-out
Maintenant, vous êtes prêt à exécuter des packages SSIS dans Scale Out. Pour plus d’informations, consultez [Exécuter des packages dans SSIS (SQL Server Integration Services) Scale Out](run-packages-in-integration-services-ssis-scale-out.md).

## <a name="next-steps"></a>Étapes suivantes
-   [Ajouter un Scale Out Worker avec Scale Out Manager](add-scale-out-worker.md).
