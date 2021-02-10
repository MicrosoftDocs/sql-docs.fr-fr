---
description: ConnectionString, propriété (ADO)
title: ConnectionString, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::ConnectionString
helpviewer_keywords:
- ConnectionString property [ADO]
ms.assetid: 3be75b75-4d36-4479-ab64-9a456869252a
author: rothja
ms.author: jroth
ms.openlocfilehash: 3f8fd13b1f00e7ccebf3b334573948d76aeb7242
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034639"
---
# <a name="connectionstring-property-ado"></a>ConnectionString, propriété (ADO)
Indique les informations utilisées pour établir une connexion à une source de données.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur de **chaîne** .  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **ConnectionString** pour spécifier une source de données en passant une chaîne de connexion détaillée contenant une série d’instructions *argument* *= value* séparées par des points-virgules.  
  
 ADO prend en charge cinq arguments pour la propriété **ConnectionString** . tout autre argument passe directement au fournisseur sans aucun traitement par ADO. Les arguments pris en charge par ADO sont les suivants :  
  
|Argument|Description|  
|--------------|-----------------|  
|*Fournisseur =*|Spécifie le nom d’un fournisseur à utiliser pour la connexion.|  
|*Nom de fichier =*|Spécifie le nom d’un fichier spécifique au fournisseur (par exemple, un objet de source de données persistant) contenant les informations de connexion prédéfinies.|  
|*Fournisseur distant =*|Spécifie le nom d’un fournisseur à utiliser lors de l’ouverture d’une connexion côté client. (Service de données distant uniquement.)|  
|*Serveur distant =*|Spécifie le nom du chemin d’accès du serveur à utiliser lors de l’ouverture d’une connexion côté client. (Service de données distant uniquement.)|  
|*URL =*|Spécifie la chaîne de connexion comme une URL absolue identifiant une ressource, telle qu’un fichier ou un répertoire.|  
  
 Une fois que vous avez défini la propriété **ConnectionString** et ouvert l’objet de [connexion](./connection-object-ado.md) , le fournisseur peut modifier le contenu de la propriété, par exemple, en mappant les noms d’arguments définis par ADO à leurs équivalents pour le fournisseur spécifique.  
  
 La propriété **ConnectionString** hérite automatiquement de la valeur utilisée pour l’argument *ConnectionString* de la méthode [Open](./open-method-ado-connection.md) , ce qui vous permet de remplacer la propriété **ConnectionString** actuelle pendant l’appel de la méthode **Open** .  
  
 Étant donné que l’argument de *nom de fichier* entraîne le chargement du fournisseur associé par ADO, vous ne pouvez pas transmettre à la fois le *fournisseur* et les arguments de *nom de fichier* .  
  
 La propriété **ConnectionString** est en lecture/écriture lorsque la connexion est fermée et en lecture seule lorsqu’elle est ouverte.  
  
 Les doublons d’un argument dans la propriété **ConnectionString** sont ignorés. La dernière instance de n’importe quel argument est utilisée.  
  
> [!NOTE]
>  **Utilisation des services de données distants** Lorsqu’elle est utilisée sur un objet de **connexion** côté client, la propriété **ConnectionString** peut inclure uniquement les paramètres du *fournisseur distant* et du *serveur distant* .  
  
 Le tableau suivant répertorie le fournisseur ADO par défaut pour chaque système d’exploitation Windows :  
  
|Fournisseur ADO par défaut|Système d’exploitation Windows|  
|--------------------------|------------------------------|  
|MSDASQL<br /><br /> (Pour améliorer la lisibilité du code source, spécifiez explicitement le nom du fournisseur dans la chaîne de connexion.)|Windows 2000 (32 bits)<br /><br /> Windows XP (32 bits)<br /><br /> Windows 2003 Server (32 bits)<br /><br /> Windows Vista (32 bits)<br /><br /> Windows Vista Service Pack 1 ou version ultérieure (32 bits et 64 bits)<br /><br /> Versions de Windows après Windows Vista (32 bits et 64 bits)|  
|Aucune valeur par défaut.<br /><br /> Quand une application ADO s’exécute sur les systèmes d’exploitation suivants et ne spécifie pas explicitement le fournisseur, ADO retourne l’erreur suivante : «ADODB. Connexion : le fournisseur n’est pas spécifié et il n’existe aucun fournisseur par défaut désigné.»|Windows 2000 (64 bits)<br /><br /> Windows XP (64 bits)<br /><br /> Windows 2003 Server (64 bits)<br /><br /> Windows Vista (64 bits)|  
  
## <a name="applies-to"></a>S'applique à  
 [Connection, objet (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ConnectionString, ConnectionTimeout et State, exemple de propriétés (VB)](./connectionstring-connectiontimeout-and-state-properties-example-vb.md)   
 [ConnectionString, ConnectionTimeout et State, exemple de propriétés (VC + +)](./connectionstring-connectiontimeout-and-state-properties-example-vc.md)   
 [Annexe A : Fournisseurs](../../guide/appendixes/appendix-a-providers.md)