---
description: Recordset, objet (ADO)
title: Recordset, objet (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset
helpviewer_keywords:
- Recordset object [ADO]
ms.assetid: ede1415f-c3df-4cc5-a05b-2576b2b84b60
author: rothja
ms.author: jroth
ms.openlocfilehash: 7b4e7514d0ca9b9fd94c634c1e6a17dcdb42d976
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051780"
---
# <a name="recordset-object-ado"></a>Recordset, objet (ADO)
Représente l’ensemble des enregistrements d’une table de base ou les résultats d’une commande exécutée. À tout moment, l’objet **Recordset** fait référence à un seul enregistrement dans le jeu en tant qu’enregistrement actif.  
  
## <a name="remarks"></a>Notes  
 Vous utilisez les objets **Recordset** pour manipuler les données d’un fournisseur. Lorsque vous utilisez ADO, vous manipulez des données presque entièrement à l’aide **d’objets Recordset** . Tous les objets **Recordset** se composent d’enregistrements (lignes) et de champs (colonnes). Selon les fonctionnalités prises en charge par le fournisseur, certaines méthodes ou propriétés du **Recordset** peuvent ne pas être disponibles.  
  
 ADODB. Recordset est le ProgID qui doit être utilisé pour créer un objet **Recordset** . Applications existantes qui font référence à l’ADOR obsolète. Le ProgID du Recordset continue à fonctionner sans recompilation, mais le nouveau développement doit faire référence à ADODB. Recordset.  
  
 Il existe quatre types de curseurs différents définis dans ADO :  
  
-   **Curseur dynamique** Vous permet d’afficher des ajouts, des modifications et des suppressions par d’autres utilisateurs ; autorise tous les types de déplacement via le **Recordset** qui ne repose pas sur les signets ; et autorise les signets si le fournisseur les prend en charge.  
  
-   **Curseur de jeu de clés** Se comporte comme un curseur dynamique, à ceci près qu’il vous empêche de voir les enregistrements ajoutés par d’autres utilisateurs et empêche l’accès aux enregistrements supprimés par d’autres utilisateurs. Les modifications apportées aux données par d’autres utilisateurs sont toujours visibles. Il prend toujours en charge les signets et, par conséquent, autorise tous les types de déplacement via le **Recordset**.  
  
-   **Curseur statique** Fournit une copie statique d’un ensemble d’enregistrements que vous pouvez utiliser pour rechercher des données ou générer des rapports. autorise toujours les signets et, par conséquent, autorise tous les types de déplacement via le **Recordset**. Les ajouts, modifications ou suppressions effectués par d’autres utilisateurs ne sont pas visibles. Il s’agit du seul type de curseur autorisé lorsque vous ouvrez un objet **Recordset** côté client.  
  
-   **Curseur avant uniquement** Vous permet de faire défiler le **Recordset** uniquement vers l’avant. Les ajouts, modifications ou suppressions effectués par d’autres utilisateurs ne sont pas visibles. Cela améliore les performances dans les situations où vous devez effectuer une seule passe dans un **jeu d’enregistrements**.  
  
 Définissez la propriété [CursorType](./cursortype-property-ado.md) avant d’ouvrir le **jeu d’enregistrements** pour choisir le type de curseur, ou transmettez un argument *CursorType* avec la méthode [Open](./open-method-ado-recordset.md) . Certains fournisseurs ne prennent pas en charge tous les types de curseurs. Consultez la documentation du fournisseur. Si vous ne spécifiez pas de type de curseur, ADO ouvre un curseur avant uniquement par défaut.  
  
 Si la propriété [CursorLocation](./cursorlocation-property-ado.md) a la valeur **adUseClient** pour ouvrir un **jeu d’enregistrements**, la propriété **UnderlyingValue** sur les objets [Field](./field-object.md) n’est pas disponible dans l’objet **Recordset** retourné. Lorsqu’ils sont utilisés avec certains fournisseurs (tels que le fournisseur Microsoft ODBC pour OLE DB conjointement avec Microsoft SQL Server), vous pouvez créer des objets **Recordset** indépendamment d’un objet de [connexion](./connection-object-ado.md) défini précédemment en passant une chaîne de connexion avec la méthode **Open** . ADO crée toujours un objet de [connexion](./connection-object-ado.md) , mais il n’affecte pas cet objet à une variable objet. Toutefois, si vous ouvrez plusieurs objets **Recordset** sur la même connexion, vous devez créer et ouvrir explicitement un objet de **connexion** ; Cela affecte l’objet de **connexion** à une variable objet. Si vous n’utilisez pas cette variable objet lors de l’ouverture de vos objets **Recordset** , ADO crée un objet **Connection** pour chaque nouvel ensemble **d’enregistrements**, même si vous transmettez la même chaîne de connexion.  
  
 Vous pouvez créer autant **d’objets Recordset** que nécessaire.  
  
 Lorsque vous ouvrez un **jeu d’enregistrements**, l’enregistrement actif est positionné sur le premier enregistrement (le cas échéant) et les propriétés [BOF](./bof-eof-properties-ado.md) et [EOF](./bof-eof-properties-ado.md) ont la valeur **false**. S’il n’y a aucun enregistrement, les paramètres de propriété **BOF** et **EOF** ont la **valeur true**.  
  
 Vous pouvez utiliser les méthodes [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md), **MoveLast**, **MoveNext** et **MovePrevious** . méthode [Move](./move-method-ado.md) ; et les propriétés [AbsolutePosition](./absoluteposition-property-ado.md), [AbsolutePage](./absolutepage-property-ado.md)et [Filter](./filter-property.md) pour repositionner l’enregistrement actif, en supposant que le fournisseur prend en charge les fonctionnalités pertinentes. Les objets **Recordset** avant uniquement prennent uniquement en charge la méthode [MoveNext](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) . Lorsque vous utilisez les méthodes **Move** pour visiter chaque enregistrement (ou énumérer le **Recordset**), vous pouvez utiliser les propriétés **BOF** et **EOF** pour déterminer si vous avez dépassé le début ou la fin du **Recordset**.  
  
 Avant d’utiliser une fonctionnalité d’un objet **Recordset** , vous devez appeler la méthode **supports** sur l’objet pour vérifier que la fonctionnalité est prise en charge ou disponible. Vous ne devez pas utiliser la fonctionnalité quand la méthode **prend en charge** retourne la valeur false. Par exemple, vous pouvez utiliser la méthode **MovePrevious** uniquement si `Recordset.Supports(adMovePrevious)` retourne la **valeur true**. Dans le cas contraire, vous obtiendrez une erreur, car l’objet **Recordset** a peut-être été fermé et les fonctionnalités rendues indisponibles sur l’instance. Si une fonctionnalité qui vous intéresse n’est pas prise en charge, **prend** également la valeur false. Dans ce cas, vous devez éviter d’appeler la propriété ou la méthode correspondante sur l’objet **Recordset** .  
  
 Les objets **Recordset** peuvent prendre en charge deux types de mise à jour : immédiat et par lot. Dans la mise à jour immédiate, toutes les modifications apportées aux données sont écrites immédiatement dans la source de données sous-jacente une fois que vous avez appelé la méthode [Update](./update-method.md) . Vous pouvez également passer des tableaux de valeurs en tant que paramètres avec les méthodes [AddNew](./addnew-method-ado.md) et **Update** et mettre à jour plusieurs champs simultanément dans un enregistrement.  
  
 Si un fournisseur prend en charge la mise à jour par lot, vous pouvez faire en sorte que le cache du fournisseur soit modifié en plusieurs enregistrements, puis les transmette en un seul appel à la base de données avec la méthode [UpdateBatch](./updatebatch-method.md) . Cela s’applique aux modifications apportées avec les méthodes **AddNew**, **Update** et [Delete](./delete-method-ado-recordset.md) . Après avoir appelé la méthode **UpdateBatch** , vous pouvez utiliser la propriété [Status](./status-property-ado-recordset.md) pour rechercher les éventuels conflits de données afin de les résoudre.  
  
> [!NOTE]
>  Pour exécuter une requête sans utiliser d’objet [Command](./command-object-ado.md) , transmettez une chaîne de requête à la méthode **Open** d’un objet **Recordset** . Toutefois, un objet **Command** est requis lorsque vous souhaitez conserver le texte de la commande et le réexécuter, ou utiliser des paramètres de requête.  
  
 La propriété [mode](./mode-property-ado.md) régit les autorisations d’accès.  
  
 La collection **Fields** est le membre par défaut de l’objet **Recordset** . Par conséquent, les deux instructions de code suivantes sont équivalentes.  
  
```  
Debug.Print objRs.Fields.Item(0)  ' Both statements print   
Debug.Print objRs(0)              '  the Value of Item(0).  
```  
  
 Lorsqu’un objet **Recordset** est transmis entre les processus, seules les valeurs de l' **ensemble de lignes** sont marshalées, et les propriétés de l’objet **Recordset** sont ignorées. Lors du démarshaling, l' **ensemble de lignes** est décompressé dans un objet **Recordset** nouvellement créé, ce qui affecte également aux propriétés les valeurs par défaut.  
  
 L’objet **Recordset** est sécurisé pour l’écriture de scripts.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de l’objet Recordset](./recordset-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Connection, objet (ADO)](./connection-object-ado.md)   
 [Fields, collection (ADO)](./fields-collection-ado.md)   
 [Properties, collection (ADO)](./properties-collection-ado.md)   
 [Annexe A : Fournisseurs](../../guide/appendixes/appendix-a-providers.md)