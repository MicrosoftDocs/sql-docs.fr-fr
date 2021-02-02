---
description: Compteurs de performances
title: Compteurs de performances | Microsoft Docs
ms.custom: supportability
ms.date: 08/27/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- logs [Integration Services], performance counters
- performance counters [Integration Services]
- data flow [Integration Services], performance
- counters [Integration Services]
- data flow engine [Integration Services]
ms.assetid: 11e17f4e-72ed-44d7-a71d-a68937a78e4c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7ae8429bb51ec2a58a167784b701b7ae0532bb92
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076927"
---
# <a name="performance-counters"></a>Compteurs de performance

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] installe un ensemble de compteurs de performances qui vous permettent d’analyser les performances du moteur de flux de données. Par exemple, le compteur Mémoires tampon spoulées permet de déterminer si des tampons de données sont écrits temporairement sur le disque lors de l'exécution d'un package. Cette permutation diminue les performances et indique que la mémoire de l'ordinateur est insuffisante.  
  
> **REMARQUE :** Si vous installez [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur un ordinateur qui exécute [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)], puis que vous mettez à niveau cet ordinateur vers [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)], le processus de mise à niveau supprime les compteurs de performances de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de l’ordinateur. Pour restaurer les compteurs de performances de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur l’ordinateur, exécutez l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode réparation.  
  
 Le tableau suivant décrit les compteurs de performance.  
  
|Compteur de performances|Description|  
|-------------------------|-----------------|  
|Octets BLOB lus|Le nombre d'octets des données d'objet BLOB (Binary Large Object) que le moteur de flux de données a lu à partir de toutes les sources.|  
|Octets BLOB écrits|Le nombre d'octets des données BLOB que le moteur de flux de données a écrit sur toutes les destinations.|  
|Fichiers BLOB utilisés|Nombre de fichiers BLOB que le moteur de flux de données utilise actuellement pour la mise en file d'attente.|  
|Mémoire tampon|La quantité de mémoire en cours d'utilisation. Cela peut inclure à la fois la mémoire physique et la mémoire virtuelle. Quand ce nombre est supérieur à la quantité de mémoire physique, le compte **Mémoires tampon spoulées** augmente, ce qui indique une augmentation de l’échange de mémoire. L'augmentation de l'échange de mémoire ralentit les performances du moteur de flux de données.|  
|Tampons en cours d'utilisation|Nombre d'objets de mémoire tampon, de tous les types, que tous les composants de flux de données et le moteur de flux de données utilisent actuellement.|  
|Mémoires tampon spoulées|Nombre de mémoires tampon actuellement écrites sur le disque. Si le moteur de flux de données est à cours de mémoire physique, les mémoires tampons non utilisées actuellement sont écrites sur le disque puis rechargées en cas de besoin.|  
|Mémoire tampon plate|Le volume total de mémoire, en octets, que toutes les mémoires tampons plates utilisent. Les mémoires tampons plates sont des blocs de mémoire utilisés par un composant pour stocker des données. Une mémoire tampon plate est un grand bloc d'octets, auquel l'accès se fait octet par octet.|  
|Mémoires tampons plates en cours d'utilisation|Nombre de mémoires tampons plates que le moteur de flux de données utilise. Toutes les mémoires tampons plates sont des mémoires tampons privées.|  
|Mémoire tampon privée|Le volume total de mémoire utilisé par toutes les mémoires tampons privées. Une mémoire tampon n'est pas privée si le moteur de flux de données la crée pour prendre en charge le flux de données. Une mémoire tampon privée est une mémoire tampon qu'une transformation utilise seulement pour un travail temporaire. Par exemple, la transformation d'agrégation utilise des mémoires tampons privées pour son travail.|  
|Mémoires tampons privées en cours d'utilisation|Le nombre de mémoires tampons utilisées par les transformations.|  
|Lignes lues|Le nombre de lignes produites par une source. Le nombre n'inclut pas les lignes lues à partir des tables de références par la transformation de recherche.|  
|Lignes écrites|Le nombre de lignes offertes vers une destination. Le nombre ne reflète pas les lignes écrites vers la banque de données de destination.|  
  
 Vous utilisez le composant logiciel enfichable MMC (Microsoft Management Console) Performance pour créer un journal qui capture les compteurs de performances.  
  
 Pour plus d’informations sur l’amélioration des performances, consultez [Fonctionnalités de performances de flux de données](../../integration-services/data-flow/data-flow-performance-features.md).  
  
## <a name="obtain-performance-counter-statistics"></a>Obtenir des statistiques sur les compteurs de performance  
 Pour les projets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] déployés sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vous pouvez obtenir des statistiques sur les compteurs de performances à l’aide de la fonction [dm_execution_performance_counters &#40;base de données SSISDB&#41;](../../integration-services/functions-dm-execution-performance-counters.md).  
  
 Dans l'exemple suivant, la fonction retourne des statistiques pour une exécution en cours ayant l'ID 34.  
  
```sql
select * from [catalog].[dm_execution_performance_counters] (34)  
```  
  
 Dans l’exemple suivant, la fonction retourne des statistiques pour toutes les exécutions en cours sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
```sql
select * from [catalog].[dm_execution_performance_counters] (NULL)  
  
```  
  
> **IMPORTANT** Si vous êtes membre du rôle de base de données **ssis_admin** , les statistiques de performances de toutes les exécutions en cours sont retournées.  Si vous n’êtes pas membre du rôle de base de données **ssis_admin** , les statistiques de performances des exécutions en cours pour lesquelles vous disposez d’autorisations de lecture sont retournées.  
  
## <a name="related-content"></a>Contenu associé  
  
-   Outil : [SSIS Performance Visualization for Business Intelligence Development Studio (projet CodePlex)](https://go.microsoft.com/fwlink/?LinkId=146626), sur le site codeplex.com.  
  
-   Vidéo : [Mesure et présentation des performances de vos packages SSIS dans l’entreprise (Vidéo liée à SQL Server)](/previous-versions/sql/sql-server-2008/dd795223(v=sql.100)), sur le site msdn.microsoft.com.  
  
-   Article du Support technique : [Le compteur de performance SSIS n’est plus disponible dans l’Analyseur de performances après la mise à niveau vers Windows Server 2008](https://web.archive.org/web/20111112202346/http://support.microsoft.com:80/kb/955632), sur le site support.microsoft.com.  

## <a name="add-a-log-for-data-flow-performance-counters"></a>Ajouter un journal pour les compteurs de performances de flux de données
  Cette procédure décrit comment ajouter un journal pour les compteurs de performances fournis par le moteur de flux de données.  
  
> [!NOTE]  
>  Si vous installez [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur un ordinateur qui exécute [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)], puis que vous mettez à niveau cet ordinateur vers [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)], le processus de mise à niveau supprime les compteurs de performances de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de l’ordinateur. Pour restaurer les compteurs de performances de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur l’ordinateur, exécutez l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode réparation.  
  
### <a name="to-add-logging-of-performance-counters"></a>Pour ajouter un journal des compteurs de performances  
  
1.  Dans le **Panneau de configuration**, si vous utilisez l’affichage classique, cliquez sur **Outils d’administration**. Si vous utilisez l’affichage des catégories, cliquez sur **Performances et maintenance** , puis sur **Outils d’administration**.  
  
2.  Cliquez sur **Performances**.  
  
3.  Dans la boîte de dialogue **Performances** , développez **Journaux et alertes de performances**, cliquez avec le bouton droit sur **Journaux de compteurs**, puis cliquez sur **Nouveaux paramètres de journal**. Tapez le nom du journal. Par exemple, tapez **MonJournal**.  
  
4.  Cliquez sur **OK**.  
  
5.  Dans la boîte de dialogue **MonJournal** , cliquez sur **Ajouter des compteurs**.  
  
6.  Cliquez sur **Utiliser les compteurs locaux de l’ordinateur** pour journaliser les compteurs de performances sur l’ordinateur local ou cliquez sur **Choisir les compteurs sur :** et sélectionnez un ordinateur dans la liste pour journaliser les compteurs de performances sur l’ordinateur spécifié.  
  
7.  Dans la boîte de dialogue **Ajouter des compteurs** , sélectionnez **SQL Server : pipeline SSIS** dans la liste **Objet de performance** .  
  
8.  Pour sélectionner les compteurs de performances, effectuez l'une des actions suivantes :  
  
    -   Sélectionnez **Tous les compteurs** pour journaliser tous les compteurs de performances.  
  
    -   Sélectionnez **Sélectionner les compteurs dans la liste** et sélectionnez les compteurs de performances à utiliser.  
  
9. Cliquez sur **Add**.  
  
10. Cliquez sur **Fermer**.  
  
11. Dans la boîte de dialogue **MonJournal** , vérifiez la liste des compteurs de performances journalisés dans la liste **Compteurs** .  
  
12. Pour ajouter des compteurs supplémentaires, répétez les étapes 5 à 10.  
  
13. Cliquez sur **OK**.  
  
    > [!NOTE]  
    >  Vous devez démarrer le service Journaux et alertes de performance à l'aide d'un compte local ou d'un compte de domaine membre du groupe Administrateurs.  

## <a name="see-also"></a>Voir aussi  
 [Exécution de projets et de packages](../packages/run-integration-services-ssis-packages.md) [Événements journalisés par un package Integration Services](../../integration-services/performance/events-logged-by-an-integration-services-package.md)
