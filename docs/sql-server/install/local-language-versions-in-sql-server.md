---
title: Versions linguistiques locales dans SQL Server | Microsoft Docs
description: Les versions en anglais des systèmes d'exploitation pris en charge prennent en charge les versions localisées de SQL Server à l’aide des paramètres du Pack d'interface utilisateur multilingue Windows.
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 20b99363-0490-4aa3-9a3d-262f827d81e8
author: cawrites
ms.author: chadam
ms.openlocfilehash: cabe727dd72136f2b4848620bdee0a02d8c915e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336150"
---
# <a name="local-language-versions-in-sql-server"></a>Versions linguistiques locales dans SQL Server
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge toutes les langues prises en charge par les systèmes d'exploitation Windows.  
  
## <a name="cross-language-support"></a>Prise en charge de langues différentes  
  
-   La version anglaise de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est prise en charge sur toutes les versions localisées des systèmes d'exploitation.  
  
-   Les versions localisées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont prises en charge sur les systèmes d'exploitation localisés dans la langue correspondante ou sur les versions anglaises des systèmes d'exploitation pris en charge grâce aux paramètres du Pack Windows d'interface utilisateur multilingue. Pour plus d'informations, voir [Configure Operating System to Support Localized Versions](../../sql-server/install/local-language-versions-in-sql-server.md#BK_ConfigureOS).  
  
-   Les versions localisées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent être mises à niveau uniquement vers des versions localisées dans la même langue et ne peuvent pas être mises à niveau vers la version anglaise.  
  
-   Les versions localisées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent également être installées côte à côte avec des instances en langue anglaise de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="configure-operating-system-to-support-localized-versions"></a><a name="BK_ConfigureOS"></a> Configure Operating System to Support Localized Versions  
 Les versions localisées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont prises en charge dans les versions anglaises des systèmes d'exploitation pris en charge via l'utilisation des paramètres du Pack d'interface utilisateur multilingue de Windows (MUI, Multilingual User Interface).  
  
 Vous devez toutefois vérifier certains paramètres de système d'exploitation avant d'installer une version localisée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un serveur qui exécute un système d'exploitation en langue anglaise avec un paramétrage MUI non anglais. Vous devez vérifier que les paramètres de système d'exploitation suivants correspondent à la langue de la version localisée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui va être installée.  
  
-   Le paramètre de l'interface utilisateur du système d'exploitation  
  
-   Les paramètres régionaux utilisateur du système d'exploitation  
  
-   Les paramètres régionaux système  
  
 Si les paramètres ne correspondent pas à la langue de la version localisée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à installer, procédez comme suit pour définir correctement ces paramètres de système d'exploitation.  
  
> [!CAUTION]  
>  Les installations de différentes versions linguistiques d'instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le même ordinateur ne sont pas prises en charge.  
  
#### <a name="to-change-the-operating-system-user-interface-setting"></a>Pour modifier le paramètre de l'interface utilisateur du système d'exploitation  
  
1.  Si elle n'est pas déjà installée, installez l'interface utilisateur multilingue du système d'exploitation qui correspond à votre version localisée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
2.  Dans le Panneau de configuration, ouvrez **Options régionales et linguistiques**.  
  
3.  Dans l'onglet **Langues** , pour **Langue utilisée dans les menus et les dialogues**, sélectionnez une valeur dans la liste.  
  
     Ce paramètre affecte la langue de l'interface utilisateur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]qui doit correspondre à votre version localisée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
4.  Cliquez sur **Appliquer** pour confirmer la modification et sur **OK** pour fermer la fenêtre.  
  
#### <a name="to-change-the-operating-system-user-locale-setting"></a>Pour modifier les paramètres régionaux utilisateur du système d'exploitation  
  
1.  Si elle n'est pas déjà installée, installez l'interface utilisateur multilingue du système d'exploitation qui correspond à votre version localisée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
2.  Dans le Panneau de configuration, ouvrez **Options régionales et linguistiques**.  
  
3.  Dans l'onglet **Options régionales** , pour **Sélectionner un élément pour afficher ses paramètres**, sélectionnez une valeur dans la liste.  
  
     Ce paramètre affecte la mise en forme des données propre à une culture spécifique.  
  
4.  Cliquez sur **Appliquer** pour confirmer la modification et sur **OK** pour fermer la fenêtre.  
  
#### <a name="to-change-the-system-locale-setting"></a>Pour modifier les paramètres régionaux système  
  
1.  Si elle n'est pas déjà installée, installez l'interface utilisateur multilingue du système d'exploitation qui correspond à votre version localisée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
2.  Dans le Panneau de configuration, ouvrez **Options régionales et linguistiques**.  
  
3.  Sous l’onglet **Options avancées** , pour **Sélectionnez une langue qui corresponde à la version des programmes non Unicode que vous voulez utiliser**, sélectionnez une valeur dans la liste.  
  
     Ce paramètre permet au programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de choisir le meilleur classement par défaut pour votre installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
4.  Cliquez sur **Appliquer** pour confirmer la modification et sur **OK** pour fermer la fenêtre.  
  
## <a name="see-also"></a>Voir aussi  
 [Configurations matérielle et logicielle requises pour l’installation de SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
 [Installer SQL Server](../../database-engine/install-windows/install-sql-server.md)  
  
  
