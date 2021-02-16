---
description: 'Leçon 1-4 : Ajout de configurations au package'
title: 'Étape 4 : Ajout de configurations au package | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: e04a5321-63d5-4ec5-85b9-cb4eaf6c87f6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ca2d3bb08a1059970b8c72da5541b2ad3d9d1c8d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354328"
---
# <a name="lesson-1-4---adding-package-configurations"></a>Leçon 1-4 : Ajout de configurations au package

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Au cours de cette tâche, vous allez ajouter une configuration à chaque package. Les configurations mettent à jour les valeurs des propriétés de package et des objets de package au moment de l'exécution.  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] fournit un éventail de types de configuration. Vous pouvez stocker des configurations dans des variables d'environnement, des entrées de registre, des variables définies par l'utilisateur, des tables [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et des fichiers XML. Pour une souplesse accrue, [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] prend en charge l'utilisation des configurations indirectes. Cela signifie que vous utilisez une variable d'environnement pour spécifier l'emplacement de la configuration qui de son côté spécifie les valeurs réelles. Les packages du projet Didacticiel de déploiement utilisent une combinaison des fichiers de configuration XML et des configurations indirectes. Un fichier de configuration XML peut inclure des configurations destinées à plusieurs propriétés et, le cas échéant, peut être référencé par plusieurs packages. Dans ce didacticiel, vous allez utiliser un fichier de configuration séparé pour chaque package.  
  
Les fichiers de configuration contiennent fréquemment des informations sensibles, telles que les chaînes de connexion. Par conséquent, vous devez utiliser une liste de contrôle d'accès (ACL) pour restreindre l'accès à l'emplacement ou au dossier où vous stockez les fichiers, et donner accès uniquement aux utilisateurs ou aux comptes autorisés à exécuter des packages. Pour plus d’informations, consultez [Accéder aux fichiers utilisés par des packages](../integration-services/security/security-overview-integration-services.md#files).  
  
Les packages (DataTransfer et LoadXMLData) que vous avez ajoutés au projet Didacticiel de déploiement dans la tâche précédente nécessitent des configurations pour s'exécuter correctement après leur déploiement sur le serveur cible. Pour implémenter des configurations, vous allez d'abord créer les configurations indirectes pour les fichiers de configuration XML, puis créer les fichiers de configuration XML.  
  
Vous allez créer deux fichiers de configuration, DataTransferConfig.dtsConfig et LoadXMLData.dtsConfig. Ces fichiers contiennent des paires nom-valeur qui mettent à jour les propriétés des packages spécifiant l'emplacement des données et des fichiers journaux utilisés par le package. Dans le cadre du processus de déploiement, vous serez amené à mettre à jour les valeurs dans les fichiers de configuration pour refléter le nouvel emplacement des fichiers sur l'ordinateur de destination.  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] reconnaît que DataTransferConfig.dtsConfig et LoadXMLData.dtsConfig sont des dépendances des packages DataTransfer et LoadXMLData, et inclut automatiquement les fichiers de configuration lorsque vous créez l'application de déploiement dans la leçon suivante.  
  
### <a name="to-create-indirect-configuration-for-the-datatransfer-package"></a>Pour créer des configurations indirectes pour le package DataTransfer  

Vérifiez le modèle de déploiement actuel du projet et définissez-le si nécessaire sur **Modèle de déploiement de package**. Dans le menu **Projet**, cliquez sur **Convertir en modèle de déploiement de package**.
  
1.  Dans l'Explorateur de solutions, double-cliquez sur DataTransfer.dtsx.  
  
2.  Dans le Concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] , cliquez n'importe où dans l'arrière-plan de la surface de dessin du flux de contrôle.  
  
3.  Dans le menu **SSIS** , cliquez sur **Configurations du package**.  
  
4.  Dans la boîte de dialogue **Bibliothèque des configurations du package**, sélectionnez **Activer les configurations du package** le cas échéant, et cliquez sur **Ajouter**.  
  
5.  Dans la page Assistant Configuration de package, cliquez sur **Suivant**.  
  
6.  Dans la page Sélectionner le type de configuration, sélectionnez **Fichier de configuration XML** dans la liste **Type de configuration** , sélectionnez l'option **L'emplacement de la configuration est stocké dans une variable d'environnement** , et tapez **DataTransfer** ou sélectionnez la variable d'environnement **DataTransfer** dans la liste.  
  
    > [!NOTE]  
    > Pour que la variable d'environnement soit disponible dans la liste, vous devrez peut-être redémarrer votre ordinateur après l'ajout de la variable. Si vous ne souhaitez pas redémarrer l'ordinateur, vous pouvez taper le nom de la variable d'environnement.  
  
7.  Cliquez sur **Suivant**.  
  
8.  Dans la page Fin de l'Assistant, tapez **DataTransfer EV Configuration** dans la zone **Nom de la configuration** , examinez le contenu de la configuration dans le volet **Aperçu** , puis cliquez sur **Terminer**.  
  
9. Fermez la boîte de dialogue **Bibliothèque des configurations du package**.  
  
### <a name="to-create-the-xml-configuration-for-the-datatransfer-package"></a>Pour créer des configurations XML pour le package DataTransfer  
  
1.  Dans l'Explorateur de solutions, double-cliquez sur DataTransfer.dtsx.  
  
2.  Dans le Concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] , cliquez n'importe où dans l'arrière-plan de la surface de dessin du flux de contrôle.  
  
3.  Dans le menu **SSIS** , cliquez sur **Configurations du package**.  
  
4.  Dans la boîte de dialogue Bibliothèque des configurations du package, cochez la case **Activer les configurations du package** , puis cliquez sur **Ajouter**.  
  
5.  Dans la page Assistant Configuration de package, cliquez sur **Suivant**.  
  
6.  Dans la page Sélectionner le type de configuration, sélectionnez **Fichier de configuration XML** dans la liste **Type de configuration** , puis cliquez sur **Parcourir**.  
  
7.  Dans la boîte de dialogue **Sélectionner l’emplacement du fichier de configuration** , accédez à C:\DeploymentTutorial et tapez **DataTransferConfig** dans la zone **Nom de fichier** , puis cliquez sur **Enregistrer**.  
  
8.  Dans la page Sélectionner le type de configuration, cliquez sur **Suivant**.  
  
9. Dans la page Sélectionner les propriétés à exporter, développez successivement DataTransfer, Gestionnaires de connexions, Journal Didacticiel de déploiement et Propriétés, puis cochez la case **Chaîne de connexion** .  
  
10. Dans Gestionnaires de connexions, développez NewCustomers, puis cochez la case **Chaîne de connexion** .  
  
11. Cliquez sur **Suivant**.  
  
12. Dans la page Fin de l'Assistant, tapez **DataTransfer Configuration** dans la zone **Nom de la configuration** , examinez le contenu de la configuration puis cliquez sur **Terminer**.  
  
13. Dans la boîte de dialogue **Bibliothèque des configurations du package** , vérifiez que DataTransfer EV Configuration est en haut de la liste et que DataTransfer Configuration est second, puis cliquez sur **Fermer**.  
  
### <a name="to-create-indirect-configuration-for-the-loadxmldata-package"></a>Pour créer des configurations indirectes pour le package LoadXMLData  
  
1.  Dans l'Explorateur de solutions, double-cliquez sur LoadXMLData.dtsx.  
  
2.  Dans le Concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] , cliquez n'importe où dans l'arrière-plan de la surface de dessin du flux de contrôle.  
  
3.  Dans le menu **SSIS** , cliquez sur **Configurations du package**.  
  
4.  Dans la boîte de dialogue **Bibliothèque des configurations du package**, cliquez sur **Ajouter**.  
  
5.  Dans la page Assistant Configuration de package, cliquez sur **Suivant**.  
  
6.  Dans la page Sélectionner le type de configuration, sélectionnez **Fichier de configuration XML** dans la liste **Type de configuration** , sélectionnez l'option **L'emplacement de la configuration est stocké dans une variable d'environnement** , et tapez **LoadXMLData** ou sélectionnez la variable d'environnement **LoadXMLData** dans la liste.  
  
    > [!NOTE]  
    > Pour que la variable d'environnement soit disponible dans la liste, vous devrez peut-être redémarrer votre ordinateur après l'ajout de la variable.  
  
7.  Cliquez sur **Suivant**.  
  
8.  Dans la page Fin de l'Assistant, tapez **LoadXMLData EV Configuration** dans la zone **Nom de la configuration** , examinez le contenu de la configuration puis cliquez sur **Terminer**.  
  
### <a name="to-create-the-xml-configuration-for-the-loadxmldata-package"></a>Pour créer la configuration XML pour le package LoadXMLData  
  
1.  Dans l'Explorateur de solutions, double-cliquez sur LoadXMLData.dtsx.  
  
2.  Dans le Concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] , cliquez n'importe où dans l'arrière-plan de la surface de dessin du flux de contrôle.  
  
3.  Dans le menu **SSIS** , cliquez sur **Configurations du package**.  
  
4.  Dans la boîte de dialogue Bibliothèque des configurations du package, cochez la case **Activer les configurations du package** et cliquez sur **Ajouter**.  
  
5.  Dans la page Assistant Configuration de package, cliquez sur **Suivant**.  
  
6.  Dans la page Sélectionner le type de configuration, sélectionnez **Fichier de configuration XML** dans la liste **Type de configuration** , puis cliquez sur **Parcourir**.  
  
7.  Dans la boîte de dialogue **Sélectionner l’emplacement du fichier de configuration** , accédez à C:\DeploymentTutorial et tapez **LoadXMLDataConfig** dans la zone **Nom de fichier** , puis cliquez sur **Enregistrer**.  
  
8.  Dans la page Sélectionner le type de configuration, cliquez sur **Suivant**.  
  
9. Dans la page Sélectionner les propriétés à exporter, développez successivement LoadXMLData, Exécutables, Charger les données XML et Propriétés, puis cochez les cases **[XMLSource].[XMLData]** et **[XMLSource].[XMLSchemaDefinition]** .  
  
10. Cliquez sur **Suivant**.  
  
11. Dans la page Fin de l'Assistant, tapez **LoadXMLData Configuration** dans la zone **Nom de la configuration** , examinez le contenu de la configuration puis cliquez sur **Terminer**.  
  
12. Dans la boîte de dialogue **Bibliothèque des configurations du package** , vérifiez que LoadXMLData EV Configuration est en haut de la liste et que LoadXMLData Configuration est second, puis cliquez sur **Fermer**.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Étape 5 : Test des packages mis à jour](../integration-services/lesson-1-5-testing-the-updated-packages.md)  
  
## <a name="see-also"></a>Voir aussi  
[Configurations de package](./packages/legacy-package-deployment-ssis.md)  
[Créer des configurations de package](./packages/legacy-package-deployment-ssis.md)  
[Accéder aux fichiers utilisés par des packages](../integration-services/security/security-overview-integration-services.md#files)