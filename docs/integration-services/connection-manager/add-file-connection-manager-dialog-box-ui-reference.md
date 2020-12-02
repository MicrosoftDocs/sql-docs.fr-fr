---
description: Référence de l'interface utilisateur de la boîte de dialogue Ajouter un gestionnaire de connexions de fichiers
title: Référence de l’interface utilisateur de la boîte de dialogue Ajouter un gestionnaire de connexions de fichiers | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.fileconnection.f1
helpviewer_keywords:
- Add File Connection Manager
ms.assetid: 9370bfb5-5993-4ad8-a9cd-2de53f320f34
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8096ccaf1fc92710e46970744a7a4f7cbe000700
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130601"
---
# <a name="add-file-connection-manager-dialog-box-ui-reference"></a>Référence de l'interface utilisateur de la boîte de dialogue Ajouter un gestionnaire de connexions de fichiers

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Utilisez la boîte de dialogue **Ajouter un gestionnaire de connexions de fichiers** pour définir une connexion à un groupe de fichiers ou dossiers.  
  
 Pour en savoir plus sur le gestionnaire de connexions pour plusieurs fichiers, consultez [Multiple Files Connection Manager](../../integration-services/connection-manager/multiple-files-connection-manager.md).  
  
> [!NOTE]  
>  Les tâches et composants de flux de données de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] n'utilisent pas le gestionnaire de connexions de fichiers multiples. Toutefois, vous pouvez utiliser ce gestionnaire de connexions dans la tâche de script ou le composant Script.  
  
## <a name="options"></a>Options  
 **Type d’utilisation**  
 Spécifiez le type de fichiers à utiliser avec le gestionnaire de connexions pour plusieurs fichiers.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Créer des fichiers**|Le gestionnaire de connexions va créer les fichiers.|  
|**Fichiers existants**|Le gestionnaire de connexions va utiliser des fichiers existants.|  
|**Créer des dossiers**|Le gestionnaire de connexions va créer les dossiers.|  
|**Dossiers existants**|Le gestionnaire de connexions va utiliser des dossiers existants.|  
  
 **Fichiers / Dossiers**  
 Affichez les fichiers ou dossiers que vous avez ajoutés à l'aide des boutons décrits ci-après.  
  
 **Ajouter**  
 Ajoutez un fichier à l’aide de la boîte de dialogue **Sélectionner les fichiers** ou ajoutez un dossier à l’aide de la boîte de dialogue **Rechercher un dossier** .  
  
 **Modifier**  
 Sélectionnez un fichier ou dossier, puis remplacez-le par un autre fichier ou dossier à l’aide de la boîte de dialogue **Sélectionner les fichiers** ou **Rechercher un dossier** .  
  
 **Remove**  
 Sélectionnez un fichier ou dossier, puis supprimez-le de la liste à l’aide du bouton **Supprimer** .  
  
 **Boutons de direction**  
 Sélectionnez un fichier ou dossier, puis utilisez les boutons de direction pour le déplacer vers le haut ou vers le bas afin de spécifier l'ordre d'accès.  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des erreurs et des messages propres à Integration Services](../../integration-services/integration-services-error-and-message-reference.md)  
  
  
