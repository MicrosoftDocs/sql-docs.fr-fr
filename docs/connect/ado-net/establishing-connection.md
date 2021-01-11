---
title: Établir une connexion
description: Instructions pour la connexion à un SQL Server par le fournisseur SqlClient.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 3af512f3-87d9-4005-9e2f-abb1060ff43f
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: b3d92f49fb4eae6184a673ffc25ff6bbdf521626
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771591"
---
# <a name="establishing-connection"></a>Établir une connexion

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Pour vous connecter à Microsoft SQL Server, utilisez l'objet <xref:Microsoft.Data.SqlClient.SqlConnection> du Fournisseur de données Microsoft SqlClient pour SQL Server. Pour stocker et extraire des chaînes de connexion en toute sécurité, consultez [Protection des informations de connexion](protecting-connection-information.md).

## <a name="closing-connections"></a>Fermeture de connexions

Nous vous recommandons de toujours fermer la connexion lorsque vous avez fini de l'utiliser, de façon à ce qu'elle puisse être retournée au pool. Le bloc `Using` dans Visual Basic ou C# supprime automatiquement la connexion lorsque le code quitte le bloc, même dans le cas d'une exception non traitée. Pour plus d'informations, consultez [Instruction using](/dotnet/csharp/language-reference/keywords/using-statement) et [Instruction Using](/dotnet/visual-basic/language-reference/statements/using-statement).

Vous pouvez également utiliser les méthodes `Close` ou `Dispose` de l’objet de connexion. Les connexions qui ne sont pas explicitement fermées risquent de ne pas être ajoutées ni retournées au pool. Par exemple, une connexion devenue hors de portée mais qui n'a pas été explicitement fermée sera retournée au pool de connexion seulement si la taille maximale de celui-ci a été atteinte et si la connexion est toujours valide.

> [!NOTE]
> N'appelez pas `Close` ni `Dispose` sur un objet managé de type **Connexion**, **DataReader** ou autre dans la méthode `Finalize` de votre classe. Dans un finaliseur, libérez seulement les ressources non managées que votre classe possède directement. Si votre classe ne possède pas de ressource non managée, n'incluez pas une méthode `Finalize` dans la définition de classe. Pour plus d’informations, consultez [Nettoyage de la mémoire](/dotnet/standard/garbage-collection/index).

> [!NOTE]
> Les événements de connexion et de déconnexion ne seront pas déclenchés sur le serveur si une connexion est récupérée depuis le pool de connexions ou qu’elle est retournée au pool, car elle n’est pas réellement fermée lorsqu’elle est retournée au pool. Pour plus d’informations, consultez [Regroupement de connexions SQL Server (ADO.NET)](sql-server-connection-pooling.md).

## <a name="connecting-to-sql-server"></a>Connexion à SQL Server

Pour obtenir les noms et les valeurs de format de chaîne valides, voir la propriété <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> de l'objet <xref:Microsoft.Data.SqlClient.SqlConnection>. Vous pouvez également utiliser la classe <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> pour créer des chaînes de connexion valides du point de vue de la syntaxe au moment de l'exécution. Pour plus d’informations, consultez [Builders de chaînes de connexion](connection-string-builders.md).

L'exemple de code suivant illustre la création et l'ouverture d'une connexion à une base de données SQL Server.

[!code-csharp[SqlConnection.Open#1](~/../sqlclient/doc/samples/SqlConnection_Open.cs#1)]

### <a name="integrated-security-and-aspnet"></a>Sécurité intégrée et ASP.NET

La sécurité intégrée SQL Server (également appelée « connexions approuvées ») permet de fournir une protection lors de la connexion à SQL Server car elle n’expose pas d’ID utilisateur et de mot de passe dans la chaîne de connexion et constitue la méthode recommandée pour l’authentification d’une connexion. La sécurité intégrée utilise l'identité de sécurité active, ou jeton, du processus en cours d'exécution. Pour les applications de bureau, cette identité est généralement l’identité de l’utilisateur actuellement connecté.

Dans les applications ASP.NET, l'identité de sécurité peut être définie à l'aide d'une option parmi plusieurs options différentes. Pour mieux comprendre l'identité de sécurité qu'une application ASP.NET utilise lorsqu'elle se connecte à SQL Server, consultez [Emprunt d’identité ASP.NET](/previous-versions/aspnet/xh507fc5(v=vs.100)), [Authentification ASP.NET](/previous-versions/aspnet/eeyk640h(v=vs.100)) et [Procédure : Accéder à SQL Server à l'aide de la sécurité intégrée de Windows](/previous-versions/aspnet/bsz5788z(v=vs.100)).

## <a name="see-also"></a>Voir aussi

- [Connexion à une source de données](connecting-to-data-source.md)
- [Chaînes de connexion](connection-strings.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
