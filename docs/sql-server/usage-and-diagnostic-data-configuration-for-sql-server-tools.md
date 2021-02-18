---
title: Configurer la collecte de données d’utilisation et de diagnostic pour les outils SQL Server (CEIP) | Microsoft Docs
description: En savoir plus sur les informations que le Programme d'amélioration de l'expérience utilisateur (CEIP) collecte auprès des utilisateurs pour améliorer les produits. Découvrez comment rejoindre ou quitter le programme dans SQL Server Data Tools (SSDT).
ms.custom: ''
ms.date: 10/21/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: baf3a205-a6bb-4564-8b64-3a0475bb9273
author: stevestein
ms.author: sstein
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016'
ms.openlocfilehash: 4c0dd7d5371d298a07439f193763b7b394b160bc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353560"
---
# <a name="configure-usage-and-diagnostic-data-collection-for-sql-server-tools-ceip"></a>Configurer les données d’utilisation et de diagnostic pour les outils SQL Server (CEIP)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Découvrez comment le Programme d’amélioration du produit (CEIP) aide Microsoft à identifier les méthodes permettant d’améliorer nos logiciels.  Vous pouvez configurer les outils afin de vous abonner ou d’annuler l’abonnement au programme à tout moment.  
  
> [!NOTE]  
> Pour obtenir une explication des pratiques de collecte et d’utilisation des données utilisateur des versions de Microsoft SQL Server, consultez cette [déclaration de confidentialité](./sql-server-privacy.md).  
  
## <a name="opting-in-and-out-of-ceip-for-sql-server-data-tools"></a>Inscription et désinscription au CEIP pour SQL Server Data Tools  

 Le Programme d’amélioration du produit (CEIP) est un programme conçu pour aider Microsoft à améliorer ses produits au fil du temps. Ce programme collecte des informations sur le matériel informatique et la manière dont les clients utilisent notre produit, sans interrompre les utilisateurs dans leurs tâches sur l’ordinateur. Les informations recueillies aident Microsoft à identifier les fonctionnalités à améliorer. Dans ce document, nous abordons l’inscription et la désinscription au Programme d’amélioration du produit SQL Server Data Tools (SSDT) pour Visual Studio 2017, Visual Studio 2015 et Visual Studio 2013.  Pour plus d’informations sur la désactivation du CEIP pour SQL Server, consultez [Désactivation de l’audit local pour SQL Server](usage-and-diagnostic-data-in-local-audit.md#turning-local-audit-on-or-off).

### <a name="choice-and-control-over--ceip-and-sql-server-data-tools-for-visual-studio-2017"></a>Choix et contrôle relatifs au Programme d’amélioration du produit et SQL Server Data Tools pour Visual Studio 2017

 SSDT pour Visual Studio 2017 est l’outil de modélisation de données qui est fourni avec SQL Server 2017. Il utilise les options du Programme d’amélioration du produit qui sont intégrées à Visual Studio 2017. Pour plus d’informations sur l’envoi des commentaires par le biais du Programme d’amélioration du produit dans Visual Studio 2017, consultez ce [document d’aide pour Visual Studio](https://www.visualstudio.com/docs/work/connect/give-feedback).  
  
 Pour les préversions de SQL Server 2017, le Programme d’amélioration du produit est activé par défaut. Vous pouvez le désactiver ou le réactiver, en suivant les instructions ci-dessous.  
  
 **Dans Visual Studio (s’applique aux installations linguistiques complètes de Visual Studio 2017)**  
  
 Si vous exécutez le programme d’installation de SSDT sur un ordinateur équipé de Visual Studio, seuls les modèles de projet SQL Server et Business Intelligence sont ajoutés. Pour ce scénario, les options de commentaires client fournies par Visual Studio peuvent être utilisées pour s’inscrire ou se désinscrire du Programme d’amélioration du produit (CEIP).  
  
1.  Démarrez Visual Studio.  
  
2.  Dans le menu Aide, sélectionnez **Envoyer des commentaires** > **Paramètres**.  
  
3.  Pour désactiver le Programme d’amélioration du produit (CEIP), cliquez sur **Non, je ne souhaite pas participer**, puis cliquez sur **OK**.  
  
     Pour activer le Programme d’amélioration du produit (CEIP), cliquez sur **Oui, je souhaite participer**, puis cliquez sur **OK**.  
  

  
 **Utilisation d’une stratégie basée sur le registre ou d’une stratégie de groupe**  
  
 Si vous exécutez le programme d’installation de SSDT sur un ordinateur qui ne dispose pas de Visual Studio 2017, seul le shell Visual Studio est installé. Le shell ne fournit pas d’options pour les commentaires client. Dans ce cas, une mise à jour du registre est la seule option de configuration du Programme d’amélioration du produit(CEIP)  
  
 Les clients en entreprise peuvent construire une stratégie de groupe pour s’abonner ou annuler l’abonnement en définissant une stratégie basée sur le registre pour SQL Server 2017.  
  
 La clé de Registre et les paramètres pertinents se présentent comme suit :  
  
- Système d’exploitation 64 bits, Clé = HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\VSCommon\15.0\SQM
- Système d’exploitation 32 bits, Clé = HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\VSCommon\15.0\SQM

Quand la stratégie de groupe est activée, Clé = HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\VisualStudio\SQM 

Entrée = OptIn

Valeur = (DWORD)
- 0 est refusé (désactivation du VSCEIP)
- 1 est accepté (activation du VSCEIP)

  
> [!CAUTION]  
>  Une modification incorrecte du Registre peut endommager gravement votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur. Vous pouvez également utiliser l'option de démarrage Dernière configuration valide connue si vous rencontrez des problèmes après l'application de modifications manuelles.  
  
 Pour en savoir plus sur les informations recueillies, traitées ou transmises par le Programme d’amélioration du produit, consultez la [déclaration de confidentialité](./sql-server-privacy.md).  
 
### <a name="choice-and-control-over-ceip-and-sql-server-data-tools-for-visual-studio-2015"></a>Choix et contrôle relatifs au Programme d’amélioration du produit et SQL Server Data Tools pour Visual Studio 2015  
 SSDT pour Visual Studio 2015 est l’outil de modélisation de données qui est fourni avec SQL Server 2016. Il utilise les options du Programme d’amélioration du produit qui sont intégrées à Visual Studio 2015. Pour plus d’informations sur l’envoi des commentaires par le biais du Programme d’amélioration du produit dans Visual Studio 2015, consultez ce [document d’aide pour Visual Studio](/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017).  
  
 Pour les versions préliminaires de SQL Server 2016, le Programme d’amélioration du produit est activé par défaut. Vous pouvez le désactiver ou le réactiver, en suivant les instructions ci-dessous.  
  
 **Dans Visual Studio (s’applique aux installations linguistiques complètes de Visual Studio 2015)**  
  
 Si vous exécutez le programme d’installation de SSDT sur un ordinateur équipé de Visual Studio, seuls les modèles de projet SQL Server et Business Intelligence sont ajoutés. Pour ce scénario, les options de commentaires client fournies par Visual Studio peuvent être utilisées pour s’inscrire ou se désinscrire du Programme d’amélioration du produit (CEIP).  
  
1.  Démarrez Visual Studio.  
  
2.  Dans le menu Aide, sélectionnez **Envoyer des commentaires** > **Paramètres**.  
  
3.  Pour désactiver le Programme d’amélioration du produit (CEIP), cliquez sur **Non, je ne souhaite pas participer**, puis cliquez sur **OK**.  
  
     Pour activer le Programme d’amélioration du produit (CEIP), cliquez sur **Oui, je souhaite participer**, puis cliquez sur **OK**.  
  

  
 **Utilisation d’une stratégie basée sur le registre ou d’une stratégie de groupe**  
  
 Si vous exécutez le programme d’installation de SSDT sur un ordinateur qui ne dispose pas de Visual Studio 2015, seul le shell Visual Studio est installé. Le shell ne fournit pas d’options pour les commentaires client. Dans ce cas, une mise à jour du registre est la seule option de configuration du Programme d’amélioration du produit(CEIP)  
  
 Les clients d’entreprises peuvent construire une stratégie de groupe pour s’abonner ou annuler l’abonnement en définissant une stratégie basée sur le registre pour SQL Server 2016.  
  
 La clé de Registre et les paramètres pertinents se présentent comme suit :  
  
 Key = HKEY_CURRENT_USER\Software\Microsoft\VSCommon\14.0\SQM  
  
 RegEntry name = OptIn  
  
 Type d'entrée DWORD :  
  
-   0 pour annuler l'abonnement  
  
-   1 pour s'abonner  
  
> [!CAUTION]  
>  Une modification incorrecte du Registre peut endommager gravement votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur. Vous pouvez également utiliser l'option de démarrage Dernière configuration valide connue si vous rencontrez des problèmes après l'application de modifications manuelles.  
  
 Pour en savoir plus sur les informations recueillies, traitées ou transmises par le Programme d’amélioration du produit, consultez la [déclaration de confidentialité](./sql-server-privacy.md).  
  
### <a name="choice-and-control-for-ceip-and-sql-server-data-tools---bi-ssdt-bi"></a>Choix et contrôle relatifs au CEIP et SQL Server Data Tools - BI (SSDT-BI)  
 Si vous utilisez SSDT-BI, vous aurez l’opportunité de participer au Programme d’amélioration du produit durant l’installation. Ultérieurement, les modifications de la configuration du Programme d’amélioration du produit (CEIP) pour SSDT-BI peuvent être effectuées à l’aide des outils clients ou en modifiant les paramètres du registre.  
  
 **Dans SSDT et SSDT-BI pour Visual studio 2013**  
  
1.  Démarrez l’outil et ouvrez un projet nouveau ou existant pour Analysis Services ou Integration Services.  
  
2.  Dans le menu Aide, sélectionnez **Options pour les commentaires client Microsoft SQL Server**.  
  
3.  Pour désactiver le Programme d’amélioration du produit, cliquez sur **Non, je ne souhaite pas participer**.  
  
     Pour activer le Programme d’amélioration du produit, cliquez sur **Oui, je souhaite participer**.  
  
4.  Cliquez sur **OK**.  
  
 **Utilisation d’une stratégie basée sur le registre ou d’une stratégie de groupe**  
  
 Les clients d’entreprises peuvent construire une stratégie de groupe pour s’abonner ou annuler l’abonnement en définissant une stratégie basée sur le registre pour SQL Server 2014.  
  
 La clé de Registre et les paramètres pertinents se présentent comme suit :  
  
 Clé = HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\120  
  
 Nom de l'entrée de Registre = CustomerFeedback  
  
 Type d'entrée DWORD :  
  
-   0 pour annuler l'abonnement  
  
-   1 pour s'abonner  
  
