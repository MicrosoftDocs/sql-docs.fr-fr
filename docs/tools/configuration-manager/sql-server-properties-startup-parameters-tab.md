---
title: Propriétés SQL Server (onglet Paramètres de démarrage)
description: Utilisez l’onglet Paramètres de démarrage de la boîte de dialogue Propriétés de SQL Server pour ajouter ou supprimer des paramètres de démarrage, ce qui peut affecter les performances du Moteur de base de données.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 16942624-5374-446c-8de4-ee6ed34d6e94
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a80683b2e2092cf6a0eff2519edbcd893c2357ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349588"
---
# <a name="sql-server-properties-startup-parameters-tab"></a>Propriétés SQL Server (onglet Paramètres de démarrage)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Utilisez cette boîte de dialogue pour ajouter ou supprimer des paramètres de démarrage pour le [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Les paramètres de démarrage peuvent avoir un effet considérable sur les performances du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Avant d'ajouter ou de modifier des paramètres de démarrage, consultez la rubrique « Utilisation des options de démarrage du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="options"></a>Options  
 **Spécifiez un paramètre de démarrage**  
 Pour ajouter un paramètre, tapez-le, puis cliquez sur **Ajouter**.  
  
 Pour modifier l'un des paramètres obligatoires, sélectionnez-le dans la zone **Paramètres existants** , modifiez les valeurs dans la zone **Spécifiez un paramètre de démarrage** , puis cliquez sur **Mise à jour**.  
  
 **Paramètres existants**  
 Pour supprimer un paramètre, sélectionnez-le, puis cliquez sur **Supprimer**.  
  
## <a name="parameter-format"></a>Format du paramètre  
 N'entrez aucun séparateur entre les paramètres. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ajoute automatiquement le séparateur. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] impose les spécifications de paramètre suivantes.  
  
-   Les espaces de début et de fin sont supprimés des paramètres de démarrage.  
  
-   Tous les paramètres de démarrage commencent par un - (tiret) et la deuxième valeur est une lettre.  
  
## <a name="required-parameters"></a>Paramètres obligatoires  
 Les paramètres suivants sont obligatoires. Ils peuvent être modifiés, mais pas supprimés.  
  
-   -d est le chemin du fichier **master.mdf** (fichier de données de la base de données MASTER).  
  
-   -l est le chemin du fichier **master.ldf** (fichier journal de la base de données MASTER).  
  
-   -e est le chemin des fichiers journaux des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!CAUTION]  
>  Si les paramètres de chemin sont incorrects, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] risque de ne pas démarrer.  
  
 Pour plus d'informations sur le déplacement de la base de données master, consultez la rubrique « Déplacement des bases de données système » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="optional-parameters"></a>Paramètres facultatifs  
 Tous les paramètres de démarrage pris en charge sont décrits dans la rubrique « Utilisation des options de démarrage du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] », dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un paramètre de démarrage de -T *trace#* indique qu'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être démarrée avec un indicateur de trace déterminé (*trace#* ) activé. Les indicateurs de trace permettent de démarrer le serveur avec un comportement non standard. Pour plus d’informations sur les indicateurs de trace, consultez la rubrique « Indicateurs de trace ([!INCLUDE[tsql](../../includes/tsql-md.md)]) » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!CAUTION]  
>  Vous pouvez consulter des paramètres de démarrage et des indicateurs de trace supplémentaires non documentés, décrits sur Internet. Les paramètres de démarrage et les indicateurs de trace non documentés sont créés pour résoudre des problèmes rares ou imposer des conditions obligatoires à des fins de test. L'utilisation de paramètres de démarrage non documentés peut produire des résultats inattendus. N'utilisez pas de paramètres non documentés, sauf sur indication du support technique de Microsoft.  
  
 La liste suivante décrit des paramètres facultatifs courants.  
  
|Paramètre|Description courte|  
|---------------|-----------------------|  
|-M|Démarre une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode mono-utilisateur.|  
|-T1204|Retourne les ressources et les types de verrous participant à l'interblocage et la commande active affectée.|  
|-T1224|Désactive l'escalade de verrous en fonction du nombre de verrous.|  
|-T3608|Empêche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de démarrer automatiquement et de récupérer des bases de données, sauf la base de données master.|  
|-T7806|Active une connexion administrateur dédiée (DAC) sur [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)].|  
  
> [!CAUTION]  
>  Certains paramètres facultatifs peuvent modifier le comportement du serveur et affecter les performances.  
  
## <a name="permissions"></a>Autorisations  
 L'utilisation de cette page est limitée aux utilisateurs qui peuvent modifier les entrées associées dans le Registre. Notamment :  
  
-   les membres du groupe Administrateurs local ;  
  
-   Le compte de domaine utilisé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] est configuré pour l'exécution sous un compte de domaine.  
  
## <a name="books-online-references"></a>Références de la documentation en ligne  
 Pour plus d’informations sur les paramètres de démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez « Procédure : configurer les options de démarrage du serveur (Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
  
