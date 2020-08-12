---
title: Propriétés de Protocoles pour MSSQLSERVER (onglet Avancé)
description: En savoir plus sur les avantages et les exigences de la protection étendue de l’authentification pour le Moteur de base de données SQL Server. Découvrez comment l’activer et la configurer.
ms.custom: seo-lt-2019
ms.date: 01/24/2019
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
ms.assetid: abd5ca68-825f-4c07-b27c-3b3a79d03d74
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: ccf8f2a6c80dbaf13c6e47ca835c7dc3c3e0d4d0
ms.sourcegitcommit: f7ac1976d4bfa224332edd9ef2f4377a4d55a2c9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85897064"
---
# <a name="protocols-for-mssqlserver-properties-advanced-tab"></a>Propriétés de Protocoles pour MSSQLSERVER (onglet Avancé)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Utilisez l'onglet **Avancé** dans la boîte de dialogue **Propriétés de Protocoles pour MSSQLSERVER** pour configurer la **protection étendue de l'authentification** du moteur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. La**Protection étendue** est une fonctionnalité des composants réseau implémentée par le système d'exploitation. La**protection étendue** est disponible dans Windows 7 et Windows Server 2008 R2 et est incluse dans les Service Packs pour les précédents systèmes d'exploitation. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est plus sécurisé lorsque les connexions sont établies à l'aide de la **protection étendue**. Certains avantages de la **Protection étendue** requièrent de sélectionner l'option **Forcer le chiffrement** sous l'onglet **Indicateurs** .

> [!IMPORTANT]  
> Windows n'active pas la **protection étendue** par défaut. Pour plus d’informations sur l’activation de **Protection étendue**, consultez les rubriques suivantes :
> - [Protection étendue Windows \<extendedProtection\>](https://docs.microsoft.com/iis/configuration/system.webserver/security/authentication/windowsauthentication/extendedprotection/)
> - [Vue d’ensemble de la protection étendue de l'authentification](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/extended-protection-for-authentication-overview)

Pour plus d’informations sur la configuration d’autres services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et pour obtenir une description complète de la **protection étendue**, consultez les informations plus récentes sur [Microsoft.com](https://go.microsoft.com/fwlink/?LinkId=177752).

La**protection étendue** est totalement prise en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client à partir de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. La prise en charge de la **protection étendue** pour d'autres fournisseurs du client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'est actuellement pas prise en charge.

## <a name="options"></a>Options

### <a name="extended-protection"></a>protection étendue

Il existe trois valeurs possibles :  

- **Off** : signifie que la **protection étendue** est désactivée. L'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] acceptera les connexions de tous les clients, qu'ils soient protégés ou non. La valeur**Off** est compatible avec les anciens systèmes d'exploitation non corrigés, mais elle est moins sécurisée. Utilisez ce paramètre uniquement lorsque vous savez que les systèmes d'exploitation clients ne prennent pas en charge la Protection étendue.

- **Autorisée** : la **protection étendue** est requise pour les connexions établies à partir de systèmes d'exploitation qui la prennent en charge**protection étendue**. Les connexions provenant d'applications clientes non protégées qui s'exécutent sur des systèmes d'exploitation clients protégés sont rejetées. La**Protection étendue** est ignorée pour les connexions provenant de systèmes d'exploitation non protégés. Ce paramètre offre une meilleure protection que la valeur **Off**, mais ce n'est pas la configuration la plus sécurisée. Utilisez ce paramètre dans des environnements mixtes, dans lesquels certains systèmes d'exploitation ou applications prennent en charge la **protection étendue** et d'autres non.

- **Obligatoire** : signifie que pour qu’une connexion soit acceptée, elle doit provenir d’une application protégée sur un système d’exploitation protégé. Ce paramètre est le plus sécurisé des trois options. Cependant, les connexions des systèmes d'exploitation qui ne prennent pas en charge la **protection étendue** ne pourront pas se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

### <a name="accepted-ntlm-spns"></a>SPN NTLM acceptés

Une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être identifiée par plusieurs noms de principal du service (SPN) NTLM. Vous répertoriez les noms de principal du service sous la forme d’une série de chaînes séparées par des points-virgules. Par exemple, la valeur **MSSQLSvc/HostName1.Contoso.com;MSSQLSvc/HostName2.Contoso.com** indique que les clients qui essaient de se connecter au nom SPN **MSSQLSvc/HOST1.Contoso.com** ou **MSSQLSvc/HOST2.Contoso.com** sont autorisés. La variable a une longueur maximale de 2048 caractères.

## <a name="see-also"></a>Voir aussi

[Protection étendue de l'authentification avec Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)

