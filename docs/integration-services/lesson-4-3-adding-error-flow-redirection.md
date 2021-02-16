---
description: 'Leçon 4-3 : Ajouter une redirection de flux d’erreurs'
title: 'Étape 3 : Ajouter une redirection de flux d’erreurs | Microsoft Docs'
ms.custom: ''
ms.date: 01/07/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 5683a45d-9e73-4cd5-83ca-fae8b26b488c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ac63d922423bcbb9c0c7f513d34a5834b3f0c1b3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352018"
---
# <a name="lesson-4-3-add-error-flow-redirection"></a>Leçon 4-3 : Ajouter une redirection de flux d’erreurs

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Dans la tâche précédente, la transformation Lookup Currency Key ne peut pas générer de correspondance lorsqu’elle tente de traiter l’exemple de fichier plat endommagé qui a généré une erreur. Du fait que la transformation utilise les paramètres par défaut pour l'affichage en sortie des erreurs, toute erreur qui survient entraîne un échec de la transformation. Si la transformation échoue, le reste du package échoue également.  
  
Au lieu de permettre à la transformation d’échouer, vous pouvez configurer le composant afin qu’il redirige la ligne qui a échoué vers un autre chemin de traitement en utilisant la sortie d’erreur. L’utilisation d’un autre chemin de traitement des erreurs offre deux possibilités. Par exemple, vous pouvez nettoyer les données, puis retraiter la ligne qui a échoué. Ou vous pouvez enregistrer la ligne qui a échoué avec ses informations d’erreur pour la vérifier et la retraiter ultérieurement.  
  
Au cours de cette tâche, vous allez configurer la transformation Lookup Currency Key afin qu’elle redirige toutes les lignes qui échouent vers la sortie d’erreur. Dans la branche d’erreur du flux de données, [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] écrit ces lignes dans un fichier.  
  
Par défaut, les deux colonnes supplémentaires d’une sortie d’erreur [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], **ErrorCode** et **ErrorColumn**, contiennent uniquement un code d’erreur numérique et l’ID de la colonne dans laquelle l’erreur s’est produite. Au cours de cette tâche, avant que le package n’écrive les lignes qui ont échoué dans le fichier, vous utilisez un composant Script pour accéder à l’API [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] afin d’obtenir la description de l’erreur.  
  
## <a name="configure-an-error-output"></a>Configurer une sortie d’erreur  
  
1.  Dans la **Boîte à outils SSIS**, développez **Commun**, puis faites glisser **Composant Script** dans l'aire de conception de l'onglet **Flux de données** . Placez le composant **Script** à droite de la transformation **Lookup Currency Key** .  
  
2.  Dans la boîte de dialogue **Sélectionner le type de composant de script**, sélectionnez **Transformation**, puis **OK**.  
  
3.  Pour connecter les deux composants, sélectionnez la transformation **Lookup Currency Key** et faites glisser sa flèche rouge vers la nouvelle transformation **Script**.  
  
    La flèche rouge représente la sortie d'erreur de la transformation **Lookup Currency Key** . En utilisant la flèche rouge pour connecter la transformation avec le composant Script, vous redirigez toutes les erreurs de traitement vers le composant Script, lequel traite ensuite les erreurs et les transmet à la de destination.  
  
4.  Dans la boîte de dialogue **Configurer la sortie d’erreur**, dans la colonne **Erreur**, sélectionnez **Réacheminer la ligne**, puis **OK**.  
  
5.  Dans l’aire de conception **Flux de données**, sélectionnez le nom **Composant Script** dans le nouveau **Composant Script**, puis remplacez ce nom par **Get Error Description**.  
  
6.  Double-cliquez sur la transformation **Get Error Description** .  
  
7.  Dans la boîte de dialogue **Éditeur de transformation de script** , dans la page **Colonnes d'entrée** , sélectionnez la colonne **ErrorCode** .  
  
8.  Dans la page **Entrées et sorties**, développez **Sortie 0**, sélectionnez **Colonnes de sortie**, puis **Ajouter une colonne**.  
  
9. Dans la propriété **Name**, entrez *ErrorDescription* et attribuez à la propriété **DataType** la valeur **Unicode string [DT_WSTR]**.  
  
10. Dans la page **Script**, vérifiez que la propriété **LocaleID** a la valeur **Anglais (États-Unis)** .
  
11. Sélectionnez **Modifier le script** pour ouvrir [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] Tools for Applications (VSTA). Dans la méthode **Input0_ProcessInputRow**, entrez ou collez le code suivant :  
  
    [Visual Basic]  
  
    ```vb  
    Row.ErrorDescription =   
      Me.ComponentMetaData.GetErrorDescription(Row.ErrorCode)  
    ```  
  
    [Visual C#]  
  
    ```cs
    Row.ErrorDescription = this.ComponentMetaData.GetErrorDescription(Row.ErrorCode);  
    ```  
  
    La sous-routine terminée doit ressembler au code suivant :  
  
    [Visual Basic]  
  
    ```vb
    Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)  
  
      Row.ErrorDescription =   
        Me.ComponentMetaData.GetErrorDescription(Row.ErrorCode)  
  
    End Sub  
    ```  
  
    [Visual C#]  
  
    ```cs
    public override void Input0_ProcessInputRow(Input0Buffer Row)  
        {  
  
            Row.ErrorDescription = this.ComponentMetaData.GetErrorDescription(Row.ErrorCode);  
  
        }  
    ```  
  
12. Dans le menu **Générer**, sélectionnez **Générer la solution** pour créer le script et enregistrer vos modifications, puis fermez VSTA.  
  
13. Sélectionnez **OK** pour fermer la boîte de dialogue **Éditeur de transformation de script**.  
  
## <a name="go-to-next-task"></a>Passer à la tâche suivante
[Étape 4 : Ajouter une destination de fichier plat](../integration-services/lesson-4-4-adding-a-flat-file-destination.md)  
  
  
  
