---
title: Liste des bogues corrigés
description: Cette page contient la liste des bogues résolus dans chaque version, à partir du pilote Microsoft ODBC 17 pour SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
author: v-chojas
ms.author: v-jizho2
manager: kenvh
ms.openlocfilehash: 1816f1a8255041d6211931ffc41963acc460e15d
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770470"
---
# <a name="list-of-bugs-fixed"></a>Liste des bogues corrigés

Cette page contient la liste des bogues résolus dans chaque version, à partir du pilote [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC 17 pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

### <a name="bug-fixes-in-the-msconame-odbc-driver-1772-for-ssnoversion"></a>Correctifs de bogues dans la version 17.7.2 de [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Correction du problème lié aux erreurs 404 Introuvable qui apparaissent quand l’authentification Managed Service Identity est utilisée
- Correction des erreurs intermittentes de type Chiffrement non pris en charge sous des charges multithread élevées
- Correction des incidents intermittents sous des charges multithread élevées

### <a name="bug-fixes-in-the-msconame-odbc-driver-177-for-ssnoversion"></a>Correctifs de bogues dans la version 17.7 de [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Correction de l’encodage des caractères des colonnes VARIANT en mode BCP NATIVE
- Correction de la définition de SQL_ATTR_PARAMS_PROCESSED_PTR dans des conditions spécifiques
- Correction de SQLDescribeParam en mode FMTONLY pour les instructions contenant des commentaires
- Correction d’un problème avec l’authentification fédérée lors de l’utilisation d’Okta
- Correction de l’utilisation excessive de la mémoire sur les systèmes multiprocesseurs
- Correction de l’authentification Azure AD pour certaines variantes d’Azure SQL DB

### <a name="bug-fixes-in-the-msconame-odbc-driver-176-for-ssnoversion"></a>Correctifs de bogues dans la version 17.6 de [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Corriger l’erreur ADAL lors de l’authentification avec un compte fédéré (Windows)
- Résolution du problème à cause duquel le pilote cesse de répondre lorsqu’un délai d’attente se produit pendant une opération de notification asynchrone
- Correction du nombre de références du pilote lors de la mise à niveau dans Alpine Linux
- Correction de la version de dépendance libc6 pour Ubuntu
- Ajout des définitions manquantes à msodbcsql.h Linux/Mac

### <a name="bug-fixes-in-the-msconame-odbc-driver-17522-for-ssnoversion-alpine-linux-only"></a>Correctifs de bogues dans la version 17.5.2.2 de [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (Alpine Linux uniquement)

- Correction d’un plantage quand Always Encrypted avec enclaves sécurisées est utilisé sur Alpine Linux

### <a name="bug-fixes-in-the-msconame-odbc-driver-1752-for-ssnoversion"></a>Correctifs de bogues dans la version 17.5.2 de [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Ajout de msodbcsql.h au package Linux Alpine

### <a name="bug-fixes-in-the-msconame-odbc-driver-175-for-ssnoversion"></a>Correctifs de bogues dans le pilote [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC 17.5 pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Correction du calcul du hachage des métadonnées CMK AKV sur Linux/macOS
- Correction de l’erreur lors du chargement d’OpenSSL 1.0.0
- Résolution des problèmes de conversion lors de l’utilisation des pages de codes ISO-8859-1 et ISO-8859-2
- Correction du nom de la bibliothèque interne sur macOS pour inclure le numéro de version
- Correction du paramètre de l’indicateur null lorsque des liaisons de longueur et d’indicateur distincts sont utilisées

### <a name="bug-fixes-in-the-msconame-odbc-driver-1742-for-ssnoversion"></a>Correctifs de bogues dans le pilote [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC 17.4.2 pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

 - Résolution d’un problème où l’ID de processus et le nom de l’application n’étaient pas envoyés correctement à SQL Server (pour l’analyse de sys.dm_exec_sessions) (Linux)
 - Suppression de la dépendance redondante sur libuuid (Linux)
 - Correction d’un bogue lors de l’envoi de données UTF8 à SQL Server 2019
 - Correction d’un bogue où les paramètres régionaux qui se terminent par « @euro » n’étaient pas été détectés correctement (Linux)
 - Correction du fait que les données XML n’étaient pas renvoyées correctement lorsqu’elles étaient extraites en tant que paramètre de sortie lors de l’utilisation de Always Encrypted

### <a name="bug-fixes-in-the-msconame-odbc-driver-174-for-ssnoversion"></a>Correctifs de bogues dans le pilote [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC 17.4 pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Correctif du problème intermittent lorsque MARS (Multiple Active Result Sets) est activé là où le pilote cesse de répondre
- Corriger le problème de résilience de la connexion lorsque la notification asynchrone est activée là où le pilote cesse de répondre
- Résolution d’un plantage lors de la récupération des enregistrements de diagnostic pour les tentatives de connexion multithread
- Correction du chiffrement non pris en charge lors de la reconnexion après avoir appelé SQLGetInfo() avec SQL_USER_NAME et SQL_DATA_SOURCE_READ_ONLY
- Correction de l’erreur d’initialisation COM lors de l’authentification interactive à Azure Active Directory
- Correction de SQLGetData() pour les données UTF8 sur plusieurs octets
- Correction de la récupération de la longueur des colonnes sql_variant à l’aide de SQLGetData()
- Correction de l’importation des colonnes sql_variant contenant plus de 7 992 octets à l’aide de bcp
- Correction de l’envoi de l’encodage vers le serveur pour les données à caractères étroits

### <a name="bug-fixes-in-the-msconame-odbc-driver-173-for-ssnoversion"></a>Correctifs de bogues dans le pilote [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC 17.3 pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Correction d’une fuite de mémoire du handle d’événement de notification d’envoi TCP
- Correction du problème de redéfinition de l’énumération _SQL_FILESTREAM_DESIRED_ACCESS dans le fichier d’en-tête msodbcsql.h
- Correction de l’ACCESS_TOKEN manquant et de la définition AUTHENTICATION liée dans le fichier d’en-tête msodbcsql.h pour Linux

### <a name="bug-fixes-in-the-msconame-odbc-driver-172-for-ssnoversion"></a>Correctifs de bogues dans le pilote [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC 17.2 pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Correction d’un message d’erreur concernant l’authentification Azure Active Directory
- Correction de la détection du codage lorsque les variables d’environnement des paramètres régionaux sont définies différemment
- Résolution d’un incident lors de la déconnexion avec récupération de la connexion en cours
- Correction de la détection de l’activité de la connexion
- Correction de la détection incorrecte des sockets fermés
- Correction d’une attente infinie lors de la tentative de libération d’un descripteur d’instruction pendant une récupération ayant échoué
- Correction du comportement de désinstallation incorrect lors de l’installation des versions 13 et 17 sur Windows
- Correction du comportement de déchiffrement sur les anciennes plateformes Windows (Windows 7, 8 et Server 2012)
- Résolution d’un problème de cache lors de l’utilisation de l’authentification ADAL sur Windows
- Résolution d’un problème qui verrouillait et remplaçait les journaux de suivi sur Windows

### <a name="bug-fixes-in-the-msconame-odbc-driver-171-for-ssnoversion"></a>Correctifs de bogues dans le pilote [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC 17.1 pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Correction du délai de 1 seconde lors de l’appel de SQLFreeHandle avec MARS activé et l’attribut de connexion « Encrypt = Yes »
- Correction d’une erreur de plantage 22003 dans SQLGetData lorsque la taille de la mémoire tampon passée est inférieure aux données récupérées (Windows)
- Correction des messages d’erreur ADAL tronqués
- Correction d’un bogue rare sur Windows 32 bits lors de la conversion d’un nombre à virgule flottante en entier
- Correction d’un problème où l’insertion de double dans un champ décimal avec Always Encrypted activé engendrait une erreur de troncation de données
- Correction d’un avertissement dans le programme d’installation macOS
- Correction de l’envoi d’un état incorrect à SQL Server lors de la tentative de récupération de session lorsque la résilience des connexions et le regroupement de connexions sont activés, ce qui provoquait la suppression de la session par le serveur

### <a name="bug-fixes-in-the-msconame-odbc-driver-17-for-ssnoversion"></a>Correctifs de bogues dans le pilote [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC 17 pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Correction d’un bogue qui, lors de l’utilisation de l’authentification Kerberos, faisait échouer l’insertion en bloc avec l’erreur « Accès refusé »
- Suppression de la solution de contournement pour un bogue unixODBC présent dans les versions antérieures à 2.3.1 (le pilote doublait la taille de certaines mémoires tampons transmises à unixODBC)
- Correction de la résilience des connexions (reconnexion) qui cesse de répondre lors de l’utilisation de ColumnEncryption=enabled
- Correction du bogue de création d’un nom de source de données, où l’option « Authentification interactive Active Directory » pouvait cesser de répondre (Windows)
- Correction d’un incident rare pendant l’arrêt de ODBC lorsque l’exécution asynchrone est activée (se produisait lors de l’effacement du descripteur de connexion)
- Résolution d’un problème où le pilote SQL provoquait une consommation élevée du processeur lors de l’exécution de procédures stockées longues
- Correction de l’impossibilité de récupérer des données dans une colonne varbinary(max) chiffrée sans conversion
- Résolution d’un problème où après extraction d’une colonne chiffrée de type varchar(max) null à l’aide de SQLGetData() sur un curseur statique, la colonne suivante était également null, même si elle contenait des données
- Correction d’un problème lors de l’extraction du champ varbinary(max) avec Always Encrypted activé
- Résolution d’un problème de setlocale() qui ne fonctionnait pas avec Always Encrypted
- Correction d’un problème avec SQLDescribeParam() retournant une erreur lors de l’appel sur un paramètre de procédure stockée de type XML avec Always Encrypted activé
- Correction des traits de soulignement avec caractère d’échappement qui ne fonctionnaient pas dans SQLTables
- Correction d’un bogue à cause duquel les données hébraïques (varchar) étaient tronquées lorsqu’elles étaient renvoyées sous forme de caractères larges sur Linux
- Correction d’un problème lors de l’interrogation de char/varchar encodés en Shift-JIS à partir d’une application UTF-8
- Correction du bogue à cause duquel l’appel de SQLGetInfo avec le paramètre SQL_DRIVER_NAME renvoyait un nom de fichier de type Linux sur macOS
- Résolution d’un problème où le chargement de données de caractères Windows-1252 avec des fichiers d’entrée de plus de 32 ko dans des colonnes VARCHAR avec l’utilitaire BCP entraînait des échecs
