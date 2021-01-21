---
description: Configurer l'authentification personnalisée ou par formulaire sur le serveur de rapports
title: Configurer l’authentification personnalisée ou par formulaire sur le serveur de rapports| Microsoft Docs
ms.date: 04/18/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Forms authentication, configuring
- custom authentication [Reporting Services]
ms.assetid: e8601a8f-e66d-4649-8e4d-a46ca20ec7d0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bca083e29d0d03a5a5470f58b4e004d81cd27a36
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596630"
---
# <a name="configure-custom-or-forms-authentication-on-the-report-server"></a>Configurer l'authentification personnalisée ou par formulaire sur le serveur de rapports

Reporting Services fournit une architecture extensible vous permettant d’incorporer des modules d'authentification personnalisés ou par formulaires. Vous pouvez envisager d'implémenter une extension d'authentification personnalisée si les spécifications de déploiement n'incluent pas la sécurité intégrée de Windows ou l’authentification de base. Le scénario d’utilisation de l'authentification personnalisée le plus courant est la prise en charge d’un accès Internet ou extranet à une application Web. Le remplacement de l’extension d’authentification Windows par défaut par une extension d'authentification personnalisée vous permet de mieux contrôler l'habilitation des utilisateurs externes à accéder au serveur de rapports.  

En pratique, le déploiement d'une extension d'authentification personnalisée requiert plusieurs étapes, notamment la copie des assemblys et des fichiers d'application, la modification des fichiers de configuration et le test du système. Cette rubrique traite uniquement des paramètres d'authentification spécifiés dans les fichiers de configuration.  

> [!NOTE]
>  La création d’une extension d’authentification personnalisée nécessite du code personnalisé et des compétences en matière de sécurité [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] . Si vous ne souhaitez pas créer de code pour une extension d'authentification personnalisée, vous pouvez utiliser des comptes et des groupes [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory, mais vous devez réduire grandement l'étendue d'un déploiement de serveur de rapports. Pour plus d’informations sur l’authentification personnalisée, consultez [Implémentation d’une extension de sécurité](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md).

En outre, si vous souhaitez utiliser une authentification par formulaire ou une extension d’authentification personnalisée dans un environnement SQL Server Reporting Services intégré à un produit SharePoint, vous devez configurer le site SharePoint pour qu’il utilise la méthode d’authentification que vous choisissez. Pour plus d’informations sur la configuration de l’authentification dans SharePoint, consultez [Exemples d’authentification](/previous-versions/office/sharepoint-2007-products-and-technologies/cc262069(v=office.12)) sur [!INCLUDE[msCoName](../../includes/msconame-md.md)] Developer Network (MSDN).



### <a name="to-configure-a-report-server-to-use-custom-authentication"></a>Pour configurer un serveur de rapports pour utiliser l'authentification personnalisée

1.  Ouvrez RSReportServer.config dans un éditeur de texte.

2.  Recherchez \<**Authentication**>.

3.  Copiez la structure XML suivante :

    ```
    <Authentication>
          <AuthenticationTypes>
                 <Custom />
          </AuthenticationTypes>
          <EnableAuthPersistence>true</EnableAuthPersistence>
    </Authentication>
    ```

4.  Collez-la sur les entrées existantes de \<**Authentication**>.

     Notez que vous ne pouvez pas utiliser **Custom** avec d'autres types d'authentification.

5.  Enregistrez le fichier .

6.  Ouvrez le fichier Web.config du serveur de rapports. Par défaut, celui-ci se trouve dans le dossier \Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\ReportServer.

7.  Recherchez **authentication mode** et affectez-lui la valeur **Forms**.

    ```
    <authentication mode = "Forms" />
    ```

8.  Recherchez **identity impersonate** et affectez-lui la valeur **False**.

    ```
    <identity impersonate = "false" />  
    ```
9. Ajoutez la structure d’éléments **PassThroughCookies** au fichier de configuration. Pour plus d’informations, consultez [Configurer le portail web pour passer des cookies d’authentification personnalisée](../../reporting-services/security/configure-the-web-portal-to-pass-custom-authentication-cookies.md).
  
10. Enregistrez le fichier .  
  
11. Si vous avez configuré un déploiement avec montée en puissance parallèle, répétez l’ensemble des étapes précédentes pour d'autres serveurs de rapports du déploiement.  
  
12. Redémarrez le serveur de rapports pour effacer toutes les sessions qui sont actuellement ouvertes.  

## <a name="see-also"></a>Voir aussi

[Implémentation d'une extension de sécurité](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md)  
[Exemple de sécurité personnalisées Reporting Services (GitHub)](https://github.com/Microsoft/Reporting-Services/tree/master/CustomSecuritySample)  
[Authentification avec le serveur de rapports](../../reporting-services/security/authentication-with-the-report-server.md)   
[RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[Configurer une authentification de base sur le serveur de rapports](../../reporting-services/security/configure-basic-authentication-on-the-report-server.md)   
[Configurer une authentification Windows sur le serveur de rapports](../../reporting-services/security/configure-windows-authentication-on-the-report-server.md)  
D’autres questions ? [Essayez le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)