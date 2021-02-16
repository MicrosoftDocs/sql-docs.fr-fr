---
description: Utilitaire d’exécution de package (dtexecui)
title: Utilitaire d’exécution de package (dtexecui) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.dtexecui.setvalues.f1
- sql13.dts.dtexecui.reporting.f1
- sql13.dts.dtexecui.datasources.f1
- sql13.dts.dtexecui.commandfiles.f1
- sql13.dts.dtexecui.logging.f1
- sql13.dts.dtexecui.general.f1
- sql13.dts.dtexecui.verification.f1
- sql13.dts.dtexecui.executionoptions.f1
- sql13.dts.dtexecui.commandline.f1
- sql13.dts.dtexecui.configuration.f1
helpviewer_keywords:
- DTExecUI utility
ms.assetid: 3d71df39-126b-4c8e-bd77-128bbd5b0887
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 14392191ab3d11165f75f1637c2fb3330aa886ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346912"
---
# <a name="execute-package-utility-dtexecui"></a>Utilitaire d’exécution de package (dtexecui)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Utilisez l' **Utilitaire d'exécution de package** pour exécuter des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . L’utilitaire exécute les packages stockés à l’un des trois emplacements suivants : la base de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le magasin de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] et le système de fichiers. Cette interface utilisateur, qui peut être ouverte à partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou en tapant **dtexecui** à partir d’une invite de commandes, offre un autre moyen d’exécuter des packages à l’aide de l’outil d’invite de commandes **DTExec** .  
  
 Les packages sont exécutés dans le même processus que l’utilitaire **dtexecui.exe** . Cet utilitaire étant un outil 32 bits, les packages exécutés à l’aide de **dtexecui.exe** dans un environnement 64 bits s’exécutent dans Windows sur Win32 (WOW). Quand vous développez et testez des commandes à l’aide de l’utilitaire dtexecui.exe sur un ordinateur 64 bits, effectuez les tests en mode 64 bits en utilisant la version 64 bits de **dtexec.exe** avant de déployer ou de planifier les commandes sur un serveur de production.  
  
 L’ **utilitaire d’exécution de package** est une interface utilisateur graphique de l’outil d’invite de commandes **DTExec** . L’interface utilisateur facilite la configuration des options et assemble automatiquement la ligne de commande qui est transmise à l’outil d’invite de commandes **DTExec** lorsque vous exécutez le package à partir des options spécifiées.  
  
 L’ **utilitaire d’exécution de package** permet également d’assembler les lignes de commande que vous utilisez lorsque vous exécutez **DTExec** directement.  
  
### <a name="to-open-execute-package-utility-in-sql-server-management-studio"></a>Pour ouvrir l'Utilitaire d'exécution de package dans SQL Server Management Studio  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], cliquez sur **Explorateur d'objets** dans le menu **Affichage**.  
  
2.  Dans l'Explorateur d'objets, cliquez sur **Se connecter**, puis sur **Integration Services**.  
  
3.  Dans la boîte de dialogue **Se connecter au serveur** , entrez le nom du serveur dans la liste **Nom du serveur** , puis cliquez sur **Se connecter**.  
  
4.  Développez le dossier **Packages stockés** et ses sous-dossiers, cliquez avec le bouton droit sur le package à exécuter, puis cliquez sur **Exécuter le package**.  
  
### <a name="to-open-the-execute-package-utility-at-the-command-prompt"></a>Pour ouvrir l'Utilitaire d'exécution de package à l'invite de commandes  
  
-   Dans une fenêtre d'invite de commandes, exécutez **dtexecui**.  
  
 Les sections suivantes décrivent les pages de la boîte de dialogue **Utilitaire d'exécution de package** .  
  
## <a name="general-page"></a>Page Général  
 Utilisez la page **Général** de la boîte de dialogue **Utilitaire d'exécution de package** pour spécifier le nom et l'emplacement d'un package.  
  
 L'utilitaire d'exécution de package (dtexecui.exe) exécute toujours un package sur l'ordinateur local, même si le package est enregistré sur un serveur distant. Si le package distant utilise des fichiers de configuration qui sont toujours enregistrés sur le serveur distant, l'utilitaire d'exécution de package risque de ne pas trouver l'emplacement des configurations, faisant ainsi échouer le package. Pour éviter ce problème, les configurations doivent être référencées par un nom de partage UNC, tel que \\\monserveur\monfichier.  
  
### <a name="static-options"></a>Options statiques  
 **Source du package**  
 Spécifiez l'emplacement du package à exécuter, à l'aide des options suivantes :  
  
|Valeur|Description|  
|-|-|  
|**SQL Server**|Sélectionnez cette option lorsque le package se trouve dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Spécifiez une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et fournissez un nom d'utilisateur et un mot de passe pour l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Chaque nom d’utilisateur et chaque mot de passe ajoutent les options **/USER** _nom_utilisateur_ et **/PASSWORD** _mot_de_passe_ à l’invite de commandes.|  
|**Système de fichiers**|Sélectionnez cette option lorsque le package se trouve dans le système de fichiers.|  
|**Magasin de packages SSIS**|Sélectionnez cette option lorsque le package se trouve dans le magasin de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] .|  
  
 Chacune des sélections ci-dessus comporte la série d'options suivante :  
  
 **Execute**  
 Permet d'exécuter le package.  
  
 **Close**  
 Ferme la boîte de dialogue **Utilitaire d’exécution de package** .  
  
### <a name="dynamic-options"></a>Options dynamiques  
  
#### <a name="package-source--sql-server"></a>Source du package = SQL Server  
 **Serveur**  
 Tapez le nom du serveur où se trouve le package ou choisissez un serveur dans la liste.  
  
 **Connexion au serveur**  
 Indiquez si le package doit utiliser l’authentification Windows ou l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'authentification Windows est recommandée pour renforcer la sécurité. Avec l'authentification Windows, vous n'avez pas à spécifier un nom d'utilisateur et un mot de passe.  
  
 **Utiliser l'authentification Windows**  
 Sélectionnez cette option pour utiliser l’authentification Windows et connectez-vous à l’aide d’un compte d’utilisateur [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 **Utiliser l’authentification SQL Server**  
 Sélectionnez cette option pour utiliser l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Quand un utilisateur se connecte avec un nom d’accès et un mot de passe spécifiés à partir d’une connexion non autorisée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] réalise l’authentification en vérifiant si un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été défini et si le mot de passe spécifié correspond à celui enregistré précédemment. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas trouver le compte de connexion, l'authentification échoue et l'utilisateur reçoit un message d'erreur.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, utilisez l'authentification Windows.  
  
 **Package**  
 Tapez le nom du package ou cliquez sur le bouton de sélection **(...)** pour identifier l’emplacement d’un package grâce à la boîte de dialogue **Sélectionner un package SSIS**.  
  
#### <a name="package-source--file-system"></a>Source du package = Système de fichiers  
 **Package**  
 Tapez le nom du package ou cliquez sur le bouton qui contient des points de suspension **(...)** pour identifier l’emplacement d’un package grâce à la boîte de dialogue Ouvrir. Par défaut, cette boîte de dialogue répertorie uniquement les fichiers dotés de l'extension .dtsx.  
  
#### <a name="package-source--ssis-package-store"></a>Source du package = Magasin de packages SSIS  
 **Serveur**  
 Tapez le nom de l'ordinateur où se trouve le package ou choisissez un ordinateur dans la liste.  
  
 **Connexion au serveur**  
 Indiquez si le package doit utiliser l'authentification Microsoft Windows pour se connecter à la source du package. L'authentification Windows est recommandée pour renforcer la sécurité. Avec l'authentification Windows, vous n'avez pas à spécifier un nom d'utilisateur et un mot de passe.  
  
 **Utiliser l'authentification Windows**  
 Sélectionnez cette option pour utiliser l'authentification Windows et connectez-vous à l'aide d'un compte d'utilisateur Microsoft Windows.  
  
 **Utiliser l’authentification SQL Server**  
 Cette option est désactivée quand vous exécutez un package stocké dans le **Magasin de packages SSIS**.  
  
 **Package**  
 Tapez le nom du package ou cliquez sur le bouton de sélection **(...)** pour identifier l’emplacement d’un package grâce à la boîte de dialogue **Sélectionner un package SSIS**.  
  
## <a name="configurations-page"></a>Page Configurations  
 Utilisez la page **Configurations** de la boîte de dialogue **Utilitaire d'exécution de package** pour sélectionner les fichiers de configuration à charger au moment de l'exécution et spécifier l'ordre de chargement.  
  
### <a name="options"></a>Options  
 **Fichiers de configuration**  
 Répertorie les configurations que le package utilise. Chaque fichier de configuration ajoute une option **/CONFIGFILE nom_fichier** à l’invite de commandes.  
  
 **Touches de direction**  
 Sélectionnez un fichier de configuration dans la liste et utilisez les touches de direction à droite pour modifier l'ordre de chargement. Les configurations se chargent à partir du haut de la liste.  
  
> [!NOTE]  
>  Si plusieurs configurations modifient la même propriété, la dernière configuration chargée est utilisée.  
  
 **Ajouter**  
 Ajoute des configurations au moyen de la boîte de dialogue **Ouvrir** . Par défaut, cette boîte de dialogue affiche uniquement les fichiers ayant l'extension .dtsconfig.  
  
 **Remove**  
 Sélectionnez un fichier de configuration dans la liste, puis cliquez sur **Supprimer**.  
  
 **Execute**  
 Permet d'exécuter le package.  
  
 **Close**  
 Ferme la boîte de dialogue **Utilitaire d’exécution de package** .  
  
## <a name="command-files-page"></a>Page Fichiers de commandes  
 La page **Fichiers de commandes** de la boîte de dialogue **Utilitaire d'exécution de package** permet de sélectionner les fichiers de commandes devant être chargés au moment de l'exécution du programme.  
  
### <a name="options"></a>Options  
 **Fichiers de commandes**  
 Répertorie les fichiers de commandes que le package utilise. Un package peut utiliser plusieurs fichiers afin de définir des options de ligne de commande.  
  
 **Touches de direction**  
 Permet de choisir un fichier de commandes dans la liste puis de changer l'ordre de séquence de chargement des fichiers par les touches fléchées de droite. Ces fichiers se chargent dans l'ordre en partant du haut de la liste.  
  
 **Ajouter**  
 Ce bouton vous permet d’ouvrir la boîte de dialogue **Ouvrir** pour ajouter un fichier de commandes.  
  
 **Remove**  
 Permet de sélectionner un fichier de commandes dans la zone de texte, puis de le supprimer par le biais du bouton **Supprimer** .  
  
 **Execute**  
 Permet d'exécuter le package.  
  
 **Close**  
 Ferme la boîte de dialogue **Utilitaire d’exécution de package** .  
  
## <a name="connection-managers-page"></a>Page Gestionnaires de connexions  
 Utilisez la page **Gestionnaires de connexions** de la boîte de dialogue **Utilitaire d'exécution de package** pour modifier les chaînes de connexion des gestionnaires de connexions qu'utilise le package.  
  
### <a name="options"></a>Options  
 **Gestionnaire de connexions**  
 Cochez cette case pour pouvoir modifier la colonne **Chaîne de connexion** .  
  
 **Description**  
 Affiche la description de chaque gestionnaire de connexions. Les descriptions ne peuvent pas être modifiées.  
  
 **Chaîne de connexion**  
 Modifie la chaîne de connexion d'un gestionnaire de connexions. Ce champ est modifiable uniquement lorsque la case à cocher **Gestionnaire de connexions** est activée.  
  
 **Execute**  
 Permet d'exécuter le package.  
  
 **Close**  
 Ferme la boîte de dialogue **Utilitaire d’exécution de package** .  
  
## <a name="execution-options-page"></a>Page Options d'exécution  
 Utilisez la page **Options d’exécution** de la boîte de dialogue **Utilitaire d’exécution de package** pour spécifier les options d’exécution du package.  
  
### <a name="options"></a>Options  
 **Mettre le package en échec en cas d'avertissements de validation**  
 Indique si le package échoue si un avertissement de validation est détecté.  
  
 **Valider le package sans l'exécuter**  
 Indique si le package est seulement validé.  
  
 **Maximum d'exécutables simultanés**  
 Indique si vous souhaitez spécifier le nombre maximal d'exécutables qui peuvent s'exécuter simultanément dans le package. Après avoir activé cette case à cocher, utilisez la zone de sélection numérique pour spécifier le nombre maximal d'exécutables.  
  
 **Activer les points de contrôle de package**  
 Indique s'il faut activer les points de contrôle du package.  
  
 **Fichier de point de contrôle**  
 Affiche le fichier de points de contrôle que le package utilise, si vous activez les points de contrôle du package.  
  
 **Parcourir**  
 Cliquez sur le bouton Parcourir **(...)** pour rechercher le fichier de points de contrôle au moyen de la boîte de dialogue **Ouvrir**, si vous activez les points de contrôle du package. Si un fichier de points de contrôle est déjà spécifié, il est remplacé par le fichier sélectionné.  
  
 **Substituer les options de redémarrage**  
 Indique s'il faut ignorer les options de redémarrage, si vous activez les points de contrôle du package.  
  
 **Option de redémarrage**  
 Indique comment utiliser les points de contrôle si vous ignorez les options de redémarrage.  
  
 **Execute**  
 Permet d'exécuter le package.  
  
 **Close**  
 Ferme la boîte de dialogue **Utilitaire d’exécution de package** .  
  
## <a name="reporting-page"></a>Page Création de rapports  
 La page **Création de rapports** de la boîte de dialogue **Utilitaire d'exécution de package** permet de spécifier les événements et informations relatifs au package à consigner dans la console lors de l'exécution du package.  
  
### <a name="options"></a>Options  
 **Événements de la console**  
 Indique les événements et types de messages à reporter.  
  
 **Aucun**  
 Sélectionnez cette option pour ne pas générer de rapports.  
  
 **Erreurs**  
 Sélectionnez cette option pour générer un rapport sur les messages d'erreur.  
  
 **Avertissements**  
 Sélectionnez cette option pour générer un rapport sur les messages d'avertissement.  
  
 **Événements personnalisés**  
 Sélectionnez cette option pour générer un rapport sur les événements personnalisés.  
  
 **Événements de pipeline**  
 Sélectionnez cette option pour générer un rapport sur les messages d'événements de flux de données.  
  
 **Informations**  
 Sélectionnez cette option pour générer un rapport sur les messages d'information.  
  
 **Verbose**  
 Sélectionnez cette option pour utiliser les rapports de commentaire.  
  
 **Écriture dans le journal de la console**  
 Spécifiez les informations à écrire dans le journal lorsque l'événement sélectionné se produit.  
  
 **Nom**  
 Sélectionnez cette option pour générer un rapport sur le nom de la personne qui a créé le package.  
  
 **Ordinateur**  
 Sélectionnez cette option pour générer un rapport sur le nom de l'ordinateur sur lequel le package s'exécute.  
  
 **Opérateur**  
 Sélectionnez cette option pour générer un rapport sur le nom de la personne qui a lancé le package.  
  
 **Nom de la source**  
 Sélectionnez cette option pour générer un rapport sur le nom du package.  
  
 **GUID source**  
 Sélectionnez cette option pour générer un rapport sur le GUID du package.  
  
 **GUID d'exécution**  
 Sélectionnez cette option pour générer un rapport sur le GUID de l'instance d'exécution du package.  
  
 **Message**  
 Sélectionnez cette option pour générer un rapport sur les messages.  
  
 **Heure de début et heure de fin**  
 Sélectionnez cette option pour générer un rapport sur l'heure à laquelle le package a commencé et s'est terminé.  
  
 **Execute**  
 Permet d'exécuter le package.  
  
 **Close**  
 Ferme la boîte de dialogue **Utilitaire d’exécution de package** .  
  
## <a name="logging-page"></a>Page Enregistrement  
 La page **Enregistrement** de la boîte de dialogue **Utilitaire d'exécution de package** permet de créer des modules fournisseur d'informations mis à disposition du package au moment de l'exécution du programme. Vous devez fournir le type du module fournisseur d'informations du package ainsi que la chaîne de connexion afin de pouvoir vous connecter au journal. Chaque entrée de module fournisseur d’informations ajoute une option **/LOGGER**_ID_classe_ à l’invite de commandes.  
  
### <a name="options"></a>Options  
 **Module fournisseur d’informations**  
 Permet de choisir un module fournisseur d'informations dans la liste.  
  
 **Chaîne de configuration**  
 Permet de choisir un nom pour le gestionnaire de connexions à partir du package pointant vers l'emplacement du journal ou de saisir complètement la chaîne de connexion afin de pouvoir se connecter au module fournisseur d'informations.  
  
 **Remove**  
 Permet de sélectionner un module fournisseur d'informations pour le supprimer.  
  
 **Execute**  
 Permet d'exécuter le package.  
  
 **Close**  
 Ferme la boîte de dialogue **Utilitaire d’exécution de package** .  
  
## <a name="set-values-page"></a>Page Valeurs définies  
 Utilisez la page **Valeurs définies** de la boîte de dialogue **Utilitaire d'exécution de package** pour définir les valeurs des propriétés des packages, des fichiers exécutables, des connexions, des variables et des modules fournisseur d'informations. Pour cela, tapez les chemins d'accès aux propriétés et leurs valeurs. Chaque entrée de chemin d’accès ajoute une option **/SET**_chemin_propriété;valeur_ à l’invite de commandes.  
  
### <a name="options"></a>Options  
 **Chemin de la propriété**  
 Tapez le chemin d'accès à la propriété. Le chemin doit comporter une barre oblique inverse (\\) pour indiquer que l’élément suivant est un conteneur, un point pour indiquer que l’élément suivant est une propriété et des parenthèses pour indiquer un membre d’une collection. Il est possible d'identifier le membre par son index ou son nom. Par exemple, le chemin d'accès à une variable d'un package est : \Package.Variables[MyVariable].Value.  
  
 **Valeur**  
 Tapez la valeur de la propriété.  
  
 **Remove**  
 Sélectionnez le chemin d'accès à une propriété et cliquez dessus pour la supprimer.  
  
 **Execute**  
 Permet d'exécuter le package.  
  
 **Close**  
 Ferme la boîte de dialogue **Utilitaire d’exécution de package** .  
  
## <a name="verification-page"></a>Page Vérification  
 Utilisez la page **Vérification** de la boîte de dialogue **Exécuter le package** pour spécifier les critères de vérification du package.  
  
### <a name="options"></a>Options  
 **Exécuter uniquement les packages signés**  
 Exécute uniquement les packages signés.  
  
 **Vérifier la build du package**  
 Vérifie la build du package.  
  
 Build  
 Spécifiez le numéro séquentiel de la build du package.  
  
 **Vérifier l'ID de package**  
 Vérifie la version l'ID du package.  
  
 ID du package  
 Spécifiez le numéro d'identification du package.  
  
 **Vérifier l'ID de version**  
 Vérifie l'ID de la version du package.  
  
 ID de version  
 Spécifiez le numéro d'identification de la version.  
  
 **Execute**  
 Permet d'exécuter le package.  
  
 **Close**  
 Ferme la boîte de dialogue **Utilitaire d’exécution de package** .  
  
## <a name="command-line-page"></a>Page Ligne de commande  
 Le nœud **Ligne de commande** de la boîte de dialogue **Utilitaire d'exécution du package** permet de modifier la ligne de commande ayant été générée par les options créées par les différentes boîtes de dialogue.  
  
### <a name="options"></a>Options  
 **Restaurer les options d'origine**  
 Permet de ramener la ligne de commande à son état initial. Utilisez cette option si vous avez apporté des modifications par le biais de l’option **Modifier la ligne de commande manuellement** et que vous voulez restaurer les options de ligne de commande initiales.  
  
 **Modifier la ligne de commande manuellement**  
 Permet de modifier la ligne de commande figurant dans la zone de texte **Ligne de commande** .  
  
 **Ligne de commande**  
 Affiche la ligne de commande actuelle. Cet élément est modifiable si vous avez activé l'option de modifier la ligne de commande manuellement.  
  
 **Execute**  
 Permet d'exécuter le package.  
  
 **Close**  
 Ferme la boîte de dialogue **Utilitaire d’exécution de package** .  
  
## <a name="see-also"></a>Voir aussi  
 [Utilitaire dtexec](../../integration-services/packages/dtexec-utility.md)  
  
  
