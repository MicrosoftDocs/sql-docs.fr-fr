---
description: Open, méthode (objet Recordset ADO)
title: Open, méthode (objet Recordset ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::raw_Open
- Recordset15::Open
helpviewer_keywords:
- Open method [ADO]
ms.assetid: 3236749c-4b71-4235-89e2-ccdfaaa9319d
author: rothja
ms.author: jroth
ms.openlocfilehash: 0f9cf81ef9845875db984e4cba0fcd7bc23e380c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041399"
---
# <a name="open-method-ado-recordset"></a>Open, méthode (objet Recordset ADO)
Ouvre un curseur sur un objet [Recordset](./recordset-object-ado.md) .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.Open Source, ActiveConnection, CursorType, LockType, Options  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Source*  
 facultatif. **Variante** qui prend la valeur d’un objet [Command](./command-object-ado.md) valide, d’une instruction SQL, d’un nom de table, d’un appel de procédure stockée, d’une URL ou du nom d’un fichier ou d’un objet de [flux](./stream-object-ado.md) contenant un [Recordset](./recordset-object-ado.md)stocké de façon permanente.  
  
 *ActiveConnection*  
 facultatif. Valeur **Variant** qui prend la valeur d’un nom de variable objet de [connexion](./connection-object-ado.md) valide, ou **chaîne** qui contient les paramètres [ConnectionString](./connectionstring-property-ado.md) .  
  
 *CursorType*  
 facultatif. Valeur [CursorTypeEnum](./cursortypeenum.md) qui détermine le type de curseur que le fournisseur doit utiliser lors de l’ouverture du **Recordset**. La valeur par défaut est **adOpenForwardOnly**.  
  
 *Verrou*  
 facultatif. Valeur [LockTypeEnum](./locktypeenum.md) qui détermine le type de verrouillage (accès concurrentiel) que le fournisseur doit utiliser lors de l’ouverture du **Recordset**. La valeur par défaut est **adLockReadOnly**.  
  
 *Options*  
 facultatif. Valeur de **type long** qui indique comment le fournisseur doit évaluer l’argument *source* s’il représente autre chose qu’un objet **Command** ou si le **jeu d’enregistrements** doit être restauré à partir d’un fichier dans lequel il a été précédemment enregistré. Il peut s’agir d’une ou plusieurs valeurs [CommandTypeEnum](./commandtypeenum.md) ou [ExecuteOptionEnum](./executeoptionenum.md) , qui peuvent être combinées avec un opérateur or au niveau du bit.  
  
> [!NOTE]
>  Si vous ouvrez un **jeu d’enregistrements** à partir d’un **flux** contenant un **Recordset** persistant, l’utilisation d’une valeur [ExecuteOptionEnum](./executeoptionenum.md) de **adAsyncFetchNonBlocking** n’aura aucun effet. l’extraction est synchrone et bloque.  
  
> [!NOTE]
>  Les valeurs **ExecuteOpenEnum** de **adExecuteNoRecords** ou **adExecuteStream** ne doivent pas être utilisées avec **Open**.  
  
## <a name="remarks"></a>Notes  
 Le curseur par défaut d’un **jeu d’enregistrements** ADO est un curseur avant uniquement, en lecture seule, situé sur le serveur.  
  
 L’utilisation de la méthode **Open** sur un objet **Recordset** ouvre un curseur qui représente les enregistrements d’une table de base, les résultats d’une requête ou un **Recordset** précédemment enregistré.  
  
 Utilisez l’argument *source* facultatif pour spécifier une source de données à l’aide d’un des éléments suivants : une variable d’objet de **commande** , une instruction SQL, une procédure stockée, un nom de table, une URL ou un nom de chemin d’accès complet au fichier. Si *source* est un nom de chemin d’accès de fichier, il peut s’agir d’un chemin d’accès complet (« c:\dir\file.RST »), d’un chemin d’accès relatif («.. \File.RST ") ou une URL ( `https://files/file.rst` ).  
  
 Il n’est pas judicieux d’utiliser l’argument *source* de la méthode **Open** pour exécuter une requête d’action qui ne retourne pas d’enregistrements, car il n’existe aucun moyen simple de déterminer si l’appel a réussi. Le **jeu d’enregistrements** renvoyé par une telle requête sera fermé. Pour exécuter une requête qui ne retourne pas d’enregistrements, telle qu’une instruction SQL INSERT, appelez la méthode [Execute](./execute-method-ado-command.md) d’un objet **Command** ou la méthode [Execute](./execute-method-ado-connection.md) d’un objet [Connection](./connection-object-ado.md) à la place.  
  
 L’argument *ActiveConnection* correspond à la propriété [ActiveConnection](./activeconnection-property-ado.md) et spécifie la connexion dans laquelle ouvrir l’objet **Recordset** . Si vous transmettez une définition de connexion pour cet argument, ADO ouvre une nouvelle connexion à l’aide des paramètres spécifiés. Après avoir ouvert le **Recordset** avec un curseur côté client en affectant à la propriété [CursorLocation](./cursorlocation-property-ado.md) la valeur **adUseClient**, vous pouvez modifier la valeur de cette propriété pour envoyer des mises à jour à un autre fournisseur. Ou vous pouvez affecter à cette propriété la valeur **Nothing** (dans Microsoft Visual Basic) ou la valeur null pour déconnecter le **Recordset** de n’importe quel fournisseur. La modification de *ActiveConnection* pour un curseur côté serveur génère toutefois une erreur.  
  
 Pour les autres arguments qui correspondent directement aux propriétés d’un objet **Recordset** (*source*, *CursorType* et *LockType*), la relation des arguments avec les propriétés se présente comme suit :  
  
-   La propriété est en lecture/écriture avant l’ouverture de l’objet **Recordset** .  
  
-   Les paramètres de propriété sont utilisés, sauf si vous transmettez les arguments correspondants lors de l’exécution de la méthode **Open** . Si vous passez un argument, il remplace le paramètre de propriété correspondant, et le paramètre de propriété est mis à jour avec la valeur d’argument.  
  
-   Une fois que vous avez ouvert l’objet **Recordset** , ces propriétés sont en lecture seule.  
  
> [!NOTE]
>  La propriété **ActiveConnection** est en lecture seule pour les objets **Recordset** dont la propriété [source](./source-property-ado-recordset.md) est définie sur un objet **Command** valide, même si l’objet **Recordset** n’est pas ouvert.  
  
 Si vous transmettez un objet **Command** dans l’argument *source* et que vous transmettez également un argument *ActiveConnection* , une erreur se produit. La propriété **ActiveConnection** de l’objet de **commande** doit déjà être définie sur un objet de **connexion** ou une chaîne de connexion valide.  
  
 Si vous transmettez autre chose qu’un objet de **commande** dans l’argument *source* , vous pouvez utiliser l’argument *options* pour optimiser l’évaluation de l’argument *source* . Si l’argument *options* n’est pas défini, vous pouvez constater une baisse des performances, car ADO doit effectuer des appels au fournisseur pour déterminer si l’argument est une instruction SQL, une procédure stockée, une URL ou un nom de table. Si vous connaissez le type de *source* que vous utilisez, la définition de l’argument *options* indique à ADO d’accéder directement au code approprié. Si l’argument *options* ne correspond pas au type de *source* , une erreur se produit.  
  
 Si vous transmettez un objet de **flux** dans l’argument *source* , vous ne devez pas passer d’informations dans les autres arguments. Cela générera une erreur. Les informations **ActiveConnection** ne sont pas conservées lorsqu’un **jeu d’enregistrements** est ouvert à partir d’un **flux**.  
  
 La valeur par défaut de l’argument *options* est **adCmdFile** si aucune connexion n’est associée au **Recordset**. C’est généralement le cas pour les objets **Recordset** stockés de manière permanente.  
  
 Si la source de données ne retourne aucun enregistrement, le fournisseur affecte à la fois les propriétés [BOF](./bof-eof-properties-ado.md) et [EOF](./bof-eof-properties-ado.md) la **valeur true**, et la position actuelle de l’enregistrement n’est pas définie. Vous pouvez toujours ajouter de nouvelles données à cet objet **Recordset** vide si le type de curseur le permet.  
  
 Lorsque vous avez terminé vos opérations sur un objet **Recordset** ouvert, utilisez la méthode [Close](./close-method-ado.md) pour libérer toutes les ressources système associées. La fermeture d’un objet ne le supprime pas de la mémoire ; vous pouvez modifier ses paramètres de propriété et utiliser la méthode **Open** pour l’ouvrir à nouveau ultérieurement. Pour éliminer complètement un objet de la mémoire, affectez à la variable objet la valeur *Nothing*.  
  
 Avant de définir la propriété **ActiveConnection** , appelez **Open** sans opérandes pour créer une instance d’un **jeu d’enregistrements** créé en ajoutant des champs à la collection de [champs](./fields-collection-ado.md) du **Recordset** .  
  
 Si vous avez défini la propriété [CursorLocation](./cursorlocation-property-ado.md) sur **adUseClient**, vous pouvez récupérer des lignes de manière asynchrone de l’une des deux façons suivantes. La méthode recommandée consiste à définir des *options* sur **adAsyncFetch**. Vous pouvez également utiliser la propriété dynamique « traitement des ensembles de lignes asynchrones » dans la collection de [Propriétés](./properties-collection-ado.md) , mais les événements récupérés associés peuvent être perdus si vous ne définissez pas le paramètre *options* sur **adAsyncFetch**.  
  
> [!NOTE]
>  L’extraction en arrière-plan dans le fournisseur distant MS est prise en charge uniquement via le paramètre *options* de la méthode **Open** .  
  
> [!NOTE]
>  Les URL utilisant le schéma http appellera automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../guide/data/absolute-and-relative-urls.md).  
  
 Certaines combinaisons de valeurs [CommandTypeEnum](./commandtypeenum.md) et [ExecuteOptionEnum](./executeoptionenum.md) ne sont pas valides. Pour plus d’informations sur les options qui ne peuvent pas être combinées, consultez les rubriques relatives à [ExecuteOptionEnum](./executeoptionenum.md)et [CommandTypeEnum](./commandtypeenum.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Open et Close, exemple de méthodes (VB)](./open-and-close-methods-example-vb.md)   
 [Open et Close, exemple de méthode (VBScript)](./open-and-close-methods-example-vbscript.md)   
 [Open et Close, exemple de méthodes (VC + +)](./open-and-close-methods-example-vc.md)   
 [Save et Open, exemples de méthodes (VB)](./save-and-open-methods-example-vb.md)   
 [Open, méthode (connexion ADO)](./open-method-ado-connection.md)   
 [Open, méthode (ADO record)](./open-method-ado-record.md)   
 [Open, méthode (objet Stream ADO)](./open-method-ado-stream.md)   
 [OpenSchema, méthode](./openschema-method.md)   
 [Save, méthode](./save-method.md)