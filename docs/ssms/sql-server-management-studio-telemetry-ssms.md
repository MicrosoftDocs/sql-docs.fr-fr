---
description: Audit local pour l’utilisation de SSMS et collecte des données d’utilisation et de diagnostic
title: Données d’utilisation et de diagnostic
ms.custom: seo-lt-2019
ms.date: 04/16/2019
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b2e188200daf0469f80cf74db901bcc240fd8706
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340688"
---
# <a name="local-audit-for-ssms-usage-and-diagnostic-data-collection"></a>Audit local pour l’utilisation de SSMS et collecte des données d’utilisation et de diagnostic
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SQL Server Management Studio (SSMS) intègre des fonctionnalités Internet qui peuvent recueillir et envoyer à Microsoft des données d’utilisation et de diagnostic anonymes. SSMS peut recueillir des informations standard sur l’ordinateur et des informations sur l’utilisation et les performances qui peuvent être transmises à Microsoft et analysées dans le but d’améliorer la qualité, la sécurité et la fiabilité de SSMS. Microsoft ne collecte ni votre nom, ni votre adresse, ni aucune autre information permettant de vous contacter. Pour plus d’informations, consultez la [Déclaration de confidentialité Microsoft](https://privacy.microsoft.com/privacystatement) et l’[Avenant à la déclaration de confidentialité de SQL Server](../sql-server/sql-server-privacy.md).

## <a name="audit-feature-usage-and-diagnostic-data"></a>Utilisation de la fonctionnalité d’audit et données de diagnostic

Pour voir les données d’utilisation qui sont recueillies par SSMS, effectuez les étapes suivantes :

1.  Lancez SSMS.
2.  Cliquez sur **Affichage** et sur **Sortie** dans le menu principal pour afficher la fenêtre **Sortie**. 
3.  Une fois la fenêtre **Sortie** affichée à l’écran, choisissez **Télémétrie** dans le menu **Afficher la sortie à partir de :**.

Quand vous utilisez SSMS pour interagir avec vos bases de données, la fenêtre **Sortie** indique les données qui sont recueillies.

## <a name="enable-or-disable-usage-and-diagnostic-data-collection-in-ssms"></a>Activer ou désactiver la collecte des données d’utilisation et de diagnostic dans SSMS

Pour accepter ou refuser la collecte des données d’utilisation pour SSMS :

- Pour SQL Server Management Studio 17 :

  `Subkey = HKEY_CURRENT_USER\Software\Microsoft\SQL Server Management Studio\14.0`

  Nom RegEntry = `UserFeedbackOptIn`

  Type d’entrée `DWORD`: `0` pour refuser ; `1` pour accepter

  De plus, SSMS 17.x est basé sur l’interpréteur de commandes (shell) Visual Studio 2015, et l’installation de Visual Studio active les commentaires client par défaut.  

  Pour configurer Visual Studio de façon à désactiver les commentaires client sur des ordinateurs individuels, attribuez à la sous-clé de Registre suivante la chaîne `0` : `HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\VisualStudio\SQM OptIn`

  Par exemple, modifiez la sous-clé comme suit :  
  `HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\VisualStudio\SQM OptIn `=` 0`

  La stratégie de groupe basée sur le Registre pour ces sous-clés de Registre est respectée par la collecte des données d’utilisation et de diagnostic de SQL Server 2017.

- Pour SQL Server Management Studio 18 :

  `Subkey = HKEY_CURRENT_USER\Software\Microsoft\SQL Server Management Studio\18.0_IsoShell`

  Nom RegEntry = `UserFeedbackOptIn`

  Type d’entrée `DWORD`: `0` pour refuser ; `1` pour accepter

## <a name="see-also"></a>Voir aussi

- [Configurer les données d’utilisation et de diagnostic pour SQL Server](../sql-server/usage-and-diagnostic-data-configuration-for-sql-server.md)
- [Audit local pour l’utilisation de SQL Server et collecte des données d’utilisation et de diagnostic](../sql-server/usage-and-diagnostic-data-in-local-audit.md)