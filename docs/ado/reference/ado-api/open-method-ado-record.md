---
description: Open, méthode (objet Record ADO)
title: Open, méthode (ADO record) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::raw_Open
- _Record::Open
helpviewer_keywords:
- Open method [ADO]
ms.assetid: ab79a623-88a9-40b6-a017-a658bf19b778
author: rothja
ms.author: jroth
ms.openlocfilehash: 63872db8007b6f3b51a840eb6d079f8b4df0c042
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041408"
---
# <a name="open-method-ado-record"></a>Open, méthode (objet Record ADO)
Ouvre un objet [enregistrement](./record-object-ado.md) existant ou crée un nouvel élément représenté par l' **enregistrement**, tel qu’un fichier ou un répertoire.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Open Source, ActiveConnection, Mode, CreateOptions, Options, UserName, Password  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Source*  
 facultatif. **Variante** qui peut représenter l’URL de l’entité à représenter par cet objet **Record** , une **commande**, un [Recordset](./recordset-object-ado.md) ouvert ou un autre objet **Record** , une chaîne qui contient une instruction SQL SELECT ou un nom de table.  
  
 *ActiveConnection*  
 facultatif. **Variant** qui représente la chaîne de connexion ou l’objet de [connexion](./connection-object-ado.md) ouvert.  
  
 *Mode*  
 facultatif. Valeur [ConnectModeEnum](./connectmodeenum.md) qui spécifie le mode d’accès pour l’objet **enregistrement** obtenu. La valeur par défaut est **adModeUnknown**.  
  
 *CreateOptions*  
 facultatif. Valeur de [RecordCreateOptionsEnum](./recordcreateoptionsenum.md) qui spécifie si un fichier ou un répertoire existant doit être ouvert ou si un nouveau fichier ou répertoire doit être créé. La valeur par défaut est **adFailIfNotExists**. Si la valeur par défaut est définie, le mode d’accès est obtenu à partir de la propriété [mode](./mode-property-ado.md) . Ce paramètre est ignoré lorsque le paramètre *source* ne contient pas d’URL.  
  
 *Options*  
 facultatif. Valeur de [RecordOpenOptionsEnum](./recordopenoptionsenum.md) qui spécifie les options d’ouverture de l' **enregistrement**. La valeur par défaut est **adOpenRecordUnspecified**. Ces valeurs peuvent être combinées.  
  
 *UserName*  
 facultatif. Valeur de **chaîne** qui contient l’ID d’utilisateur qui, s’il est requis, autorise l’accès à la *source*.  
  
 *Mot de passe*  
 facultatif. Valeur de **chaîne** qui contient le mot de passe qui, le cas échéant, vérifie le *nom d’utilisateur*.  
  
## <a name="remarks"></a>Notes  
 La *source* peut être :  
  
-   Une URL. Si le protocole de l’URL est http, le fournisseur Internet est appelé par défaut. Si l’URL pointe vers un nœud qui contient un script exécutable (tel qu’un. ASP), un **enregistrement** qui contient la source à la place du contenu exécuté est ouvert par défaut. Utilisez l’argument *options* pour modifier ce comportement.  
  
-   Objet **enregistrement** . Un objet **Record** ouvert à partir d’un autre **enregistrement** clone l’objet **enregistrement** d’origine.  
  
-   Objet **Command** . L’objet **Record** ouvert représente la ligne unique retournée par l’exécution de la **commande**. Si les résultats contiennent plusieurs lignes, le contenu de la première ligne est placé dans l’enregistrement et une erreur peut être ajoutée à la collection d' **Erreurs** .  
  
-   Instruction SQL SELECT. L’objet **Record** ouvert représente la ligne unique retournée en exécutant le contenu de la chaîne. Si les résultats contiennent plusieurs lignes, le contenu de la première ligne est placé dans l’enregistrement et une erreur peut être ajoutée à la collection d' **Erreurs** .  
  
-   Nom de la table.  
  
 Si l’objet **Record** représente une entité qui n’est pas accessible à l’aide d’une URL (par exemple, une ligne d’un **jeu d’enregistrements** dérivé d’une base de données), les valeurs de la propriété [ParentURL](./parenturl-property-ado.md) et du champ accessible avec la constante **adRecordURL** sont null.  
  
> [!NOTE]
>  Les URL utilisant le schéma http appellera automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Record, objet (ADO)](./record-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Open, méthode (connexion ADO)](./open-method-ado-connection.md)   
 [Open, méthode (objet Recordset ADO)](./open-method-ado-recordset.md)   
 [Open, méthode (objet Stream ADO)](./open-method-ado-stream.md)   
 [OpenSchema, méthode](./openschema-method.md)