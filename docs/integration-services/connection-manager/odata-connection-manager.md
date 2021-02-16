---
description: Gestionnaire de connexions OData
title: Gestionnaire de connexions OData | Microsoft Docs
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.custom: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 3caa4372-aff3-4c0f-9ecd-97870948b8d0
f1_keywords:
- sql13.dts.designer.odatasource.connectionmanager.f1
- sql13.dts.designer.odataconnectionmanager.f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7d02fbd8a7029a794b933053bec5f676646eced1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338418"
---
# <a name="odata-connection-manager"></a>Gestionnaire de connexions OData

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


 Connectez-vous à une source de données OData via un gestionnaire de connexions OData. Un composant source OData utilise un gestionnaire de connexions ODatase pour se connecter à une source de données OData et consommer les données du service. Pour plus d'informations, consultez [OData Source](../../integration-services/data-flow/odata-source.md).  
  
## <a name="adding-an-odata-connection-manager-to-an-ssis-package"></a>Ajout d’un gestionnaire de connexions OData à un package SSIS  
 Vous pouvez ajouter un nouveau gestionnaire de connexions OData à un package SSIS de trois manières :  
  
-   Cliquez sur le bouton **Nouveau...** dans **Éditeur de source OData**.  
  
-   Cliquez avec le bouton droit sur le dossier **Gestionnaires de connexions** dans **l’Explorateur de solutions**, puis cliquez sur **Nouveau gestionnaire de connexions**. Sélectionnez **ODATA** pour **Type du gestionnaire de connexions**.  
  
-   Cliquez avec le bouton droit dans le volet **Gestionnaires de connexions** au bas du concepteur de packages, puis sélectionnez **Nouvelle connexion**. Sélectionnez **ODATA** pour **Type du gestionnaire de connexions**.  
  
## <a name="connection-manager-authentication"></a>Authentification du gestionnaire de connexions  
 Le gestionnaire de connexions OData prend en charge cinq modes d’authentification.  
  
-   Authentification Windows  
  
-   Authentification de base (avec nom d’utilisateur et mot de passe)  

-   Microsoft Dynamics AX Online (avec nom d’utilisateur et mot de passe)
  
-   Microsoft Dynamics CRM Online (avec nom d’utilisateur et mot de passe)
  
-   Microsoft Online Services (avec nom d’utilisateur et mot de passe)  
  
Pour un accès anonyme, sélectionnez l’option Authentification Windows.  

Pour vous connecter à Microsoft Dynamics AX Online ou Microsoft Dynamics CRM Online, vous ne pouvez pas utiliser l’option d’authentification **Microsoft Online Services**. Par ailleurs, vous ne pouvez utiliser aucun option qui est configurée pour l’authentification multifacteur. L’authentification moderne n’est pas prise en charge pour l’instant. 
  
### <a name="specifying-and-securing-credentials"></a>Spécification des informations d'identification  
 Si votre service OData requiert l’authentification de base, spécifiez un nom d’utilisateur et un mot de passe dans [OData Connection Manager Editor](). Les valeurs que vous entrez dans l'éditeur sont conservées dans le package. La valeur du mot de passe est chiffrée selon le niveau de protection du package.  
  
 Il existe plusieurs façons de paramétrer les valeurs nom d’utilisateur et mot de passe ou de les stocker en dehors du package. Vous pouvez par exemple utiliser des paramètres ou définir directement les propriétés du gestionnaire de connexions lorsque vous exécutez le package à partir de SQL Server Management Studio.  
  
## <a name="odata-connection-manager-properties"></a>Propriétés du gestionnaire de connexions OData  
 Le tableau suivant répertorie les propriétés du gestionnaire de connexions OData.  
  
|Property|Description|  
|-|-|  
|Url|URL vers le document de service.|  
|UserName|Nom d’utilisateur à utiliser pour l’authentification, si nécessaire.|  
|Mot de passe|Mot de passe à utiliser pour l’authentification, si nécessaire.|  
|ConnectionString|Inclut d’autres propriétés du gestionnaire de connexions.|  
  
## <a name="odata-connection-manager-editor"></a>Éditeur du gestionnaire de connexions OData
  La boîte de dialogue **Éditeur du gestionnaire de connexions OData** vous permet d’ajouter ou de modifier une connexion à une source de données OData existante.  
  
### <a name="options"></a>Options  
 **Nom du gestionnaire de connexions**  
 Nom du gestionnaire de connexions.  
  
 **Emplacement du document de service**  
 URL du service OData. Par exemple : https://services.odata.org/V3/Northwind/Northwind.svc/.  
  
 **Authentification**  
Sélectionnez l’une des options suivantes :
-   **Authentification Windows**. Pour l’accès anonyme, sélectionnez cette option.
-   **Authentification de base** 
-   **Microsoft Dynamics AX Online** pour Dynamics AX Online
-   **Microsoft Dynamics CRM Online** pour Dynamics CRM Online
-   **Microsoft Online Services** pour Microsoft Online Services

Si vous sélectionnez une option autre que Authentification Windows, entrez le **nom d’utilisateur** et le **mot de passe**. 

Pour vous connecter à Microsoft Dynamics AX Online ou Microsoft Dynamics CRM Online, vous ne pouvez pas utiliser l’option d’authentification **Microsoft Online Services**. Par ailleurs, vous ne pouvez utiliser aucun option qui est configurée pour l’authentification multifacteur.

 **Tester la connexion**  
 Cliquez sur ce bouton pour tester la connexion à la source OData.