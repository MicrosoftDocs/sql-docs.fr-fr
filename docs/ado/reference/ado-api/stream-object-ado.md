---
description: Stream, objet (ADO)
title: Stream, objet (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Stream
helpviewer_keywords:
- Stream object [ADO]
ms.assetid: 0514531f-009d-4519-abc3-d727014a39f1
author: rothja
ms.author: jroth
ms.openlocfilehash: b9baaa7e57ca10f1dc0e32d7fc5f477ee4f08100
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020368"
---
# <a name="stream-object-ado"></a>Stream, objet (ADO)
Représente un flux de données binaires ou de texte.  
  
 Dans les hiérarchies structurées en arborescence, comme un système de fichiers ou un système de messagerie, un [enregistrement](./record-object-ado.md) peut être associé à un flux binaire par défaut de bits qui contient le contenu du fichier ou du message électronique. Un objet de **flux** peut être utilisé pour manipuler des champs ou des enregistrements contenant ces flux de données. Un objet de **flux** peut être obtenu des manières suivantes :  
  
-   À partir d’une URL pointant vers un objet (généralement un fichier) contenant des données binaires ou de texte. Cet objet peut être un document simple, un objet **enregistrement** représentant un document structuré ou un dossier.  
  
-   En ouvrant l’objet de **flux** par défaut associé à un objet **enregistrement** . Vous pouvez obtenir le flux par défaut associé à un objet **enregistrement** lorsque l' **enregistrement** est ouvert, afin d’éliminer un aller-retour juste pour ouvrir le flux.  
  
-   En instanciant un objet de **flux** . Ces objets de **flux** peuvent être utilisés pour stocker des données dans le cadre de votre application. Contrairement à un **flux** associé à une URL ou au **flux** par défaut d’un **enregistrement**, un **flux** instancié n’a pas d’association avec une source sous-jacente par défaut.  
  
 Avec les méthodes et propriétés d’un objet de **flux** , vous pouvez effectuer les opérations suivantes :  
  
-   Ouvrez un objet de **flux** à partir d’un **enregistrement** ou d’une URL avec la méthode [Open](./open-method-ado-stream.md) .  
  
-   Ferme un **flux** à l’aide de la méthode [Close](./close-method-ado.md) .  
  
-   Entrer du texte ou des octets dans un **flux** à l’aide des méthodes [Write](./write-method.md) et [WRITETEXT](./writetext-method.md) .  
  
-   Lit les octets du **flux** à l’aide des méthodes [Read](./read-method.md) et [READTEXT](./readtext-method.md) .  
  
-   Écrivez toutes les données de **flux** toujours dans la mémoire tampon ADO vers l’objet sous-jacent avec la méthode [flush](./flush-method-ado.md) .  
  
-   Copiez le contenu d’un **flux** vers un autre **flux** à l’aide de la méthode [CopyTo](./copyto-method-ado.md) .  
  
-   Contrôlez la façon dont les lignes sont lues à partir du fichier source avec la méthode [SkipLine](./skipline-method.md)et la propriété [LineSeparator](./lineseparator-property-ado.md) .  
  
-   Déterminez la fin de la position du flux avec la propriété [EOS](./eos-property.md)et la méthode [SetEOS](./seteos-method.md) .  
  
-   Enregistrez et restaurez les données dans les fichiers à l’aide des méthodes [SaveToFile](./savetofile-method.md)et [LoadFromFile](./loadfromfile-method-ado.md) .  
  
-   Spécifiez le jeu de caractères utilisé pour stocker le **flux** avec la propriété [charset](./charset-property-ado.md) .  
  
-   Arrêtez une opération de **flux** asynchrone à l’aide de la méthode [Cancel](./cancel-method-ado.md) .  
  
-   Déterminez le nombre d’octets dans un **flux** à l’aide de la propriété [Size](./size-property-ado-stream.md) .  
  
-   Contrôle la position actuelle dans un **flux** à l’aide de la propriété [position](./position-property-ado.md) .  
  
-   Déterminez le type de données dans un **flux** à l’aide de la propriété [type](./type-property-ado-stream.md) .  
  
-   Déterminez l’état actuel du **flux** (fermé, ouvert ou en cours d’exécution) avec la propriété [State](./state-property-ado.md) .  
  
-   Spécifiez le mode d’accès du **flux** à l’aide de la propriété [mode](./mode-property-ado.md) .  
  
> [!NOTE]
>  Les URL utilisant le schéma http appellera automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../guide/data/absolute-and-relative-urls.md).  
  
 L’objet de **flux** est sécurisé pour l’écriture de scripts.  
  
 Cette section contient les rubriques suivantes :  
  
-   [Propriétés, méthodes et événements de l’objet Stream](./stream-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Enregistrements et flux](../../guide/data/records-and-streams.md)