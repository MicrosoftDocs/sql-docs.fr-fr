---
description: Refresh, méthode (ADO)
title: Refresh, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Collection::Refresh
- Parameters::Refresh
- Properties::Refresh
helpviewer_keywords:
- Refresh method [ADO]
ms.assetid: 089b7ca7-684f-4259-8032-5bd1ecc54426
author: rothja
ms.author: jroth
ms.openlocfilehash: ea82fa376046477277398012236559b08a6fe1a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051680"
---
# <a name="refresh-method-ado"></a>Refresh, méthode (ADO)
Met à jour les objets d’une collection pour refléter les objets disponibles et spécifiques au fournisseur.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
collection.Refresh  
```  
  
## <a name="remarks"></a>Notes  
 La méthode **Refresh** effectue différentes tâches en fonction de la collection à partir de laquelle vous l’appelez.  
  
### <a name="parameters"></a>Paramètres  
 L’utilisation de la méthode **Refresh** sur la collection [Parameters](./parameters-collection-ado.md) d’un objet [Command](./command-object-ado.md) récupère les informations sur les paramètres côté fournisseur pour la procédure stockée ou la requête paramétrable spécifiée dans l’objet **Command** . La collection est vide pour les fournisseurs qui ne prennent pas en charge les appels de procédure stockée ou les requêtes paramétrables.  
  
 Vous devez définir la propriété [ActiveConnection](./activeconnection-property-ado.md) de l’objet de **commande** sur un objet de [connexion](./connection-object-ado.md) valide, la propriété [CommandText](./commandtext-property-ado.md) sur une commande valide et la propriété [CommandType](./commandtype-property-ado.md) sur **adCmdStoredProc** avant d’appeler la méthode **Refresh** .  
  
 Si vous accédez à la collection **Parameters** avant d’appeler la méthode **Refresh** , ADO appellera automatiquement la méthode et remplira la collection pour vous.  
  
> [!NOTE]
>  Si vous utilisez la méthode **Refresh** pour obtenir des informations sur les paramètres du fournisseur et qu’elle retourne un ou plusieurs objets de [paramètre](./parameter-object.md) de type de données de longueur variable, ADO peut allouer de la mémoire pour les paramètres en fonction de leur taille maximale potentielle, ce qui entraîne une erreur pendant l’exécution. Vous devez définir explicitement la propriété [Size](./size-property-ado-parameter.md) pour ces paramètres avant d’appeler la méthode [Execute](./execute-method-ado-command.md) pour éviter les erreurs.  
  
### <a name="fields"></a>Champs  
 L’utilisation de la méthode **Refresh** sur la collection [Fields](./fields-collection-ado.md) n’a aucun effet visible. Pour récupérer les modifications de la structure de base de données sous-jacente, vous devez utiliser la méthode [Requery](./requery-method.md) ou, si l’objet [Recordset](./recordset-object-ado.md) ne prend pas en charge les signets, la méthode [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) .  
  
### <a name="properties"></a>Propriétés  
 L’utilisation de la méthode **Refresh** sur une collection **Properties** de certains objets remplit la collection avec les propriétés dynamiques exposées par le fournisseur. Ces propriétés fournissent des informations sur les fonctionnalités spécifiques au fournisseur, au-delà des propriétés intégrées prises en charge par ADO.  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Axes, collection](../ado-md-api/axes-collection-ado-md.md)  
        [Collection Columns](../adox-api/columns-collection-adox.md)  
        [CubeDefs, collection](../ado-md-api/cubedefs-collection-ado-md.md)  
        [Dimensions, collection](../ado-md-api/dimensions-collection-ado-md.md)  
        [Collection d’erreurs](./errors-collection-ado.md)  
        [Collection Fields](./fields-collection-ado.md)  
        [Collection de groupes](../adox-api/groups-collection-adox.md)  
    :::column-end:::
    :::column:::
        [Hiérarchies, collection](../ado-md-api/hierarchies-collection-ado-md.md)  
        [Indexer la collection](../adox-api/indexes-collection-adox.md)  
        [Collection Keys](../adox-api/keys-collection-adox.md)  
        [Collection de niveaux](../ado-md-api/levels-collection-ado-md.md)  
        [Collection Members](../ado-md-api/members-collection-ado-md.md)  
        [Collection Parameters](./parameters-collection-ado.md)  
    :::column-end:::
    :::column:::
        [Collection de positions](../ado-md-api/positions-collection-ado-md.md)  
        [Procedures, collection](../adox-api/procedures-collection-adox.md)  
        [Collection Properties](./properties-collection-ado.md)  
        [Tables, collection](../adox-api/tables-collection-adox.md)  
        [Collection d’utilisateurs](../adox-api/users-collection-adox.md)  
        [Views, collection](../adox-api/views-collection-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [Refresh, exemple de méthode (VB)](./refresh-method-example-vb.md)   
 [Refresh, exemple de méthode (VC + +)](./refresh-method-example-vc.md)   
 [Count, propriété (ADO)](./count-property-ado.md)   
 [Refresh, méthode (RDS)](../rds-api/refresh-method-rds.md)