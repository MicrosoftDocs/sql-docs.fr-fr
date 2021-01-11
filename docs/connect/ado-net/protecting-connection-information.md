---
title: Protection des informations de connexion
description: Découvrez les vulnérabilités de sécurité dans les chaînes de connexion qui peuvent survenir en raison de la construction et de la conservation des chaînes de connexion et du type d’authentification.
ms.date: 11/13/2020
ms.assetid: 1471f580-bcd4-4046-bdaf-d2541ecda2f4
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1d170d712269bf169d069ef7b93f975de855f8ec
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771401"
---
# <a name="protecting-connection-information"></a>Protection des informations de connexion

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La protection de l'accès à votre source de données représente l'un de vos principaux objectifs lorsque vous sécurisez une application. Une chaîne de connexion présente une vulnérabilité potentielle si elle n'est pas sécurisée. Le stockage d'informations de connexion au format texte brut ou sa conservation dans la mémoire risque de compromettre l'ensemble de votre système. Des chaînes de connexion incorporées dans votre code source peuvent être lues à l'aide de [lidasm.exe (Désassembleur IL)](/dotnet/framework/tools/ildasm-exe-il-disassembler) pour afficher MSIL (Microsoft intermediate language) dans un assembly compilé.

Des vulnérabilités de sécurité impliquant des chaînes de connexion peuvent se produire en fonction du type d'authentification utilisé, de la manière dont les chaînes de connexion sont conservées dans la mémoire et sur le disque, et des techniques utilisées pour les construire au moment de l'exécution.

## <a name="use-windows-authentication"></a>Utiliser l’authentification Windows

Pour aider à limiter l'accès à votre source de données, vous devez sécuriser des informations de connexion telles que l'ID utilisateur, le mot de passe et le nom de source de données. Afin d'éviter l'exposition d'informations utilisateur, il est recommandé d'utiliser l'authentification Windows (parfois appelée *sécurité intégrée*) dès que possible. L'authentification Windows est spécifiée dans une chaîne de connexion à l'aide des mots clés `Integrated Security` ou `Trusted_Connection`, ce qui supprime le recours à un ID utilisateur et à un mot de passe. Lors de l'utilisation de l'authentification Windows, les utilisateurs sont authentifiés par Windows et l'accès aux ressources de serveur et de base de données est déterminé en octroyant des autorisations aux utilisateurs et aux groupes Windows.

Dans les cas où l'utilisation de l'authentification Windows n'est pas possible, vous devez être extrêmement prudent, car les informations d'authentification utilisateur sont exposées dans la chaîne de connexion. Dans une application ASP.NET, vous pouvez configurer un compte Windows en tant qu'identité fixe utilisée pour la connexion à des bases de données et autres ressources réseau. Vous activez l'emprunt d'identité dans l'élément d'identité au sein du fichier **web.config** et vous spécifiez un nom d'utilisateur et un mot de passe.

```xml  
<identity impersonate="true"
        userName="MyDomain\UserAccount"
        password="*****" />  
```  

Le compte d'identité fixe doit être un compte avec des privilèges faibles auquel seules les autorisations nécessaires dans la base de données ont été octroyées. En outre, vous devez chiffrer le fichier de configuration afin que le nom d'utilisateur et le mot de passe ne soient pas exposés en texte clair.

## <a name="avoid-injection-attacks-with-connection-string-builders"></a>Évitez les attaques par injection avec des générateurs de chaînes de connexion

Une attaque par injection de chaîne de connexion peut se produire lorsqu'une concaténation de chaîne dynamique est utilisée pour générer des chaînes de connexion en fonction de l'entrée utilisateur. Si l'entrée utilisateur n'est pas validée et que du texte ou des caractères malveillants ne sont pas placés dans une séquence d'échappement, un attaquant peut éventuellement accéder à des données sensibles ou à d'autres ressources sur le serveur. Pour résoudre ce problème, le Fournisseur de données Microsoft SQL pour SQL Server introduit de nouvelles classes de générateur de chaîne de connexion pour valider la syntaxe de la chaîne de connexion et garantir que des paramètres supplémentaires ne sont pas introduits. Pour plus d’informations, consultez [Builders de chaînes de connexion](connection-string-builders.md).

## <a name="use-persist-security-infofalse"></a>Utilisez « Persist Security Info=false »

La valeur par défaut pour `Persist Security Info` est False ; nous vous recommandons d'utiliser cette valeur par défaut dans toutes les chaînes de connexion. Le paramétrage de `Persist Security Info` avec la valeur `true` ou `yes` permet d'obtenir des informations sensibles pour la sécurité, dont l'ID utilisateur et le mot de passe, à partir d'une connexion une fois celle-ci ouverte. Lorsque `Persist Security Info` possède la valeur `false` ou `no`, les informations de sécurité sont ignorées après leur utilisation pour ouvrir la connexion, ce qui garantit qu'une source qui n'est pas digne de confiance ne dispose pas d'un accès à des informations sensibles sur la sécurité.

## <a name="encrypt-configuration-files"></a>Chiffrer les fichiers de configuration

[!INCLUDE[appliesto-netfx-netcore-xxxx-md](../../includes/appliesto-netfx-netcore-xxxx-md.md)]

Vous pouvez également stocker des chaînes de connexion dans des fichiers de configuration, ce qui évite d'avoir à les incorporer dans le code de votre application. Les fichiers de configuration sont des fichiers XML standard pour lesquels le .NET Framework a défini un ensemble commun d'éléments. En général, les chaînes de connexion dans les fichiers de configuration sont stockées dans l'élément **\<connectionStrings>** dans le fichier **app.config** pour une application Windows ou dans le fichier **web.config** pour une application ASP.NET. Pour plus d'informations sur les éléments de base concernant le stockage, l'extraction et le chiffrement des chaînes de connexion à partir des fichiers config, consultez [Chaînes de connexion et fichier config](connection-strings-and-configuration-files.md).

## <a name="see-also"></a>Voir aussi

- [Chiffrement des informations de configuration à l'aide de la configuration protégée](/previous-versions/aspnet/53tyfkaw(v=vs.100))
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
