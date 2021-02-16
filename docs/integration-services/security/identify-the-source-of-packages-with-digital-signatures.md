---
description: Identifier la source de packages à l'aide de signatures numériques
title: Identifier la source de packages à l’aide de signatures numériques | Microsoft Docs
ms.custom: security
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.digitalsigning.f1
helpviewer_keywords:
- signing packages [Integration Services]
- certificates [Integration Services]
- packages [Integration Services], security
- security [Integration Services], certificates
- signing policies [Integration Services]
ms.assetid: a433fbef-1853-4740-9d5e-8a32bc4ffbb2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f4607ffe77d7fe93aff86d8dc707bb86e515d67d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340789"
---
# <a name="identify-the-source-of-packages-with-digital-signatures"></a>Identifier la source de packages à l'aide de signatures numériques

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Il est possible de signer un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] avec un certificat numérique pour identifier sa source. Après avoir signé un package avec un certificat numérique, vous pouvez utiliser [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour vérifier la signature numérique avant de charger le package. Pour vérifier la signature à l’aide d’ [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vous devez définir une option dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ou dans l’utilitaire **dtexec** (dtexec.exe), ou définir une valeur de Registre facultative.  
  
## <a name="sign-a-package-with-a-digital-certificate"></a>Signer un package à l’aide d’un certificat numérique  
 Avant de pouvoir signer un package avec un certificat numérique, vous devez obtenir ou créer le certificat. Une fois que vous possédez le certificat, vous pouvez l'utiliser pour signer le package. Pour plus d’informations sur la façon d’obtenir un certificat et de signer un package avec ce certificat, consultez [Signer un package à l’aide d’un certificat numérique](#cert).  
  
## <a name="set-an-option-to-check-the-package-signature"></a>Définir une option pour vérifier la signature d’un package  
 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] et l’utilitaire **dtexec** proposent tous les deux une option qui configure [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour la vérification de la signature numérique d’un package signé. L’utilisation de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ou de l’utilitaire **dtexec** dépend de si vous souhaitez vérifier tous les packages ou simplement des packages spécifiques :  
  
-   Pour vérifier la signature numérique de tous les packages avant de charger les packages au moment de la conception, définissez l'option **Vérifier la signature numérique lors du chargement d'un package** dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Cette option est un paramètre global pour tous les packages dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].
  
-   Pour vérifier la signature numérique d’un package individuel, spécifiez l’option **/VerifyS[igned]** quand vous utilisez l’utilitaire **dtexec** pour exécuter le package. Pour plus d'informations, consultez [Utilitaire dtexec](../../integration-services/packages/dtexec-utility.md).  
  
## <a name="set-a-registry-value-to-check-package-signature"></a>Définir une valeur du Registre pour vérifier la signature d’un package  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] prend également en charge une valeur de Registre facultative, **BlockedSignatureStates**, que vous pouvez utiliser pour gérer la stratégie de chargement des packages signés et non signés d’une organisation. La valeur de Registre peut empêcher le chargement de packages si les packages ne sont pas signés ou s'ils possèdent des signatures non valides ou non approuvées. Pour plus d’informations sur la définition de cette valeur de Registre, consultez [Implémenter une stratégie de signature en définissant une valeur du Registre](#registry).  
  
> **REMARQUE :** La valeur de Registre **BlockedSignatureStates** facultative peut spécifier un paramètre plus restrictif que l’option de signature numérique définie dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ou au niveau de la ligne de commande **dtexec** . Dans cette situation, le paramètre du Registre plus restrictif a priorité sur les autres paramètres.  

## <a name="implement-a-signing-policy-by-setting-a-registry-value"></a><a name="registry"></a> Implémenter une stratégie de signature en définissant une valeur du Registre
  Vous pouvez utiliser une valeur du Registre facultative pour gérer la stratégie d'une organisation pour charger des packages signés ou non signés. Si vous utilisez cette valeur du Registre, vous devez créer cette valeur du Registre sur tous les ordinateurs sur lesquels les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] s'exécuteront et sur lesquels vous souhaitez appliquer la stratégie. Une fois la valeur du Registre définie, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] vérifiera les signatures avant de charger les packages.  
  
 La procédure présentée dans cette rubrique explique comment ajouter la valeur DWORD facultative **BlockedSignatureStates** à la clé de Registre HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\100\SSIS. La valeur de données dans **BlockedSignatureStates** détermine si un package doit être bloqué s’il possède une signature non approuvée ou non valide, ou s’il n’est pas signé. En ce qui concerne l’état des signatures utilisées pour signer les packages, la valeur du Registre **BlockedSignatureStates** emploie les définitions suivantes :  
  
-   Une *signature valide* est une signature qui peut être lue correctement.  
  
-   Une *signature non valide* est une signature dont la somme de contrôle déchiffrée (hachage unidirectionnel du code de package chiffré par une clé privée) ne correspond pas à la somme de contrôle déchiffrée calculée dans le cadre du chargement des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
-   Une *signature approuvée* est une signature créée à l’aide d’un certificat numérique signé par une autorité de certification racine de confiance. Ce paramètre n'exige pas la recherche du signataire dans la liste Éditeurs approuvés de l'utilisateur.  
  
-   Une *signature non approuvée* est une signature qui ne peut pas être vérifiée en tant que signature émise par une autorité de certification racine de confiance ou bien une signature qui n’est pas actuelle.  
  
 Le tableau suivant répertorie les valeurs valides des données DWORD et leur stratégie associée.  
  
|Value|Description|  
|-----------|-----------------|  
|0|Pas de restriction administrative.|  
|1|Bloquer les signatures non valides.<br /><br /> Ce paramètre ne bloque pas les packages non signés.|  
|2|Bloquer les signatures non valides et non approuvées.<br /><br /> Ce paramètre ne bloque pas les packages non signés mais bloque les signatures générées automatiquement.|  
|3|Bloquer les signatures non valides et non approuvées et les packages non signés.<br /><br /> Ce paramètre bloque lui aussi les signatures générées automatiquement.|  
  
> [!NOTE]  
>  Le paramètre recommandé pour **BlockedSignatureStates** est 3. Ce paramètre offre une protection maximale contre des packages non signés ou des signatures non valides ou non approuvées. Néanmoins, ce paramètre recommandé peut ne pas convenir dans tous les cas. Pour plus d’informations sur la signature des ressources numériques, consultez la rubrique[Introduction to Code Signing](/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537361(v=vs.85))(Introduction à la signature du code) dans MSDN Library.  
  
### <a name="to-implement-a-signing-policy-for-packages"></a>Pour implémenter une stratégie de signature pour des packages  
  
1.  Dans le menu **Démarrer** , cliquez sur **Exécuter**.  
  
2.  Dans la boîte de dialogue Exécuter, tapez **Regedit** et cliquez sur **OK**.  
  
3.  Localisez la clé de registre HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\100\SSIS.  
  
4.  Cliquez avec le bouton droit sur **MSDTS**, pointez sur **Nouveau**, puis cliquez sur **Valeur DWORD**.  
  
5.  Mettez à jour le nom de la nouvelle valeur en utilisant **BlockedSignatureStates**.  
  
6.  Cliquez avec le bouton droit sur **BlockedSignatureStates** et cliquez sur **Modifier**.  
  
7.  Dans la boîte de dialogue **Édition de la valeur DWORD** , tapez la valeur 0, 1, 2 ou 3.  
  
8.  Cliquez sur **OK**.  
  
9. Dans le menu **Fichier** , cliquez sur **Quitter**.    

## <a name="sign-a-package-by-using-a-digital-certificate"></a><a name="cert"></a> Signer un package à l’aide d’un certificat numérique
  Cette rubrique décrit comment signer un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] à l'aide d'un certificat numérique. Vous pouvez utiliser une signature numérique avec d'autres paramètres pour empêcher le chargement et l'exécution d'un package non valide.  
  
 Avant de pouvoir signer un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vous devez effectuer les tâches suivantes :  
  
-   Créez ou obtenez une clé privée à associer au certificat et stockez cette clé privée sur l'ordinateur local.  
  
-   Obtenez un certificat en vue de la signature du code à partir d'une autorité de certification approuvée. Vous pouvez utiliser l'une des méthodes suivantes pour obtenir ou créer un certificat :  
  
    -   Obtenez un certificat à partir d'une autorité de certification commerciale publique qui émet des certificats.  
  
    -   Obtenez un certificat à partir d'un serveur de certificats, qui permet à une organisation d'émettre des certificats de façon interne. Vous devez ajouter le certificat racine utilisé pour signer le certificat dans le magasin **Autorités de certification racines de confiance** . Pour ajouter le certificat racine, vous pouvez utiliser le composant logiciel enfichable MMC ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console) Certificats. Pour plus d'informations, consultez la rubrique «[Services de certificats](/windows/win32/seccrypto/certificate-services)» dans MSDN Library (éventuellement en anglais).  
  
    -   Créez votre propre certificat à des fins de test uniquement. L'outil de création de certificat Makecert.exe génère des certificats X.509 à des fins de tests. Pour plus d’informations, consultez la rubrique «[Outil Certificate Creation Tool (Makecert.exe)](/previous-versions/dotnet/netframework-2.0/bfsktky3(v=vs.80))» dans MSDN Library.  
  
     Pour plus d'informations sur les certificats, recherchez le composant logiciel enfichable Certificats dans l'aide en ligne. Pour plus d'informations sur la façon de signer des ressources numériques, consultez la rubrique «[Signature et vérification de code à l'aide d'Authenticode](/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537364(v=vs.85))» dans MSDN Library (éventuellement en anglais).  
  
-   Assurez-vous que le certificat a été activé pour la signature de code. Pour déterminer si un certificat est activé pour la signature du code, examinez les propriétés du certificat dans le composant logiciel enfichable Certificats.  
  
-   Stockez le certificat dans le magasin Personnel.  
  
 Après avoir terminé les tâches précédentes, vous pouvez utiliser la procédure suivante pour signer un package.  
  
### <a name="to-sign-a-package"></a>Pour signer un package  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package à signer.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , dans le menu **SSIS** , cliquez sur **Signature numérique**.  
  
4.  Dans la boîte de dialogue **Signature numérique** , cliquez sur **Signer**.  
  
5.  Dans la boîte de dialogue **Sélectionner un certificat** , sélectionnez un certificat.  
  
6.  (Facultatif) Cliquez sur **Afficher le certificat** pour afficher les informations sur le certificat.  
  
7.  Cliquez sur **OK** pour fermer la boîte de dialogue **Sélectionner un certificat** .  
  
8.  Cliquez sur **OK** pour fermer la boîte de dialogue **Signature numérique** .  
  
9. Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
     Bien que le package ait été signé, vous devez maintenant configurer [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour vérifier la signature numérique avant de charger le package.  

## <a name="digital-signing-dialog-box-ui-reference"></a><a name="signing_dialog"></a> Référence de l’IU de la boîte de dialogue Signature numérique
  Utilisez la boîte de dialogue **Signature numérique** pour signer un package par une signature numérique ou pour la supprimer. La boîte de dialogue **Signature numérique** est disponible via l'option **Signature numérique** dans le menu **SSIS** dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
 Pour plus d’informations, voir [Signer un package à l’aide d’un certificat numérique](#cert).  
  
### <a name="options"></a>Options  
 **Signe**  
 Cliquez sur cette option pour ouvrir la boîte de dialogue **Sélectionner un certificat** et sélectionner le certificat à utiliser.  
  
 **Remove**  
 Cliquez sur cette option pour supprimer la signature numérique.  

## <a name="see-also"></a>Voir aussi  
 [Packages Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-packages.md)   
 [Vue d’ensemble de la sécurité &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md)  
  
