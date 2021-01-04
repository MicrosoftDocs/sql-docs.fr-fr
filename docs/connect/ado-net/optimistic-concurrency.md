---
title: Accès concurrentiel optimiste
description: Décrit les accès concurrentiels optimiste et pessimiste et la manière de tester les violations d'accès concurrentiel.
ms.date: 11/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 12894591f4ee7b1db5e514b7218337d92382842b
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051307"
---
# <a name="optimistic-concurrency"></a>Accès concurrentiel optimiste

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Dans un environnement multi-utilisateur, il existe deux modèles pour la mise à jour de données dans une base de données : l'accès simultané optimiste et l'accès simultané pessimiste. L'objet <xref:System.Data.DataSet> est conçu pour privilégier l'utilisation de l'accès simultané optimiste pour les activités longues, comme lors de la communication à distance de données ou de l'interaction avec ces dernières.  
  
L'accès simultané pessimiste implique le verrouillage de lignes à la source de données pour empêcher que d'autres utilisateurs modifient des données d'une manière affectant l'utilisateur actuel. Dans un modèle pessimiste, lorsqu'un utilisateur effectue une action entraînant l'application d'un verrou, les autres utilisateurs ne peuvent pas effectuer d'actions qui créeraient un conflit avec le verrou tant que le propriétaire de ce dernier ne l'a pas libéré. Ce modèle est principalement utilisé dans les environnements où les conflits relatifs aux données sont fréquents, de sorte que le coût de la protection des données par verrous est inférieur à celui de la restauration des transactions en cas de conflits d’accès simultané.  
  
Par conséquent, dans un modèle d'accès simultané pessimiste, un utilisateur qui met à jour une ligne crée un verrou. Jusqu'à ce que cet utilisateur ait terminé sa mise à jour et libéré le verrou, personne d'autre ne peut modifier cette ligne. C'est pourquoi il est préférable d'implémenter l'accès simultané pessimiste lorsque les temps de verrouillage sont courts, comme c'est le cas pour le traitement d'enregistrements par programme. L'accès simultané pessimiste ne constitue pas la solution la plus adaptée lorsque des utilisateurs interagissent avec les données, ce qui entraîne le verrouillage d'enregistrements pendant des laps de temps relativement longs.

> [!NOTE]
> Si vous devez mettre à jour plusieurs lignes en une même opération, la création d’une transaction constitue une option plus adaptée que l’utilisation du verrouillage pessimiste.

Au contraire, les utilisateurs qui ont recours à un accès simultané optimiste ne verrouillent pas une ligne lorsqu'ils la lisent. Lorsqu'un utilisateur souhaite mettre à jour une ligne, l'application doit déterminer si un autre utilisateur a modifié cette ligne depuis sa dernière lecture. L'accès simultané optimiste est généralement utilisé dans les environnements où les conflits relatifs aux données sont rares. L'accès simultané optimiste améliore les performances, dans la mesure où aucun verrouillage des enregistrements n'est requis et où le verrouillage d'enregistrements nécessite des ressources serveur supplémentaires. Il faut également savoir que la gestion des verrous d'enregistrements requiert une connexion permanente au serveur de base de données. Parce que ce n'est pas le cas dans un modèle d'accès simultané optimiste, les connexions au serveur sont disponibles pour traiter plus rapidement un nombre important de clients.

Dans un modèle d'accès simultané optimiste, une violation est réputée avoir eu lieu si, après réception par un utilisateur d'une valeur provenant de la base de données, un autre utilisateur modifie cette valeur avant que le premier n'ait tenté de le faire. La manière dont le serveur résout une violation de l'accès simultané est bien illustrée par l'exemple suivant.

Les tableaux ci-après suivent un exemple d'accès simultané optimiste.  
  
À 13h00, l'utilisateur 1 lit une ligne de la base de données contenant les valeurs suivantes :  
  
**CustID     LastName     FirstName**  
  
101          Smith             Bob  
  
|Nom de la colonne|Valeur d'origine|Valeur actuelle|Valeur dans la base de données|  
|-----------------|--------------------|-------------------|-----------------------|  
|CustID|101|101|101|  
|LastName|Smith|Smith|Smith|  
|FirstName|Bob|Bob|Bob|  
  
À 13h01, l'utilisateur 2 lit la même ligne.  
  
À 13h03, l’utilisateur 2 modifie la valeur de **FirstName** pour remplacer « Bob » par « Robert » et met à jour la base de données.  
  
|Nom de la colonne|Valeur d'origine|Valeur actuelle|Valeur dans la base de données|  
|-----------------|--------------------|-------------------|-----------------------|  
|CustID|101|101|101|  
|LastName|Smith|Smith|Smith|  
|FirstName|Bob|Robert|Bob|  
  
La mise à jour réussit car les valeurs de la base de données au moment de la mise à jour correspondent aux valeurs d'origine dont dispose l'utilisateur 2.  
  
À 13h05, l'utilisateur 1 modifie la valeur de prénom pour remplacer « Bob » par « James » et tente de mettre à jour la base de données.  
  
|Nom de la colonne|Valeur d'origine|Valeur actuelle|Valeur dans la base de données|  
|-----------------|--------------------|-------------------|-----------------------|  
|CustID|101|101|101|  
|LastName|Smith|Smith|Smith|  
|FirstName|Bob|James|Robert|  
  
À ce stade, l'utilisateur 1 est confronté à une violation d'accès simultané optimiste, car la valeur de la base de données (« Robert ») ne correspond plus à la valeur d'origine qu'attendait l'utilisateur 1 (« Bob »). La violation d'accès concurrentiel vous indique simplement que la mise à jour a échoué. Il convient maintenant de décider si les modifications apportées par l'utilisateur 2 devront être remplacées par celles de l'utilisateur 1 ou si ces dernières devront être annulées.

## <a name="testing-for-optimistic-concurrency-violations"></a>Recherche de violations d’accès concurrentiel optimiste

Il existe plusieurs techniques qui permettent de déceler la présence d'une violation d'accès simultané optimiste. L'une d'entre elles consiste à inclure une colonne horodateur dans la table.

Les bases de données proposent généralement une fonctionnalité d'horodatage qui peut être utilisée pour connaître la date et l'heure de la dernière mise à jour d'un enregistrement. En utilisant cette technique, une colonne horodateur est incluse dans la définition de la table. Chaque fois que l'enregistrement est mis à jour, l'horodatage est également mis à jour en fonction de la date et de l'heure actuelles.

Dans un test visant à déceler la présence de violations d'accès simultané optimiste, la colonne horodateur est retournée avec toute requête liée au contenu de la table. Lors d'une tentative de mise à jour, la valeur d'horodatage figurant dans la base de données est comparée à la valeur d'origine contenue dans la ligne modifiée. Si les valeurs correspondent, la modification est apportée et la colonne horodateur est mise à jour en fonction de la date et de l'heure actuelles afin de refléter la modification. Si elles ne correspondent pas, une violation d'accès simultané optimiste s'est produite.

Une autre technique de recherche des violations d'accès simultané optimiste consiste à vérifier que toutes les valeurs de colonne d'origine d'une ligne correspondent toujours à celles qui figurent dans la base de données. Par exemple, considérez la requête suivante :

```sql
SELECT Col1, Col2, Col3 FROM Table1  
```  
  
Pour savoir s’il y a eu violation d’accès concurrentiel optimiste pendant la mise à jour d’une ligne de **Table1**, vous pouvez émettre l’instruction UPDATE suivante :  
  
```sql
UPDATE Table1 Set Col1 = @NewCol1Value,  
              Set Col2 = @NewCol2Value,  
              Set Col3 = @NewCol3Value  
WHERE Col1 = @OldCol1Value AND  
      Col2 = @OldCol2Value AND  
      Col3 = @OldCol3Value  
```
Tant que les valeurs d'origine correspondent à celles qui figurent dans la base de données, la mise à jour est effectuée. Si une valeur a été modifiée, la mise à jour ne modifiera pas la ligne car la clause WHERE ne trouvera pas de correspondance.  
  
Notez qu'il est recommandé de toujours retourner une valeur de clé primaire unique dans votre requête. Sinon, l'instruction UPDATE précédente risque de mettre à jour plusieurs lignes, ce qui n'est pas nécessairement dans vos intentions.  
  
Si une colonne de votre source de données accepte les valeurs null, vous devrez peut-être étendre votre clause WHERE pour vérifier s'il existe une référence null dans votre table locale et sa correspondance dans votre source de données. Par exemple, l'instruction UPDATE suivante vérifie qu'une référence null dans la ligne locale correspond toujours à une référence null dans la source de données, ou si la valeur dans la ligne locale correspond toujours à celle de la source de données.  
  
```sql
UPDATE Table1 Set Col1 = @NewVal1  
  WHERE (@OldVal1 IS NULL AND Col1 IS NULL) OR Col1 = @OldVal1  
```  
  
Vous pouvez aussi choisir d'appliquer des critères moins restrictifs lorsque vous utilisez un modèle d'accès simultané optimiste. Par exemple, utiliser uniquement les colonnes de clé primaire dans la clause WHERE aboutit au remplacement des données, que les autres colonnes aient ou non subi une mise à jour depuis la dernière requête. Vous pouvez aussi appliquer une clause WHERE à certaines colonnes uniquement, ce qui aura pour effet de remplacer les données, sauf si des champs spécifiques ont été mis à jour depuis la dernière requête les concernant.

### <a name="the-dataadapterrowupdated-event"></a>Événement DataAdapter.RowUpdated

L’événement **RowUpdated** de l’objet <xref:System.Data.Common.DataAdapter> peut être utilisé avec les techniques décrites précédemment afin d’adresser une notification à votre application en cas de violation d’accès concurrentiel optimiste. **RowUpdated** intervient après chaque tentative de mise à jour d’une ligne **Modified** provenant d’un **DataSet**. Cela vous permet d'ajouter un code de gestion spécial, qui traitera les exceptions le cas échéant, ajoutera des informations d'erreur personnalisées, ajoutera une logique pour les nouvelles tentatives, etc.

L’objet <xref:System.Data.Common.RowUpdatedEventArgs> retourne une propriété **RecordsAffected** contenant le nombre de lignes concernées par une commande de mise à jour donnée pour une ligne modifiée dans une table. En configurant la commande de mise à jour de sorte qu’elle teste l’accès concurrentiel optimiste, la propriété **RecordsAffected** retourne la valeur 0 si une violation d’accès concurrentiel optimiste s’est produite, car aucun enregistrement n’a été mis à jour. Dans ce cas, une exception est levée.

L’événement **RowUpdated** vous permet de gérer ce cas de figure et d’éviter l’exception en définissant une valeur **RowUpdatedEventArgs.Status** appropriée, telle que **UpdateStatus.SkipCurrentRow**. Pour plus d’informations sur l’événement **RowUpdated**, consultez [Gestion des événements DataAdapter](handle-dataadapter-events.md).

Vous pouvez éventuellement définir **DataAdapter.ContinueUpdateOnError** sur **true** avant d’appeler **Update** et répondre aux informations d’erreur stockées dans la propriété **RowError** d’une ligne donnée quand **Update** a abouti. Pour plus d’informations, consultez [Informations d’erreur de ligne](/dotnet/framework/data/adonet/dataset-datatable-dataview/row-error-information).

## <a name="optimistic-concurrency-example"></a>Exemple d’accès concurrentiel optimiste

L’exemple qui suit définit **UpdateCommand** pour un **DataAdapter** à des fins de test de l’accès concurrentiel optimiste, puis utilise l’événement **RowUpdated** pour rechercher la présence de violations d'accès concurrentiel optimiste. En cas de violation d’accès concurrentiel optimiste, l’application définit le **RowError** de la ligne pour laquelle la mise à jour a été émise pour rendre compte de la violation d’accès concurrentiel optimiste.

Notez que les valeurs de paramètre passées à la clause WHERE de la commande UPDATE sont mappées aux valeurs **Original** de leur colonne.

[!code-csharp[SqlDataAdapter_Concurrency#1](~/../sqlclient/doc/samples/SqlDataAdapter_Concurrency.cs#1)]

## <a name="see-also"></a>Voir aussi

- [Récupération et modification de données dans ADO.NET](retrieving-modifying-data.md)
- [Mise à jour des sources de données avec les DataAdapter](update-data-sources-with-dataadapters.md)
- [Transactions et accès simultané](transactions-and-concurrency.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
