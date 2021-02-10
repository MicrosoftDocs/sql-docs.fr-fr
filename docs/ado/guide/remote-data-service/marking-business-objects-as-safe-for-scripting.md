---
description: Marquage d’objets métier comme sûrs pour l’écriture de scripts
title: Marquage des objets métier comme sécurisés pour l’écriture de scripts | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- business objects in RDS [ADO]
ms.assetid: 0be98d1a-ab3d-4dce-a166-dacda10d154a
author: rothja
ms.author: jroth
ms.openlocfilehash: 1866d72f438e263ec6cdb1f66cccb247d460a270
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031921"
---
# <a name="marking-business-objects-as-safe-for-scripting"></a>Marquage d’objets métier comme sûrs pour l’écriture de scripts
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 Pour garantir un environnement Internet sécurisé, vous devez marquer tous les objets métier instanciés avec les [services Bureau à distance. ](../../reference/rds-api/dataspace-object-rds.md) Méthode [CreateObject](../../reference/rds-api/createobject-method-rds.md) de l’objet DataSpace comme « sécurisé pour les scripts ». Vous devez vous assurer qu’ils sont marqués comme tels dans la zone de licence du Registre système avant de pouvoir les utiliser dans DCOM.  
  
> [!NOTE]
>  Les objets métier marqués comme « sécurisés pour l’écriture de scripts » ou sécurisés pour l’initialisation peuvent être instanciés et initialisés par quiconque sur le réseau. Le marquage d’un objet métier « sécurisé pour l’écriture de scripts » ne le rend pas sécurisé. Il est essentiel de s’assurer que les objets métier sont codés avec la sécurité la plus élevée pour s’assurer que ces objets ne présentent pas un point d’accès non protégé pour les données sensibles.  
  
 Pour marquer manuellement votre objet métier comme sécurisé pour l’écriture de scripts, créez un fichier texte avec une extension. reg qui contient le texte suivant. Dans cet exemple, \<*MyActiveXGUID*> est le numéro de GUID hexadécimal de votre objet métier. Les deux nombres suivants activent la fonctionnalité de sécurisation de script :  
  
```console
[HKEY_CLASSES_ROOT\CLSID\<MyActiveXGUID>\Implemented   
Categories\{7DD95801-9882-11CF-9FA9-00AA006C42C4}]  
[HKEY_CLASSES_ROOT\CLSID\<MyActiveXGUID>\Implemented   
Categories\{7DD95802-9882-11CF-9FA9-00AA006C42C4}]  
```  
  
 Enregistrez le fichier et fusionnez-le dans votre registre à l’aide de l’éditeur du registre ou en double-cliquant sur le fichier. reg dans l’Explorateur Windows.  
  
 Les objets d’entreprise créés dans Microsoft Visual Basic peuvent être automatiquement marqués comme « sécurisés pour les scripts » à l’aide de l’Assistant package et déploiement. Lorsque l’Assistant vous demande de spécifier des paramètres de sécurité, sélectionnez **sécurisé pour l’initialisation** et **sécurisé pour l’écriture de scripts**.  
  
 À la dernière étape, l’Assistant Installation de l’application crée un fichier. htm et un fichier. cab. Vous pouvez ensuite copier ces deux fichiers sur l’ordinateur cible et double-cliquer sur le fichier. htm pour charger la page et inscrire correctement le serveur.  
  
 Étant donné que l’objet métier sera installé par défaut dans le répertoire Windows\System32\Occache, déplacez-le vers le répertoire Windows\System32 et **Modifiez \\** la \<*MyActiveXGUID*> \\ clé de RegistreHKEY_CLASSES_ROOT\CLSID **InprocServer32** pour qu’elle corresponde au chemin d’accès correct.