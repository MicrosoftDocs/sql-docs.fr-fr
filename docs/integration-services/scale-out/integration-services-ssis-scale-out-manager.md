---
title: SQL Server Integration Services Scale Out Manager | Microsoft Docs
description: Cet article décrit l’outil Scale Out Manager, que vous pouvez utiliser pour gérer SSIS Scale Out.
ms.custom: performance
ms.date: 12/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: a56792633259e2f8d224696f94c7095a0ec1ae96
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347424"
---
# <a name="integration-services-scale-out-manager"></a>Manager scale-out Integration Services

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Scale Out Manager est un outil de gestion qui vous permet de gérer la totalité de votre topologie SSIS Scale Out à partir d’une seule application. Vous n’avez plus besoin d’effectuer des tâches de gestion et d’exécuter des commandes Transact-SQL sur plusieurs ordinateurs.

## <a name="open-scale-out-manager"></a>Ouvrir Scale Out Manager

Vous pouvez ouvrir Scale Out Manager de deux façons.

### <a name="1-open-scale-out-manager-from-sql-server-management-studio"></a>1. Ouvrir Scale Out Manager à partir de SQL Server Management Studio
Ouvrez SSMS (SQL Server Management Studio) et connectez-vous à l’instance de SQL Server de Scale Out Master.

Dans l’Explorateur d’objets, cliquez avec le bouton droit sur **SSISDB** et sélectionnez **Gérer Scale-out**.

![Gérer Scale-out](media/manage-scale-out.PNG)

> [!NOTE]
> Nous vous recommandons d’exécuter SSMS en tant qu’administrateur, car certaines opérations de gestion Scale-out telles que l’ajout d’un Scale Out Worker nécessitent un privilège administratif.

### <a name="2-open-scale-out-manager-by-running-managementtoolexe"></a>2. Ouvrir Scale Out Manager en exécutant ManagementTool.exe

Recherchez `ManagementTool.exe` sous `%SystemDrive%\Program Files (x86)\Microsoft SQL Server\150\DTS\Binn\Management`. Cliquez avec le bouton droit sur **ManagementTool.exe** et sélectionnez **Exécuter en tant qu’administrateur**. 

Après l’ouverture de Scale Out Manager, vous devez entrer le nom de l’instance de SQL Server de Scale Out Master et vous y connecter pour gérer votre environnement Scale-out.

![Connexion au portail](media/portal-connect-new.png)

## <a name="tasks-available-in-scale-out-manager"></a>Tâches disponibles dans Scale Out Manager
Dans Scale Out Manager, vous pouvez effectuer les opérations suivantes :

### <a name="enable-scale-out"></a>Activer Scale Out
Après vous être connecté à SQL Server, si Scale-out n’est pas activé, vous pouvez sélectionner **Activer** pour l’activer.

![Portail - Activer Scale Out](media/portal-enable-scale-out-new.PNG) 

### <a name="view-scale-out-master-status"></a>Afficher l’état de Scale Out Master
L’état de Scale Out Master apparaît dans la page **Tableau de bord**.

![Tableau de bord du portail](media/portal-dashboard-new.PNG)

### <a name="view-scale-out-worker-status"></a>Afficher l’état de Scale Out Worker
L’état de Scale Out Worker apparaît dans la page **Gestionnaire de workers**. Vous pouvez sélectionner chaque Worker pour connaître son état.

![Portail - Gestionnaire de workers](media/portal-worker-manager-new.PNG)

### <a name="add-a-scale-out-worker"></a>Ajouter un Scale Out Worker
Pour ajouter un Scale Out Worker, sélectionnez **+** au bas de la liste Scale Out Worker. 

Entrez le nom de l’ordinateur du Scale Out Worker que vous souhaitez ajouter, puis cliquez sur **Valider**. Scale Out Manager vérifie si l’utilisateur actuel a accès aux magasins de certificats sur les ordinateurs Scale Out Master et Scale Out Worker

![Connecter le nœud Worker](media/connect-worker-new.PNG)

Si la validation réussit, Scale Out Manager tente de lire le fichier de configuration du serveur Worker et d’obtenir l’empreinte du certificat de ce dernier. Pour plus d’informations, consultez [Scale Out Worker](integration-services-ssis-scale-out-worker.md). Si Scale Out Manager n’est pas en mesure de lire le fichier de configuration du service Worker, vous disposez de deux autres méthodes pour fournir le certificat Worker. 

- Vous pouvez entrer l’empreinte du certificat Worker directement

    ![Certificat Worker 1](media/portal-cert1-new.PNG)

- ou fournir le fichier de certificat.

    ![Certificat Worker 2](media/portal-cert2-new.PNG)

Une fois les informations collectées, Scale Out Manager décrit les actions à effectuer. Généralement, ces actions incluent l’installation du certificat, la mise à jour du fichier de configuration du service Worker et le redémarrage du service Worker.

![Portail - Ajouter, confirmer 1](media/portal-add-confirm1-new.PNG)

Si le paramètre Worker n’est pas accessible, vous devez le mettre à jour manuellement et redémarrer le service Worker.

![Portail - Ajouter, confirmer 2](media/portal-add-confirm2-new.PNG)

Cochez la case de **confirmation** , puis sélectionnez **OK** pour commencer la procédure d’ajout d’un Scale Out Worker.

### <a name="delete-a-scale-out-worker"></a>Supprimer un Scale Out Worker
Pour supprimer un Scale Out Worker, sélectionnez-le, puis cliquez sur **-** au bas de la liste Scale Out Worker.

### <a name="enable-or-disable-a-scale-out-worker"></a>Activer ou désactiver un Scale Out Worker
Pour activer ou désactiver un Scale Out Worker, sélectionnez-le, puis cliquez sur **Activer le Worker** ou **Désactiver le Worker**. L’état du Worker affiché dans Scale Out Manager change en conséquence si le Worker n’est pas hors connexion.

## <a name="edit-a-scale-out-worker-description"></a>Modifier la description d’un Scale Out Worker
Pour modifier la description d’un Scale Out Worker, sélectionnez-le, puis cliquez sur **Modifier**. Une fois que vous avez terminé, sélectionnez **Enregistrer**.

![Portail - Enregistrer le Worker](media/portal-save-worker-new.PNG)

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations, consultez les articles suivants :
-   [SSIS (SQL Server Integration Services) Scale Out Master](integration-services-ssis-scale-out-master.md)
-   [SSIS (SQL Server Integration Services) Scale Out Worker](integration-services-ssis-scale-out-worker.md)
