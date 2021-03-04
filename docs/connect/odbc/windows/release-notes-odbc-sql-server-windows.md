---
title: Notes de publication pour ODBC Driver for SQL Server sur Windows
description: Cet article de notes de publication décrit les changements de chaque version du pilote Microsoft ODBC pour SQL Server sur Windows.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: b8459ed8-625e-4d8b-891c-e7e78c9977cc
ms.reviewer: v-daenge
author: v-chojas
ms.author: v-chojas
manager: kenvh
ms.openlocfilehash: 23af1879610f55cfccb7c6e8a9f8071752a2a35c
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837296"
---
# <a name="release-notes-for-microsoft-odbc-driver-for-sql-server-on-windows"></a>Notes de publication pour Microsoft ODBC Driver for SQL Server sur Windows

Cet article de notes de publication décrit les nouveautés du pilote Microsoft ODBC pour SQL Server sur Windows.

<!--
PLEASE USE THE STANDARD 2-COLUMN TABLE FORMAT!

For all our Release Notes articles (What's New too?), we are standardizing on the 2-column format that you see here for version "## 17.3".

Going forward, all new additions to this article must use the 2-column format.

Also, use the shorter ## H2 title format, which eliminates all the redundant constants, and appends the date-added.
One beneift of shortness is the avoidance of the annoying wrapping of unnecessarily long H2 titles in the rightNav.
- OLD H2:  ## What's New in the [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 17.3 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] on Windows
- NEW H2:  ## 17.3, February 2019

By the way, in GitHub, the file name is changing today 2019/03/30:
- FROM:  docs/connect/odbc/windows/release-notes.md
- TO  :  docs/connect/odbc/windows/release-notes-odbc-sql-server-windows.md

Thank you.
GeneMi (and CraigG).  2019/03/30.
-->

## <a name="177"></a>17,7

![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x64](https://go.microsoft.com/fwlink/?linkid=2153471)  
![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x86](https://go.microsoft.com/fwlink/?linkid=2153469)  

Numéro de version : 17.7.1.1  
Publication : 29 janvier 2021

Si vous avez besoin de télécharger le programme d’installation dans une langue autre que celle détectée pour vous, vous pouvez utiliser ces liens directs.  
Pour le pilote x64 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x40a)  
Pour le pilote x86 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x40a)

| Fonctionnalité ajoutée | Détails |
| :------- | :------ |
| Authentification interactive d’Azure Key Vault | Consultez [Utilisation d’Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md). |
| Authentification d’un principal de service | Consultez [Attributs et mots clés de chaîne de connexion et DSN](../dsn-connection-string-attribute.md). |
| Possibilité d’insertion dans des colonnes money et smallmoney chiffrées | Consultez [Utilisation d’Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md). |
| Résolution des bogues. | [Correctifs de bogues](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="previous-releases"></a>Versions précédentes

## <a name="176"></a>17.6

![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x64](https://go.microsoft.com/fwlink/?linkid=2137027)  
![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x86](https://go.microsoft.com/fwlink/?linkid=2137028)  

Numéro de version : 17.6.1.1  
Publication : 31 juillet 2020

Si vous avez besoin de télécharger le programme d’installation dans une langue autre que celle détectée pour vous, vous pouvez utiliser ces liens directs.  
Pour le pilote x64 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x40a)  
Pour le pilote x86 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x40a)

| Fonctionnalité ajoutée | Détails |
| :------- | :------ |
| Mise en cache des métadonnées pour les instructions préparées | Consultez [Utilisation d’Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md). |
| L’attribut de connexion SQL_COPT_SS_AUTOBEGINTXN pour contrôler si BEGIN TRANSACTION se produit automatiquement après ROLLBACK ou COMMIT | Consultez [Attributs et mots clés de chaîne de connexion et DSN](../dsn-connection-string-attribute.md). |
| Résolution des bogues. | [Correctifs de bogues](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="1752"></a>17.5.2

![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x64](https://go.microsoft.com/fwlink/?linkid=2120137)  
![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x86](https://go.microsoft.com/fwlink/?linkid=2120140)  

Numéro de version : 17.5.2.1  
Publication : 6 mars 2020

Si vous avez besoin de télécharger le programme d’installation dans une langue autre que celle détectée pour vous, vous pouvez utiliser ces liens directs.  
Pour le pilote x64 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x40a)  
Pour le pilote x86 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x40a)  

### <a name="features-added-in-1752"></a>Fonctionnalités ajoutées dans la version 17.5.2

| Fonctionnalité ajoutée | Détails |
| :------------ | :------ |
| Prise en charge de l’authentification avec Managed Identity pour Azure Key Vault | Voir [Utilisation d’Always Encrypted avec ODBC Driver](../using-always-encrypted-with-the-odbc-driver.md). |
| Prise en charge de points de terminaison Azure Key Vault supplémentaires | Voir [Utilisation d’Always Encrypted avec ODBC Driver](../using-always-encrypted-with-the-odbc-driver.md). |
| Résolution des bogues. | Consultez [Correctifs de bogues](../bug-fixes.md). |
| &nbsp; | &nbsp; |

Téléchargez les versions précédentes du pilote ODBC en cliquant sur les liens de téléchargement dans les sections suivantes :

## <a name="175"></a>17.5

![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x64](https://go.microsoft.com/fwlink/?linkid=2120248)  
![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x86](https://go.microsoft.com/fwlink/?linkid=2120353)  

Numéro de version : 17.5.1.1  
Publication : 31 janvier 2020

Si vous avez besoin de télécharger le programme d’installation dans une langue autre que celle détectée pour vous, vous pouvez utiliser ces liens directs.  
Pour le pilote x64 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x40a)  
Pour le pilote x86 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x40a)  

### <a name="features-added-in-175"></a>Fonctionnalités ajoutées dans la version 17.5

| Fonctionnalité ajoutée | Détails |
| :------------ | :------ |
| Attribut de connexion SQL_COPT_SS_SPID pour récupérer le SPID sans aller-retour avec le serveur | Consultez [Attributs et mots clés de chaîne de connexion et DSN](../dsn-connection-string-attribute.md). |
| Résolution des bogues. | Consultez [Correctifs de bogues](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="1742"></a>17.4.2

![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x64](https://go.microsoft.com/fwlink/?linkid=2120354)  
![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x86](https://go.microsoft.com/fwlink/?linkid=2120249)  

Numéro de version : 17.4.2.1  
Publication : 2 octobre 2019

Si vous avez besoin de télécharger le programme d’installation dans une langue autre que celle détectée pour vous, vous pouvez utiliser ces liens directs.  
Pour le pilote x64 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x40a)  
Pour le pilote x86 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x40a)  

### <a name="features-added-in-1742"></a>Fonctionnalités ajoutées dans la version 17.4.2

| Fonctionnalité ajoutée | Détails |
| :------------ | :------ |
| Prise en charge de points de terminaison Azure Key Vault supplémentaires | Voir [Utilisation d’Always Encrypted avec ODBC Driver](../using-always-encrypted-with-the-odbc-driver.md). |
| Prise en charge de la définition de la version de la classification des données | Consultez [Classification des données](../data-classification.md#bkmk-version). |
| Inclure la bibliothèque d’authentification Azure Active Directory (adal.dll) dans le programme d’installation | Maintenant inclus dans l’installation du pilote de base, le programme d’installation ODBC met à niveau les installations existantes de la Bibliothèque d’authentification Microsoft Active Directory pour SQL Server et les supprime de la liste des applications installées dans Windows. |
| Résolution des bogues. | Consultez [Correctifs de bogues](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="174"></a>17.4

![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x64](https://go.microsoft.com/fwlink/?linkid=2120149)  
![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x86](https://go.microsoft.com/fwlink/?linkid=2120150)  

Numéro de version : 17.4.1.1  
Publication : Juillet 2019

Si vous avez besoin de télécharger le programme d’installation dans une langue autre que celle détectée pour vous, vous pouvez utiliser ces liens directs.  
Pour le pilote x64 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x40a)  
Pour le pilote x86 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x40a)  

### <a name="features-added-in-174"></a>Fonctionnalités ajoutées dans la version 17.4

| Fonctionnalité ajoutée | Détails |
| :------------ | :------ |
| Always Encrypted avec les enclaves sécurisées. | Voir [Utilisation d’Always Encrypted avec ODBC Driver](../using-always-encrypted-with-the-odbc-driver.md). |
| Paramètres TCP Keep Alive configurables. | Voir [Connexion à SQL Server](../linux-mac/connection-string-keywords-and-data-source-names-dsns.md). |
| Résolution des bogues. | Consultez [Correctifs de bogues](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="173"></a>17.3

![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x64](https://go.microsoft.com/fwlink/?linkid=2120355)  
![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x86](https://go.microsoft.com/fwlink/?linkid=2120356)  

Numéro de version : 17.3.1.1  
Publication : Février 2019

Si vous avez besoin de télécharger le programme d’installation dans une langue autre que celle détectée pour vous, vous pouvez utiliser ces liens directs.  
Pour le pilote x64 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x40a)  
Pour le pilote x86 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x40a)  

### <a name="features-added-in-173"></a>Fonctionnalités ajoutées dans la version 17.3

| Fonctionnalité ajoutée | Détails |
| :------------ | :------ |
| Mode d’authentification Azure Active Directory Managed Identity (avec attribution par le système et l’utilisateur). | Consultez [Utilisation d’Azure Active Directory avec ODBC Driver](../using-azure-active-directory.md). |
| Possibilité d’envoyer des paramètres d’entrée sur les colonnes Always Encrypted. | Consultez [Limitations du pilote ODBC lors de l’utilisation d’Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md#limitations-of-the-odbc-driver-when-using-always-encrypted). |
| Transactions distribuées XA. | [Utilisation de transactions XA](../use-xa-with-dtc.md). |
| Visual C++ Redistributable mis à jour | Mise à niveau de la dépendance du runtime avec Visual C++ 2017 Redistributable ([téléchargement x64](https://aka.ms/vs/15/release/vc_redist.x64.exe), [téléchargement x86](https://aka.ms/vs/15/release/vc_redist.x86.exe)) |
| Résolution des bogues. | Consultez [Correctifs de bogues](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="172"></a>17.2

![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x64](https://go.microsoft.com/fwlink/?linkid=2120250)  
![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x86](https://go.microsoft.com/fwlink/?linkid=2120357)  

Numéro de version : 17.2.0.1  
Publication : Juillet 2018

Si vous avez besoin de télécharger le programme d’installation dans une langue autre que celle détectée pour vous, vous pouvez utiliser ces liens directs.  
Pour le pilote x64 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x40a)  
Pour le pilote x86 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x40a)  

### <a name="features-added-in-172"></a>Fonctionnalités ajoutées dans la version 17.2

| Fonctionnalité ajoutée | Détails |
| :------------ | :------ |
| Classification des données pour Azure SQL Database et SQL Server. | Consultez [Classification des données](../data-classification.md). |
| Prise en charge de l’encodage serveur UTF-8. | &nbsp; |
| Résolution des bogues. | Consultez [Correctifs de bogues](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="171"></a>17.1

![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x64](https://go.microsoft.com/fwlink/?linkid=2120151)  
![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x86](https://go.microsoft.com/fwlink/?linkid=2120443)  

Numéro de version : 17.1.0.1  
Publication : Mars 2018

Si vous avez besoin de télécharger le programme d’installation dans une langue autre que celle détectée pour vous, vous pouvez utiliser ces liens directs.  
Pour le pilote x64 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x40a)  
Pour le pilote x86 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x40a)  

### <a name="features-added-in-171"></a>Fonctionnalités ajoutées dans la version 17.1

| Fonctionnalité ajoutée | Détails |
| :------------ | :------ |
| Prise en charge des attributs de connexion `SQL_COPT_SS_CEKCACHETTL` et `SQL_COPT_SS_TRUSTEDCMKPATHS`. | &bull; &nbsp; `SQL_COPT_SS_CEKCACHETTL`<br/>Permet de contrôler la durée d’existence du cache local des clés de chiffrement de colonne, ainsi que son vidage.<br/><br/>&bull; &nbsp; `SQL_COPT_SS_TRUSTEDCMKPATHS`<br/>Permet à l’application de limiter les opérations AE à la seule utilisation de la liste spécifiée de clés principales de colonne.<br/><br/> Pour plus d’informations, consultez [Utilisation d’Always Encrypted avec ODBC Driver for SQL Server](../using-always-encrypted-with-the-odbc-driver.md). |
| Prise en charge de l’authentification interactive Azure Active Directory | &nbsp; |
| Résolution des bogues. | Consultez [Correctifs de bogues](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="170"></a>17.0

![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x64](https://go.microsoft.com/fwlink/?linkid=2120444)  
![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x86](https://go.microsoft.com/fwlink/?linkid=2120152)  

Numéro de version : 17.0.1.1  
Publication : Février 2018

Si vous avez besoin de télécharger le programme d’installation dans une langue autre que celle détectée pour vous, vous pouvez utiliser ces liens directs.  
Pour le pilote x64 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x40a)  
Pour le pilote x86 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x40a)  

### <a name="features-added-in-170"></a>Fonctionnalités ajoutées dans la version 17.0

| Fonctionnalité ajoutée | Détails |
| :------------ | :------ |
| Prise en charge d’Always Encrypted pour l’API BCP. | &nbsp; |
| Nouvel attribut de chaîne de connexion `UseFMTOnly`. | Oblige le pilote à utiliser les métadonnées héritées dans les cas spéciaux qui nécessitent des tables temporaires. |
| Prise en charge d’Azure SQL Managed Instance. | Consultez la liste suivante de [différences lors de l’utilisation de Managed Instance (ODBC version 17)](#diffs-managed-instance-17). |
| &nbsp; | &nbsp; |

| Dépendance modifiée | Détails |
| :------------ | :------ |
| Suppression de l’Assistant de connexion aux services en ligne Microsoft | La dépendance a été supprimée. |
| &nbsp; | &nbsp; |

### <a name="differences-when-using-managed-instance-odbc-version-17"></a><a name="diffs-managed-instance-17"></a> Différences lors de l’utilisation de Managed Instance (ODBC version 17)

Cette version d’ODBC prend en charge Azure SQL Managed Instance. Consultez la liste suivante de différences lors de l’utilisation de Managed Instance.

> [!NOTE]
> Il existe plusieurs différences lors de l’utilisation de Managed Instance :
>
> - FILESTREAM n’est pas pris en charge.
> - L’accès au système de fichiers local n’est pas pris en charge, mais est nécessaire pour certaines choses telles que les fichiers de trace.
> - La création d’un UDT depuis le chemin local n’est pas prise en charge.
> - L’authentification intégrée de Windows n’est pas prise en charge.
> - DTC n’est pas pris en charge.
> - Le compte `sa` n’est pas présent (le compte par défaut est appelé `cloudSA`).
> - L’erreur de jeton TDS (0xAA) retourne un nom de serveur incorrect.
> - Les caractères spéciaux dans les noms de base de données ne sont pas pris en charge.
> - ALTER DATABASE [nom_bd_1] MODIFY NAME = [nom_bd_2] n’est pas pris en charge.
> - Les messages d’erreur sont toujours affichés en anglais, quels que soient les paramètres de langue (à l’image d’Azure).

## <a name="131"></a>13.1

![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x64](https://go.microsoft.com/fwlink/?linkid=2121020)  
![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x86](https://go.microsoft.com/fwlink/?linkid=2120923)  

Numéro de version : 13.1  

Si vous avez besoin de télécharger le programme d’installation dans une langue autre que celle détectée pour vous, vous pouvez utiliser ces liens directs.  
Pour le pilote x64 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x40a)  
Pour le pilote x86 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x40a)  

[Télécharger les utilitaires de ligne de commande Microsoft 13.1 pour SQL Server](https://www.microsoft.com/download/details.aspx?id=53591)

### <a name="features-added-in-131"></a>Fonctionnalités ajoutées dans la version 13.1

| Fonctionnalité ajoutée | Détails |
| :------------ | :------ |
| ODBC Driver 13.1 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ajoute la prise en charge d’[Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md) et d’[Azure Active Directory](../using-azure-active-directory.md). | Ces nouvelles prises en charge sont disponibles lors de la connexion à Microsoft SQL Server version 2016 ou ultérieure. |
| Il existe des mots clés et attributs de regroupement de connexions, qui correspondent aux prises en charge d’Always Encrypted et d’Azure Active Directory. | Ces mots clés et attributs sont décrits dans [Regroupement de connexions prenant en charge le pilote dans ODBC Driver for SQL Server](driver-aware-connection-pooling-in-the-odbc-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

## <a name="13"></a>13

![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x64](https://go.microsoft.com/fwlink/?linkid=2121118)  
![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x86](https://go.microsoft.com/fwlink/?linkid=2120924)  

Numéro de version : 13  

Si vous avez besoin de télécharger le programme d’installation dans une langue autre que celle détectée pour vous, vous pouvez utiliser ces liens directs.  
Pour le pilote x64 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x40a)  
Pour le pilote x86 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x40a)  

[Télécharger les utilitaires de ligne de commande Microsoft 13 pour SQL Server](https://www.microsoft.com/download/details.aspx?id=52680)

### <a name="features-added-in-13"></a>Fonctionnalités ajoutées dans la version 13

| Fonctionnalité ajoutée | Détails |
| :------------ | :------ |
| Ajoute la prise en charge de Microsoft SQL Server 2016. | Conserve les fonctionnalités du pilote ODBC version 11. |
| &nbsp; | &nbsp; |

## <a name="11"></a>11

![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x64](https://go.microsoft.com/fwlink/?linkid=2121206)  
![Télécharger](../../../ssms/media/download-icon.png) [Télécharger le programme d’installation x86](https://go.microsoft.com/fwlink/?linkid=2121021)  

Numéro de version : 11  

Si vous avez besoin de télécharger le programme d’installation dans une langue autre que celle détectée pour vous, vous pouvez utiliser ces liens directs.  
Pour le pilote x64 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x40a)  
Pour le pilote x86 : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x40a)  

[Télécharger les utilitaires de ligne de commande Microsoft 11 pour SQL Server](https://www.microsoft.com/download/details.aspx?id=36433)  

### <a name="features-added-in-11"></a>Fonctionnalités ajoutées dans la version 11

| Fonctionnalité ajoutée | Détails |
| :------------ | :------ |
| Contient de nouvelles fonctionnalités. | Consultez [Fonctionnalités de Microsoft ODBC Driver for SQL Server sur Windows](features-of-the-microsoft-odbc-driver-for-sql-server-on-windows.md). |
| Contient toutes les fonctionnalités fournies avec ODBC dans SQL Server 2012 Native Client. | &nbsp; |
| &nbsp; | &nbsp; |
