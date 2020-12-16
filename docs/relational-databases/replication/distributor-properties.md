---
title: boîte de dialogue Propriétés du serveur de distribution
description: Décrit les différentes pages de la boîte de dialogue « Propriétés du serveur de distribution » dans SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.configdistwizard.distdbproperties.f1
- sql13.rep.configdistwizard.distproperties.general.f1
- sql13.rep.configdistwizard.distproperties.publishers.f1
ms.assetid: f643c7c3-f238-4835-b81e-2c2b3b53b23f
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 51e617c26aad4261d0d337ff2fd2a3dcd19c79b8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484861"
---
# <a name="sql-server-replication-distributor-properties-dialog-box"></a>Réplication SQL Server, boîte de dialogue Propriétés du serveur de distribution 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Cette page décrit les pages disponibles dans la boîte de dialogue Propriétés du serveur de distribution. 

## <a name="general"></a>Général
La page **Général** de la boîte de dialogue **Propriétés du serveur de distribution** vous permet d'ajouter et de supprimer des bases de données de distribution et d'en définir des propriétés.  
  
 La base de données de distribution stocke les métadonnées et les données d'historique pour tous les types de réplications, et les transactions pour la réplication transactionnelle. Dans de nombreux cas, une seule base de données de distribution est suffisante. Mais si plusieurs serveurs de publication n'utilisent qu'un seul serveur de distribution, étudiez l'éventualité de créer une base de données de distribution propre à chaque serveur de publication. Ainsi, vous vous assurerez que les données transitant entre chaque base de données de distribution seront bien séparées.  

 **Bases de données**  
 La grille de propriétés **Bases de données** répertorie le nom et les propriétés de rétention des bases de données de distribution se trouvant sur le serveur de distribution. La **rétention des transactions** est la durée pendant laquelle les transactions sont stockées en vue de leur réplication transactionnelle (la rétention des transactions est également connue sous le nom de rétention de distribution). La **rétention des historiques** est la durée pendant laquelle les métadonnées des historiques sont stockées en vue de leur réplication de quelque type que ce soit. Pour plus d’informations sur la rétention de la distribution, consultez [Expiration et désactivation des abonnements](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
 Cliquez sur le bouton représenté par des points de suspension ( **...** ) dans la grille des propriétés **Bases de données** pour ouvrir la boîte de dialogue **Propriétés de la base de données de distribution** .  
  
 **Nouveau**  
 Permet de créer une base de données de distribution.  
  
 **Supprimer**  
 Sélectionnez une base de données de distribution dans la grille des propriétés **Bases de données** , puis cliquez sur **Supprimer** pour supprimer la base de données. Vous ne pouvez pas supprimer la base de données de distribution s'il n'existe qu'une seule base de données de ce type. Chaque serveur de distribution doit en effet disposer d'au moins une base de données de distribution. Pour les supprimer toutes, vous devez dans ce cas désactiver la distribution sur l'ordinateur. Pour plus d’informations, consultez [Désactiver la publication et la distribution](../../relational-databases/replication/disable-publishing-and-distribution.md).  
  
 **Valeurs par défaut du profil**  
 Permet d'accéder aux profils des agents de réplication dans la boîte de dialogue **Profils de l'Agent** . Pour plus d'informations sur ces profils, consultez [Replication Agent Profiles](../../relational-databases/replication/agents/replication-agent-profiles.md).  

## <a name="publishers"></a>Serveurs de publication
La page **Serveurs de publication** de la boîte de dialogue **Propriétés du serveur de distribution** vous permet de faire en sorte que les serveurs de publication utilisent ce serveur de distribution. Vous pouvez également définir les propriétés associées à ces serveurs de publication. Notez que l'activation d'un serveur de publication pour utiliser ce serveur en tant que serveur de distribution distant ne fait pas de ce serveur un serveur de publication. Vous devez vous connecter au serveur de publication, le configurer pour la publication, et choisir ce serveur comme distributeur. Vous pouvez configurer le serveur de publication et choisir un serveur de distribution à l'aide de l'Assistant Nouvelle publication.  
  
 **Serveurs de publication**  
 Sélectionnez les serveurs autorisés à utiliser ce serveur de distribution. Cliquez sur le bouton Propriétés **(...)** situé en regard d'un serveur de publication pour voir et définir d'autres propriétés.  
  
 **Ajouter**  
 Si le serveur souhaité ne figure pas dans la liste, cliquez sur **Ajouter** afin d'ajouter un serveur de publication [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou un serveur de publication Oracle à la liste des serveurs de publication disponibles. Si le serveur ajouté est le premier à utiliser ce serveur de distribution en tant que serveur de distribution distant, vous serez invité à fournir un mot de passe de lien d'administration.  
  
 **Mot de passe du lien d'administration**  
 Permet de spécifier ou de mettre à jour le mot de passe utilisé pour assurer la réplication de connexion entre le serveur de publication et le serveur de distribution distant à l'aide de la connexion **distributor_admin** :  
  
-   Si le serveur de distribution sert uniquement au niveau local, ce mot de passe est généré de manière aléatoire et configuré automatiquement.   
-   Si le serveur de distribution possède déjà un serveur de publication distant, un mot de passe a déjà été fourni sur cette page ou la page **Mot de passe du serveur de distribution** de l'Assistant Configuration de la distribution.    
-   Si vous activez le premier serveur de publication distant pour ce serveur de distribution, vous serez invité à entrer un mot de passe.  Pour plus d’informations sur la sécurité des serveurs de distribution, consultez [Sécuriser le serveur de distribution](../../relational-databases/replication/security/secure-the-distributor.md).  

## <a name="distribution-database"></a>Base de données de distribution 
 La boîte de dialogue **Propriétés de la base de données de distribution** vous permet d'afficher certaines propriétés et de définir la période de rétention de transaction ainsi que la période de rétention d'historique pour la base de données.  
  
 **Nom**  
 Nom de la base de données de distribution, dont la valeur par défaut est « distribution » (en lecture seule).  
  
 **Emplacements des fichiers**  
 Emplacement du fichier de base de données et du fichier journal (en lecture seule).  
  
 **Période de rétention des transactions**  
 Également appelée période de rétention de distribution. Durée de stockage des transactions pour une réplication transactionnelle. Pour plus d’informations, voir [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
 **Période de rétention de l'historique**  
 Durée de stockage des métadonnées de l'historique pour tous les types de réplications.  
  
 **Sécurité de l'Agent de lecture de la file d'attente**  
 L'Agent de lecture de la file d'attente est utilisé par la réplication transactionnelle avec les abonnements mis à jour en file d'attente. Un Agent de lecture de la file d'attente est créé automatiquement si vous sélectionnez l'option **Publication transactionnelle avec abonnements pouvant être mis à jour** dans la page **Type de publication** de l'Assistant Nouvelle publication. Cliquez sur **Paramètres de sécurité…** pour modifier le compte sous lequel l'Agent est exécuté et effectue des connexions avec le serveur de distribution.  
  
 Un Agent de lecture de la file d'attente peut également être créé en sélectionnant l'option **Créer un Agent de lecture de la file d'attente** dans cette page (cette option est désactivée si l'Agent a déjà été créé).  
  
 Des informations de connexion supplémentaires pour l'Agent de lecture de la file d'attente sont spécifiées dans deux endroits :    
-   L'Agent se connecte au serveur de publication à l'aide des informations d'identification spécifiées dans la boîte de dialogue **Propriétés du serveur de publication** , qui est accessible à partir de la page **Serveurs de publication** de la boîte de dialogue **Propriétés du serveur de distribution** .    
-   L'Agent se connecte à l'Abonné à l'aide des informations d'identification spécifiées pour l'Agent de distribution dans l'Assistant Nouvel abonnement.  Pour plus d’informations, consultez  [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md). 
  
## <a name="see-also"></a>Voir aussi  
 [Configurer la distribution](../../relational-databases/replication/configure-distribution.md)   
 [Afficher et modifier les propriétés d’un serveur de distribution ou d’un serveur de publication](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
  
  
