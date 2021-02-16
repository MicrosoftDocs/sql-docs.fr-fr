---
description: Fournir une requête de source OData au moment de l’exécution
title: Fournir une requête de source OData au moment de l’exécution | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: bcbba7f4-6e5d-46e6-a73a-3f17d3ff376a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3cddac6ff27883aac6840f9ce3c04ba4441d1e71
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343242"
---
# <a name="provide-an-odata-source-query-at-runtime"></a>Fournir une requête de source OData au moment de l’exécution

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


 Vous pouvez modifier la requête de source OData au moment de l’exécution en ajoutant une *expression* à la propriété **[OData Source].[Query]** de la tâche de flux de données.  
  
 Les colonnes retournées doivent être identiques aux colonnes retournées au moment du design ; dans le cas contraire, vous obtenez une erreur à l’exécution du package. Veillez à spécifier les mêmes colonnes (dans le même ordre) lorsque vous utilisez l'option de requête $select. Une alternative plus sûre à l’utilisation de l’option $select est de désélectionner les colonnes que vous ne souhaitez pas utiliser directement de l’interface utilisateur du composant source.  
  
 Il existe d'autres méthodes de définition dynamique de la valeur de la requête à l'exécution. Voici quelques-unes des méthodes les plus courantes.  
  
## <a name="provide-the-query-as-a-parameter"></a>Fournir la requête en tant que paramètre  
 La procédure suivante montre comment fournir la requête utilisée par un composant source OData en tant que paramètre du package.  
  
1.  Cliquez avec le bouton droit sur **Tâche de flux de données** et sélectionnez l’option **Paramétrer...** .  
  
2.  Dans la boîte de dialogue **Paramétrer**, sélectionnez **[\<Name of the OData Source Component>].[Query]** pour **Propriété**.  
  
3.  Vous pouvez soit **créer un paramètre** , soit **utiliser un paramètre existant**.  
  
4.  Si vous sélectionnez **Créer un paramètre** :  
  
    1.  Entrez un **nom** et une **description** pour le paramètre.  
  
    2.  Spécifiez la **valeur** par défaut du paramètre.  
  
    3.  Spécifiez l’ **étendue** (**package** ou **projet**) du paramètre.  
  
    4.  Spécifiez si le paramètre est **obligatoire** ou non.  
  
5.  Cliquez sur **OK** pour fermer la boîte de dialogue.  
  
## <a name="provide-the-query-with-an-expression"></a>Fournir la requête avec une expression
 Cette méthode est utile lorsque vous souhaitez construire dynamiquement la chaîne de requête au moment de l’exécution.
  
1.  Sélectionnez la **tâche de flux de données** qui contient votre **source OData**.  
  
2.  La fenêtre **Propriétés** met en surbrillance la propriété **Expressions** .  
  
3.  Cliquez sur les points de suspension (...) pour ouvrir l’**Éditeur d’expressions de la propriété**.  
  
4.  Sélectionnez la propriété **[OData Source].[Query]** .  
  
5.  Cliquez sur les points de suspension (...) pour **Expression**.  
  
6.  Entrez l’ **expression**.  
  
7.  Cliquez sur **OK**.  
  
> [!NOTE]  
> En utilisant cette approche, vous devez vous assurer que les valeurs définies sont codées correctement pour l’URL. Lorsque vous recevez des valeurs de l'entrée utilisateur (par exemple, lorsque vous définissez des valeurs d'option de requête individuelles), vous devez vous assurer que les valeurs sont validées pour éviter des attaques potentielles par injection SQL.  
  
  
