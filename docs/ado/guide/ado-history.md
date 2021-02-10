---
description: Fonctionnalités ADO pour chaque version
title: Historique ADO | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
helpviewer_keywords:
- ADO, what's new
ms.assetid: 667673f2-3151-432b-894a-3fc60b704ea4
author: rothja
ms.author: jroth
ms.openlocfilehash: 7247523daf68116c949d4ec8ad3eec90fe4c8fe2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030617"
---
# <a name="ado-features-for-each-release"></a>Fonctionnalités ADO pour chaque version

Cette rubrique répertorie les nouvelles fonctionnalités introduites par chaque version d’ADO, ADO MD et ADOX.

## <a name="ado-60"></a>ADO 6,0

ADO 6,0 est inclus dans Windows Vista, dans le cadre de Windows Data Access Components (Windows DAC) 6,0. ADO 6,0 est fonctionnellement équivalent à ADO 2,8.

## <a name="ado-28"></a>ADO 2,8

ADO 2,8 était inclus dans Windows XP et Windows Server 2003, dans le cadre de Microsoft Data Access Components (MDAC) 2,8. Une version redistribuable de MDAC 2,8 est également disponible. Notez que cette version redistribuable doit être installée uniquement sur Windows 2000. ADO 2,8 résout plusieurs problèmes liés à la sécurité :

*L’accès au disque dur n’est pas autorisé en dehors d’une zone approuvée.*
Dans les scripts inter-domaines impliquant des sites non approuvés, les opérations suivantes sont désactivées : **Stream. SaveToFile**, **Stream. LoadFromFile**, **Recordset. Save** et **Recordset. Open**, utilisés conjointement avec l’indicateur **adCmdFile** ou avec le fournisseur de persistance Microsoft OLE DB (MSPersist).

**Recordset. Open** _,_  **Recordset. Save** _,_  **Stream. SaveToFile** _et_  **Stream. LoadFromFile**  _fonctionnent uniquement sur les fichiers physiques._
Ces méthodes vérifient à présent que les handles de fichiers pointent vers des fichiers physiques uniquement.

**Recordset. ActiveCommand**  _retourne une erreur lorsqu’elle est appelée à partir d’une page HTML/ASP._
Cela empêche l’utilisation inutilisée de l’objet de **commande** .

_Le nombre d'_**ensembles d’enregistrements**_retournés par une commande de forme imbriquée__a une limite supérieure._        
Une commande SHAPE imbriquée retourne à présent un maximum de 512 **recordsets**. Cela signifie qu’une commande **Shape** ne peut plus être imbriquée à aucune profondeur. Au lieu de cela, la profondeur de niveau maximale est de 512, si chaque commande produit un seul **Recordset**(enfant). Si, à un niveau quelconque, une commande **Shape** retourne plusieurs **recordsets**, le niveau maximal de profondeur sera inférieur à 512.

## <a name="ado-27"></a>ADO 2,7

*prise en charge des plateformes 64 bits* ADO 2,7 introduit la prise en charge des processeurs 64 bits.

## <a name="ado-26"></a>ADO 2,6

_Méthode_ **CubDef. GETSCHEMAOBJECT** commençant par ADO 2,6, ADO MD objets peuvent être récupérés à l’aide de noms uniques, comme spécifié par la [Propriété UniqueName (ADO MD)](../reference/ado-md-api/uniquename-property-ado-md.md).   Les noms des objets parents n’ont pas besoin d’être connus, et les collections parentes n’ont pas besoin d’être remplies pour récupérer un objet de schéma. Consultez [méthode GetSchemaObject (ADO MD)](../reference/ado-md-api/getschemaobject-method-ado-md.md).

*Flux de commandes* L’objet **Command** prend en charge les commandes au format de flux comme alternative à l’utilisation de la propriété **CommandText** . La [propriété CommandStream (ADO)](../reference/ado-api/commandstream-property-ado.md) peut être utilisée pour spécifier des modèles XML ou codes en tant qu’entrée de **commande** avec le fournisseur Microsoft OLE DB pour SQL Server.

Dialecte de la propriété **dialecte** 
 [](../reference/ado-api/dialect-property.md) est une nouvelle propriété qui définit la syntaxe et les règles générales que le fournisseur utilise pour analyser la chaîne ou le flux.  

**Command.Exe**  _méthode_ , la [méthode Execute](../reference/ado-api/execute-method-ado-command.md) de l’objet **Command** ADO a été améliorée pour utiliser des flux pour l’entrée et la sortie.

*Champ statusvalues* Si l’utilisateur rencontre une erreur DB_E_ERRORSOCCURRED lors de la modification d’un **champ** d’un **jeu d’enregistrements**, ADO remplit à présent la propriété **Field. Status** avec les informations d’État appropriées afin que l’utilisateur ait plus d’informations sur ce qui s’est produit. Consultez [Status, propriété (champ ADO)](../reference/ado-api/status-property-ado-field.md).

La propriété **NamedParameters** 
 [NamedParameters](../reference/ado-api/namedparameters-property-ado.md) est une nouvelle propriété de l’objet de **commande** qui indique que le fournisseur doit utiliser des paramètres nommés.  

*Jeux de résultats dans les flux* ADO peut retourner des jeux de résultats à partir d’une source de données dans un **flux**, plutôt qu’un objet **Recordset** . À l’aide de la dernière version du fournisseur Microsoft OLE DB pour SQL Server, vous pouvez obtenir des résultats XML à partir du fournisseur en exécutant une requête « for XML ». Un **flux** qui reçoit le jeu de résultats peut être ouvert avec une commande « for XML » comme source. Consultez [extraction de jeux de résultats dans des flux](./data/retrieving-resultsets-into-streams.md).

*Jeu de résultats de ligne unique* L’objet ADO **Record** peut maintenant être ouvert sur une chaîne de commande ou un objet de **commande** qui retourne une ligne de données à partir du fournisseur. Cela permet d’améliorer les performances avec les fournisseurs MDAC 2,6. Consultez [Open, méthode (ADO record)](../reference/ado-api/open-method-ado-record.md).

## <a name="ado-25"></a>ADO 2,5

 _Objet_ Record ADO 2,5 introduit l’objet **Record** pour représenter et gérer une ligne d’un **jeu d’enregistrements** ou d’un fournisseur de données, ou un objet qui encapsule des données semi-structurées, telles qu’un fichier ou un répertoire.

 _Objet_ Stream ADO 2,5 introduit également l’objet *et* Stream * * pour représenter un flux de données binaires ou de texte.

*Liaison d’URL* ADO 2,5 introduit l’utilisation d’une URL, comme alternative à une chaîne de connexion et un texte de commande, pour nommer les objets de banque de données. Une URL peut être utilisée avec les objets **Connection** et **Recordset** existants, ainsi qu’avec les nouveaux objets **Record** et **Stream** .

*Fournisseurs de données prenant en charge la liaison d’URL* ADO 2,5 prend en charge les fournisseurs de OLE DB qui reconnaissent les schémas d’URL. Cela comprend OLE DB fournisseur pour la publication Internet, qui accède au système de fichiers Windows 2000 et reconnaît le schéma HTTP existant.
