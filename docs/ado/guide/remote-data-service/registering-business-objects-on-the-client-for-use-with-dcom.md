---
description: Inscription d’objets métier sur le client en vue d’une utilisation avec DCOM
title: Enregistrement d’objets métier sur le client pour une utilisation avec DCOM | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- business objects in RDS [ADO]
ms.assetid: 75a21910-607f-463a-ae18-a17130dafb7e
author: rothja
ms.author: jroth
ms.openlocfilehash: 708a59b8c931b723c8f4142f036849da4965fc13
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031821"
---
# <a name="registering-business-objects-on-the-client-for-use-with-dcom"></a>Inscription d’objets métier sur le client en vue d’une utilisation avec DCOM
Les objets métier personnalisés doivent s’assurer que le côté client peut mapper leur nom de programme (ProgId) à un identificateur (CLSID) qui peut être utilisé sur DCOM. Pour cette raison, le ProgID de l’objet DCOM doit se trouver dans le registre côté client et être mappé à l’ID de classe de l’objet métier côté serveur. Pour les autres protocoles pris en charge (HTTP, HTTPs et in-process), cela n’est pas nécessaire.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 Par exemple, si vous exposez un objet métier côté serveur appelé MyBObj avec un ID de classe spécifique, par exemple, « {00112233-4455-6677-8899-00AABBCCDDEE} », assurez-vous que les entrées suivantes sont ajoutées au registre côté client :  
  
```console
[HKEY_CLASSES_ROOT]  
\MyBObj\Clsid\(Default) "{00112233-4455-6677-8899-00AABBCCDDEE}"  
```