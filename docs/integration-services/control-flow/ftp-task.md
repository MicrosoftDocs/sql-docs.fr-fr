---
description: Tâche FTP
title: XML, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.ftptask.f1
- sql13.dts.designer.ftptask.general.f1
- sql13.dts.designer.ftptask.filetransfer.f1
helpviewer_keywords:
- FTP task [Integration Services]
ms.assetid: 41c3f2c4-ee04-460a-9822-bb9ae4036c2e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 839f9cc803dfcfe6c753851e53f0db1f2ef14daf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348960"
---
# <a name="ftp-task"></a>Tâche FTP

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tâche FTP télécharge des fichiers de données et gère des répertoires sur les serveurs. Par exemple, un package peut télécharger des fichiers de données à partir d’un serveur distant ou d’un emplacement Internet dans le cadre d’un flux de travail de package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Vous pouvez utiliser la tâche FTP aux fins suivantes :  
  
-   Copie de répertoires et de fichiers de données depuis un répertoire vers un autre, avant ou après le déplacement de données, et application de transformations aux données  
  
-   Connexion à un emplacement FTP source et copie des fichiers ou des packages vers un répertoire de destination  
  
-   Téléchargement de fichiers depuis un emplacement FTP et application de transformations aux données de colonne avant de charger les données dans une base de données  
  
 À l'exécution, la tâche FTP se connecte à un serveur à l'aide d'un gestionnaire de connexions FTP. Le gestionnaire de connexions FTP est configuré indépendamment de la tâche FTP, puis il est référencé dans celle-ci. Le gestionnaire de connexions FTP comprend les paramètres de serveur, les informations d'identification d'accès au serveur FTP et des options telles que le délai d'attente et le nombre de tentatives de connexion au serveur. Pour plus d’informations, consultez [Gestionnaires de connexion FTP](../../integration-services/connection-manager/ftp-connection-manager.md).  
  
> [!IMPORTANT]  
>  Le gestionnaire de connexions FTP prend en charge uniquement l'authentification anonyme et l'authentification de base. Il ne prend pas en charge l'authentification Windows.  
  
 Pour accéder à un fichier local ou à un répertoire local, la tâche FTP utilise un gestionnaire de connexions de fichiers ou des informations de chemin d'accès stockées dans une variable. À l'inverse, pour accéder à un fichier distant ou à un répertoire distant, la tâche FTP utilise un chemin d'accès directement spécifié sur le serveur distant, tel qu'indiqué dans le gestionnaire de connexions FTP, ou des informations de chemin d'accès stockées dans une variable. Pour plus d’informations, consultez [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md) et [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md).  
  
 Cela signifie que la tâche FTP peut recevoir plusieurs fichiers et supprimer plusieurs fichiers distants ; toutefois, la tâche peut envoyer seulement un fichier et supprimer seulement un fichier local si elle utilise un gestionnaire de connexions, car un gestionnaire de connexions de fichiers ne peut accéder qu'à un fichier. Pour accéder à plusieurs fichiers locaux, la tâche FTP doit utiliser une variable afin d'indiquer les informations de chemin d'accès. Par exemple, une variable contenant le texte « C:\Test\&#42;.txt » désigne un chemin qui prend en charge la suppression ou l’envoi de tous les fichiers ayant pour extension .txt et figurant dans le répertoire Test.  
  
 Pour envoyer plusieurs fichiers et accéder à plusieurs fichiers et répertoires locaux, vous pouvez également exécuter la tâche FTP plusieurs fois en l'incluant dans une boucle Foreach. La boucle Foreach peut passer en revue tous les fichiers d'un répertoire à l'aide de l'énumérateur For Each File. Pour plus d’informations, consultez [Conteneur de boucles Foreach](../../integration-services/control-flow/foreach-loop-container.md).  
  
 La tâche FTP prend en charge les caractères génériques *?* et *\** dans les chemins. Cela lui permet d'accéder à plusieurs fichiers. Toutefois, vous pouvez utiliser des caractères génériques seulement dans la partie du chemin d'accès qui spécifie le nom de fichier. Par exemple, « C:\MyDirectory\\*.txt » est un chemin valide, contrairement à « C:\\\*MyText.txt ».  
  
 Vous pouvez configurer les opérations FTP de manière à ce que la tâche de système de fichiers soit arrêtée en cas d'échec des opérations ou de manière à transférer les fichiers en mode ASCII. De même, vous pouvez configurer les opérations qui envoient et reçoivent des fichiers de manière à ce que les fichiers et les répertoires de destination soient écrasés.  
  
## <a name="predefined-ftp-operations"></a>Opérations FTP prédéfinies  
 La tâche FTP comprend un ensemble prédéfini d'opérations. Le tableau suivant décrit ces opérations.  
  
|Opération|Description|  
|---------------|-----------------|  
|Envoyer des fichiers|Envoie un fichier depuis l'ordinateur local vers le serveur FTP.|  
|Recevoir des fichiers|Enregistre sur l'ordinateur local un fichier provenant du serveur FTP.|  
|Créer un répertoire local|Crée un dossier sur l'ordinateur local.|  
|Créer un répertoire distant|Crée un dossier sur le serveur FTP.|  
|Supprimer le répertoire local|Supprime un dossier de l'ordinateur local.|  
|Supprimer le répertoire distant|Supprime un dossier du serveur FTP.|  
|Supprimer des fichiers locaux|Supprime un fichier de l'ordinateur local.|  
|Supprimer des fichiers distants|Supprime un fichier du serveur FTP.|  
  
## <a name="custom-log-entries-available-on-the-ftp-task"></a>Entrées de journal personnalisées disponibles dans la tâche FTP  
 Le tableau suivant répertorie les entrées de journal personnalisées pour la tâche FTP. Pour plus d’informations, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**FTPConnectingToServer**|Indique que la tâche a lancé une connexion au serveur FTP.|  
|**FTPOperation**|Indique le démarrage et le type d'une opération FTP effectuée par la tâche.|  
  
## <a name="related-tasks"></a>Tâches associées  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d’informations sur la façon de définir ces propriétés dans le Concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez [Définir les propriétés d’une tâche ou d’un conteneur](./add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
 Pour plus d’informations sur la définition de ces propriétés par programmation, consultez <xref:Microsoft.SqlServer.Dts.Tasks.FtpTask.FtpTask>.  
  
## <a name="ftp-task-editor-general-page"></a>Éditeur de tâche FTP (page Général)
  La page **Général** de la boîte de dialogue **Éditeur de tâche FTP** permet de spécifier le gestionnaire de connexions FTP qui se connecte au serveur FTP avec lequel la tâche communique. Vous pouvez également nommer et décrire cette tâche FTP.  
  
### <a name="options"></a>Options  
 **Connexion FTP**  
 Sélectionnez un gestionnaire de connexions FTP ou cliquez sur \<**New connection...**> pour créer un gestionnaire de connexions.  
  
> [!IMPORTANT]  
>  Le gestionnaire de connexions FTP prend en charge uniquement l'authentification anonyme et l'authentification de base. Il ne prend pas en charge l'authentification Windows.  
  
 **Rubriques connexes :** [Gestionnaires de connexions FTP](../../integration-services/connection-manager/ftp-connection-manager.md), [Éditeur du gestionnaire de connexions FTP](../connection-manager/ftp-connection-manager.md)  
  
 **Arrêt en cas d'échec de l'opération**  
 Indique si la tâche FTP se termine en cas d'échec de l'opération.  
  
 **Nom**  
 Fournit un nom unique pour la tâche FTP. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Tapez une description de la tâche FTP.  
  
## <a name="ftp-task-editor-file-transfer-page"></a>Éditeur de tâche FTP (page Transfert de fichiers)
  Utilisez la page **Transfert de fichiers** de l' **Éditeur de tâche FTP** pour configurer l'opération FTP qu'exécute la tâche.  
  
### <a name="options"></a>Options  
 **IsRemotePathVariable**  
 Indique si le chemin distant est stocké dans une variable. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**True**|Le chemin d'accès de destination est stocké dans une variable. Cette valeur affiche l'option dynamique **RemoteVariable**.|  
|**False**|Le chemin d'accès de destination est défini dans un gestionnaire de connexions de fichiers. Cette valeur affiche l'option dynamique **RemovePath**.|  
  
 **OverwriteFileAtDestination**  
 Indique si un fichier peut être remplacé dans la destination.  
  
 **IsLocalPathVariable**  
 Indique si le chemin local est stocké dans une variable. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**True**|Le chemin d'accès de destination est stocké dans une variable. Cette valeur affiche l'option dynamique **LocalVariable**.|  
|**False**|Le chemin d'accès de destination est défini dans un gestionnaire de connexions de fichiers. Cette valeur affiche l'option dynamique **LocalPath**.|  
  
 **opération**  
 Sélectionnez l'opération FTP à exécuter. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Envoyer des fichiers**|Envoie des fichiers. Cette valeur affiche les options dynamiques **LocalVariable**, **LocalPathRemoteVariable** et **RemotePath**.|  
|**Recevoir des fichiers**|Reçoit des fichiers. Cette valeur affiche les options dynamiques **LocalVariable**, **LocalPathRemoteVariable** et **RemotePath**.|  
|**Créer un répertoire local**|Crée un répertoire local. Cette valeur affiche les options dynamiques **LocalVariable** et **LocalPath**.|  
|**Créer un répertoire distant**|Crée un répertoire distant. Cette valeur affiche les options dynamiques **RemoteVariable** et **RemotePath**.|  
|**Supprimer le répertoire local**|Supprime un répertoire local. Cette valeur affiche les options dynamiques **LocalVariable** et **LocalPath**.|  
|**Supprimer le répertoire distant**|Supprime un répertoire distant. Cette valeur affiche les options dynamiques **RemoteVariable** et **RemotePath**.|  
|**Supprimer des fichiers locaux**|Supprime des fichiers locaux Cette valeur affiche les options dynamiques **LocalVariable** et **LocalPath**.|  
|**Supprimer des fichiers distants**|Supprime des fichiers distants. Cette valeur affiche les options dynamiques **RemoteVariable** et **RemotePath**.|  
  
 **IsTransferASCII**  
 Indique si les fichiers transférés vers et depuis le serveur FTP distant doivent être transférés en mode ASCII.  
  
### <a name="isremotepathvariable-dynamic-options"></a>Options dynamiques IsRemotePathVariable  
  
#### <a name="isremotepathvariable--true"></a>IsRemotePathVariable = True  
 **RemoteVariable**  
 Sélectionnez une variable définie par l’utilisateur existante ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), Ajouter une variable  
  
#### <a name="isremotepathvariable--false"></a>IsRemotePathVariable = False  
 **RemovePath**  
 Sélectionnez un gestionnaire de connexions FTP ou cliquez sur \<**New connection...**> pour créer un gestionnaire de connexions.  
  
 **Rubriques connexes :** [Gestionnaires de connexions FTP](../../integration-services/connection-manager/ftp-connection-manager.md), [Éditeur du gestionnaire de connexions FTP](../connection-manager/ftp-connection-manager.md)  
  
### <a name="islocalpathvariable-dynamic-options"></a>Options dynamiques IsLocalPathVariable  
  
#### <a name="islocalpathvariable--true"></a>IsLocalPathVariable = True  
 **LocalVariable**  
 Sélectionnez une variable définie par l’utilisateur existante ou cliquez sur \<**New variable...**> pour créer une variable.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), Ajouter une variable  
  
#### <a name="islocalpathvariable--false"></a>IsLocalPathVariable = False  
 **LocalPath**  
 Sélectionnez un gestionnaire de connexions de fichiers existant ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers plats](../../integration-services/connection-manager/flat-file-connection-manager.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flux de contrôle](../../integration-services/control-flow/control-flow.md)  
  
