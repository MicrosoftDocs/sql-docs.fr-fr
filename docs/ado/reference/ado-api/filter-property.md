---
description: Filter, propriété
title: Filter, propriété | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Filter
helpviewer_keywords:
- Filter property
ms.assetid: 80263a7a-5d21-45d1-84fc-34b7a9be4c22
author: rothja
ms.author: jroth
ms.openlocfilehash: 7366ba67d36049f1c5df701bc633d76f56bb9af5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167324"
---
# <a name="filter-property"></a>Filter, propriété
Indique un filtre pour les données d’un [jeu d’enregistrements](./recordset-object-ado.md).  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour

Définit ou retourne une valeur de **type Variant** , qui peut contenir l’un des éléments suivants :  
  
-   **Chaîne de critères :** Chaîne constituée d’une ou plusieurs clauses individuelles concaténées avec les opérateurs **and** ou **or.**  
  
-   **Tableau de signets :** Tableau de valeurs de signets uniques qui pointent vers des enregistrements dans l’objet **Recordset** .  
  
-   Valeur [FilterGroupEnum](./filtergroupenum.md) .  
  
## <a name="remarks"></a>Notes

Utilisez la propriété **Filter pour filtrer** de manière sélective les enregistrements d’un objet **Recordset** . Le **Recordset** filtré devient le curseur actuel. Les autres propriétés qui retournent des valeurs basées sur le **curseur** actuel sont affectées, telles que [ABSOLUTEPOSITION Property (ADO)](./absoluteposition-property-ado.md), [AbsolutePage Property (ADO)](./absolutepage-property-ado.md), [RecordCount Property (ADO)](./recordcount-property-ado.md)et [PageCount Property (ADO)](./pagecount-property-ado.md). La définition de la propriété **Filter** sur une nouvelle valeur spécifique déplace l’enregistrement actif vers le premier enregistrement qui satisfait la nouvelle valeur.
  
La chaîne de critères est composée de clauses au format *fieldName-Operator-value* (par exemple, `"LastName = 'Smith'"` ). Vous pouvez créer des clauses composées en concaténant des clauses individuelles avec **et** (par exemple, `"LastName = 'Smith' AND FirstName = 'John'"` ) ou **ou** (par exemple, `"LastName = 'Smith' OR LastName = 'Jones'"` ). Pour les chaînes de critères, utilisez les instructions suivantes :

-   *FieldName* doit être un nom de champ valide du **Recordset**. Si le nom du champ contient des espaces, vous devez placer le nom entre crochets.  
  
-   L’opérateur doit être l’un des suivants : \<, > , \<=, > =,  <>, = ou **Like**.  
  
-   La valeur est la valeur avec laquelle vous allez comparer les valeurs de champ (par exemple, « Smith », #8/24/95 #, 12,345 ou $50,00). Utilisez des guillemets simples avec des chaînes et des signes dièse (#) avec des dates. Pour les nombres, vous pouvez utiliser des points décimaux, des signes dollar et une notation scientifique. Si l’opérateur est **comme**, la valeur peut utiliser des caractères génériques. Uniquement l’astérisque (*) et le signe de pourcentage (%) les caractères génériques sont autorisés et doivent être le dernier caractère de la chaîne. Value cannot be null.  
  
> [!NOTE]
>  Pour inclure des guillemets simples (') dans la valeur de filtre, utilisez deux guillemets simples pour en représenter un. Par exemple, pour filtrer sur O’Malley, la chaîne de critères doit être `"col1 = 'O''Malley'"` . Pour inclure des guillemets simples à la fois au début et à la fin de la valeur de filtre, mettez la chaîne entre signes dièse (#). Par exemple, pour filtrer sur « 1 », la chaîne de critères doit être `"col1 = #'1'#"` .  
  
-   Il n’existe aucune priorité entre AND et OR. Les clauses peuvent être regroupées entre parenthèses. Toutefois, vous ne pouvez pas regrouper les clauses jointes par ou, puis joindre le groupe à une autre clause avec un et, comme dans l’extrait de code suivant :  
 `(LastName = 'Smith' OR LastName = 'Jones') AND FirstName = 'John'`  
  
-   Au lieu de cela, vous construisez ce filtre en tant que  
 `(LastName = 'Smith' AND FirstName = 'John') OR (LastName = 'Jones' AND FirstName = 'John')`  
  
-   Dans une clause **Like** , vous pouvez utiliser un caractère générique au début et à la fin du modèle. Par exemple, vous pouvez utiliser `LastName Like '*mit*'`. Ou avec **Like** , vous pouvez utiliser un caractère générique uniquement à la fin du modèle. Par exemple : `LastName Like 'Smit*'`.  
  
 Les constantes de filtre facilitent la résolution des conflits d’enregistrements individuels pendant le mode de mise à jour par lot en vous permettant d’afficher, par exemple, uniquement les enregistrements qui ont été affectés lors du dernier appel de la méthode [UpdateBatch](./updatebatch-method.md) .  
  
La définition de la propriété de **filtre** peut échouer en raison d’un conflit avec les données sous-jacentes. Par exemple, cet échec peut se produire lorsqu’un enregistrement a déjà été supprimé par un autre utilisateur. Dans ce cas, le fournisseur renvoie des avertissements à la collection d' [Erreurs (ADO)](./errors-collection-ado.md) , mais n’interrompt pas l’exécution du programme. Une erreur au moment de l’exécution se produit uniquement en cas de conflit sur tous les enregistrements demandés. Utilisez la propriété [Status Property (ADO Recordset)](./status-property-ado-recordset.md) pour localiser les enregistrements avec des conflits.  
  
La définition de la propriété **Filter** sur une chaîne de longueur nulle ("") a le même effet que l’utilisation de la constante **adFilterNone** .
  
Chaque fois que la propriété **Filter** est définie, la position actuelle de l’enregistrement est déplacée vers le premier enregistrement du **jeu** d’enregistrements filtré. De même, lorsque la propriété **Filter** est désactivée, la position de l’enregistrement actif est déplacée vers le premier enregistrement du **jeu d’enregistrements**.

Supposez qu’un **jeu d’enregistrements** est filtré en fonction d’un champ d’un type Variant, tel que le type sql_variant. Une erreur (DISP_E_TYPEMISMATCH ou 80020005) se produit lorsque les sous-types des valeurs de champ et de filtre utilisées dans la chaîne de critères ne correspondent pas. Par exemple, supposons que :

- Un objet **Recordset** (RS) contient une colonne (C) du type sql_variant.
- Une valeur de 1 du type I4 a été affectée à un champ de cette colonne. La chaîne de critères est définie `rs.Filter = "C='A'"` sur sur le champ.

Cette configuration génère l’erreur au moment de l’exécution. Toutefois, `rs.Filter = "C=2"` appliquée au même champ, aucune erreur n’est générée. Et le champ est exclu du jeu d’enregistrements actuel.

Pour obtenir une explication des valeurs de signet à partir desquelles vous pouvez créer un tableau à utiliser avec la propriété de filtre, consultez la propriété [Bookmark, propriété (ADO)](./bookmark-property-ado.md) .

Seuls les filtres sous la forme de chaînes de critères affectent le contenu d’un **jeu d’enregistrements** persistant. Un exemple de chaîne de critères est `OrderDate > '12/31/1999'` . Les filtres créés avec un tableau de signets, ou à l’aide d’une valeur de **FilterGroupEnum**, n’affectent pas le contenu de l' **objet Recordset** persistant. Ces règles s’appliquent aux jeux d’enregistrements créés à l’aide de curseurs côté client ou côté serveur.
  
> [!NOTE]
>  Lorsque vous appliquez l’indicateur adFilterPendingRecords à un **Recordset** filtré et modifié dans le mode de mise à jour par lot, le **jeu d’enregistrements** résultant est vide si le filtrage était basé sur le champ clé d’une table à clé unique et que la modification a été apportée sur les valeurs de champ clés. Le **jeu d’enregistrements** résultant ne sera pas vide si l’une des affirmations suivantes est vraie :  
  
-   Le filtrage était basé sur des champs non-clés dans une table à clé unique.  
  
-   Le filtrage était basé sur tous les champs d’une table à clés multiples.  
  
-   Des modifications ont été apportées sur les champs non-clés d’une table à clé unique.  
  
-   Des modifications ont été apportées sur les champs d’une table à clé multiple.  
  
Le tableau suivant récapitule les effets de **adFilterPendingRecords** dans différentes combinaisons de filtrage et de modifications. La colonne de gauche affiche les modifications possibles. Des modifications peuvent être apportées sur l’un des champs non-clés, sur le champ clé d’une table à clé unique ou sur n’importe quel champ clé dans une table à clé multiple. La ligne du haut affiche le critère de filtrage. Le filtrage peut être basé sur n’importe quel champ non indexé, sur le champ clé d’une table à clé unique ou sur l’un des champs clés d’une table à clé multiple. Les cellules qui se croisent affichent les résultats. Un **+** signe plus signifie que l’application de **adFilterPendingRecords** produit un **jeu d’enregistrements** non vide. Un **-** signe moins signifie un **jeu d’enregistrements** vide.  
  
|Combinaisons|Non-clés|Clé unique|Clés multiples|
|-|--------------|----------------|-------------------|
|**Non-clés**|+|+|+|
|**Clé unique**|+|-|N/A|
|**Clés multiples**|+|N/A|+|
|||||
  
## <a name="applies-to"></a>S'applique à

[Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi

[Filter et RecordCount, exemple de propriétés (VB)](./filter-and-recordcount-properties-example-vb.md) 
 [Filter et RecordCount, exemples de propriétés (VC + +)](./filter-and-recordcount-properties-example-vc.md) 
 [Clear, méthode (ADO)](./clear-method-ado.md) 
 [Property-Dynamic d’optimisation (ADO)](./optimize-property-dynamic-ado.md)