---
description: 'HelloData : une application ADO simple'
title: 'HelloData : une application ADO simple | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- HelloData sample application [ADO]
- ADO, samples
ms.assetid: de4bcd56-dac2-45e6-95ab-9fd7f25878fc
author: rothja
ms.author: jroth
ms.openlocfilehash: ec46ed0f94ffe9bd50d0576ab7b3855011bcf775
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033159"
---
# <a name="hellodata-a-simple-ado-application"></a>HelloData : une application ADO simple
Cette simple application passe par chacune des quatre principales opérations ADO : obtention, examen, modification et mise à jour des données. Ces opérations sont effectuées sur l’exemple de base de données Northwind inclus avec Microsoft® SQL Server. Pour vous concentrer sur les notions de base d’ADO et pour empêcher l’encombrement du code, la gestion des erreurs dans l’exemple est minime.  
  
### <a name="to-run-hellodata"></a>Pour exécuter HelloData  
  
1.  Créez un projet de Visual Basic EXE standard qui fait référence à la bibliothèque ADO. Pour plus d’informations, consultez [référencement des bibliothèques ADO](../referencing-the-ado-libraries.md).  
  
2.  Créez quatre boutons de commande en haut du formulaire, en définissant les propriétés **Name** et **Caption** sur les valeurs indiquées dans le tableau à la fin de cette rubrique.  
  
3.  Sous les boutons, ajoutez un **contrôle Microsoft DataGrid** (msdatgrd. ocx). Le fichier msdatgrd. ocx est inclus avec Visual Basic et se trouve dans votre répertoire \windows\system32 ou \Winnt\System32. Pour ajouter le contrôle DataGrid à votre Visual Basic volet boîte à outils, sélectionnez **composants...** dans le menu **projet** . Activez ensuite la case à cocher en regard de « contrôle Microsoft DataGrid 6,0 (SP3) (OLEDB) », puis cliquez sur **OK**. Pour ajouter le contrôle au projet, faites glisser le contrôle DataGrid de la boîte à outils vers le formulaire Visual Basic.  
  
4.  Créez une **zone de texte** sur le formulaire sous la grille et définissez ses propriétés comme indiqué dans le tableau. Lorsque vous avez terminé, le formulaire doit ressembler à l’illustration suivante.  
  
5.  Enfin, copiez le code figurant dans le [code HelloData](./hellodata-code.md), puis collez-le dans la fenêtre de l’éditeur de code du formulaire. Appuyez sur **F5** pour exécuter le code.  
  
> [!NOTE]
>  Dans l’exemple suivant, et tout au long de ce guide, l’ID d’utilisateur « MyId » avec le mot de passe « 123aBc » est utilisé pour l’authentification auprès du serveur. Vous devez remplacer ces valeurs par des informations d’identification d’ouverture de session valides pour votre serveur. Remplacez également la valeur « MySQLServer » par le nom de votre serveur.  
  
 Pour obtenir une description détaillée du code, consultez [Commentaires sur HelloData](./comments-on-hellodata.md).  
  
 ![Affiche Formulaire1 pour l'application HelloData VB](../../../ado/guide/data/media/hellodata.gif "HelloData")  
  
|Type de contrôle|Propriété|Valeur|  
|------------------|--------------|-----------|  
|Formulaire|Nom|Form1|  
||Hauteur|6500|  
||Largeur|6500|  
|MS DataGrid|Nom|grdDisplay1|  
|TextBox|Nom|txtDisplay1|  
||Multiline|true|  
|Bouton de commande|Nom|cmdGetData|  
||Caption|Get Data|  
|Bouton de commande|Nom|cmdExamineData|  
||Caption|Examiner les données|  
|Bouton de commande|Nom|cmdEditData|  
||Caption| Modification des données|  
|Bouton de commande|Nom|cmdUpdateData|  
||Caption|Données mises à jour|