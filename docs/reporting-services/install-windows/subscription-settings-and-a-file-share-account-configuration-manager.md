---
title: Paramètres d’abonnement et compte de partage de fichiers (Gestionnaire de configuration) | Microsoft Docs
description: Utilisez la page Paramètres d’abonnement du Gestionnaire de configuration du serveur de rapports afin de configurer un compte de partage de fichiers pour les serveurs de rapports en mode natif et les abonnements de partage de fichiers.
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
f1_keywords:
- SQL13.rsconfigtool.subscriptionsettings.F1
ms.assetid: fefa7bdb-b5f2-4db7-b91c-b58869279f3c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 90e585e887e9415a8cf1f9d0104595547f2d1c87
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171981"
---
# <a name="subscription-settings-and-a-file-share-account-report-server-configuration-manager"></a>Paramètres d’abonnement et compte de partage de fichiers (Gestionnaire de configuration du serveur de rapports)
  Utilisez la page **Paramètres d'abonnement** du Gestionnaire de configuration [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] afin de configurer un compte de partage de fichiers pour les serveurs de rapports en mode natif et les abonnements de partage de fichiers. Le compte de partage de fichiers vous permet d'utiliser un jeu unique d'informations d'identification dans plusieurs abonnements qui fournissent des rapports à un partage de fichiers. Au moment de modifier les informations d'identification, vous configurez la modification pour le compte de partage de fichiers et vous n'avez pas besoin de mettre à jour chaque abonnement individuel.  
  
 Les abonnements de partage de fichiers [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] incluent deux flux de travail :  
  
-   (Nouveauté de la version [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] ) Votre administrateur [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] peut configurer un compte de partage de fichiers unique, utilisé pour un ou plusieurs abonnements. Une fois l’option **Spécifier un compte de partage de fichiers** configurée, les utilisateurs sélectionnent l’option **Utiliser le compte Partage de fichiers** sur les pages de configuration de chaque abonnement.  
  
-   Configurez les abonnements individuels avec des informations d'identification spécifiques pour le partage de fichiers de destination.  
  
-   Vous pouvez également combiner les deux approches de sorte que certains abonnements de partage de fichiers utilisent le compte de partage de fichiers central tandis que d’autres abonnements utilisent des informations d’identification spécifiques.  
  
 [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif.  
  
## <a name="specify-a-file-share-account"></a>Spécifier un compte de partage de fichiers  
 Si cette option est sélectionnée, vous pouvez fournir un compte pour accéder aux partages de fichiers à partir du serveur de rapports. Si vous configurez le compte de partage de fichiers, tous les utilisateurs peuvent sélectionner le compte pour tous les abonnements configurés pour envoyer des rapports à un partage de fichiers. Si cette option n'est pas sélectionnée, le compte de partage de fichiers n’est disponible sur **aucun** abonnement.  
  
 Notez que vous devez vérifier que le compte que vous configurez comme compte de partage de fichiers dispose de droits d’accès en lecture et en écriture sur tous les partages de fichiers dont les utilisateurs se serviront pour la remise par partage de fichiers.  
  
 L'illustration suivante montre ce que voient les utilisateurs dans les abonnements configurés pour la remise par partage de fichiers. L’option **Utiliser le compte Partage de fichiers** est désactivée si aucun compte de partage de fichiers n’a été configuré.  
  
 ![compte de partage de fichiers du gestionnaire de configuration](../../reporting-services/install-windows/media/ssrs-fileshare-account.png "compte de partage de fichiers du gestionnaire de configuration")  
  
## <a name="prevent-privilege-escalation-or-elevated-privileges"></a>Empêcher l’escalade des privilèges ou des privilèges élevés  
  
> [!IMPORTANT]
> Le compte de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] contrôle la remise des abonnements et interagit avec le compte utilisé pour les abonnements de partage de fichiers. Les fonctionnalités de sécurité Windows limitent les combinaisons 1) du compte de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et 2) du compte utilisé pour les comptes de partage de fichiers. Par exemple, si un compte de système d’exploitation intégré est utilisé comme compte de partage de fichiers, le compte de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] doit être un autre compte de service avec des autorisations d’emprunt d’identité. Si un compte de partage de fichiers explicite et un mot de passe sont configurés, le compte de partage de fichiers nécessite un droit d'ouverture de session sur l'ordinateur exécutant le service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Si le compte de partage de fichiers n'a pas les autorisations requises, les abonnements qui utilisent le compte de partage de fichiers échouent et un message d'erreur semblable à ce qui suit s’affiche :  
>   
>  `"Failure writing file {file} : An impersonation error occurred using the security context of the current user."`  
  
## <a name="powershell-sample-to-audit-use-of-the-file-share-account"></a>Exemple PowerShell pour l’audit de l'utilisation du compte de partage de fichiers  
 Exécutez le script Windows PowerShell suivant pour répertorier tous les abonnements [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] configurés pour utiliser le **compte de partage de fichiers**. Mettez à jour `SERVERNAME` à l’aide d’une valeur appropriée pour votre serveur de rapports.  
  
```  
# get all file share subscriptions using the default file share account  
$extensionNameMatch = "Report Server FileShare"  
$extensionSettingMatch = "DEFAULTCREDENTIALS"  
$valueMatch = "True"  
  
# filter for subscriptions that have a given extension setting  
filter script:extensionSettingFilter  
{  
    # subscription must match the extension name  
    if($_.DeliverySettings.Extension -eq $extensionNameMatch)  
    {  
        # locate the extension parameter of interest  
        ForEach($extensionParameter in $_.DeliverySettings.ParameterValues)  
        {  
            # if the setting has the desired value, return the subscription  
            if($extensionParameter.Name -eq $extensionSettingMatch -and $extensionParameter.Value -eq $valueMatch)  
            {  
                $_  
                break  
            }  
        }  
    }  
}  
  
$rs2010 = New-WebServiceProxy -Uri "https:// SERVERNAME/ReportServer/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential;  
$subscriptions = $rs2010.ListSubscriptions("/");  
  
Write-Host "----- File share subscriptions using the default file share account ----";  
Write-Host "-------------------------------------------------------------------------- ";  
$subscriptions | extensionSettingFilter | select report, owner, status, lastexecuted, description, subscriptionid | format-table -auto  
  
```  
  
 La sortie du script ressemble à ce qui suit :  
  
 `----- File share subscriptions using the default file share account ----`  
  
 `-----------------------------------------------------------------------------------------------------`  
  
 `Report                    Owner          Status   LastExecuted         SubscriptionID`  
  
 `------------------------  -------------- -------- -------------------- ------------------------------------`  
  
 `Aworks_sales_by_territory DOMAIN\UserName Disabled 10/5/2014 1:04:04 PM e843bc2b-023e-45a3-ba23-22f9dc9a0934`  
  
## <a name="see-also"></a>Voir aussi  
 [Remise par partage de fichiers dans Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md)   
 [Créer et gérer des abonnements pour les serveurs de rapports en mode natif](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md)
  
  
