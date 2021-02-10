---
description: Tutoriel RDS
title: Didacticiel RDS | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO]
ms.assetid: 6e3305a0-7bc7-40d1-9122-235c15d23ab2
author: rothja
ms.author: jroth
ms.openlocfilehash: b2b9294b3024a3d4a7dbfd51baadb2474b7715ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036409"
---
# <a name="rds-tutorial"></a>Tutoriel RDS
Ce didacticiel illustre l’utilisation du modèle de programmation RDS pour interroger et mettre à jour une source de données. Tout d’abord, il décrit les étapes nécessaires à la réalisation de cette tâche. Le didacticiel est ensuite répété dans Microsoft® Visual Basic Scripting Edition (avec ADO for Windows Foundation classes (ADO/WFC)).  
  
 Ce didacticiel est codé dans différents langages pour deux raisons :  
  
-   La documentation de RDS suppose les codes de lecteur dans Visual Basic. Cela rend la documentation commode pour les programmeurs Visual Basic, mais moins utile pour les programmeurs qui utilisent d’autres langages.  
  
-   Si vous n’êtes pas certain d’une fonctionnalité RDS particulière et que vous connaissez un peu d’autres langages, vous pouvez peut-être résoudre votre question en recherchant la même fonctionnalité dans une autre langue.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="how-the-tutorial-is-presented"></a>Présentation du didacticiel  
 Ce didacticiel est basé sur le modèle de programmation RDS. Elle traite chaque étape du modèle de programmation individuellement. En outre, il illustre chaque étape avec un fragment de code Visual Basic.  
  
 L’exemple de code est répété dans d’autres langages avec une discussion minime. Chaque étape d’un didacticiel de langage de programmation donné est marquée avec l’étape correspondante dans le modèle de programmation et le didacticiel descriptif. Utilisez le numéro de l’étape pour faire référence à la discussion dans le didacticiel descriptif.  
  
 Le modèle de programmation RDS est indiqué dans la section suivante. Utilisez-le comme feuille de route à mesure que vous suivez le didacticiel.  
  
## <a name="rds-programming-model-with-objects"></a>Modèle de programmation RDS avec des objets  
  
-   Spécifiez le programme à appeler sur le serveur et obtenez une méthode (proxy) pour y faire référence à partir du client.  
  
-   Appelez le programme serveur. Transmettez des paramètres au programme serveur qui identifie la source de données et la commande à émettre.  
  
-   Le programme serveur obtient un objet [Recordset](../../reference/ado-api/recordset-object-ado.md) à partir de la source de données, généralement à l’aide d’ADO. Le cas échéant, l’objet **Recordset** est traité sur le serveur.  
  
-   Le programme serveur retourne l’objet **Recordset** final à l’application cliente.  
  
-   Sur le client, l’objet **Recordset** est éventuellement placé dans un formulaire qui peut être facilement utilisé par les contrôles visuels.  
  
-   Les modifications apportées à l’objet **Recordset** sont renvoyées au serveur et utilisées pour mettre à jour la source de données.  
  
 Ce didacticiel contient les rubriques suivantes.  
  
-   [Étape 1 : Spécifier un programme serveur (tutoriel RDS)](./step-1-specify-a-server-program-rds-tutorial.md)  
  
-   [Étape 2 : Appeler le programme serveur (tutoriel RDS)](./step-2-invoke-the-server-program-rds-tutorial.md)  
  
-   [Étape 3 : Le serveur obtient un recordset (tutoriel RDS)](./step-3-server-obtains-a-recordset-rds-tutorial.md)  
  
-   [Étape 4 : Le serveur retourne un recordset (tutoriel RDS)](./step-4-server-returns-the-recordset-rds-tutorial.md)  
  
-   [Étape 5 : DataControl devient utilisable (tutoriel RDS)](./step-5-datacontrol-is-made-usable-rds-tutorial.md)  
  
-   [Étape 6 : Les changements sont envoyés au serveur (tutoriel RDS)](./step-6-changes-are-sent-to-the-server-rds-tutorial.md)  
  
-   [Tutoriel RDS (VBScript)](./rds-tutorial-vbscript.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Étape 1 : spécifier un programme serveur (didacticiel RDS)](./step-1-specify-a-server-program-rds-tutorial.md)   
 [Tutoriel RDS (VBScript)](./rds-tutorial-vbscript.md)