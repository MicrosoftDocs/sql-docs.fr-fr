---
description: Fournisseur Microsoft OLE DB Remoting (fournisseur de services ADO)
title: Fournisseur Microsoft OLE DB Remoting (fournisseur de services ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB remoting provider [ADO]
- providers [ADO], OLE DB remoting provider
- remoting provider [ADO]
ms.assetid: a4360ed4-b70f-4734-9041-4025d033346b
author: rothja
ms.author: jroth
ms.openlocfilehash: c73122e5fe1b06fb44ad69219145972ef4009202
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029243"
---
# <a name="microsoft-ole-db-remoting-provider-overview"></a>Vue d’ensemble du fournisseur Microsoft OLE DB Remoting
Le fournisseur Microsoft OLE DB Remoting permet à un utilisateur local sur un ordinateur client d’appeler des fournisseurs de données sur un ordinateur distant. Spécifiez les paramètres du fournisseur de données pour la machine distante comme vous le feriez pour un utilisateur local sur l’ordinateur distant. Spécifiez ensuite les paramètres utilisés par le fournisseur de communication à distance pour accéder à la machine distante. Vous pouvez ensuite accéder à la machine distante comme si vous étiez un utilisateur local.

> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le  [service de données WCF](/dotnet/framework/wcf/).

## <a name="provider-keyword"></a>Mot clé Provider
 Pour appeler le fournisseur de communication à distance OLE DB, spécifiez le mot clé et la valeur suivants dans la chaîne de connexion. (Notez l’espace blanc dans le nom du fournisseur.)

```vb
"Provider=MS Remote"
```

## <a name="additional-keywords"></a>Mots clés supplémentaires
 Lorsque ce fournisseur de services est appelé, les mots clés supplémentaires suivants sont pertinents.

|Mot clé|Description|
|-------------|-----------------|
|**Source de données**|Spécifie le nom de la source de données distante. Elle est passée au fournisseur de communication à distance OLE DB pour le traitement.<br /><br /> Ce mot clé est équivalent au [RDS. ](../../reference/rds-api/datacontrol-object-rds.md) Propriété [Connect](../../reference/rds-api/connect-property-rds.md) de l’objet DataControl.|

## <a name="dynamic-properties"></a>Propriétés dynamiques
 Lorsque ce fournisseur de services est appelé, les propriétés dynamiques suivantes sont ajoutées à la collection de [Propriétés](../../reference/ado-api/properties-collection-ado.md) de l’objet de [connexion](../../reference/ado-api/connection-object-ado.md).

|Nom de la propriété dynamique|Description|
|---------------------------|-----------------|
|**DFMode**|Indique le mode DataFactory. Chaîne qui spécifie la version souhaitée de l’objet [DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) sur le serveur. Définissez cette propriété avant d’ouvrir une connexion pour demander une version particulière du **DataFactory**. Si la version demandée n’est pas disponible, une tentative d’utilisation de la version précédente est effectuée. S’il n’existe pas de version précédente, une erreur se produit. Si **DFMode** est inférieur à la version disponible, une erreur se produit. Cette propriété est en lecture seule après l’établissement d’une connexion.<br /><br /> Il peut s’agir de l’une des valeurs de chaîne valides suivantes :<br /><br /> -"25"-version 2,5 (par défaut)<br />-« 21 »-version 2,1<br />-« 20 »-version 2,0<br />-« 15 »-version 1,5|
|**Propriétés de commande**|Indique les valeurs qui seront ajoutées à la chaîne de propriétés de commande (rowset) envoyées au serveur par le fournisseur distant MS. La valeur par défaut de cette chaîne est vt_empty.|
|**DFMode actuel**|Indique le numéro de version réel du **DataFactory** sur le serveur. Vérifiez cette propriété pour voir si la version demandée dans la propriété **DFMode** a été respectée.<br /><br /> Il peut s’agir de l’une des valeurs entières longues valides suivantes :<br /><br /> -25-version 2,5 (par défaut)<br />-21-version 2,1<br />-20-version 2,0<br />-15-version 1,5<br /><br /> L’ajout de « DFMode = 20 ; » à votre chaîne de connexion lors de l’utilisation du fournisseur **MSRemote** peut améliorer les performances de votre serveur lors de la mise à jour des données. Avec ce paramètre, l’objet **RDSServer. DataFactory** sur le serveur utilise un mode moins gourmand en ressources. Toutefois, les fonctionnalités suivantes ne sont pas disponibles dans cette configuration :<br /><br /> -Utilisation de requêtes paramétrables.<br />-Obtention des informations sur les paramètres ou les colonnes avant d’appeler la méthode **Execute** .<br />-Affectation de la **valeur true** à la **mise à jour de Transact** .<br />-Obtention de l’état de la ligne.<br />-Appel de la méthode **Resync** .<br />-Actualisation (explicite ou automatique) via la propriété **mettre à jour la resynchronisation** .<br />-Définition des propriétés de la **commande** ou du **Recordset** .<br />-Utilisation de **adCmdTableDirect**.|
|**Handler**|Indique le nom d’un programme de personnalisation côté serveur (ou gestionnaire) qui étend les fonctionnalités de [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md)et de tous les paramètres utilisés par le gestionnaire, séparés par des virgules (","). Valeur de **chaîne**.|
|**Délai d’expiration Internet**|Indique le nombre maximal de millisecondes d’attente d’une demande de déplacement vers et à partir du serveur. (La valeur par défaut est 5 minutes.)|
|**Fournisseur distant**|Indique le nom du fournisseur de données à utiliser sur le serveur distant.|
|**Serveur distant**|Indique le nom du serveur et le protocole de communication à utiliser par cette connexion. Cette propriété est équivalente à l' [objet RDS. ](../../reference/rds-api/datacontrol-object-rds.md) Propriété du [serveur](../../reference/rds-api/server-property-rds.md) d’objets DataContro.|
|**Mises à jour de Transact**|Lorsqu’elle est définie sur **true**, cette valeur indique que lorsque la procédure [UpdateBatch](../../reference/ado-api/updatebatch-method.md) est effectuée sur le serveur, elle est effectuée à l’intérieur d’une transaction. La valeur par défaut de cette propriété dynamique booléenne est **false**.|

 Vous pouvez également définir des propriétés dynamiques en écriture en spécifiant leurs noms en tant que Mots clés dans la chaîne de connexion. Par exemple, définissez la propriété dynamique **délai d’expiration Internet** sur cinq secondes en spécifiant :

```vb
Dim cn as New ADODB.Connection
cn.Open "Provider=MS Remote;Internet Timeout=5000"
```

 Vous pouvez également définir ou récupérer une propriété dynamique en spécifiant son nom en tant qu’index de la propriété **Properties** . L’exemple suivant montre comment obtenir et imprimer la valeur actuelle de la propriété dynamique de **délai d’attente Internet** , puis définir une nouvelle valeur :

```vb
Debug.Print cn.Properties("Internet Timeout")
cn.Properties("Internet Timeout") = 5000
```

## <a name="remarks"></a>Notes
 Dans ADO 2,0, le fournisseur de communication à distance OLE DB ne peut être spécifié que dans le paramètre *ActiveConnection* de la méthode **Open** de l’objet [Recordset](../../reference/ado-api/recordset-object-ado.md) . À compter d’ADO 2,1, le fournisseur peut également être spécifié dans le paramètre *ConnectionString* de la méthode **Open** de l’objet de [connexion](../../reference/ado-api/connection-object-ado.md) .

 Équivalent de l' **objet RDS.** La propriété [SQL](../../reference/rds-api/sql-property.md) de l’objet DataControl n’est pas disponible. L’argument de *source* de la méthode d' **ouverture** de l’objet [Recordset](../../reference/ado-api/recordset-object-ado.md) est utilisé à la place.

 **Remarque** La spécification de «...; Fournisseur distant = MS Remote ;...» créerait un scénario à quatre niveaux. Les scénarios au-delà de trois niveaux n’ont pas été testés et ne devraient pas être nécessaires.

## <a name="example"></a>Exemple
 Cet exemple exécute une requête sur la table **Authors** de la base de données **pubs** sur un serveur nommé *yourserver*. Les noms de la source de données distante et du serveur distant sont fournis dans la méthode [Open](../../reference/ado-api/open-method-ado-connection.md) de l’objet[Connection](../../reference/ado-api/connection-object-ado.md) , et la requête SQL est spécifiée dans la méthode[Open](../../reference/ado-api/open-method-ado-recordset.md) de l’objet [Recordset](../../reference/ado-api/recordset-object-ado.md) . Un objet **Recordset** est retourné, modifié et utilisé pour mettre à jour la source de données.

```vb
Dim rs as New ADODB.Recordset
Dim cn as New ADODB.Connection
cn.Open  "Provider=MS Remote;Data Source=pubs;" & _
         "Remote Server=https://YourServer"
rs.Open "SELECT * FROM authors", cn
...                'Edit the recordset
rs.UpdateBatch     'Equivalent of RDS SubmitChanges
...
```

## <a name="see-also"></a>Voir aussi
 [Vue d’ensemble du fournisseur de communication à distance OLE DB](/previous-versions/windows/desktop/ms713673(v=vs.85))