---
description: Authentification par certificat client pour les scénarios de bouclage
title: Authentification par certificat client pour les scénarios de bouclage | Microsoft Docs
ms.custom: ''
ms.date: 07/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: peterbae
ms.author: v-hyba
ms.openlocfilehash: 168034034e5f2abe53bd47029b99fa78c250336b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836966"
---
# <a name="client-certificate-authentication-for-loopback-scenarios"></a>Authentification par certificat client pour les scénarios de bouclage

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Une nouvelle procédure stockée appelée sp_execute_external_script (SPEES) a été ajoutée dans SQL Server 2016. Cette procédure stockée permet à SQL Server de lancer et d’exécuter un script externe en dehors de SQL Server, dans le cadre d’un effort d’extensibilité. Elle s’accompagne de la prise en charge des scripts R et Python, qui possèdent tous deux des bibliothèques qui peuvent utiliser un pilote JDBC pour se connecter à SQL Server. Bien que SQL Server sur Windows puisse utiliser l’authentification intégrée de Windows pour authentifier ces connexions de bouclage avec les mêmes informations d’identification que l’utilisateur qui a démarré la requête, Linux SQL Server ne peut pas faire de même. Par conséquent, l’authentification par certificat client est ajoutée pour permettre aux utilisateurs de s’authentifier avec un certificat et une clé.

## <a name="connecting-using-client-certificate-authentication"></a>Connexion à l’aide de l’authentification du certificat client

Le pilote JDBC ajoute trois propriétés de connexion pour cette fonctionnalité :

* clientCertificate : spécifie le certificat à utiliser pour l’authentification. Le pilote JDBC prend en charge les extensions de fichier PFX, PEM, DER et CER.

Format
```
clientCertificate=<file_location>
``` 
Le pilote utilise un fichier de certificat. Pour les certificats aux formats PEM, DER et CER, l’attribut clientKey est requis. L’emplacement du fichier peut être relatif ou absolu.
 
* clientKey : spécifie un emplacement de fichier de la clé privée pour les certificats PEM, DER ou CER spécifiés par l’attribut clientCertificate.

Format
```
clientKey=<file_location>
```
Spécifie l’emplacement du fichier de la clé privée. Si le fichier de la clé privée est protégé par un mot de passe, le mot de passe est requis. L’emplacement du fichier peut être relatif ou absolu.

* clientKeyPassword : chaîne de mot de passe facultative fournie pour accéder à la clé privée du fichier clientKey.

Cette fonctionnalité est officiellement prise en charge uniquement pour les scénarios d’authentification de bouclage sur SQL Server 2019 Linux et versions ultérieurs.

## <a name="see-also"></a>Voir aussi

[Connexion à SQL Server avec le pilote JDBC](../../connect/jdbc/connecting-to-sql-server-with-the-jdbc-driver.md)  
[sp_execute_external_script (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)
