---
description: Configuration d’une DLL pour s’exécuter sur DCOM
title: Activation d’une DLL pour une exécution sur DCOM | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- DLL on DCOM in RDS [ADO]
- DCOM in RDS [ADO]
- business objects in RDS [ADO]
ms.assetid: 5f1c2205-191c-4fb4-9bd9-84c878ea46ed
author: rothja
ms.author: jroth
ms.openlocfilehash: 31aa1fde6ddce21bc314bf14aea3ea563567d4a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031971"
---
# <a name="enabling-a-dll-to-run-on-dcom"></a>Configuration d’une DLL pour s’exécuter sur DCOM
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 Les étapes suivantes décrivent comment permettre à un objet métier. dll d’utiliser DCOM et Microsoft Internet Information Services (HTTP) via les services de composants.  
  
1.  Créez un nouveau package vide dans le composant logiciel enfichable MMC services de composants.  
  
     Vous allez utiliser le composant logiciel enfichable MMC services de composants pour créer un package et ajouter la DLL à ce package. Cela rend le fichier. dll accessible via DCOM, mais il supprime l’accessibilité via IIS. (Si vous archivez le registre pour le fichier. dll, la clé **InProc** est maintenant vide ; la définition de l’attribut d’activation, expliquée plus loin dans cette rubrique, ajoute une valeur dans la clé **InProc** .)  
  
2.  Installez un objet métier dans le package.  
  
     -ou-  
  
     Importez l’objet [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) dans le package.  
  
3.  Affectez à l’attribut activation du package la valeur **dans le processus du créateur** (application de bibliothèque).  
  
     Pour rendre le fichier. dll accessible via DCOM et IIS sur le même ordinateur, vous devez définir l’attribut d’activation du composant dans le composant logiciel enfichable MMC services de composants. Une fois que vous avez défini l’attribut sur **dans le processus du créateur**, vous remarquerez qu’une clé de serveur **InProc** dans le registre a été ajoutée et pointe vers un fichier de substitution de services de composants.  
  
 Pour plus d’informations sur les services de composants (ou Microsoft Transaction service, si vous utilisez Windows NT) et sur la façon d’effectuer ces étapes, visitez le site Web Microsoft Transaction Server.