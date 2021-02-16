---
description: Message Queue Task
title: Tâches MSMQ | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.messagequeuetask.f1
- sql13.dts.designer.msgqueuetask.general.f1
- sql13.dts.designer.msgqueuetask.send.f1
- sql13.dts.designer.msgqueuetask.receive.f1
- sql13.dts.designer.selectvariables.f1
helpviewer_keywords:
- Message Queue task [Integration Services]
- receiving messages
- messages [Integration Services]
- sending messages
ms.assetid: ae1d8fad-6649-4e93-b589-14a32d07da33
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e39187e32afbcf72bf1eb950f18d5f21cef38211
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348302"
---
# <a name="message-queue-task"></a>Message Queue Task

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tâche MSMQ vous permet d’utiliser Message Queuing (MSMQ) pour envoyer et recevoir des messages entre des packages [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ou pour envoyer des messages à une file d’attente d’application traitée par une application personnalisée. Ces messages peuvent être composés de texte brut, de fichiers ou de variables et leurs valeurs.  
  
 L'utilisation de la tâche MSMQ vous permet de coordonner des opérations à l'échelle de votre entreprise. Les messages peuvent être placés dans la file d'attente et remis ultérieurement si la destination est indisponible ou occupée ; par exemple, la tâche peut mettre en file d'attente les messages destinés à l'ordinateur portable hors connexion des représentants commerciaux, qui reçoivent leurs messages lorsqu'ils se connectent au réseau. Vous pouvez utiliser la tâche MSMQ pour effectuer les opérations suivantes :  
  
-   Retarder l'exécution d'une tâche jusqu'à la validation d'autres packages. Par exemple, sur chacun de vos sites de vente au détail, après une maintenance nocturne, une tâche MSMQ envoie un message à l'ordinateur de l'entreprise. Un package s'exécutant sur cet ordinateur contient des tâches MSMQ, chacune attendant le message d'un site de vente au détail spécifique. Lorsqu'un message d'un site arrive, une tâche télécharge des données à partir de ce site. Une fois tous les sites validés, le package calcule les totaux récapitulatifs.  
  
-   Envoyer des fichiers de données à l'ordinateur qui les traite. Par exemple, le résultat de la caisse enregistreuse d'un restaurant peut être envoyé dans un message de fichier de données au système du paiement des salaires de l'entreprise, où sont extraites les données liées aux pourboires des serveurs.  
  
-   Distribuer des fichiers à l'intérieur de votre entreprise. Par exemple, un package peut utiliser une tâche MSMQ pour envoyer un fichier de package à un autre ordinateur. Un package s'exécutant sur l'ordinateur de destination utilise alors une tâche MSMQ pour récupérer et enregistrer le package localement.  
  
 Lors de l'envoi ou de la réception de messages, la tâche MSMQ utilise l'un des quatre types de messages suivants : fichier de données, chaîne, message de type chaîne pour la variable ou variable. Le type de message « message de type chaîne pour la variable » peut être utilisé uniquement lors de la réception de messages.  
  
 La tâche utilise un gestionnaire de connexions MSMQ pour se connecter à une file d'attente de messages. Pour plus d’informations, consultez [Gestionnaire de connexions MSMQ](../../integration-services/connection-manager/msmq-connection-manager.md). Pour plus d’informations sur Message Queuing (MSMQ), consultez [MSDN Library](../../sql-server/index.yml).  
  
 La tâche MSMQ exige que le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] soit installé. Parmi les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous pouvez sélectionner dans la page **Composants à installer** ou **Sélection de fonctionnalités** de l’Assistant Installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , certains n’installent qu’un sous-ensemble partiel des composants [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Ces composants sont utiles pour des tâches spécifiques, mais les fonctionnalités de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] s'en trouvent limitées. Par exemple, l'option [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] installe les composants [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] nécessaires pour concevoir un package mais le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] n'est pas installé, par conséquent la tâche MSMQ ne fonctionne pas. Pour garantir une installation complète de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vous devez sélectionner [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] dans la page **Composants à installer** . Pour plus d’informations sur l’installation et l’exécution de la tâche MSMQ, consultez [Installer Integration Services](../../integration-services/install-windows/install-integration-services.md).  
  
> [!NOTE]  
>  La tâche MSMQ ne parvient pas à assurer la conformité avec le standard FIPS (Federal Information Processing Standard) 140-2 lorsque le système d'exploitation de l'ordinateur est configuré en mode FIPS et que la tâche utilise le chiffrement. Si la tâche MSMQ n'utilise pas le chiffrement, elle peut s'exécuter correctement.  
  
## <a name="message-types"></a>Types de messages  
 Vous pouvez configurer les types de messages fournis par la tâche MSMQ des manières suivantes :  
  
-   **Message de type fichier de données** spécifie que le message est contenu dans un fichier. Lors de la réception de messages, vous pouvez configurer la tâche de façon à enregistrer le fichier, remplacer un fichier existant et spécifier le package à partir duquel la tâche peut recevoir des messages.  
  
-   **Message de type chaîne** spécifie le message en tant que chaîne. Lors de la réception de messages, vous pouvez configurer la tâche de façon à comparer la chaîne reçue avec une chaîne définie par l'utilisateur et exécuter une action en fonction du résultat de la comparaison. La comparaison de chaînes peut être exacte, sensible ou non à la casse, ou utiliser une sous-chaîne.  
  
-   **Message de type chaîne pour la variable** spécifie le message source en tant que chaîne envoyée à une variable de destination. Vous pouvez configurer la tâche de façon à comparer la chaîne reçue avec une chaîne définie par l'utilisateur à l'aide d'une comparaison exacte, non sensible à la casse ou de sous-chaîne. Ce type de message est disponible uniquement lorsque la tâche reçoit des messages.  
  
-   **Variable** spécifie que le message contient une ou plusieurs variables. Vous pouvez configurer la tâche de façon à spécifier les noms des variables contenues dans le message. Lors de la réception de messages, vous pouvez configurer la tâche de façon à spécifier à la fois le package à partir duquel elle peut recevoir des messages et la variable qui est la destination du message.  
  
## <a name="sending-messages"></a>sending messages  
 Lorsque vous configurez la tâche MSMQ pour envoyer des messages, vous pouvez utiliser l'un des algorithmes de chiffrement actuellement pris en charge par la technologie Message Queuing, RC2 et RC4, pour chiffrer le message. Ces deux algorithmes de chiffrement sont aujourd'hui considérés comme faibles d'un point de vue du chiffrement par rapport aux algorithmes plus récents, non encore pris en charge par la technologie Microsoft Message Queuing. Par conséquent, vous devez minutieusement évaluer vos besoins en matière de chiffrement si vous souhaitez envoyer des messages à l'aide de la tâche MSMQ.  
  
## <a name="receiving-messages"></a>réception de messages  
 Lors de la réception de messages, la tâche MSMQ peut être configurée des manières suivantes :  
  
-   Contournement du message ou suppression du message de la file d'attente.  
  
-   Spécification d'un délai d'attente.  
  
-   Échec en cas d'expiration du délai d'attente.  
  
-   Remplacement d’un fichier existant si le message est stocké dans un **fichier de données**.  
  
-   Enregistrement du fichier de message sous un nom de fichier différent si le message utilise le type **Message de type fichier de données** .  
  
## <a name="custom-logging-messages-available-on-the-message-queue-task"></a>Messages de journalisation personnalisés disponibles dans la tâche MSMQ  
 Le tableau suivant répertorie les entrées de journal personnalisées de la tâche MSMQ. Pour plus d’informations, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**MSMQAfterOpen**|Indique que la tâche a fini d'ouvrir la file d'attente des messages.|  
|**MSMQBeforeOpen**|Indique que la tâche a commencé l'ouverture de la file d'attente des messages.|  
|**MSMQBeginReceive**|Indique que la tâche a commencé la réception d'un message.|  
|**MSMQBeginSend**|Indique que la tâche a commencé l'envoi d'un message.|  
|**MSMQEndReceive**|Indique que la tâche a terminé la réception d'un message.|  
|**MSMQEndSend**|Indique que la tâche a terminé l'envoi d'un message.|  
|**MSMQTaskInfo**|Fournit des informations détaillées concernant la tâche.|  
|**MSMQTaskTimeOut**|Indique que le délai de la tâche a expiré.|  
  
## <a name="configuration-of-the-message-queue-task"></a>Configuration de la tâche MSMQ  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation. Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d’informations sur la définition par programmation de ces propriétés, consultez la documentation relative à la classe **Microsoft.SqlServer.Dts.Tasks.MessageQueueTask.MessageQueueTask** dans le Guide du développeur.  
  
## <a name="related-tasks"></a>Tâches associées  
 Pour plus d’informations sur la façon de définir ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez [Définir les propriétés d’une tâche ou d’un conteneur](./add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
## <a name="message-queue-task-editor-general-page"></a>Éditeur de tâche MSMQ (page Général)
  Utilisez la page **Général** de la boîte de dialogue **Éditeur de tâche MSMQ** pour nommer et décrire la tâche MSMQ, pour spécifier le format du message et indiquer si la tâche envoie ou reçoit des messages.  
  
### <a name="options"></a>Options  
 **Nom**  
 Attribuez un nom unique à la tâche MSMQ. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Tapez la description de la tâche MSMQ.  
  
 **Use2000Format**  
 Indiquez si le format 2000 de Message Queuing (ou MSMQ) doit être utilisé. La valeur par défaut est **False**.  
  
 **MSMQConnection**  
 Sélectionnez un gestionnaire de connexions MSMQ existant ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions MSMQ](../../integration-services/connection-manager/msmq-connection-manager.md), [Éditeur du gestionnaire de connexions MSMQ](../connection-manager/msmq-connection-manager.md)  
  
 **Message**  
 Spécifiez si la tâche MSMQ envoie ou reçoit des messages. Si vous sélectionnez l’option **Envoyer un message**, la page Envoyer est répertoriée dans le volet gauche de la boîte de dialogue ; si vous sélectionnez l’option **Recevoir un message**, la page Recevoir est répertoriée. Par défaut, cette valeur est définie sur **Envoyer un message**.  
  
## <a name="message-queue-task-editor-send-page"></a>Éditeur de tâche MSMQ (page Envoyer)
  Utilisez la page **Envoyer** de la boîte de dialogue **Éditeur de tâche MSMQ** pour configurer une tâche MSMQ afin d’envoyer des messages depuis un package [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
### <a name="options"></a>Options  
 **UseEncryption**  
 Indiquez si le message doit être chiffré. La valeur par défaut est **False**.  
  
 **EncryptionAlgorithm**  
 Si vous utilisez le chiffrement, définissez le nom de l'algorithme de chiffrement à utiliser. La tâche MSMQ peut utiliser les algorithmes RC2 et RC4. La valeur par défaut est **RC2**.  
  
> [!NOTE]  
>  L'algorithme RC4 est uniquement pris en charge pour des raisons de compatibilité descendante. Le nouveau matériel ne peut être chiffré à l'aide de RC4 ou de RC4_128 que lorsque la base de données se trouve dans le niveau de compatibilité 90 ou 100. (Non recommandé.) Utilisez à la place un algorithme plus récent, tel qu'un des algorithmes AES. Dans la version actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le matériel chiffré à l'aide de RC4 ou de RC4_128 peut être déchiffré dans n'importe quel niveau de compatibilité.  
  
> [!IMPORTANT]  
>  Il s'agit des algorithmes de chiffrement que la technologie Message Queuing (ou MSMQ) prend en charge. Ces deux algorithmes de chiffrement sont aujourd'hui considérés comme faibles du point de vue du chiffrement par rapport aux algorithmes plus récents, non encore pris en charge par la technologie Microsoft Message Queuing. Par conséquent, vous devez minutieusement évaluer vos besoins en matière de chiffrement si vous souhaitez envoyer des messages à l'aide de la tâche MSMQ.  
  
 **MessageType**  
 Sélectionnez le type de message : Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Message de fichiers de données**|Le message est stocké dans un fichier. La sélection de cette valeur affiche l'option dynamique **DataFileMessage**.|  
|**Message de type variable**|Le message est stocké dans une variable. Cette valeur affiche l'option dynamique **VariableMessage**.|  
|**Message de type chaîne**|Le message est stocké dans la tâche MSMQ. Cette valeur affiche l'option dynamique **StringMessage**.|  
  
### <a name="messagetype-dynamic-options"></a>Options dynamiques MessageType  
  
#### <a name="messagetype--data-file-message"></a>MessageType = Message de fichiers de données  
 **DataFileMessage**  
 Tapez le chemin du fichier de données ou cliquez sur le bouton avec les points de suspension **(...)** et recherchez le fichier.  
  
#### <a name="messagetype--variable-message"></a>MessageType = Message de type variable  
 **VariableMessage**  
 Tapez les noms de variables ou cliquez sur les points de suspension **(...)** et sélectionnez les variables. Les variables sont séparées par des virgules.  
  
 **Rubriques connexes :** Sélectionner des variables  
  
#### <a name="messagetype--string-message"></a>MessageType = Message de type chaîne  
 **StringMessage**  
 Tapez le message de type chaîne ou cliquez sur les points de suspension **(...)** et entrez le message dans la boîte de dialogue **Entrer le message de type chaîne** .  
  
## <a name="message-queue-task-editor-receive-page"></a>Éditeur de tâche MSMQ (page Recevoir)
  La page **Recevoir** de la boîte de dialogue **Éditeur de tâche MSMQ** permet de configurer une tâche MSMQ pour recevoir des messages MSMQ (Message Queuing) [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
### <a name="options"></a>Options  
 **RemoveFromMessageQueue**  
 Indiquez si vous voulez supprimer le message de la file d'attente après sa réception. La valeur par défaut est **False**.  
  
 **ErrorIfMessageTimeOut**  
 Indiquez si la tâche échoue lorsque le message expire, en affichant un message d'erreur. La valeur par défaut est **False**.  
  
 **TimeoutAfter**  
 Si vous choisissez d'afficher un message d'erreur sur l'échec de la tâche, définissez le nombre de secondes qui précèdent le message d'expiration.  
  
 **MessageType**  
 Sélectionnez le type de message : Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Message de fichiers de données**|Le message est stocké dans un fichier. La sélection de cette valeur affiche l'option dynamique **DataFileMessage**.|  
|**Message de type variable**|Le message est stocké dans une variable. Cette valeur affiche l'option dynamique **VariableMessage**.|  
|**Message de type chaîne**|Le message est stocké dans la tâche MSMQ. Cette valeur affiche l'option dynamique **StringMessage**.|  
|**Message de type chaîne pour la variable**|Le message<br /><br /> Cette valeur affiche l'option dynamique **StringMessage**.|  
  
### <a name="messagetype-dynamic-options"></a>Options dynamiques MessageType  
  
#### <a name="messagetype--data-file-message"></a>MessageType = Message de fichiers de données  
 **SaveFileAs**  
 Tapez le chemin du fichier à utiliser ou cliquez sur le bouton avec des points de suspension **(...)** et recherchez le fichier.  
  
 **Remplacer**  
 Indiquez si vous voulez remplacer les données dans un fichier existant lors de l'enregistrement du contenu d'un message de fichiers de données. La valeur par défaut est **False**.  
  
 **Filter**  
 Indiquez si vous voulez appliquer un filtre au message. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Aucun filtre**|La tâche ne filtre pas les messages. Cette valeur affiche l’option dynamique **IdentifierReadOnly**.|  
|**À partir du package**|Le message reçoit uniquement les messages du package spécifié. Cette valeur affiche l’option dynamique **Identifier**.|  
  
#### <a name="filter-dynamic-options"></a>Options dynamiques de filtrage  
  
##### <a name="filter--no-filter"></a>Filtrer = Aucun filtre  
 **IdentifierReadOnly**  
 Cette option est en lecture seule. Elle peut être vide ou contenir le GUID d'un package lorsque la propriété Filtrer a été définie.  
  
##### <a name="filter--from-package"></a>Filtrer = À partir du package  
 **Identificateur**  
 Si vous choisissez d’appliquer un filtre, tapez l’identificateur unique du package à partir duquel les messages peuvent être reçus, ou cliquez sur le bouton de sélection **(...)** et spécifiez le package.  
  
 **Rubriques connexes :** [Sélectionner un package](../../integration-services/control-flow/select-a-package.md)  
  
#### <a name="messagetype--variable-message"></a>MessageType = Message de type variable  
 **Filter**  
 Indiquez si vous voulez appliquer un filtre aux messages. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Aucun filtre**|La tâche ne filtre pas les messages. Cette valeur affiche l’option dynamique **IdentifierReadOnly**.|  
|**À partir du package**|Le message reçoit uniquement les messages du package spécifié. Cette valeur affiche l’option dynamique **Identifier**.|  
  
 **Variable**  
 Tapez le nom de la variable, ou cliquez sur \<**New variable...**> et définissez une nouvelle variable.  
  
 **Rubriques connexes :** [Ajouter une variable](../integration-services-ssis-variables.md)  
  
#### <a name="filter-dynamic-options"></a>Options dynamiques de filtrage  
  
##### <a name="filter--no-filter"></a>Filtrer = Aucun filtre  
 **IdentifierReadOnly**  
 Cette option est vide.  
  
##### <a name="filter--from-package"></a>Filtrer = À partir du package  
 **Identificateur**  
 Si vous choisissez d’appliquer un filtre, tapez l’identificateur unique du package à partir duquel les messages peuvent être reçus, ou cliquez sur le bouton de sélection **(...)** et spécifiez le package.  
  
 **Rubriques connexes :** [Sélectionner un package](../../integration-services/control-flow/select-a-package.md)  
  
#### <a name="messagetype--string-message"></a>MessageType = Message de type chaîne  
 **Compare**  
 Indiquez si vous voulez appliquer un filtre aux messages. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Aucun**|Les messages ne sont pas comparés.|  
|**Correspondance exacte**|Les messages doivent correspondre exactement à la chaîne figurant dans l’option **CompareString** .|  
|**Ignorer la casse**|Le message doit correspondre à la chaîne figurant dans l’option **CompareString** , mais la comparaison ne tient pas compte de la casse.|  
|**Contenant**|Le message doit contenir la chaîne figurant dans l’option **CompareString** .|  
  
 **CompareString**  
 Si l’option **Comparer** n’est pas définie sur **Aucun**, indiquez la chaîne à laquelle le message doit être comparé.  
  
#### <a name="messagetype--string-message-to-variable"></a>MessageType = Message de type chaîne pour la variable  
 **Compare**  
 Indiquez si vous voulez appliquer un filtre aux messages. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Aucun**|Les messages ne sont pas comparés.|  
|**Correspondance exacte**|Le message doit correspondre exactement à la chaîne figurant dans l’option **CompareString** .|  
|**Ignorer la casse**|Le message doit correspondre à la chaîne figurant dans l’option **CompareString** , mais la comparaison ne tient pas compte de la casse.|  
|**Contenant**|Le message doit contenir la chaîne figurant dans l’option **CompareString** .|  
  
 **CompareString**  
 Si l’option **Comparer** n’est pas définie sur **Aucun**, indiquez la chaîne à laquelle le message doit être comparé.  
  
 **Variable**  
 Tapez le nom de la variable qui doit contenir le message reçu, ou cliquez sur \<**New variable...**> et définissez une nouvelle variable.  
  
 **Rubriques connexes :** [Ajouter une variable](../integration-services-ssis-variables.md)  
  
## <a name="select-variables"></a>Sélectionner des variables
  Utilisez la boîte de dialogue **Sélectionner des variables** pour spécifier les variables à utiliser dans une opération d'envoi de message de la tâche MSMQ. La liste **Variables disponibles** contient les variables système et définies par l’utilisateur exploitables par la tâche MSMQ ou son conteneur parent. La tâche utilise des variables de la liste **Variables sélectionnées** .  
  
### <a name="options"></a>Options  
 **Variables disponibles**  
 Sélectionnez une ou plusieurs variables.  
  
 **Variables sélectionnées**  
 Sélectionnez une ou plusieurs variables.  
  
 **Flèches vers la droite**  
 Déplacez les variables sélectionnées dans la liste **Variables sélectionnées** .  
  
 **Flèches vers la gauche**  
 Replacez les variables sélectionnées dans la liste **Variables disponibles** .  
  
 **Nouvelle variable**  
 Créez une nouvelle variable.  
  
 **Rubriques connexes :** [Ajouter une variable](../integration-services-ssis-variables.md)  
## <a name="see-also"></a>Voir aussi  
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flux de contrôle](../../integration-services/control-flow/control-flow.md)  
  
