---
description: tache Envoyer un message
title: Envoyer un message, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sendmailtask.f1
- sql13.dts.designer.sendmailtask.general.f1
- sql13.dts.designer.sendmailtask.mail.f1
helpviewer_keywords:
- mail [Integration Services]
- Send Mail task
- e-mail [Integration Services]
- messages [Integration Services]
- sending messages
ms.assetid: fe0b7cbc-fe8e-4fe2-95b4-2953efff5869
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 362da93d1048564812826fceb3c0f2e76a47977a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348279"
---
# <a name="send-mail-task"></a>tache Envoyer un message

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tâche Envoyer un message envoie un message électronique. La tâche Envoyer un message permet à un package d'envoyer des messages en cas de réussite ou d'échec des tâches du flux de travail du package, ou d'envoyer des messages en réponse à un événement déclenché par le package au moment de l'exécution. Par exemple, la tâche peut notifier à un administrateur de base de données la réussite ou l'échec de la tâche de sauvegarde de base de données.  
  
 Vous pouvez configurer la tâche Envoyer un message comme suit :  
  
-   Fournissez le texte du message électronique.  
  
-   Indiquez une ligne d'objet pour le message électronique.  
  
-   Définissez le niveau de priorité du message. La tâche prend en charge trois niveaux de priorité : normale, basse et haute.  
  
-   Indiquez les destinataires dans les lignes À, Cc et Cci. Si la tâche spécifie plusieurs destinataires, ceux-ci sont séparés par des points-virgules.  
  
    > [!NOTE]  
    >  Les lignes À, Cc et Cci sont limitées chacune à 256 caractères conformément aux normes Internet.  
  
-   Incluez les pièces jointes. Si la tâche spécifie plusieurs pièces jointes, celles-ci sont séparées par la barre verticale ( | ).  
  
    > [!NOTE]  
    >  Si un fichier de pièce jointe n'existe pas lors de l'exécution du package, une erreur se produit.  
  
-   Spécifiez le gestionnaire de connexions SMTP à utiliser.  
  
    > [!IMPORTANT]  
    >  Le gestionnaire de connexions SMTP prend en charge uniquement l'authentification anonyme et l'authentification Windows. Il ne prend pas en charge l'authentification de base.  
  
 Le texte du message peut être une chaîne indiquée par vos soins, ou bien une connexion à un fichier, ou le nom d'une variable qui contient le texte. La tâche utilise un gestionnaire de connexions de fichiers pour se connecter à un fichier. Pour plus d'informations, consultez [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
 La tâche utilise un gestionnaire de connexions SMTP pour se connecter à un serveur de messagerie. Pour plus d’informations, consultez [Gestionnaire de connexions SMTP](../../integration-services/connection-manager/smtp-connection-manager.md).  
  
## <a name="custom-logging-messages-available-on-the-send-mail-task"></a>Messages de journalisation personnalisés disponibles dans la tâche Envoyer un message  
 Le tableau suivant répertorie les entrées de journal personnalisées pour la tâche Envoyer un message. Pour plus d’informations, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**SendMailTaskBegin**|Indique que la tâche a commencé l'envoi d'un message électronique.|  
|**SendMailTaskEnd**|Indique que la tâche a terminé l'envoi d'un message électronique.|  
|**SendMailTaskInfo**|Fournit des informations détaillées concernant la tâche.|  
  
## <a name="configuring-the-send-mail-task"></a>Configuration de la tâche Envoyer un message  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d'informations sur la définition par programmation de ces propriétés, cliquez sur la rubrique suivante :  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.SendMailTask.SendMailTask>  
  
## <a name="related-tasks"></a>Tâches associées  
 Pour obtenir des informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur [Définir les propriétés d’une tâche ou d’un conteneur](./add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
## <a name="related-content"></a>Contenu associé  
  
-   Article technique [Procédure d'envoi de courrier électronique avec notification de remise en C#](https://go.microsoft.com/fwlink/?LinkId=237625)(Procédure d’envoi d’e-mail avec notification de remise en C#) sur shareourideas.com  
  
## <a name="send-mail-task-editor-general-page"></a>Éditeur de tâche Envoyer un message (page Général)
  Utilisez la page **Général** de la boîte de dialogue **Éditeur de tâche Envoyer un message** pour définir le nom de la tâche d'envoi d'un message et décrire la tâche.  
  
### <a name="options"></a>Options  
 **Nom**  
 Fournissez un nom unique pour la tâche d'envoi de message. Ce nom sert d'étiquette à l'icône de la tâche.  
  
 **Remarque** Les noms de tâches doivent être uniques dans un package.  
  
 **Description**  
 Entrez une description de la tâche d'envoi d'un message.  
  
## <a name="send-mail-task-editor-mail-page"></a>Éditeur de tâche Envoyer un message (page Courrier)
  Utilisez la page **Courrier** de la boîte de dialogue **Éditeur de tâche Envoyer un message** pour indiquer les destinataires d'un message, son type et sa priorité. Vous pouvez également joindre des fichiers au message. Le texte de ce courrier peut se présenter sous la forme d'une chaîne que vous précisez, d'une connexion à un fichier comportant le texte voulu ou le nom d'une variable contenant le texte en question.  
  
### <a name="options"></a>Options  
 **SMTPConnection**  
 Sélectionnez un gestionnaire de connexions SMTP dans la liste, ou cliquez sur **\<New connection...>** pour en créer un.  
  
> [!IMPORTANT]  
>  Le gestionnaire de connexions SMTP prend en charge uniquement l'authentification anonyme et l'authentification Windows. Il ne prend pas en charge l'authentification de base.  
  
 **Rubriques connexes :** [Gestionnaire de connexions SMTP](../../integration-services/connection-manager/smtp-connection-manager.md)  
  
 **From**  
 Indique l'adresse électronique de l'expéditeur.  
  
 **To**  
 Fournit les adresses de messagerie des destinataires séparées par un point-virgule.  
  
 **Cc**  
 Fournit les adresses de messagerie des individus destinés à recevoir une copie du message, séparées par un point-virgule.  
  
 **Cci**  
 Fournit les adresses de messagerie des individus destinés à recevoir une copie carbone invisible (Cci) du message, séparées par un point-virgule.  
  
 **Subject**  
 Permet de spécifier l'objet du message électronique.  
  
 **MessageSourceType**  
 Permet de sélectionner le type de source du message. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Entrée directe**|Permet de définir la source du texte du message. Si cette valeur est sélectionnée, les options dynamiques incluses dans **MessageSource** s'affichent alors.|  
|**Connexion de fichiers**|Permet de définir la source du fichier contenant le texte du message. Si cette valeur est sélectionnée, les options dynamiques incluses dans **MessageSource** s'affichent alors.|  
|**Variable**|Permet d'attribuer à la source une variable contenant le texte du message. Si cette valeur est sélectionnée, les options dynamiques incluses dans **MessageSource** s'affichent alors.|  
  
 **Priorité**  
 Permet d'indiquer la priorité à appliquer au message.  
  
 **Pièces jointes**  
 Indique le nom des fichiers joints au message électronique, séparés par le caractère Barre verticale (|).  
  
> [!NOTE]  
>  Les lignes À, Cc et Cci sont limitées à 256 caractères, conformément aux Internet standards.  
  
### <a name="messagesourcetype-dynamic-options"></a>Options dynamiques de MessageSourceType  
  
#### <a name="messagesourcetype--direct-input"></a>MessageSourceType = Entrée directe  
 **MessageSource**  
 Permet d’entrer le texte du message ou de cliquer sur le bouton d’exploration représenté par les points de suspension (...), puis de taper le message dans la boîte de dialogue **Source du message**.  
  
#### <a name="messagesourcetype--file-connection"></a>MessageSourceType = Connexion de fichiers  
 **MessageSource**  
 Sélectionnez un gestionnaire de connexions de fichiers dans la liste ou cliquez sur \<**New connection...**> pour créer un gestionnaire de connexions.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
#### <a name="messagesourcetype--variable"></a>MessageSourceType = Variable  
 **MessageSource**  
 Sélectionnez une variable dans la liste, ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flux de contrôle](../../integration-services/control-flow/control-flow.md)  
  
