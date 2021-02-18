---
title: Options de relecture
titleSuffix: SQL Server Profiler
description: Explorez les paramètres que SQL Server Profiler utilise pour relire une trace capturée. Découvrez comment utiliser la boîte de dialogue Configuration de la relecture pour ajuster les paramètres.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 58761a25-a84f-4a90-9c61-97700bc5ad9c
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: af342941ca797d9054492e59a5004096f901bf69
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353372"
---
# <a name="replay-options-sql-server-profiler"></a>Options de relecture (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Avant de relire une trace capturée avec [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], spécifiez les options de relecture dans la boîte de dialogue **Configuration de la relecture** . Pour accéder à cette boîte de dialogue, ouvrez le fichier ou la table de trace de relecture dans [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], puis cliquez sur **Démarrer** dans le menu **Relire**. Pour savoir quelles autorisations sont nécessaires pour relire une trace, consultez [Autorisations nécessaires pour exécuter SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md).  
  
 Cette rubrique décrit les options spécifiées avec la boîte de dialogue **Configuration de la relecture** .  
  
> [!NOTE]  
>  Nous recommandons d'utiliser Distributed Replay Utility pour relire des applications OLTP exigeantes (avec de nombreuses connexions simultanées actives ou un débit élevé). Distributed Replay Utility peut relire les données de trace de plusieurs ordinateurs, en simulant mieux les charges de travail sensibles. Pour plus d'informations, consultez [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md).  
  
## <a name="basic-replay-options"></a>Options de relecture de base  
 **Serveur de relecture**  
 Le serveur est le nom de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur lequel vous souhaitez relire la trace. Le serveur doit remplir les conditions préalables de relecture mentionnées dans la section [Conditions préalables à la relecture](../../tools/sql-server-profiler/replay-requirements.md).  
  
 **Enregistrer dans le fichier**  
 Le fichier de sortie contenant le résultat de la relecture de la trace est écrit et pourra être affiché ultérieurement. Par défaut, le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] n'affiche à l'écran que les résultats de la relecture de la trace.  
  
 **Enregistrer dans la table**  
 La table de base de données contenant le résultat de la relecture de la trace est écrite et pourra être affichée ultérieurement.  
  
 **Nombre de threads de relecture**  
 Spécifiez le nombre de threads de relecture à utiliser simultanément. Un nombre élevé consomme davantage de ressources pendant la relecture, mais accélère la relecture. L'ordre des événements n'est pas totalement conservé en cas d'utilisation de plusieurs threads.  
  
 **Relire les événements selon leur ordre de suivi**  
 Permet d’utiliser des méthodes de débogage telles que la trace pas à pas dans chaque trace. Si cette option n'est pas sélectionnée, la relecture ne garantit pas que les événements sont relus dans un ordre cohérent avec l'ordre de leur capture initiale.  
  
 **Relire les événements en utilisant plusieurs threads**  
 Optimise les performances et désactive le débogage. Les événements sont relus dans l'ordre dans lequel ils ont été enregistrés pour un SPID (ID du processus serveur) particulier, mais l'ordre des SPID n'est pas garanti.  
  
 **Afficher les résultats de relecture**  
 Affiche les résultats de la relecture. Il s'agit de l'option par défaut. Si la trace que vous relisez est très importante, vous pouvez désactiver cette option pour libérer de l’espace disque.  
  
> [!NOTE]  
>  Pour des performances de relecture optimales, il est recommandé de sélectionner de relire les événements à l’aide de plusieurs threads, et de ne pas choisir d'afficher les résultats de la relecture.  
  
## <a name="advanced-replay-options"></a>Options de relecture avancées  
 **Relire les SPID système**  
 Relit tous les SPID. Il s'agit de l'option par défaut.  
  
 **Relire un SPID uniquement**  
 Relit le numéro de SPID que vous choisissez dans la liste.  
  
 **Limiter la relecture par date et heure**  
 Relit la trace pour l’ **Heure de début** et l’ **Heure de fin** spécifiées.  
  
 **Délai d'attente du moniteur d'intégrité**  
 Définit la durée pendant laquelle un processus est autorisé à s'exécuter avant que le moniteur d'intégrité y mette fin.  
  
 **Intervalle d'interrogation du moniteur d'intégrité**  
 Définit la fréquence à laquelle le moniteur d'intégrité interroge les candidats pour les arrêter.  
  
 **Activer le moniteur de processus bloqués de SQL Server**  
 Définit la fréquence à laquelle le moniteur de processus recherche les processus bloqués ou les processus de blocage.  
  
## <a name="about-the-health-monitor"></a>À propos du moniteur d'intégrité  
 Le moniteur d'intégrité est un thread d'application qui surveille les processus simulés impliqués dans la relecture d'une trace, et met fin aux processus bloqués au cours de la relecture. Sous l’onglet **Options de relecture avancées** de la boîte de dialogue **Configuration de la relecture** , vous pouvez spécifier combien de temps le moniteur d’intégrité doit attendre (en secondes) avant de mettre fin à un processus bloqué (**Délai d’attente du moniteur d’intégrité**). Si vous affectez à cet intervalle la valeur 0, le moniteur d'intégrité ne met jamais fin aux processus de blocage simulés dans la trace en cours de relecture.  
  
## <a name="see-also"></a>Voir aussi  
 [Relire des traces](../../tools/sql-server-profiler/replay-traces.md)   
 [Conditions requises pour la relecture](../../tools/sql-server-profiler/replay-requirements.md)   
 [Considérations sur la relecture des traces &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/considerations-for-replaying-traces-sql-server-profiler.md)  
  
  
