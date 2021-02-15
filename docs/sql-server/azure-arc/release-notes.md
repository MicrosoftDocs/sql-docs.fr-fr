---
title: Azure Arc enabled SQL Server- Notes de publication
description: Dernières notes de publication
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 12/08/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: f53731685f5ba1723ebdd8d20064342808205566
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070304"
---
# <a name="release-notes---azure-arc-enabled-sql-server-preview"></a>Notes de publication - Azure Arc enabled SQL Server (préversion)

> [!NOTE]
> En tant que fonctionnalité en préversion, la technologie présentée dans cet article est soumise aux [conditions d’utilisation supplémentaires des préversions de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="december-2020"></a>Décembre 2020

### <a name="breaking-change"></a>Modification avec rupture

Cette version inaugure un [fournisseur de ressources](/azure/azure-resource-manager/management/azure-services-resource-providers) mis à jour appelé `Microsoft.AzureArcData`. Pour pouvoir continuer à utiliser SQL Server avec Azure Arc, vous devez inscrire ce fournisseur de ressources. Consultez les instructions d’inscription du fournisseur de ressources dans la section [Prérequis](connect.md#prerequisites).

Si vous disposez de ressources SQL Server-Azure Arc existantes, suivez ces étapes pour les migrer vers l’espace de noms Microsoft.AzureArcData.

1. Lancez [Cloud Shell](https://shell.azure.com/). Pour en savoir plus, [découvrez plus en détail PowerShell dans Cloud Shell](/azure/cloud-shell/quickstart-powershell).

2. Chargez le script dans l’interpréteur de commandes à l’aide de la commande suivante :

    ```console
    curl https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/manage/azure-arc-enabled-sql-server/migrate-to-azure-arc-data.ps1 -o migrate-to-azure-arc-data.ps1
    ```
3. Exécutez le script.  

    ```console
   ./migrate-to-azure-arc-data.ps1
    ```

> [!NOTE]
> - Pour coller les commandes dans l’interpréteur de commandes, utilisez `Ctrl-Shift-V` sur Windows ou `Cmd-v` sur MacOS.
> - La commande `curl` copie le script directement dans le dossier de base associé à votre session Cloud Shell.
> - Le script vous invite à entrer le nom du groupe de ressources et à imprimer un message une fois la migration terminée.

### <a name="other-changes"></a>Autres modifications

* La propriété *TCPPorts* du type de ressource **SQL Server – Azure Arc** a été renommée *TCPStaticPorts*
* Les autorisations nécessaires ne sont pas aussi larges qu’auparavant. Pour plus d’informations, consultez la section [Autorisations nécessaires](overview.md#required-permissions).

### <a name="known-issues"></a>Problèmes connus

* La propriété *CreateTime* n’est pas ajoutée aux ressources nouvellement créées dans l’espace de noms AzureArcData, ni aux ressources **SQL Server – Azure Arc**.

## <a name="october-2020"></a>Octobre 2020

La mise à jour d’octobre inclut les améliorations suivantes :

* Le panneau Inscrire Azure Arc enabled SQL Server comprend désormais l’onglet **Étiquettes**. Les étiquettes sont incluses dans le script d’inscription et sont reflétées dans les ressources **SQL Server - Azure Arc**. Pour plus d’informations, consultez [Connecter votre SQL Server à Azure Arc](connect.md).

* L’entrée **Intégrité de l’environnement** prend désormais en charge l’activation de **SQL Assessment** à partir du portail par le biais du déploiement d’un *CustomScriptExtension*. Pour plus d’informations, consultez [Configurer SQL Assessment](assess.md#run-on-demand-sql-assessment).

### <a name="known-issues"></a>Problèmes connus

Les problèmes suivants concernent la version d’octobre :

* Pour connecter des instances SQL Server à Azure Arc, un compte avec un grand nombre d’autorisations est nécessaire. Pour plus d’informations, consultez [Autorisations requises](overview.md#required-permissions).

## <a name="september-2020"></a>Septembre 2020

Azure Arc enabled SQL Server est disponible en préversion publique. Il étend les services Azure aux instances SQL Server hébergées en dehors d’Azure dans le centre de données du client, à la périphérie ou dans un environnement multicloud.

Pour plus d’informations, consultez [Vue d’ensemble d’Azure Arc enabled SQL Server](overview.md)

### <a name="known-issues"></a>Problèmes connus

Les problèmes suivants concernent la version de septembre :

* Le panneau **Inscrire Azure Arc enabled SQL Server** ne prend pas en charge la configuration d’étiquettes personnalisées. Pour ajouter des étiquettes personnalisées, ouvrez la ressource **SQL Server - Azure Arc** après l’inscription et modifiez les étiquettes dans la page **Vue d’ensemble**.

* Pour connecter des instances SQL Server à Azure Arc, un compte avec un grand nombre d’autorisations est nécessaire. Pour plus d’informations, consultez [Autorisations requises](overview.md#required-permissions).

## <a name="next-steps"></a>Étapes suivantes

**Vous voulez juste essayer ?**  Devenez vite opérationnel avec le [guide de démarrage rapide d’Azure Arc enabled SQL Server](https://aka.ms/AzureArcSqlServerJumpstart).