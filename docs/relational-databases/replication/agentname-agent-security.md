---
description: Sécurité de l’agent &lt;nom_agent&gt;
title: Sécurité de l’agent &lt;nom_agent&gt; | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newsubwizard.agentnameagentsecurity.f1
ms.assetid: d34c7ef8-cf77-4ffd-887f-3c4214dfd71e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 283f19912bcb5f3c57f84c845d9bf051a7c211d0
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88470348"
---
# <a name="ltagentnamegt-agent-security"></a>Sécurité de l’agent &lt;nom_agent&gt;
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  La page **\<AgentName>Sécurité de l’agent** permet de spécifier les comptes sous lesquels l’Agent de distribution (pour la réplication transactionnelle ou la réplication de capture instantanée) ou l’Agent de fusion (pour la réplication de fusion) s’exécutent et établissent les connexions aux ordinateurs d’une topologie de réplication. Pour plus d’informations sur les autorisations exigées par les agents et les bonnes pratiques en matière de sécurité de la réplication, consultez [Modèle de sécurité de l’Agent de réplication](../../relational-databases/replication/security/replication-agent-security-model.md) et [Bonnes pratiques en matière de sécurité de la réplication](../../relational-databases/replication/security/replication-security-best-practices.md).  
  
## <a name="options"></a>Options  
 Cliquez sur le bouton de propriétés ( **...** ) dans la ligne de chaque Abonné pour accéder à la boîte de dialogue **Sécurité de l'Agent de distribution** ou **Sécurité de l'Agent de fusion** . Cliquez sur **Aide** dans la boîte de dialogue qui s'affiche pour fournir plus d'informations sur les autorisations indispensables pour les comptes utilisés par les agents.  
  
 Après avoir entré les paramètres dans l'une des boîtes de dialogue, les informations de connexion de l'Abonné s'affichent dans la grille.  
  
 **Agent de l'Abonné**  
 Nom de chaque Abonné.  
  
 **Connexion au serveur de distribution**  
 S'affiche pour les réplications d'instantané et transactionnelle. Le contexte dans lequel la connexion au serveur de distribution s'établit. Les connexions locales sont toujours établies en utilisant le contexte du compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows dans lequel s'exécute l'agent :  
  
-   Pour les abonnements par envoi de données, la connexion locale est celle du serveur de distribution. Ce champ affiche donc toujours : **Emprunter l’identité ’\<Domain>>\\<Connexion\>** ou **Emprunter l’identité ’\<Computer>\\<Connexion\>** pour les abonnements par émission de données.  
  
-   Pour les abonnements par extraction, la connexion peut également avoir lieu dans le contexte d’une connexion [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le champ affiche l’un des éléments suivants : **Utiliser la connexion « \<Login> »** , **Emprunter l’identité « \<Domain>\\<Connexion\> »** ou **Emprunter l’identité « \<Computer>\\<Connexion\> »** . [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande d'établir toutes les connexions dans le contexte du compte Windows.  
  
 **Connexion au serveur de publication et serveur de distribution**  
 S'affiche pour la réplication de fusion. Le contexte des connexions aux serveurs de publication et de distribution s'établit. Les connexions locales sont toujours établies en utilisant le contexte du compte Windows dans lequel s'exécute l'agent :  
  
-   Pour les abonnements par envoi de données, la connexion locale est celle des serveurs de publication et de distribution. Ce champ affiche donc toujours : **Emprunter l’identité ’\<Domain>>\\<Connexion\>** ou **Emprunter l’identité ’\<Computer>\\<Connexion\>** pour les abonnements par émission de données.  
  
-   Pour les abonnements par extraction, la connexion peut également avoir lieu dans le contexte d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le champ affiche l’un des éléments suivants : **Utiliser la connexion « \<Login> »** , **Emprunter l’identité « \<Domain>\\<Connexion\> »** ou **Emprunter l’identité « \<Computer>\\<Connexion\> »** . [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande d'établir toutes les connexions dans le contexte du compte Windows.  
  
 **Connexion à l'Abonné**  
 Le contexte dans lequel la connexion à l'abonné s'établit. Les connexions locales sont toujours établies en utilisant le contexte du compte Windows dans lequel s'exécute l'agent :  
  
-   Pour les abonnements par extraction, la connexion locale est celle de l’abonné. Ce champ affiche donc toujours : **Emprunter l’identité ’\<Domain>>\\<Connexion\>** ou **Emprunter l’identité ’\<Computer>\\<Connexion\>** pour les abonnements par émission de données.  
  
-   Pour les abonnements par émission de données, la connexion peut également avoir lieu dans le contexte d'une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le champ affiche l’un des éléments suivants : **Utiliser la connexion « \<Login> »** , **Emprunter l’identité « \<Domain>\\<Connexion\> »** ou **Emprunter l’identité « \<Computer>\\<Connexion\> »** . [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande d'établir toutes les connexions dans le contexte du compte Windows.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’un abonnement par extraction](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)   
 [Afficher et modifier les propriétés d’un abonnement par émission de données](../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [Identité et contrôle d’accès pour la réplication](../../relational-databases/replication/security/identity-and-access-control-replication.md)   
 [Modèle de sécurité de l’Agent de réplication](../../relational-databases/replication/security/replication-agent-security-model.md)   
 [Afficher et modifier les paramètres de sécurité de la réplication](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)  
  
  
