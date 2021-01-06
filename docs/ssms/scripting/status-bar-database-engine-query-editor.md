---
title: Barre d'état (éditeur de requête du moteur de base de données)
description: Découvrez comment coder en couleur la barre d’état d’une fenêtre de l’éditeur de requête du moteur de base de données pour identifier l’instance du moteur de base de données à laquelle la fenêtre est connectée.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: e7f2d6f4-bb94-4cf5-a096-c34397e679af
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f0772c115a05cc357960512187a8db48153f32aa
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642316"
---
# <a name="status-bar-database-engine-query-editor"></a>Barre d'état (éditeur de requête du moteur de base de données)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La barre d'état des fenêtres de l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] peut être codée par couleur pour indiquer l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] auquel chaque fenêtre est connectée.

1. **Avant de commencer :**  [Couleurs de la barre d'état](#StatusBarColors)  

2. **Pour définir une couleur d'état de serveur dans :**  [Explorateur d’objets](#SetOEServerColor), [Serveur inscrit](#SetRegServerColor)  

3. **Pour utiliser une couleur d’état :**  [Ouvrir un Éditeur de requête à l'aide d'une couleur de serveur](#OpenServerColor), [Ouvrir un Éditeur de requête en spécifiant une couleur d'état](#OpenSpecColor)  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

##  <a name="status-bar-colors"></a><a name="StatusBarColors"></a> Couleurs de la barre d'état

Vous pouvez associer une couleur de barre d'état à un nœud serveur spécifique dans l' **Explorateur d'objets** ou dans **Serveurs inscrits**. Les couleurs peuvent être spécifiées uniquement pour des nœuds serveurs connectés à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)], et non pour des nœuds serveurs pour d'autres technologies SQL Server. Vous pouvez également spécifier une couleur de barre d'état personnalisée chaque fois que vous connectez une nouvelle fenêtre de l'Éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Vous pouvez ensuite ouvrir une fenêtre d'Éditeur de requête soit en utilisant la couleur d'état définie pour le nœud serveur, soit en spécifiant une couleur unique pour cette fenêtre d'éditeur.  

La définition d'une couleur de barre d'état personnalisée pour un nœud serveur dans l'Explorateur d'objets doit être effectuée lors de l'établissement de la connexion. Pour modifier la couleur associée à un nœud serveur existant, vous devez vous déconnecter puis vous reconnecter en spécifiant la nouvelle couleur.  

##  <a name="set-the-status-color-for-a-server-in-object-explorer"></a><a name="SetOEServerColor"></a> Définir la couleur d'état pour un serveur dans l'Explorateur d'objets

**Pour définir une couleur d'état de serveur dans l'Explorateur d'objets**  
  
1.  Dans l’**Explorateur d’objets**, sélectionnez **Se connecter**, puis **Moteur de base de données...** .  
  
2.  Dans la boîte de dialogue **Se connecter au serveur**, sélectionnez **Options >>** .  
  
3.  Activez la case à cocher **Utiliser une couleur personnalisée** .  
  
4.  Pour choisir la couleur, sélectionnez le bouton **Sélectionner...** .  
  
5.  Sélectionnez une couleur de base ou personnalisée, puis cliquez sur OK.  
  
6.  Remplissez le reste des informations de connexion, puis cliquez sur le bouton **Se connecter** .  
  
##  <a name="set-the-status-color-for-a-registered-server"></a><a name="SetRegServerColor"></a> Définir la couleur d'état pour un serveur inscrit  
 **Pour définir une couleur de serveur pour un serveur inscrit**  
  
1.  Dans **Serveurs inscrits**, cliquez avec le bouton droit sur le nœud de serveur, puis sélectionnez **Propriétés...** .  
  
2.  Dans la boîte de dialogue **Modifier les propriétés d'inscription du serveur** , sélectionnez l'onglet **Propriétés de connexion** .  
  
3.  Activez la case à cocher **Utiliser une couleur personnalisée** .  
  
4.  Pour choisir la couleur, sélectionnez le bouton **Sélectionner...** .  
  
5.  Sélectionnez une couleur de base ou personnalisée, puis cliquez sur OK.  
  
6.  Cliquez sur le bouton **Enregistrer** dans la boîte de dialogue **Modifier les propriétés d'inscription du serveur** .  
  
##  <a name="open-an-editor-using-a-server-color"></a><a name="OpenServerColor"></a> Ouvrir un éditeur à l'aide d'une couleur de serveur  
 **Pour ouvrir une fenêtre d'éditeur à l'aide d'une couleur de serveur**  
  
-   Cliquez avec le bouton droit sur un nœud serveur dans l’ **Explorateur d’objets** ou dans **Serveurs inscrits**, puis sélectionnez **Nouvelle requête**.  
  
-   En guise d'alternative, mettez en surbrillance un nœud serveur, puis sélectionnez le bouton **Nouvelle requête** dans la barre d'outils.  
  
-   La barre d'état dans la fenêtre de l'éditeur utilisera la couleur définie pour le serveur associé.  
  
##  <a name="open-an-editor-specifying-a-status-color"></a><a name="OpenSpecColor"></a> Ouvrir un éditeur en spécifiant une couleur d'état  
 **Pour ouvrir une fenêtre d'éditeur en spécifiant une couleur d'état**  
  
-   Ouvrez le menu **Fichier** , sélectionnez **Nouveau**, puis **Requête de moteur de base de données**.  
  
-   Dans la boîte de dialogue **Se connecter au serveur**, sélectionnez **Options >>** .  
  
-   Activez la case à cocher **Utiliser une couleur personnalisée** .  
  
-   Pour choisir la couleur, sélectionnez le bouton **Sélectionner...** .  
  
-   Sélectionnez une couleur de base ou personnalisée, puis cliquez sur OK.  
  
-   Remplissez le reste des informations de connexion, puis cliquez sur le bouton **Se connecter** .  
  
## <a name="see-also"></a>Voir aussi  
 [Éditeurs de texte et de requête &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)  
  
