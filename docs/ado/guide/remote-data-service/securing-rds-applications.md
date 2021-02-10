---
description: Sécurisation des applications RDS
title: Sécurisation des applications RDS | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS security [ADO]
ms.assetid: 82fb1330-d6c6-4c17-ad3e-d417ff822b25
author: rothja
ms.author: jroth
ms.openlocfilehash: d91b2ef0d4efba7453e3acedb12c8f55d528b1b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031801"
---
# <a name="securing-rds-applications"></a>Sécurisation des applications RDS
Cette rubrique fournit des informations de sécurité pour RDS.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="microsoft-internet-explorer-security-issues"></a>Problèmes de sécurité de Microsoft Internet Explorer  
 Avec les nouvelles améliorations de sécurité ajoutées à Microsoft Internet Explorer, certains objets ADO et RDS sont limités à l’exécution uniquement dans un environnement en mode « sécurisé ». Pour cela, vous devez être conscient de ces problèmes, y compris les différentes zones, les niveaux de sécurité, le comportement restrictif, les opérations risquées et les paramètres de sécurité personnalisés.  
  
## <a name="security-and-your-web-server"></a>Sécurité et votre serveur Web  
 Si vous utilisez l’objet [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) sur votre serveur Web Internet, n’oubliez pas que cela crée un risque de sécurité potentiel. Les utilisateurs externes qui obtiennent des informations valides sur le nom de source de données (DSN), l’ID d’utilisateur et le mot de passe peuvent écrire des pages pour envoyer une requête à cette source de données. Si vous souhaitez un accès plus restreint à une source de données, l’une des options consiste à annuler l’inscription et à supprimer l’objet **RDSServer. DataFactory** (msadcf.dll) et à utiliser à la place des objets métier personnalisés avec des requêtes codées en dur.  
  
 Pour plus d’informations sur les implications en matière de sécurité de l’utilisation de l’objet RDSServer. DataFactory, consultez le bulletin de sécurité Microsoft MS99-025 sur le site Web Microsoft sur la sécurité.  
  
## <a name="client-impersonation-and-security"></a>Emprunt d’identité du client et sécurité  
 Si la propriété **authentification de mot de passe** de votre serveur Web IIS est définie sur authentification stimulation/réponse de Windows NT (pour windows NT 4,0) ou sur authentification Windows intégrée (pour Windows 2000), les objets métier sont appelés dans le contexte de sécurité du client. Il s’agit d’une nouvelle fonctionnalité de RDS 1,5 qui autorise l’emprunt d’identité du client via HTTP. Lorsque vous travaillez dans ce mode, l’ouverture de session sur le serveur Web (IIS) n’est pas anonyme, mais utilise l’ID d’utilisateur et le mot de passe sous lesquels l’ordinateur client s’exécute. Si les noms de sources de données ODBC sont configurés pour utiliser une connexion approuvée, l’accès aux bases de données, telles que SQL Server, se produit également dans le contexte de sécurité du client. Mais cela ne fonctionne que si la base de données se trouve sur le même ordinateur que le serveur IIS ; les informations d’identification du client ne peuvent pas être reportées sur un autre ordinateur.  
  
 Par exemple, un client, John Doe, avec userid = « JohnD » et Password = « secret », est connecté à un ordinateur client. Il exécute une application basée sur un navigateur qui doit accéder à l’objet **RDSServer. DataFactory** pour créer un [jeu d’enregistrements](../../reference/ado-api/recordset-object-ado.md) en exécutant une requête SQL sur l’ordinateur « MyServer » exécutant IIS. MyServer, un système exécutant Windows NT Server 4,0, est configuré pour utiliser l’authentification stimulation/réponse de Windows NT, son nom de source de données ODBC est sélectionné « utiliser une connexion approuvée », et le serveur contient également la source de données SQL Server. Lorsqu’une demande est reçue sur le serveur Web, elle demande au client l’ID d’utilisateur et le mot de passe. Par conséquent, la demande est consignée sur MyServer comme provenant de « JohnD »/« secret » au lieu de IUSER_MyServer (ce qui est la valeur par défaut lorsque l’authentification par mot de passe anonyme est activée). De même, lors de la connexion à SQL Server, « JohnD »/« secret » est utilisé.  
  
 Par conséquent, le mode d’authentification stimulation/réponse de Windows NT de IIS permet de créer des pages HTML sans que l’utilisateur ne soit invité explicitement à entrer l’ID d’utilisateur et les informations de mot de passe nécessaires pour se connecter à la base de données. Si l’authentification de base IIS était utilisée, cela est également nécessaire.  
  
## <a name="password-authentication"></a>Authentification par mot de passe  
 Les services Bureau à distance peuvent communiquer avec un serveur Web IIS exécutant dans l’un des trois modes d’authentification de mot de passe : authentification anonyme, de base ou authentification stimulation/réponse NT (appelée authentification Windows intégrée dans Windows 2000). Ces paramètres définissent la façon dont un serveur Web contrôle l’accès via celui-ci, par exemple exiger qu’un ordinateur client dispose de privilèges d’accès explicites sur le serveur Web NT.