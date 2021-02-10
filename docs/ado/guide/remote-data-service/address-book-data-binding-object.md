---
description: Objet de liaison de données de l’application Carnet d’adresses
title: Objet de Data-Binding de carnet d’adresses | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS scenarios [ADO], data-binding object
- address book application scenario [ADO], data-binding object
ms.assetid: 080c1925-d453-4b89-92ac-c93591490518
author: rothja
ms.author: jroth
ms.openlocfilehash: 91489445aef2034ed273a6d13cc5d6ae02b7f519
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036589"
---
# <a name="address-book-data-binding-object"></a>Objet de liaison de données de l’application Carnet d’adresses
L’application Carnet d’adresses utilise le [RDS. DataControl](../../reference/rds-api/datacontrol-object-rds.md) pour lier les données de la base de données SQL Server à un objet visuel (dans ce cas, un tableau DHTML) dans la page HTML cliente de l’application. La logique du programme VBScript piloté par les événements utilise le [RDS. DataControl](../../reference/rds-api/datacontrol-object-rds.md) à :  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
-   Interrogez la base de données, envoyez des mises à jour à la base de données, puis actualisez la grille de données.  
  
-   Permet aux utilisateurs de passer au premier enregistrement, suivant, précédent ou précédent dans la grille de données.  
  
 Le code suivant définit le **RDS. Composant DataControl** :  
  
```vb
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"  
   ID=DC1 Width=1 Height=1>  
   <PARAM NAME="SERVER" VALUE="https://<%=Request.ServerVariables("SERVER_NAME")%>">  
   <PARAM NAME="CONNECT" VALUE="Provider=sqloledb;  
Initial Catalog=AddrBookDb;Integrated Security=SSPI;">  
</OBJECT>  
```  
  
 La balise OBJECT définit le **RDS. Composant DataControl** dans le programme. La balise comprend deux types de paramètres :  
  
-   Celles associées à la balise d’objet générique.  
  
-   Celles spécifiques au **RDS. DataControl** .  
  
## <a name="generic-object-tag-parameters"></a>Paramètres de balise d’objet générique  
 Le tableau suivant décrit les paramètres associés à la balise OBJECT.  
  
|Paramètre|Description|  
|---------------|-----------------|  
|***ClassID** _|Nombre 128 bits unique qui identifie le type d’objet incorporé sur le système. Cet identificateur est conservé dans le registre système de l’ordinateur local. (Pour les ID de classe de _ *RDS. DataControl**, consultez [RDS. DataControl, objet](../../reference/rds-api/datacontrol-object-rds.md).)|  
|***Identifiant***|Définit un identificateur à l’ensemble du document pour l’objet incorporé qui est utilisé pour l’identifier dans le code.|  
  
## <a name="rdsdatacontrol-tag-parameters"></a>Licence. Paramètres de balise DataControl  
 Le tableau suivant décrit les paramètres spécifiques à l' **objet RDS. DataControl** . (Pour obtenir la liste complète des **services Bureau à distance.** Les paramètres d’objet DataControl et le moment où les implémentent, consultez [RDS. DataControl, objet](../../reference/rds-api/datacontrol-object-rds.md).)  
  
|Paramètre|Description|  
|---------------|-----------------|  
|[SERVEURS](../../reference/rds-api/server-property-rds.md)|Si vous utilisez le protocole HTTP, la valeur est le nom de l’ordinateur serveur précédé de `https://` .|  
|[CONNECT](../../reference/rds-api/connect-property-rds.md)|Fournit les informations de connexion nécessaires pour le **RDS. DataControl** pour la connexion à SQL Server.|  
|[SQL](../../reference/rds-api/sql-property.md)|Définit ou retourne la chaîne de requête utilisée pour récupérer le [jeu d’enregistrements](../../reference/ado-api/recordset-object-ado.md).|  
  
## <a name="see-also"></a>Voir aussi  
 [Boutons de commande de l’application Carnet d’adresses](./address-book-command-buttons.md)