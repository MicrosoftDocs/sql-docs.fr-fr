---
title: Notes de publication des pilotes Microsoft pour PHP
description: Cette page décrit ce qui a été modifié dans chaque version des pilotes Microsoft pour PHP pour SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- what's new in version 1.1
ms.assetid: 91cca3d2-ba99-4a6d-b0de-beb9699cb3f8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2d62068af3e6ec4a8f2711a5c11ab1f01388be8e
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076301"
---
# <a name="release-notes-for-the-microsoft-drivers-for-php-for-sql-server"></a>Notes de publication de Microsoft Drivers for PHP for SQL Server

Cette page décrit ce qui a été ajouté dans chaque version de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  

<!--
Hello, We are standardizing the format of content inside our Release Notes (or What's New) articles.
Instead of bullets (or paragraphs), we have shifted to the 2-column format you see for H2 **What's New in Version 5.6**.
It is not necessary to reformat all the older H2 sections in this Release Notes file, but.....

Going forward, please be sure to use the 2-column format.

Also, all Release Notes .md file names now must begin with 'release-notes-*.md'.  And no filler words.
The 5.6 edition of this file is being renamed.....
FROM:  'release-notes-for-the-php-sql-driver.md'
TO  :  'release-notes-php-sql-driver.md'

For any questions, ask GeneMi or CraigG.
Thanks a lot.  2019-03-28  (DevO= 1467988)
-->

## <a name="59"></a>5.9

![télécharger](../../ssms/media/download-icon.png) [Télécharger le package Windows](https://go.microsoft.com/fwlink/?linkid=?????)
[Balise de version GitHub (les packages Linux et macOS sont disponibles ici)](https://github.com/Microsoft/msphpsql/releases/tag/v5.9.0)

### <a name="version-information"></a>Informations sur la version

- Numéro de version : 5.9.0<sup>1</sup>
- Publication : 29 janvier 2021

### <a name="whats-new-in-59"></a>Nouveautés de la version 5.9

| Nouvel élément | Détails |
| :------- | :------ |
| Ajout de la prise en charge de PHP 8.0. | &nbsp; |
| Prise en charge de PHP 7.2 abandonnée. | &nbsp; |
| Ajout de la prise en charge de Microsoft ODBC Driver 17.7 sur toutes les plateformes. | &nbsp; |
| Ajout de la prise en charge de macOS Big Sur, Ubuntu 20.04, Ubuntu 20.10 et Alpine 3.12. | Certains nécessitent ODBC 17.5 ou ultérieur. |
| Suppression de la prise en charge de macOS High Sierra, Debian 8 et Ubuntu 19.10. | &nbsp; |
| Prise en charge des paramètres régionaux GB18030. | &nbsp; |
| Extension de PDO errorinfo pour y inclure des messages ODBC supplémentaires s’ils sont disponibles. | &nbsp; |
| Prise en charge de la classification des données avec des informations sur les degrés. | Nécessite SQL Server 2019 et le pilote ODBC 17.4.2 ou une version ultérieure. |
| Ajout de la prise en charge de l’authentification par principal de service Azure Active Directory. | Nécessite ODBC Drive 17.7 ou ultérieur. |
| Amélioration des performances lors du traitement des nombres décimaux en tant qu’entrées ou sorties, et suppression des conversions inutiles pour les valeurs numériques. | &nbsp; |
| Amélioration des performances lors de l’extraction de nombres en utilisant des mémoires tampon clientes. | &nbsp; |
| Définition du délai d’expiration des requêtes sans utilisation de LOCK TIMEOUT, ce qui permet d’économiser un aller-retour vers le serveur. | &nbsp; |
| &nbsp; | &nbsp; |

<sup>1</sup> Cette version nécessite ODBC Driver 17.4.2 ou ultérieur. Sinon, un avertissement d’échec de définition d’un attribut se produit. Cet avertissement peut être supprimé lors de l’utilisation d’un pilote ODBC plus ancien. Si vous utilisez SQLSRV, consultez [Procédure : configurer la gestion des erreurs et des avertissements à l’aide du pilote SQLSRV](https://docs.microsoft.com/sql/connect/php/how-to-configure-error-and-warning-handling-using-the-sqlsrv-driver). Si vous utilisez PDO_SQLSRV, les avertissements sont supprimés par défaut, mais ils peuvent être journalisés. Pour plus d’informations, consultez [Journalisation de l’activité](https://docs.microsoft.com/sql/connect/php/logging-activity).

## <a name="581"></a>5.8.1

Cette version s’applique uniquement à Linux et macOS.

[Balise de version GitHub (packages Linux et macOS disponibles ici)](https://github.com/Microsoft/msphpsql/releases/tag/v5.8.1)

### <a name="version-information"></a>Informations sur la version

- Numéro de version : 5.8.1
- Publication : 15 avril 2020

### <a name="whats-new-in-581"></a>Nouveautés de la version 5.8.1

| Nouvel élément | Détails |
| :------- | :------ |
| Correctif de bogue | Correction des problèmes de paramètres régionaux par défaut dans Alpine Linux. |
| Correctif de bogue | Suppression de la structure de données inutile pour la prise en charge de la fonctionnalité de curseurs côté client dans Alpine Linux. |
| Correctif de bogue | Correction des problèmes de journalisation lorsque les deux pilotes sont activés dans Alpine Linux. |
| &nbsp; | &nbsp; |

## <a name="58"></a>5.8

![Téléchargement](../../ssms/media/download-icon.png) [Télécharger le package Windows](https://go.microsoft.com/fwlink/?linkid=2120362)  
[Balise de version GitHub (packages Linux et macOS disponibles ici)](https://github.com/Microsoft/msphpsql/releases/tag/v5.8.0)

### <a name="version-information"></a>Informations sur la version

- Numéro de version : 5.8.0
- Publication : 31 janvier 2020

### <a name="whats-new-in-58"></a>Nouveautés de la version 5.8

| Nouvel élément | Détails |
| :------- | :------ |
| Ajout de la prise en charge de PHP 7.4. | &nbsp; |
| Prise en charge de PHP 7.1 abandonnée. | &nbsp; |
| Ajout de la prise en charge de Microsoft ODBC Driver 17.5 sur toutes les plateformes. | &nbsp; |
| Ajout de la prise en charge de Debian 10 et Red Hat 8. | Les deux nécessitent le pilote ODBC 17.4 ou une version ultérieure. |
| Ajout de la prise en charge de macOS Catalina, Alpine Linux 3.11<sup>1</sup> et Ubuntu 19.10. | Tous nécessitent le pilote ODBC 17.5 ou une version ultérieure. |
| Suppression de la prise en charge de SQL Server 2008 R2, macOS Sierra, Ubuntu 18.10 et Ubuntu 19.04. | &nbsp; |
| Prise en charge de l’option de langue lors de la connexion à SQL Server. | &nbsp; |
| Prise en charge des types de chaînes étendus introduits dans PHP 7.2. | &nbsp; |
| Prise en charge de la récupération des métadonnées de sensibilité de la classification des données. | Nécessite SQL Server 2019 et le pilote ODBC 17.4.2 ou une version ultérieure. |
| Prise en charge d’Always Encrypted avec enclaves sécurisées. | Requiert le pilote ODBC 17.4 ou une version ultérieure. |
| Prise en charge des options configurables pour les paramètres régionaux dans Linux et macOS. |
| Amélioration des performances grâce à la mise en cache des métadonnées lors des extractions et l’omission des appels redondants. | &nbsp; |
| &nbsp; | &nbsp; |

<sup>1</sup> La prise en charge d’Alpine Linux est expérimentale pour la version 5.8.

## <a name="previous-releases"></a>Versions précédentes

## <a name="561"></a>5.6.1

![Téléchargement](../../ssms/media/download-icon.png) [Télécharger le package Windows](https://go.microsoft.com/fwlink/?linkid=2120446)  
[Balise de version GitHub (packages Linux et macOS disponibles ici)](https://github.com/Microsoft/msphpsql/releases/tag/v5.6.1)

### <a name="version-information"></a>Informations sur la version

- Numéro de version : 5.6.1
- Publication : 19 mars 2019

### <a name="whats-new-in-561"></a>Nouveautés de la version 5.6.1

| Nouvel élément | Détails |
| :------- | :------ |
| Correctif de bogue | Correction des hypothèses effectuées lors du calcul des métadonnées de champ ou de colonne susceptibles d’avoir provoqué l’arrêt de l’application. |
| Correctif de bogue | Modification du fichier de configuration sqlsrv de sorte qu’il puisse être compilé indépendamment de pdo_sqlsrv. |
| Correctif de bogue | Correction de PDOStatement::getColumnMeta() de sorte qu’il retourne la valeur false en cas de problème. |
| &nbsp; | &nbsp; |

## <a name="56"></a>5.6

![Téléchargement](../../ssms/media/download-icon.png) [Télécharger le package Windows](https://go.microsoft.com/fwlink/?linkid=2120450)  
[Balise de version GitHub (packages Linux et macOS disponibles ici)](https://github.com/Microsoft/msphpsql/releases/tag/v5.6.0)

### <a name="version-information"></a>Informations sur la version

- Numéro de version : 5.6.0
- Publication : 21 février 2019

### <a name="whats-new-in-56"></a>Nouveautés de la version 5.6

| Nouvel élément | Détails |
| :------- | :------ |
| Prise en charge de PHP 7.3. | &nbsp; |
| Prise en charge de PHP 7.0 abandonnée. | &nbsp; |
| Prise en charge de Microsoft ODBC Driver 17.3 sur toutes les plateformes. | &nbsp; |
| Prise en charge de macOS Mojave. | Requiert le pilote ODBC 17.3 ou une version ultérieure. |
| Prise en charge d’Ubuntu 18.10 et SUSE Linux 15. | Les deux nécessitent le pilote ODBC 17.3 ou une version ultérieure. |
| Suppression de la prise en charge de Linux Ubuntu 17.10 et macOS El Capitan. | &nbsp; |
| Prise en charge du jeton d’accès Azure AD. | Sur Linux et macOS, requiert le pilote ODBC 17.2+ et unixODBC 2.3.6+. |
| Prise en charge de l’authentification avec Azure AD à l’aide de l’identité managée pour les ressources Azure. | Nécessite ODBC Driver 17.3+. |
| Nouvelles fonctionnalités d’extraction | &bull; &nbsp; Nouvel indicateur PDO::SQLSRV_ATTR_FETCHES_DATETIME_TYPE pour que pdo_sqlsrv renvoie des datetime en tant qu’objets.<br/><br/>&bull; &nbsp; Ajout de l’option ReturnDatesAsStrings au niveau de l’instruction pour sqlsrv.<br/><br/>&bull; &nbsp; Nouvelles options au niveau de la connexion et des instructions pour les deux pilotes afin de mettre en forme les valeurs décimales dans les résultats récupérés. |
| Prise en charge de la compilation statique des pilotes si les utilisateurs choisissent de générer à partir de la source. | &nbsp; |
| Amélioration des performances en mettant en cache les métadonnées lors des extractions et en accélérant les conversions de chaînes Unicode. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="53"></a>5.3

![Téléchargement](../../ssms/media/download-icon.png) [Télécharger le package Windows](https://go.microsoft.com/fwlink/?linkid=2120447)  
[Balise de version GitHub (packages Linux et macOS disponibles ici)](https://github.com/Microsoft/msphpsql/releases/tag/v5.3.0)

### <a name="version-information"></a>Informations sur la version

- Numéro de version : 5.3.0
- Publication : 20 juillet 2018

### <a name="whats-new-in-53"></a>Nouveautés de la version 5.3

- Prise en charge de Microsoft ODBC Driver 17.2 sur toutes les plateformes
- Prise en charge de macOS High Sierra (requiert le pilote ODBC 17 ou une version ultérieure)
- Prise en charge d’Azure Key Vault pour Always Encrypted pour les fonctionnalités CRUD de base, car la fonctionnalité Always Encrypted est disponible pour toutes les plateformes Windows, Linux ou macOS prises en charge [Utilisation d’Always Encrypted avec Microsoft Drivers for PHP for SQL Server](using-always-encrypted-php-drivers.md)
- Prise en charge d’Ubuntu 18.04 LTS (requiert le pilote ODBC 17.2)
- Prise en charge de la résilience des connexions dans Linux ou macOS (nécessite le pilote ODBC 17.2)

## <a name="52"></a>5.2

![Téléchargement](../../ssms/media/download-icon.png) [Télécharger le package Windows](https://go.microsoft.com/fwlink/?linkid=2120451)  
[Balise de version GitHub (packages Linux et macOS disponibles ici)](https://github.com/Microsoft/msphpsql/releases/tag/v5.2.0)

### <a name="version-information"></a>Informations sur la version

- Numéro de version : 5.2.0
- Publication : 23 mars 2018

### <a name="whats-new-in-52"></a>Nouveautés de la version 5.2

- Prise en charge de PHP 7.2.1 et versions ultérieures sur Windows, et de la version 7.2.0 sur les autres plateformes
- Prise en charge de Microsoft ODBC Driver 17
  - La version 17 est désormais la version par défaut sur toutes les plateformes
- Prise en charge d’Ubuntu 17.10, Debian 9 et SuSE Enterprise Linux 12
- Prise en charge d’Ubuntu 15.10 abandonnée
- Prise en charge d’Always Encrypted avec des fonctionnalités CRUD sur Windows. Pour plus d’informations, consultez [Utilisation d’Always Encrypted avec les pilotes PHP pour SQL Server](using-always-encrypted-php-drivers.md)
  - Prise en charge du Magasin de certificats Windows
  - Always Encrypted est pris en charge uniquement avec le pilote Microsoft ODBC 17 et versions ultérieures
- Prise en charge des paramètres régionaux non UTF8 sur Linux et macOS
  - Les paramètres régionaux non UTF8 sur Linux et macOS sont uniquement pris en charge avec le pilote Microsoft ODBC 17 et versions ultérieures
- Prise en charge pour Azure Synapse Analytics
- Prise en charge d’Azure SQL Managed Instance

## <a name="43"></a>4.3

![Téléchargement](../../ssms/media/download-icon.png) [Télécharger le package Windows](https://go.microsoft.com/fwlink/?linkid=2120616)  
[Balise de version GitHub (packages Linux et macOS disponibles ici)](https://github.com/Microsoft/msphpsql/releases/tag/v4.3.0)

### <a name="version-information"></a>Informations sur la version

- Numéro de version : 4.3.0
- Publication : 6 juillet 2017

### <a name="whats-new-in-43"></a>Nouveautés de la version 4.3

- Prise en charge de PHP 7.1
- Prise en charge de macOS Sierra et de macOS El Capitan
- Prise en charge d’Ubuntu 15.10 et de Debian 8
- Prise en charge d’Ubuntu 15.04 abandonnée
- Prise en charge des groupes de disponibilité Always On via une résolution d’adresse IP réseau transparente. Pour plus d’informations, consultez [Connection Options](connection-options.md).
- Ajout de la prise en charge du type de données sql_variant avec des limitations.
- Prise en charge de la résilience des connexions inactives dans Windows. Pour plus d’informations, consultez [Connection Options](connection-options.md).
- Prise en charge du regroupement de connexions pour Linux et macOS. Pour plus d’informations, consultez [Regroupement de connexions](connection-pooling-microsoft-drivers-for-php-for-sql-server.md).
- Prise en charge de l’authentification Azure Active Directory avec ActiveDirectoryPassword et SqlPassword. Pour plus d’informations, consultez [Connection Options](connection-options.md).

## <a name="40"></a>4.0

![Téléchargement](../../ssms/media/download-icon.png) [Télécharger le package Windows](https://go.microsoft.com/fwlink/?linkid=2120448)  
[Étiquette de mise en production GitHub](https://github.com/microsoft/msphpsql/releases/tag/v4.0-RTW)

### <a name="version-information"></a>Informations sur la version

- Numéro de version : 4.0
- Publication : 1er juillet 2016

### <a name="whats-new-in-40"></a>Nouveautés de la version 4.0

- Prise en charge de PHP 7.0  
- Prise en charge 64 bits complète
- Prise en charge d’Ubuntu 15.04, Ubuntu 16.04 et RedHat 7

## <a name="32"></a>3.2

![Téléchargement](../../ssms/media/download-icon.png) [Télécharger le package Windows](https://go.microsoft.com/fwlink/?linkid=2120449)  
[Étiquette de mise en production GitHub](https://github.com/microsoft/msphpsql/releases/tag/v3.2.0.0)

### <a name="version-information"></a>Informations sur la version

- Numéro de version : 3.2
- Publication : 9 mars 2015

### <a name="whats-new-in-32"></a>Nouveautés de la version 3.2

- Prise en charge de PHP 5.6  
- Inclut les dernières mises à jour pour les versions antérieures PHP 5.5 et 5.4  
- Nécessite Microsoft ODBC Driver 11 for SQL Server  

## <a name="31"></a>3.1

![Téléchargement](../../ssms/media/download-icon.png) [Télécharger le package Windows](https://go.microsoft.com/fwlink/?linkid=2143027)  
[Étiquette de mise en production GitHub](https://github.com/microsoft/msphpsql/releases/tag/v3.1.0.0)

### <a name="version-information"></a>Informations sur la version

- Numéro de version : 3.1
- Publication : 12 décembre 2014

### <a name="whats-new-in-31"></a>Nouveautés de la version 3.1

- Prise en charge de PHP 5.5  
- Nécessite Microsoft ODBC Driver 11 for SQL Server. Les versions précédentes nécessitaient SQL Native Client.  

## <a name="30"></a>3.0

![Téléchargement](../../ssms/media/download-icon.png) [Télécharger le package Windows](https://go.microsoft.com/fwlink/?linkid=2143026)  

### <a name="whats-new-in-30"></a>Nouveautés de la version 3.0  

- Prise en charge de PHP 5.4.  PHP 5.2 n’est pas pris en charge dans la version 3 du [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
- Ajout de l’option de connexion AttachDBFileName. Pour plus d’informations, consultez [Connection Options](connection-options.md).  
- Prise en charge de la Base de données locale, qui a été ajoutée dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. Pour plus d’informations, consultez [Prise en charge de LocalDB](php-driver-for-sql-server-support-for-localdb.md).
- Ajout de l’option de connexion AttachDBFileName. Pour plus d’informations, consultez [Connection Options](connection-options.md).  
- Prise en charge des fonctionnalités de récupération d’urgence/haute disponibilité. Pour plus d’informations, consultez [Prise en charge de la haute disponibilité et de la récupération d'urgence](php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).
- Prise en charge des curseurs côté client (mise en cache d’un jeu de résultats en mémoire). Pour plus d’informations, consultez [Types de curseur &#40;pilote SQLSRV&#41;](cursor-types-sqlsrv-driver.md) et [Types de curseur &#40;pilote PDO_SQLSRV&#41;](cursor-types-pdo-sqlsrv-driver.md).
- Ajout de l’attribut PDO::ATTR_EMULATE_PREPARES. Pour plus d’informations, consultez [PDO::prepare](pdo-prepare.md).  

## <a name="20"></a>2.0

### <a name="whats-new-in-20"></a>Nouveautés de la version 2.0

Dans la version 2.0, la prise en charge du pilote PDO_SQLSRV a été ajoutée. Pour plus d’informations, consultez [Référence de pilote PDO_SQLSRV](pdo-sqlsrv-driver-reference.md).  

## <a name="see-also"></a>Voir aussi

[Vue d’ensemble de Microsoft Drivers for PHP for SQL Server](overview-of-the-php-sql-driver.md)
