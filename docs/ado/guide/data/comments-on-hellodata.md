---
description: Commentaires sur HelloData
title: Commentaires sur HelloData | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- hellodata sample application [ADO]
ms.assetid: a2831d77-7040-4b73-bbae-fe0bf78107ed
author: rothja
ms.author: jroth
ms.openlocfilehash: e0510d75c69caadd52a595d58900662cc8ef7ad4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037709"
---
# <a name="comments-on-hellodata"></a>Commentaires sur HelloData
L’application HelloData passe en revue les opérations de base d’une application ADO type : obtention, examen, modification et mise à jour des données. Quand vous démarrez l’application, cliquez sur le premier bouton, puis sur **récupérer des données**. La sous-routine **GetData** est exécutée.  
  
## <a name="getdata"></a>GetData  
 **GetData** place une chaîne de connexion valide dans une variable au niveau du module, *m_sConnStr*. Pour plus d’informations sur les chaînes de connexion, consultez [création de la chaîne de connexion](./creating-a-connection-string.md).  
  
 Assignez un gestionnaire d’erreurs à l’aide d’une Visual Basic instruction **OnError** . Pour plus d’informations sur la gestion des erreurs dans ADO, consultez [gestion des erreurs](./error-handling.md). Un nouvel objet de **connexion** est créé, et la propriété **CursorLocation** est définie sur **AdUseClient** , car l’exemple HelloData crée un *jeu d’enregistrements déconnecté*. Cela signifie que, dès que les données ont été extraites de la source de données, la connexion physique avec la source de données est interrompue, mais vous pouvez continuer à utiliser les données mises en cache localement dans votre objet **Recordset** .  
  
 Une fois la connexion ouverte, assignez une chaîne SQL à une variable (sSQL). Créez ensuite une instance d’un nouvel objet **Recordset** , `m_oRecordset1` . Dans la ligne de code suivante, ouvrez le **Recordset** sur la **connexion** existante, en transmettant en `sSQL` tant que source du **Recordset**. Vous aidez ADO à déterminer que la chaîne SQL que vous avez passée comme source pour le **Recordset** est une définition textuelle d’une commande en passant **adCmdText** dans le dernier argument à la méthode **Open Recordset** . Cette ligne définit également le **LockType** et le **CursorType** associés à l’ensemble **d’enregistrements**.  
  
 La ligne de code suivante affecte à la propriété **MarshalOptions** la valeur **adMarshalModifiedOnly**. **MarshalOptions** indique les enregistrements qui doivent être marshalés vers la couche intermédiaire (ou serveur Web). Pour plus d’informations sur le marshaling, consultez la documentation COM. Lorsque vous utilisez **adMarshalModifiedOnly** avec un curseur côté client ([CursorLocation](../../reference/ado-api/cursorlocation-property-ado.md)  =  **adUseClient**), seuls les enregistrements qui ont été modifiés sur le client sont réécrits dans la couche intermédiaire. La définition de **MarshalOptions** sur **adMarshalModifiedOnly** peut améliorer les performances, car moins de lignes sont marshalées.  
  
 Ensuite, déconnectez l’ensemble **d’enregistrements** en affectant à sa propriété **ActiveConnection** la valeur **Nothing**. Pour plus d’informations, consultez la section « déconnexion et reconnexion du Recordset » dans [mise à jour et persistance des données](./updating-and-persisting-data.md).  
  
 Fermez la connexion à la source de données et détruisez l’objet de **connexion** existant. Cela libère les ressources qu’il a consommées.  
  
 La dernière étape consiste à définir le **Recordset** comme **source** de données pour le contrôle Microsoft DataGrid sur le formulaire afin que vous puissiez facilement afficher les données du **jeu d’enregistrements** sur le formulaire.  
  
 Cliquez sur le deuxième bouton, **examiner les données**. La sous-routine **ExamineData** s’exécute.  
  
## <a name="examinedata"></a>ExamineData  
 ExamineData utilise différentes méthodes et propriétés de l’objet **Recordset** pour afficher des informations sur les données contenues dans le **jeu d’enregistrements**. Il signale le nombre d’enregistrements à l’aide de la propriété **RecordCount** . Elle parcourt le **Recordset** et imprime la valeur de la propriété **AbsolutePosition** dans la zone de texte d’affichage du formulaire. En outre, dans la boucle, la valeur de la propriété **Bookmark** pour le troisième enregistrement est placée dans une variable Variant, *vBookmark*, pour une utilisation ultérieure.  
  
 La routine accède directement au troisième enregistrement à l’aide de la variable de signet qu’elle a stockée précédemment. La routine appelle la sous-routine **WalkFields** , qui parcourt la collection **Fields** du **Recordset** et affiche des détails sur chaque **champ** de la collection.  
  
 Enfin, **ExamineData** utilise la propriété **Filter** de l' **objet Recordset** pour filtrer uniquement les enregistrements dont la valeur **RéfCatégorie** est égale à 2. Le résultat de l’application de ce filtre est immédiatement visible dans la grille d’affichage du formulaire.  
  
 Pour plus d’informations sur les fonctionnalités présentées dans la sous-routine **ExamineData** , consultez [examen des données](./examining-data.md).  
  
 Cliquez ensuite sur le troisième bouton, puis sur **modifier les données**. La sous-routine **EditData** est exécutée.  
  
## <a name="editdata"></a>EditData  
 Lorsque le code entre dans la sous-routine **EditData** , le **jeu d’enregistrements** est toujours filtré sur **CategoryID** égal à 2, de sorte que seuls les éléments qui répondent aux critères de filtre sont visibles. Il effectue d’abord une boucle sur le **Recordset** et augmente le prix de chaque élément visible dans le **jeu d’enregistrements** de 10 pour cent. La valeur du champ **Price** est modifiée en affectant à la propriété **value** de ce champ une valeur égale à New, valid amount.  
  
 N’oubliez pas que le **jeu d’enregistrements** est déconnecté de la source de données. Les modifications apportées à **EditData** sont effectuées uniquement dans la copie mise en cache localement des données. Pour plus d’informations, consultez [modification des données](./editing-data.md).  
  
 Les modifications ne sont pas apportées à la source de données tant que vous n’avez pas cliqué sur le quatrième bouton, **mettre à jour les données**. La sous-routine **UpdateData** est exécutée.  
  
## <a name="updatedata"></a>UpdateData  
 UpdateData supprime d’abord le filtre qui a été appliqué au **Recordset**. Le code supprime et réinitialise `m_oRecordset1` comme source de la **source** de contrôle Microsoft Bound sur le formulaire afin que le **jeu d’enregistrements** non filtré apparaisse dans la grille.  
  
 Le code vérifie ensuite si vous pouvez revenir en arrière dans le **Recordset** à l’aide de la méthode **supports** avec l’argument **adMovePrevious** .  
  
 La routine se déplace vers le premier enregistrement à l’aide de la méthode **MoveFirst** et affiche les valeurs d’origine et actuelles du champ, à l’aide des propriétés **OriginalValue** et **value** de l’objet **Field** . Ces propriétés, ainsi que la propriété **UnderlyingValue** (qui n’est pas utilisée ici), sont abordées dans la rubrique [mise à jour et persistance des données](./updating-and-persisting-data.md).  
  
 Ensuite, un nouvel objet de **connexion** est créé et utilisé pour rétablir une connexion à la source de données. Vous reconnectez le **Recordset** à la source de données en définissant la nouvelle **connexion** en tant que **ActiveConnection** du **Recordset**. Pour envoyer les mises à jour au serveur, le code appelle **UpdateBatch** sur le **Recordset**.  
  
 Si la mise à jour par lot est réussie, une variable d’indicateur de niveau module, `m_flgPriceUpdated` , a la valeur true. Cela vous rappellera ultérieurement de nettoyer toutes les modifications apportées à la base de données.  
  
 Enfin, le code revient au premier enregistrement dans le **jeu d’enregistrements** et affiche les valeurs d’origine et actuelles. Les valeurs sont les mêmes après l’appel à **UpdateBatch**.  
  
 Pour plus d’informations sur la mise à jour des données, notamment sur la procédure à suivre pour modifier les données du serveur pendant la déconnexion de votre **jeu d’enregistrements** , consultez [mise à jour et persistance des données](./updating-and-persisting-data.md).  
  
## <a name="form_unload"></a>Form_Unload  
 La sous-routine **Form_Unload** est importante pour plusieurs raisons. Tout d’abord, étant donné qu’il s’agit d’un exemple d’application, Form_Unload nettoie les modifications apportées à la base de données avant la fermeture de l’application. Deuxièmement, le code montre comment une commande peut être exécutée directement à partir d’un objet de **connexion** ouvert à l’aide de la méthode **Execute** . Enfin, il montre un exemple d’exécution d’une requête de renvoi non-ligne (une requête UPDATE) sur la source de données.