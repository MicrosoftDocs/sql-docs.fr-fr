---
title: Ajouter une image d’arrière-plan (Générateur de rapports) | Microsoft Docs
description: Découvrez comment incorporer une image dans la définition de rapport pour ajouter l’image à un large éventail d’éléments de rapport, notamment des zones de texte, des listes et des en-têtes de page.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: c777fefb-8695-44a7-b5cd-a18c587583f2
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: c611c521ec3e98d205b4156e84800549341996d3
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91934818"
---
# <a name="add-a-background-image-report-builder-and-ssrs"></a>Ajouter une image d'arrière-plan (Générateur de rapports et SSRS)
  Vous pouvez ajouter une image d'arrière-plan à un élément de rapport, par exemple un rectangle, une zone de texte, une liste, une matrice, un tableau et certaines parties d'un graphique, ou à une section de rapport, par exemple l'en-tête de page, le pied de page ou le corps du rapport. Vous pouvez définir une image d’arrière-plan pour tout élément sélectionné sur l’aire de conception du rapport qui affiche **BackgroundImage** dans le volet Propriétés. Comme les autres images, l'image d'arrière-plan peut être une URL vers une image sur le serveur de rapports, une image d'un champ de dataset ou une image incorporée dans la définition de rapport. Pour utiliser une image incorporée dans le rapport, vous devez d'abord ajouter l'image à la définition de rapport avant d'ajouter l'image à l'aire de conception.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-embed-an-image-in-the-report-definition"></a>Pour incorporer une image dans la définition de rapport  
  
1.  Dans le volet des données de rapport, cliquez avec le bouton droit sur le nœud **Images** , puis cliquez sur **Ajouter une image**.  
  
    > [!NOTE]  
    >  Si le volet des données de rapport n’est pas visible, cliquez sur **Données du rapport** sous l’onglet **Affichage**.  
  
2.  Accédez à l’image à incorporer dans votre définition de rapport, puis cliquez sur **OK**.  
  
### <a name="to-add-a-background-image"></a>Pour ajouter une image d'arrière-plan  
  
1.  En mode création de rapport, sélectionnez l'élément de rapport auquel ajouter une image d'arrière-plan.  
  
2.  Si le volet Propriétés n’est pas visible, sélectionnez **Propriétés** sous l’onglet **Affichage**.  
  
3.  Dans le volet Propriétés, développez **BackgroundImage**et procédez comme suit :  
  
    -   Pour une image incorporée :  
  
         Affectez **Embedded** à **Source**.  
  
         Affectez le nom d’une image incorporée dans le rapport à **Value** .  
  
    -   Pour une image externe :  
  
         Affectez **External** à **Source**.  
  
         Affectez le chemin valide d’une image à **Value** . Il peut s'agir d'un serveur de rapports en mode natif ou en mode intégré SharePoint, ou il peut s'agir d'un autre site Web. Pour plus d’informations, consultez [Ajouter une image externe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-an-external-image-report-builder-and-ssrs.md).  
  
    -   Pour une image contenue dans un champ de la base de données à laquelle l'élément de rapport est connecté :  
  
         Affectez **Database** à **Source**.  
  
         Affectez le nom d’un champ dans le dataset du rapport à **Value** . Pour plus d’informations, consultez [Ajouter une image liée à des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-a-data-bound-image-report-builder-and-ssrs.md).  
  
         Pour **MIMEType**, ou le format de fichier, sélectionnez le type MIME approprié pour l’image, par exemple .bmp.  
  
        > [!NOTE]  
        >  MIMEType s’applique uniquement si la propriété **Source** a la valeur **Database**. Si la propriété **Source** a la valeur **External** ou **Embedded**, la valeur de **MIMEType** est ignorée.  
  
    -   Pour **BackgroundRepeat**, sélectionnez une expression : **Default**, **Repeat**, **RepeatX**ou **RepeatY**, ou **Clip**.  
  
         Pour les images d’arrière-plan d’un graphique, **BackgroundRepeat** peut avoir la valeur **Default**, **Repeat**, **Fit** **Clip**, mais pas **RepeatX** ni **RepeatY**.  
  
## <a name="see-also"></a>Voir aussi  
 [Images &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/images-report-builder-and-ssrs.md)   
 [Boîte de dialogue Propriétés de l’image, Général &#40;Générateur de rapports et SSRS&#41;](./images-report-builder-and-ssrs.md)  
  
