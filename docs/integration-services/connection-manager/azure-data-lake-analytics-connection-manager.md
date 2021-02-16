---
title: Gestionnaire de connexions Azure Data Lake Analytics | Microsoft Docs
description: Un package SQL Server Integration Services (SSIS) peut utiliser le gestionnaire de connexions Azure Data Lake Analytics pour se connecter à un compte Data Lake Analytics.
ms.custom: ''
ms.date: 05/18/2018
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPADLSCM.F1
- sql14.dts.designer.afpadlscm.f1
ms.assetid: f4c44553-0f08-4731-ac47-7534990b8c8d
author: yanancai
ms.author: yanacai
ms.reviewer: maghan
ms.openlocfilehash: ff7ad8e742469f7afea8770a7323352e99ee41ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338455"
---
# <a name="azure-data-lake-analytics-connection-manager"></a>Gestionnaire de connexions Azure Data Lake Analytics

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Un package SQL Server Integration Services (SSIS) peut utiliser le gestionnaire de connexions Azure Data Lake Analytics pour se connecter à un compte Data Lake Analytics avec l’un de ces deux types d’authentification suivants :
-   Identité de l’utilisateur Azure Active Directory (Azure AD)
-   Identité du service Azure AD 

Le gestionnaire de connexions Data Lake Analytics est un composant de [SQL Server Integration Services (SSIS) Feature Pack pour Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).
 
## <a name="configure-the-connection-manager"></a>Configurer le gestionnaire de connexions

1. Dans la boîte de dialogue **Ajout d’un gestionnaire de connexions SSIS**, sélectionnez **AzureDataLakeAnalytics** > **Ajouter**. La boîte de dialogue **Éditeur du gestionnaire de connexions Azure Data Lake Analytics** s’ouvre.
  
2. Dans le champ **Nom de compte ADLA** de la boîte de dialogue **Éditeur du gestionnaire de connexions Azure Data Lake Analytics**, indiquez le nom du compte Data Lake Analytics. Exemple : myadlaaccountname.
  
3. Dans le champ **Authentification**, choisissez le type d’authentification adapté pour accéder aux données dans Data Lake Analytics.

   a. Si vous sélectionnez l’option d’authentification **Identité de l’utilisateur Azure AD** :
   
      i. Renseignez les champs **Nom d’utilisateur** et **Mot de passe**.    
      ii. Sélectionnez **Tester la connexion** pour tester la connexion. Si l’administrateur client ou vous-même n’avez pas déjà autorisé SSIS à accéder à votre compte Data Lake Analytics, sélectionnez **Accepter** à l’invite. Pour plus d’informations sur cette procédure de consentement, consultez [Intégration d’applications dans Azure Active Directory](/azure/active-directory/manage-apps/plan-an-application-integration#integrating-applications-with-azure-ad).
    
   > [!NOTE] 
   > Lorsque vous sélectionnez l’option d’authentification **Identité de l’utilisateur Azure AD**, l’authentification multifacteur et l’authentification de compte Microsoft ne sont pas prises en charge.
    
   b. Si vous sélectionnez l’option d’authentification **Identité du service Azure AD** :
   
      i. Créez une application et un principal du service Azure AD pour accéder au compte Data Lake Analytics. Pour plus d’informations sur cette option d’authentification, consultez [Utiliser le portail pour créer une application Active Directory et un principal du service qui peuvent accéder aux ressources](/azure/azure-resource-manager/resource-group-create-service-principal-portal).    
      ii. Affectez les autorisations nécessaires pour permettre à cette application Azure AD d’accéder à votre compte Data Lake Analytics. Découvrez comment accorder des autorisations à votre compte Data Lake Analytics avec l’[Assistant Ajout d’utilisateurs](/azure/data-lake-analytics/data-lake-analytics-manage-use-portal#add-a-new-user).    
      iii. Renseignez les valeurs des champs **ID d’application**, **Clé d’authentification**, et **ID de locataire**.    
      iv. Sélectionnez **Tester la connexion** pour tester la connexion.  

4. Sélectionnez **OK** pour fermer la boîte de dialogue **Éditeur du gestionnaire de connexions Azure Data Lake Analytics**.  

## <a name="view-the-properties-of-the-connection-manager"></a>Afficher les propriétés du gestionnaire de connexions
Les propriétés du gestionnaire de connexions que vous avez créées apparaissent dans la fenêtre **Propriétés** .  
  
