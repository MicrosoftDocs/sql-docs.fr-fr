---
title: Mettre à niveau les runtimes Python et R (liaison)
description: Mettez à niveau les runtimes Python et R dans SQL Server Machine Learning Services ou SQL Server R Services à l’aide de sqlbindr.exe pour établir une liaison avec Machine Learning Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 09/30/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: 7cdf5b2d0c223130245604f5b27674a714ab4d8a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082372"
---
# <a name="upgrade-python-and-r-runtime-with-binding-in-sql-server-machine-learning-services"></a>Mettre à niveau le runtime Python et R avec la liaison dans SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 and 2017](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

Cet article explique comment utiliser le processus d’installation appelé **liaison** pour mettre à niveau les runtimes R ou Python dans [SQL Server 2016 R Services](../r/sql-server-r-services.md) ou [SQL Server 2017 Machine Learning Services](../sql-server-machine-learning-services.md). Vous pouvez vous procurer des [versions plus récentes de Python et R](#version-map) via une *liaison* à [Microsoft Machine Learning Server](/machine-learning-server).

> [!IMPORTANT]
> Cet article décrit une ancienne méthode de mise à niveau des runtimes R et Python appelée *liaison*. Si vous avez installé la **Mise à jour cumulative (CU) 14 ou une version ultérieure pour SQL Server 2016 Service Pack (SP) 2** ou la **Mise à jour cumulative 22 ou une version ultérieure pour SQL Server 2017**, découvrez comment [changer le runtime R ou Python par défaut vers une version plus récente](change-default-language-runtime-version.md) à la place.

## <a name="what-is-binding"></a>Qu’est-ce qu’une liaison ?

La liaison est un processus d’installation qui remplace le contenu de vos dossiers **R_SERVICES** et **PYTHON_SERVICES** par des fichiers exécutables, des bibliothèques et des outils plus récents de [Microsoft Machine Learning Server](/machine-learning-server).

Les composants chargés inclus dans le modèle de maintenance ont été modifiés. Les mises à jour du service correspondent à la [chronologie de la prise en charge pour Microsoft R Server & Machine Learning Server](/machine-learning-server/resources-servicing-support) sur le [cycle de vie moderne](https://support.microsoft.com/help/30881/modern-lifecycle-policy).

À l’exception des versions de composant et des mises à jour de service, la liaison ne modifie pas les bases de votre installation :

- L’intégration de Python et de R fait toujours partie d’une instance du moteur de base de données.
- La gestion des licences est inchangée (aucun coût supplémentaire n’est associé à la liaison).
- Les stratégies de prise en charge de SQL Server sont toujours valables pour le moteur de base de données.

Le reste de cet article décrit le mécanisme de liaison et son fonctionnement pour chaque version de SQL Server.

> [!NOTE]
> La liaison s’applique uniquement aux instances dans la base de données qui sont liées à des instances SQL Server. Dans ce cas, la liaison n’est pas nécessaire pour une installation autonome.

::: moniker range="=sql-server-2016"
**Considérations relatives à la liaison SQL Server 2016**

Pour les clients SQL Server R 2016 R Services, la liaison fournit les éléments suivants :

- Packages R mis à niveau.
- Les nouveaux packages ne font pas partie de l’installation d’origine ([MicrosoftML](/machine-learning-server/r-reference/microsoftml/microsoftml-package))
- [Modèles](/machine-learning-server/install/microsoftml-install-pretrained-models) de Machine Learning préformés pour l’analyse des sentiments et la détection d’images.

Toutes les liaisons peuvent être actualisées à chaque nouvelle version majeure et mineure de [Microsoft Machine Learning Server](/machine-learning-server/index).
::: moniker-end

## <a name="version-map"></a>Mappage de version

Les tables suivantes sont des mappages de version. Chaque mappage affiche les versions des packages entre les versions. Vous pouvez passer en revue les chemins de mise à niveau lorsque vous effectuez une liaison à Microsoft Machine Learning Server (anciennement R Server, avant l’ajout de la prise en charge de Python à compter de Machine Learning Server 9.2.1).

La liaison ne garantit pas de disposer de la dernière version de R ou d’Anaconda. Lorsque vous établissez une liaison à Microsoft Machine Learning Server, vous disposez de la version R ou Python installée via le programme d’installation. Il peut donc s’agir d’une version différente de la dernière version disponible sur le Web.

::: moniker range="=sql-server-2016"
[**SQL Server 2016 R Services**](../install/sql-r-services-windows-install.md)

Composant |Version initiale | [R Server 9.0.1](/machine-learning-server/install/r-server-install-windows) | [R Server 9.1](/machine-learning-server/install/r-server-install-windows) | [Machine Learning Server 9.2.1](/machine-learning-server/install/machine-learning-server-windows-install) | [Machine Learning Server 9.3](/machine-learning-server/install/machine-learning-server-windows-install) |  [Machine Learning Server 9.4.7](/machine-learning-server/install/machine-learning-server-windows-install)
----------|----------------|----------------|--------------|---------|-------|-------|
Microsoft R Open (MRO) sur R | R 3.2.2     | R 3.3.2   |R 3.3.3   | R 3.4.1  | R 3.4.3 | R 3.5.2
[RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler) | 8.0.3  | 9.0.1 |  9,1 |  9.2.1 |  9.3 |  9.4.7 |
[MicrosoftML](/machine-learning-server/r-reference/microsoftml/microsoftml-package)| n.a. | 9.0.1 |  9,1 |  9.2.1 |  9.3 | 9.4.7 |
[modèles pré-entraînés](/machine-learning-server/install/microsoftml-install-pretrained-models)| n.a. | 9.0.1 |  9,1 |  9.2.1 |  9.3 | 9.4.7 |
[sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils)| n.a. | 1.0 |  1.0 |  1.0 |  1.0 | 1.0 |
[olapR](/machine-learning-server/r-reference/olapr/olapr) | n.a. | 1.0 |  1.0 |  1.0 |  1.0 | 1.0 |
::: moniker-end

::: moniker range="=sql-server-2017"
[**SQL Server 2017 Machine Learning Services**](../install/sql-machine-learning-services-windows-install.md)

Composant |Version initiale | Machine Learning Server 9.3 | Machine Learning Server 9.4.7 |
----------|----------------|---------|---------|
Microsoft R Open (MRO) sur R | R 3.3.3 | R 3.4.3 | R 3.5.2 |
[RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler) |   9.2 |  9.3 | 9.4.7 |
[MicrosoftML](/machine-learning-server/r-reference/microsoftml/microsoftml-package) | 9.2  | 9.3| 9.4.7 |
[sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils)| 1.0 |  1.0 | 1.0 |
[olapR](/machine-learning-server/r-reference/olapr/olapr) | 1.0 |  1.0 | 1.0 |
Anaconda 4.2 sur Python 3.5  | 4.2/3.5.2 | 4.2/3.5.2 |
[revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) | 9.2  | 9.3| 9.4.7 |
[microsoftml](/machine-learning-server/python-reference/microsoftml/microsoftml-package) | 9.2  | 9.3| 9.4.7 |
[modèles pré-entraînés](/machine-learning-server/install/microsoftml-install-pretrained-models) | 9.2 | 9.3| 9.4.7 |
::: moniker-end

## <a name="how-component-upgrade-works"></a>Fonctionnement de la mise à niveau des composants

Les fichiers exécutables, les bibliothèques Python et R sont mis à niveau lorsque vous liez une installation existante de Python et R à Machine Learning Server.

La liaison est exécutée par le [programme d’installation Microsoft Machine Learning Server](/machine-learning-server/install/machine-learning-server-windows-install) lorsque vous l’exécutez sur une instance de moteur de base de données SQL Server existante disposant de l’intégration Python ou R. 

Le programme d’installation détecte les fonctionnalités existantes et vous invite à effectuer une nouvelle liaison avec Machine Learning Server.

Au cours de la liaison, le contenu de `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES` et `\PYTHON_SERVICES` est remplacé par les nouveaux fichiers exécutables et les nouvelles bibliothèques de `C:\Program Files\Microsoft\ML Server\R_SERVER` et de `\PYTHON_SERVER`.

La liaison s’applique uniquement aux fonctionnalités Python et R. Les packages open source pour Python et R sont constitués des éléments suivants :

- Anaconda
- Microsoft R Open
- Packages propriétaires RevoScaleR
- Revoscalepy

La liaison ne modifie pas le modèle de prise en charge pour l’instance du moteur de base de données, ni la version de SQL Server.

La liaison est réversible. Vous pouvez rétablir la maintenance SQL Server en [dissociant l’instance](#bkmk_Unbind) et en réparant votre instance de moteur de base de données SQL Server.

<a name="bkmk_BindWizard"></a>

## <a name="bind-to-machine-learning-server-using-setup"></a>Liaison à Machine Learning Server à l’aide du programme d’installation

Suivez les étapes permettant de lier SQL Server à Microsoft Machine Learning Server à l’aide du programme d’installation. 

1. Dans SSMS, exécutez `SELECT @@version` pour vérifier que le serveur répond aux conditions minimales requises pour la build.

   Pour SQL Server 2016 R Services, vous devez disposez au minimum du [Service Pack 1](https://www.microsoft.com/download/details.aspx?id=54276) et de la mise à jour cumulative [CU3](https://support.microsoft.com/help/4019916/cumulative-update-3-for-sql-server-2016-sp1).

1. Vérifiez la version des packages R Base et RevoScaleR pour vous assurer que les versions existantes sont inférieures à celles par lesquelles vous envisagez de les remplacer. 

    ```sql
    EXECUTE sp_execute_external_script
    @language=N'R'
    ,@script = N'str(OutputDataSet);
    packagematrix <- installed.packages();
    Name <- packagematrix[,1];
    Version <- packagematrix[,3];
    OutputDataSet <- data.frame(Name, Version);'
    , @input_data_1 = N''
    WITH RESULT SETS ((PackageName nvarchar(250), PackageVersion nvarchar(max) ))
    ```

1. Fermez SSMS et tous les autres outils disposant d’une connexion ouverte à SQL Server. La liaison remplace les fichiers programme. Si des sessions SQL Server sont ouvertes, la liaison échoue avec le code d’erreur de liaison 6.

1. Téléchargez Microsoft Machine Learning Server sur l’ordinateur hébergeant l’instance que vous souhaitez mettre à niveau. Nous vous recommandons de télécharger la [version la plus récente](/machine-learning-server/install/machine-learning-server-windows-install#download-machine-learning-server-installer).

1. Décompressez le dossier et lancez ServerSetup.exe, sous MLSWIN93.

1. Sur l’écran **Configurer l’installation**, confirmez les composants à mettre à niveau et passez en revue la liste des instances compatibles.

1. Sur la page **Contrat de licence**, sélectionnez **J’accepte ces termes** pour accepter les termes du contrat de licence de Machine Learning Server. 

1. Sur les pages suivantes, donnez votre accord pour les conditions de licence supplémentaires de tous les composants open source que vous avez sélectionnés, tels que Microsoft R Open ou la distribution Python Anaconda.

1. Sur la page **Vous y êtes presque**, prenez note du dossier d’installation. Le dossier par défaut est : \Program Files\Microsoft\ML Server.

    Si vous voulez changer le dossier d’installation, sélectionnez **Avancé** pour revenir à la première page de l’Assistant. Toutefois, vous devez dans ce cas répéter toutes les sélections précédentes.

Si la mise à niveau échoue, consultez les [codes d’erreur SqlBindR](#sqlbindr-error-codes) pour plus d’informations.

## <a name="offline-binding-no-internet-access"></a>Liaison hors connexion (pas d’accès Internet)

Pour les systèmes qui ne sont pas connectés à Internet, vous pouvez télécharger le programme d’installation et les fichiers .cab sur un ordinateur connecté à Internet, puis transférer ces fichiers vers le serveur isolé.

Le programme d’installation (ServerSetup.exe) inclut les packages Microsoft (RevoScaleR, MicrosoftML, olapr, sqlRUtils). Les fichiers .cab fournissent les autres composants de base. Par exemple, le fichier .cab « SRO » fournit R Open, la distribution Microsoft de R open source.

Les instructions suivantes expliquent comment placer les fichiers pour une installation hors connexion.

1. Téléchargez le programme d’installation MLSWIN93. Il est téléchargé en tant que fichier zip unique. Nous vous recommandons d’utiliser la [dernière version](/machine-learning-server/install/machine-learning-server-windows-install#download-machine-learning-server-installer), mais vous pouvez également installer les [versions antérieures](/machine-learning-server/install/r-server-install-windows-offline#download-required-components).

1. Téléchargez les fichiers .cab. Les liens suivants concernent la version 9.3. Si vous avez besoin des versions antérieures, vous trouverez des liens supplémentaires dans [R Server 9.1](/machine-learning-server/install/r-server-install-windows-offline#download-required-components). Rappelez-vous que Python/Anaconda peut uniquement être ajouté à une instance Machine Learning Services SQL Server. Des modèles pré-entraînés existent pour Python et R. Le fichier .cab fournit des modèles dans les langages que vous utilisez.

    | Fonctionnalité | Téléchargement |
    |---------|----------|
    | R       | [SRO_3.4.3.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=867186&clcid=1033) |
    | Python  | [SPO_9.3.0.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=859054) | 
    | Modèles dont l’apprentissage a déjà été effectué | [MLM_9.3.0.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=859053) |

1. Transférez les fichiers .zip et .cab vers le serveur cible.

1. Sur le serveur, tapez `%temp%` dans la commande Exécuter pour récupérer l’emplacement physique du répertoire temporaire. Le chemin physique varie en fonction de l’ordinateur, mais il s’agit généralement de `C:\Users\<your-user-name>\AppData\Local\Temp`.

1. Placez les fichiers .cab dans le dossier %temp%.

1. Décompressez le programme d’installation.

1. Exécutez le fichier ServerSetup.exe et suivez les invites à l’écran pour terminer l’installation.

## <a name="command-line-operations"></a><a name="bkmk_BindCmd"></a>Opérations de ligne de commande


> [!TIP]
> Vous ne trouvez pas SqlBindR ? Vous n’avez probablement pas exécuté le programme d’installation.
> SqlBindR est disponible uniquement après l’exécution du programme d’installation de Machine Learning Server.

1. Ouvrez une invite de commandes en tant qu’administrateur et accédez au dossier contenant sqlbindr.exe. L’emplacement par défaut est : C:\Program Files\Microsoft\MLServer\Setup.

2. Tapez la commande suivante pour afficher la liste des instances disponibles : `SqlBindR.exe /list`
  
   Notez le nom complet de l’instance tel qu’il est répertorié. Par exemple, le nom de l’instance peut être MSSQL14.MSSQLSERVER pour une instance par défaut, ou un nom de type NOMSERVEUR.MONINSTANCENOMMÉE.

3. Exécutez la commande **SqlBindR.exe** avec l’argument */bind*. Spécifiez le nom de l’instance à mettre à niveau en utilisant le nom de l’instance tel qu’il est renvoyé à l’étape précédente.

   Par exemple, pour mettre à niveau l’instance par défaut, tapez : `SqlBindR.exe /bind MSSQL14.MSSQLSERVER`

4. Une fois la mise à niveau terminée, redémarrez le service Launchpad associé à l’instance qui a été modifiée.

## <a name="revert-or-unbind-an-instance"></a><a name="bkmk_Unbind"></a>Rétablir ou annuler la liaison d’une instance

Vous pouvez restaurer une instance liée vers une installation initiale des composants Python et R, établie par le programme d’installation de SQL Server. Pour revenir à la maintenance SQL Server, vous devez suivre trois étapes.

+ [Étape 1 : Annuler la liaison à Microsoft Machine Learning Server](#step-1-unbind)
+ [Étape 2 : Restaurer l’état d’origine de l’instance](#step-2-restore)
+ [Étape 3 : Réinstaller tous les packages que vous aviez ajoutés à l’installation](#step-3-reinstall-packages)

<a name="step-1-unbind"></a> 

### <a name="step-1-unbind"></a>Étape 1 : Annuler la liaison

Vous avez deux options pour restaurer la liaison : réexécuter la configuration ou utiliser l’utilitaire de ligne de commande SqlBindR.

#### <a name="unbind-using-setup"></a><a name="bkmk_wizunbind"></a> Annulation de la liaison à l’aide du programme d’installation

1. Localisez le programme d’installation de Machine Learning Server. Si vous avez supprimé le programme d’installation, vous devrez peut-être le télécharger de nouveau ou le copier à partir d’un autre ordinateur.
2. Assurez-vous d’exécuter le programme d’installation sur l’ordinateur hébergeant l’instance que vous souhaitez séparer.
2. Le programme d’installation identifie les instances locales qui sont candidates pour l’annulation de la liaison.
3. Décochez la case en regard de l’instance pour laquelle vous souhaitez restaurer la configuration d’origine.
4. Acceptez tous les contrats de licence.
5. Sélectionnez **Terminer**. Le processus prend un certain temps.

#### <a name="unbind-using-the-command-line"></a><a name="bkmk_cmdunbind"></a> Annulation de la liaison à l’aide de la ligne de commande

1. Ouvrez une invite de commandes et accédez au dossier qui contient **sqlbindr.exe**, comme décrit dans la section précédente.

2. Exécutez la commande **SqlBindR.exe** avec l’argument */unbind*, puis spécifiez l’instance.

   Par exemple, la commande suivante rétablit l’instance par défaut :
   
    `SqlBindR.exe /unbind MSSQL14.MSSQLSERVER`

<a name="step-2-restore"></a> 

###  <a name="step-2-repair-the-sql-server-instance"></a>Étape 2 : Réparer l’instance SQL Server

Exécutez le programme d’installation de SQL Server pour réparer l’instance du moteur de base de données qui héberge les fonctionnalités Python et R. Les mises à jour préexistantes sont conservées. L’étape suivante s’applique si une mise à jour a été manquée pour les mises à jour de maintenance des packages Python et R.

Autre solution : Désinstallez et réinstallez complètement l’instance du moteur de base de données, puis appliquez toutes les mises à jour du service.

<a name="step-3-reinstall-packages"></a> 

### <a name="step-3-add-any-third-party-packages"></a>Étape 3 : Ajouter des packages tiers

Vous aviez peut-être ajouté d’autres packages tiers ou open source à votre bibliothèque de packages. Étant donné que l’inversion de la liaison modifie l’emplacement de la bibliothèque de packages par défaut, vous devez réinstaller les packages dans la bibliothèque que Python et R utilisent désormais. Pour plus d’informations, consultez les [informations sur les packages R](../package-management/r-package-information.md) et [leur installation](../package-management/install-additional-r-packages-on-sql-server.md), ainsi que les [informations sur les packages Python](../package-management/python-package-information.md) et [leur installation](../package-management/install-additional-python-packages-on-sql-server.md).

## <a name="sqlbindrexe-command-syntax"></a>Syntaxe de commande SqlBindR.exe

### <a name="usage"></a>Usage

`sqlbindr [/list] [/bind <SQL_instance_ID>] [/unbind <SQL_instance_ID>]`

### <a name="parameters"></a>Paramètres

|Nom|Description|
|------|------|
|*list*| Affiche la liste complète des ID d’instances SQL Server de l’ordinateur actuel|
|*bind*| Met à niveau l’instance SQL Server spécifiée vers la dernière version de R Server et fait en sorte qu’elle reçoive automatiquement les mises à niveau ultérieures de R Server|
|*unbind*|Désinstalle la dernière version de R Server de l’instance SQL Server spécifiée et empêche les mises à niveau ultérieures de R Server de s’appliquer à l’instance|

<a name="sqlbindr-error-codes"><a/>

## <a name="binding-errors"></a>Erreurs de liaison

Le programme d’installation de Machine Learning Server et SqlBindR renvoient les codes d’erreur et messages suivants.

|Code d'erreur  | Message           | Détails               |
|------------|-------------------|-----------------------|
|Erreur de liaison 0 | Ok (succès) | La liaison a réussi sans erreur. |
|Erreur de liaison 1 | Arguments non valides | Erreur de syntaxe. |
|Erreur de liaison 2 | Action non valide | Erreur de syntaxe. |
|Erreur de liaison 3 | Instance non valide | Il existe une instance, mais elle n’est pas valide pour la liaison. |
|Erreur de liaison 4 | Ne peut pas être lié | |
|Erreur de liaison 5 | Déjà lié | Vous avez exécuté la commande *bind* , mais l’instance spécifiée est déjà liée. |
|Erreur de liaison 6 | Échec de la liaison | Une erreur est survenue lors de l’annulation de la liaison de l’instance. Cette erreur peut se produire si vous exécutez le programme d’installation de Machine Learning Server sans sélectionner de fonctionnalités. Pour la liaison, vous devez sélectionner à la fois une instance MSSQL et Python et R, en supposant que l’instance soit SQL Server 2017. Cette erreur se produit également si SqlBindR n’a pas pu écrire dans le dossier Program Files. Les sessions ouvertes ou les descripteurs vers SQL Server provoquent cette erreur. Si vous voyez cette erreur, redémarrez l’ordinateur et recommencez les étapes de liaison avant de démarrer de nouvelles sessions.|
|Erreur de liaison 7 | Non lié | L’instance du moteur de base de données héberge R Services ou SQL Server Machine Learning Services. L’instance n’est pas liée à Microsoft Machine Learning Server. |
|Erreur de liaison 8 | Échec de l’annulation de la liaison | Une erreur est survenue lors de l’annulation de la liaison de l’instance. |
|Erreur de liaison 9 | Aucune instance trouvée | Aucune instance du moteur de base de données n’a été trouvée sur cet ordinateur. |

## <a name="known-issues"></a>Problèmes connus

Cette section répertorie les problèmes connus spécifiques à l’utilisation de l’utilitaire SqlBindR.exe, ou aux mises à niveau de Machine Learning Server susceptibles d’affecter des instances SQL Server.

### <a name="restoring-packages-that-were-previously-installed"></a>Restauration des packages précédemment installés

SqlBindR.exe ne parvient pas à restaurer les packages d’origine ou les composants R avec la mise à niveau vers Microsoft R Server 9.0.1. Utilisez l’option de réparation sur l’instance SQL Server et appliquez toutes les versions de service. Redémarrez l’instance.

La version ultérieure de SqlBindR restaure automatiquement les fonctionnalités R d’origine, ce qui évite d’avoir à réinstaller les composants R ou à réappliquer les correctifs au serveur. Toutefois, vous devez installer toutes les mises à jour du package R qui ont pu être ajoutées après l’installation initiale.

Utilisez les commandes R pour synchroniser les packages installés avec le système de fichiers à l’aide d’enregistrements de la base de données. Pour plus d’informations, consultez [Gestion des packages R pour SQL Server](../package-management/install-additional-r-packages-on-sql-server.md).

### <a name="problems-with-overwritten-sqlbinrini-file-in-sql-server"></a>Problèmes liés au fichier sqlbinr.ini remplacé dans SQL Server

Scénario : Ce problème se produit lors de la liaison de Machine Learning Server 9.4.7 à SQL Server 2017.  Quand Python est mis à jour et lié ou quand vous effectuez une mise à jour vers une nouvelle mise à jour cumulative, il ne comprend pas que Python est lié et remplace les fichiers. Il n’existe pas de problème connu avec R.

Pour contourner ce problème, créez un fichier `sqlbindr.ini` dans le répertoire PYTHON_SERVICES qui n’est pas vide. Le contenu n’a pas d’impact sur le fonctionnement du fichier.

Créez un fichier `sqlbindr.ini` contenant **9.4.7.82**, puis enregistrez-le à cet emplacement :  

`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES`

### <a name="problems-with-multiple-upgrades-from-sql-server"></a>Problèmes avec les mises à niveau multiples à partir de SQL Server

Scénario : Instance précédemment mise à niveau de SQL Server 2016 R services vers la version 9.0.1. Le nouveau programme d’installation a été exécuté pour Microsoft R Server 9.1.0. Le programme d’installation affiche une liste de toutes les instances valides.
Par défaut, le programme d’installation sélectionne les instances liées précédemment. Si vous continuez, la liaison des instances précédemment liées est annulée. La version 9.0.1 antérieure est désinstallée, y compris tous les packages associés, mais la nouvelle version de Microsoft R Server (9.1.0) n’est pas installée.

Pour résoudre ce problème, vous pouvez modifier l’installation R Server existante comme suit :
1. Dans le Panneau de configuration, ouvrez **Ajout/Suppression de programmes**.
2. Recherchez Microsoft R Server, puis sélectionnez **Modifier**.
3. Au démarrage du programme d’installation, sélectionnez les instances que vous souhaitez lier à la version 9.1.0.

Microsoft Machine Learning Server 9.2.1 et 9.3 ne présentent pas ce problème.

### <a name="binding-or-unbinding-leaves-multiple-temporary-folders"></a>La liaison ou l’annulation d’une liaison laisse plusieurs dossiers temporaires

Supprimez les dossiers temporaires une fois l’installation terminée.

> [!NOTE]
> Veillez à patienter jusqu’à la fin de l’installation. La suppression des bibliothèques R associées à une version et l’ajout des nouvelles bibliothèques R peuvent prendre longtemps. Une fois l’opération terminée, les dossiers temporaires sont supprimés.

## <a name="see-also"></a>Voir aussi

+ [Modifier la version du runtime de langage R ou Python par défaut](./change-default-language-runtime-version.md)
+ [Installer Machine Learning Server pour Windows (connecté à Internet)](/machine-learning-server/install/machine-learning-server-windows-install)
+ [Installer Machine Learning Server pour Windows (hors connexion)](/machine-learning-server/install/machine-learning-server-windows-offline)
+ [Problèmes connus dans Machine Learning Server](/machine-learning-server/resources-known-issues)
+ [Annonces de fonctionnalités de la version précédente de R Server](/r-server/whats-new-in-r-server)
+ [Fonctionnalités déconseillées, plus prises en charge ou modifiées](/machine-learning-server/resources-deprecated-features)