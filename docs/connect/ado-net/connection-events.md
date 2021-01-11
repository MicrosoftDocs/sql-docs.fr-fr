---
title: Événements de connexion
description: Les événements de connexion pour récupérer des messages d’information d’une source de données et déterminer si son état est modifié.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 5a29de74-acfc-4134-8616-829dd7ce0710
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 8151915dc6c16c6225fec9ab90cb5a88e86b992f
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771439"
---
# <a name="connection-events"></a>Événements de connexion

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Le Fournisseur de données Microsoft SqlClient ont des objets **Connexion** à deux événements que vous pouvez utiliser pour extraire des messages d'information d'une source de données ou pour déterminer si l'état d'un objet **Connexion** a été modifié. La table suivante décrit les événements de l'objet **Connexion**.

|Événement|Description|  
|-----------|-----------------|  
|**InfoMessage**|Se produit lorsqu'un message d'information est retourné à partir d'une source de données. Les messages d'information sont des messages provenant d'une source de données qui ne se traduisent pas par la levée d'une exception.|  
|**StateChange**|Se produit lorsque l'état de **Connexion** change.|  

## <a name="work-with-the-infomessage-event"></a>Utiliser l’événement InfoMessage

Vous pouvez extraire des messages d'avertissement et d'information d'une source de données SQL Server à l'aide de l'événement <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> de l'objet <xref:Microsoft.Data.SqlClient.SqlConnection>. Les erreurs retournées par la source de données, dont le niveau de gravité est compris entre 11 et 16, lèvent une exception. L'événement <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> peut néanmoins être utilisé pour obtenir des messages de la source de données qui ne sont pas associés à une erreur. Dans le cas de Microsoft SQL Server, toute erreur dont le niveau de gravité est inférieur ou égal à 10 est considérée comme étant un message d'information et peut être capturée à l'aide de l'événement <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>. Pour plus d’informations, consultez l’article [Niveaux de gravité des erreurs du moteur de base de données](/sql/relational-databases/errors-events/database-engine-error-severities).

L’événement <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> reçoit un objet <xref:Microsoft.Data.SqlClient.SqlInfoMessageEventArgs> contenant, dans sa propriété **Erreurs**, une collection des messages de la source de données. Vous pouvez interroger les objets **Erreur** dans cette collection pour obtenir le numéro d’erreur et le texte du message, ainsi que la source de l’erreur. Le Fournisseur de données Microsoft SqlClient pour SQL Server comprend également des détails sur la base de données, la procédure stockée et le numéro de ligne d’où provient le message.

### <a name="example"></a> Exemple

L'exemple de code suivant montre comment ajouter un gestionnaire d'événements pour l'événement <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>.

[!code-csharp[SqlConnection_._InfoMessage#1](~/../sqlclient/doc/samples/SqlConnection_InfoMessage_StateChange.cs#1)]

## <a name="handle-errors-as-infomessages"></a>Gérer les erreurs en tant que InfoMessages

L'événement <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> se déclenche uniquement pour les messages d'information et d'avertissement en provenance du serveur. Toutefois, quand une erreur réelle se produit, l'exécution de la méthode **ExecuteNonQuery** ou **ExecuteReader** à l'origine de l'opération du serveur est arrêtée et une exception est levée.

Si vous souhaitez continuer à traiter le reste des instructions d'une commande indépendamment des erreurs générées par le serveur, définissez la propriété <xref:Microsoft.Data.SqlClient.SqlConnection.FireInfoMessageEventOnUserErrors%2A> de l'objet <xref:Microsoft.Data.SqlClient.SqlConnection> sur `true`. Cela a pour effet de déclencher l'événement <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> pour des erreurs, au lieu de lever une exception et d'interrompre le traitement. L'application cliente peut ensuite gérer cet événement et répondre à des conditions d'erreur.

> [!NOTE]
> Une erreur dont le niveau de gravité est égal ou supérieur à 17 qui entraîne un arrêt de traitement de la commande par le serveur doit être gérée comme une exception. Dans ce cas, une exception est levée, indépendamment de la manière dont l'erreur est gérée dans l'événement <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>.

## <a name="work-with-the-statechange-event"></a>Utiliser l’événement StateChange

L'événement **StateChange** se produit lorsque l'état de **Connexion** change. L'événement **StateChange** reçoit <xref:System.Data.StateChangeEventArgs> qui vous permet de déterminer la modification de l'état de **Connexion** à l'aide des propriétés **OriginalState** et **CurrentState**. La propriété **OriginalState** est une énumération <xref:System.Data.ConnectionState> qui indique l'état de **Connexion** avant sa modification. **CurrentState** est une énumération <xref:System.Data.ConnectionState> qui indique l'état de **Connexion** après sa modification.

L'exemple de code suivant utilise l'événement **StateChange** pour écrire un message sur la console lorsque l'état de **Connexion** change.

[!code-csharp[SqlConnection_._StateChange#2](~/../sqlclient/doc/samples/SqlConnection_InfoMessage_StateChange.cs#2)]

## <a name="see-also"></a>Voir aussi

- [Connexion à une source de données](connecting-to-data-source.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
