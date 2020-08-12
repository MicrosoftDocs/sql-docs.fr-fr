---
title: Sécurisation des chaînes de connexion
description: Découvrez comment sécuriser les informations de chaîne de connexion lors de l’utilisation de JDBC Driver pour SQL Server.
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 69ce8557-5260-4ea4-81b8-d0c5481f0868
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b8fc42c778e0a8971ea36b8c0588765e82ee8fd2
ms.sourcegitcommit: cb620c77fe6bdefb975968837706750c31048d46
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/15/2020
ms.locfileid: "86391144"
---
# <a name="securing-connection-strings"></a>Sécurisation des chaînes de connexion

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

La protection de l'accès à votre source de données est l'un des principaux objectifs de la sécurisation d'une application. Pour limiter l'accès à votre source de données, vous devez veiller à sécuriser les informations de connexion, telles que l'ID utilisateur, le mot de passe et le nom de source de données. Le stockage d'un ID utilisateur et d'un mot de passe en texte libre, par exemple dans le code source, constitue un réel problème de sécurité. Même si vous fournissez une version compilée du code contenant des informations d'ID utilisateur et de mot de passe dans une source externe, vous courez le risque que le code compilé soit décompilé afin d'obtenir l'ID utilisateur et le mot de passe. En conséquence, il est impératif que le code ne contienne pas d'informations critiques telles que l'ID utilisateur et le mot de passe.

Il est déconseillé de stocker le mot de passe avec l'URL de connexion dans le code source de l'application. Au lieu de cela, songez à stocker le mot de passe dans un autre fichier auquel l'accès est restreint. L'accès à ce fichier peut être octroyé au contexte dans lequel l'application s'exécute.

Une autre approche consiste à stocker le mot de passe crypté dans un fichier. Veillez à utiliser une API de chiffrement ne nécessitant pas l'enregistrement de la clé à un emplacement quelconque et qui ne soit pas dérivée du mot de passe utilisateur. Par exemple, vous pouvez utiliser des paires de clés publiques/privées basées sur un certificat ou une approche où les deux parties utilisent un protocole d'accord de clé (algorithme Diffie-Hellman) pour générer des clés secrètes identiques pour le chiffrement sans devoir les transmettre.

Si vous utilisez des informations de chaîne de connexion d'une source externe, telle qu'un utilisateur entrant un ID et un mot de passe utilisateur, vous devez valider toute entrée provenant de la source afin de vous assurer qu'elle présente le format approprié et ne contient pas d'autres paramètres susceptibles d'affecter la connexion.

## <a name="see-also"></a>Voir aussi

[Sécurisation des applications du pilote JDBC](../../connect/jdbc/securing-jdbc-driver-applications.md)
