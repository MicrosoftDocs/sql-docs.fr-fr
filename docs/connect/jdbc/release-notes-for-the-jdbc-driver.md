---
title: Notes de publication pour le pilote JDBC
description: Cet article répertorie les versions du pilote JDBC Microsoft pour SQL Server. Pour chaque version publiée, les modifications sont nommées et décrites.
ms.custom: ''
ms.date: 03/02/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 074f211e-984a-4b76-bb15-ee36f5946f12
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b308bd441b063592a39dae710c782aaf826c3071
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464787"
---
# <a name="release-notes-for-the-microsoft-jdbc-driver-for-sql-server"></a>Notes de publication du pilote Microsoft JDBC pour SQL Server

Cet article répertorie les versions du _pilote JDBC Microsoft pour SQL Server_. Pour chaque version publiée, les modifications sont nommées et décrites.

## <a name="92"></a><a id="92"></a> 9.2

**[![Télécharger](../../ssms/media/download-icon.png) Télécharger Microsoft JDBC Driver 9.2.1 pour SQL Server (zip)](https://go.microsoft.com/fwlink/?linkid=2155948)**  
**[![Télécharger](../../ssms/media/download-icon.png) Télécharger Microsoft JDBC Driver 9.2.1 pour SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2155949)**  

Numéro de version : 9.2.1  
Publication : 2 mars 2021

Si vous avez besoin de télécharger le pilote dans une langue autre que celle détectée, vous pouvez utiliser ces liens directs.  
Pour télécharger le pilote sous forme de fichier zip : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x40a)  
Pour télécharger le pilote sous forme de fichier tar.gz : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x40a)  

### <a name="92-compliance"></a>Conformité de la version 9.2

| Modification de conformité | Détails |
| :---------------- | :------ |
| Téléchargez les dernières mises à jour pour JDBC Driver 9.2. | &bull; &nbsp; [GitHub, 9.2.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v9.2.1)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Entièrement conforme à la spécification de l’API JDBC 4.2. | Les fichiers JAR dans le package 9.2 sont nommés en fonction de la compatibilité avec les versions de Java.<br/><br/>Par exemple, le fichier mssql-jdbc-9.2.1.jre15.jar du package 9.2 doit être utilisé avec Java 15. |
| Compatible avec les versions 15.0, 11.0 et 1.8 du Kit de développement Java (JDK). | Microsoft JDBC Driver 9.2 pour SQL Server est maintenant compatible avec la version 15.0 du Kit de développement Java, en plus des versions 11.0 et 1.8. |
| &nbsp; | &nbsp; |

### <a name="92-releases"></a>Versions release 9.2

Numéro de version : 9.2.1  
Publication : 2 mars 2021

Problèmes résolus dans 9.2.1 :

Résolution d’un problème lié à la clé secrète client vide pendant l’authentification ActiveDirectoryServicePrincipal dans l’environnement Azure.

Numéro de version : 9.2.0  
Publication : 29 janvier 2021

**[![Télécharger](../../ssms/media/download-icon.png) Télécharger Microsoft JDBC Driver 9.2.0 pour SQL Server (zip)](https://go.microsoft.com/fwlink/?linkid=2153622)**  
**[![Télécharger](../../ssms/media/download-icon.png) Télécharger Microsoft JDBC Driver 9.2.0 pour SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2153521)**  

Fichier zip : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x40a)  
Fichier tar.gz : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x40a)  

### <a name="support-for-jdk-15"></a>Prise en charge de JDK 15

Microsoft JDBC Driver 9.2 pour SQL Server est maintenant compatible avec la version 15.0 du Kit de développement Java, en plus des versions 11.0 et 1.8.

### <a name="added-support-for-azure-active-directory-interactive-authentication"></a>Ajout de la prise en charge de l’authentification interactive Azure Active Directory

| Ajout du type d’authentification | Détails |
| :---------- | :------ |
| Microsoft JDBC Driver 9.2 pour SQL Server prend maintenant en charge l’authentification auprès d’Azure Key Vault avec l’authentification interactive. | Consultez [Connexion avec l’authentification Azure Active Directory](../../connect/jdbc/connecting-using-azure-active-directory-authentication.md). |
| &nbsp; | &nbsp; |

### <a name="added-support-for-azure-active-directory-service-principal-authentication"></a>Ajout de la prise en charge de l’authentification par principal de service Azure Active Directory

| Ajout du type d’authentification | Détails |
| :---------- | :------ |
| Microsoft JDBC Driver 9.2 pour SQL Server prend maintenant en charge l’authentification auprès d’Azure Key Vault en utilisant l’ID et le secret client d’une identité de principal de service. | Consultez [Connexion avec l’authentification Azure Active Directory](../../connect/jdbc/connecting-using-azure-active-directory-authentication.md). |
| &nbsp; | &nbsp; |

### <a name="updated-azure-key-vault-library-to-use-modern-azure-key-vault-library"></a>Mise à jour de la bibliothèque Azure Key Vault pour l’utilisation de la bibliothèque Azure Key Vault moderne

| Modifications de la bibliothèque pour Azure Key Vault | Détails |
| :------------------- | :------ |
| Microsoft JDBC Driver 9.2 a migré de la bibliothèque Azure Key Vault de la génération précédente et des bibliothèques ADAL vers les équivalents modernes d’Azure Key Vault et d’Azure Identity. | **MODIFICATION DES DÉPENDANCES** - Veillez à mettre à jour les dépendances de vos applications si vous tirez parti de l’authentification Azure Active Directory ou d’Azure Key Vault. Consultez les exigences de dépendances lors de l’[utilisation du fournisseur Azure Key Vault](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md#working-with-the-azure-key-vault-provider) ou de l’[authentification Azure Active Directory](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md#working-with-azure-active-directory-authentication) |
| &nbsp; | &nbsp; |

### <a name="enabled-usebulkcopyforbatchinsert-for-non-azure-synapse-analytics-servers"></a>useBulkCopyForBatchInsert activé pour les serveurs non-Azure Synapse Analytics

| Modifications de useBulkCopyForBatchInsert pour les serveurs non-Azure Synapse Analytics | Détails |
| :------------------- | :------ |
| Microsoft JDBC Driver 9.2 permet maintenant l’utilisation de l’API de copie en bloc pour l’opération d’insertion par lot sur des serveurs non-Azure Synapse Analytics. | Consultez [Utilisation de l’API de copie en bloc pour l’opération d’insertion par lot](../../connect/jdbc/use-bulk-copy-api-batch-insert-operation.md). |
| &nbsp; | &nbsp; |

## <a name="previous-releases"></a>Versions précédentes

## <a name="84"></a><a id="84"></a> 8.4

**[![Télécharger](../../ssms/media/download-icon.png) Télécharger Microsoft JDBC Driver 8.4 pour SQL Server (zip)](https://go.microsoft.com/fwlink/?linkid=2137600)**  
**[![Télécharger](../../ssms/media/download-icon.png) Télécharger Microsoft JDBC Driver 8.4 pour SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2137502)**  

Numéro de version : 8.4.1  
Publication : 27 août 2020

Si vous avez besoin de télécharger le pilote dans une langue autre que celle détectée, vous pouvez utiliser ces liens directs.  
Pour télécharger le pilote sous forme de fichier zip : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x40a)  
Pour télécharger le pilote sous forme de fichier tar.gz : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x40a)  

### <a name="84-compliance"></a>Conformité de la version 8.4

| Modification de conformité | Détails |
| :---------------- | :------ |
| Téléchargez les dernières mises à jour de JDBC Driver 8.4. | &bull; &nbsp; [GitHub, 8.4.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v8.4.1)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Entièrement conforme à la spécification de l’API JDBC 4.2. | Les fichiers JAR dans le package 8.4 sont nommés en fonction de la compatibilité avec les versions de Java.<br/><br/>Par exemple, le fichier mssql-jdbc-8.4.1.jre14.jar du package 8.4 doit être utilisé avec Java 14. |
| Compatible avec les versions 14.0, 11.0 et 1.8 du Kit de développement Java (JDK). | Microsoft JDBC Driver 8.4 pour SQL Server est maintenant compatible avec la version 14.0 du Kit de développement Java, en plus des versions 11.0 et 1.8. |
| &nbsp; | &nbsp; |

### <a name="84-releases"></a>Versions release 8.4

Numéro de version : 8.4.1  
Publication : 27 août 2020  
Problèmes résolus :  

- Correction d’un problème avec `SQLServerConnectionPoolProxy` non compatible avec `delayLoadingLobs`
- Correction d’un problème potentiel de `NullPointerException` avec `delayLoadingLobs`
- Correction d’un problème avec le déchiffrement de clés de chiffrement de colonnes lors de l’utilisation du magasin de certificats Windows

Numéro de version : 8.4.0  
Publication : 31 juillet 2020  

### <a name="support-for-jdk-14"></a>Support du JDK 14

Microsoft JDBC Driver 8.4 pour SQL Server est maintenant compatible avec la version 14.0 du Kit de développement Java, en plus des versions 11.0 et 1.8.

### <a name="added-support-for-authentication-to-azure-key-vault-using-managed-identity"></a>Ajout du support de l’authentification à Azure Key Vault à l’aide de Managed Identity

| Ajout du type d’authentification | Détails |
| :---------- | :------ |
| Microsoft JDBC Driver 8.4 pour SQL Server prend désormais en charge l’authentification sur Azure Key Vault à l’aide de Managed Identity. | Consultez [Utilisation d'Always Encrypted avec le pilote JDBC](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md). |
| &nbsp; | &nbsp; |

### <a name="extended-support-for-bulk-copy-for-azure-data-warehouse"></a>Support étendu pour la copie en bloc pour Azure Data Warehouse

| Modifications de la copie en bloc pour Data Warehouse | Détails |
| :------------------- | :------ |
| Microsoft JDBC Driver 8.4 ajoute une nouvelle propriété de connexion, `sendTemporalDataTypesAsStringForBulkCopy`. Par défaut, cette propriété booléenne est TRUE. | Consultez [Utilisation de la copie en bloc avec le pilote JDBC](../../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md). |
| &nbsp; | &nbsp; |

### <a name="added-support-for-azure-sql-dns-caching"></a>Support ajouté pour la mise en cache DNS d'Azure SQL

| Mise en cache DNS | Détails |
| :------------------- | :------ |
| Microsoft JDBC Driver 8.4 pour SQL Server prend désormais en charge la mise en cache DNS sur les serveurs SQL Azure. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="added-backwards-compatibility-for-streaming-lob-objects"></a>Ajout de la compatibilité descendante pour les objets LOB diffusés en continu

| Diffusion en continu LOB | Détails |
| :------------------- | :------ |
| Microsoft JDBC Driver 8.4 pour SQL Server ajouté à une nouvelle propriété de connexion `delayLoadingLobs`. | Si `delayLoadingLobs` est défini sur FALSE, tous les objets LOB récupérés de ResultSet ne sont pas diffusés en continu. Cela signifie que le pilote chargera l’intégralité de l’objet LOB en mémoire en une seule fois, de la même façon que le pilote fonctionnait avant la version 6.4. |
| &nbsp; | &nbsp; |

### <a name="added-support-for-client-certificate-authentication-for-loopback-scenarios"></a>Support de l’authentification par certificat client pour les scénarios de bouclage ajouté

| Authentification du certificat du client | Détails |
| :------------------- | :------ |
| Microsoft JDBC Driver 8.4 pour SQL Server a ajouté une nouvelle méthode d’authentification appelée authentification par certificat client pour les scénarios de bouclage. | Consultez [Authentification par certificat client pour les scénarios de bouclage](../../connect/jdbc/client-certification-authentication-for-loopback-scenarios.md). |

## <a name="82"></a><a id="82"></a> 8.2

**[![Télécharger](../../ssms/media/download-icon.png) Télécharger le pilote Microsoft JDBC 8.2 pour SQL Server (zip)](https://go.microsoft.com/fwlink/?linkid=2122433)**  
**[![Télécharger](../../ssms/media/download-icon.png) Télécharger le pilote Microsoft JDBC 8.2 pour SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122536)**  

Numéro de version : Publication de la version 8.2.2 : 24 mars 2020

Si vous avez besoin de télécharger le pilote dans une langue autre que celle détectée, vous pouvez utiliser ces liens directs.  
Pour télécharger le pilote sous forme de fichier zip : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x40a)  
Pour télécharger le pilote sous forme de fichier tar.gz : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x40a)  

### <a name="82-compliance"></a>Conformité de la version 8.2

| Modification de conformité | Détails |
| :---------------- | :------ |
| Téléchargez les dernières mises à jour du pilote JDBC 8.2. | &bull; &nbsp; [GitHub, 8.2.2](https://github.com/Microsoft/mssql-jdbc/releases/tag/v8.2.2)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Entièrement conforme à la spécification de l’API JDBC 4.2. | Les fichiers JAR dans le package 8.2 sont nommés en fonction de la compatibilité avec les versions de Java.<br/><br/>Par exemple, le fichier mssql-jdbc-8.2.2.jre11.jar du package 8.2 doit être utilisé avec Java 11. |
| Compatible avec le kit JDK versions 13.0, 11.0 et 1.8. | Microsoft JDBC Driver 8.2 pour SQL Server est désormais compatible avec le JDK version 13.0 en plus des versions 11.0 et 1.8. |
| &nbsp; | &nbsp; |

### <a name="82-releases"></a>Versions release 8.2

Numéro de version : 8.2.2  
Publication : 24 mars 2020  
Problèmes résolus :  

- Ajout d’une option pour configurer la liste des points de terminaison de Azure Key Vault approuvés

Numéro de version : 8.2.1  
Publication : 26 février 2020  
Problèmes résolus :  

- Correction d’un problème potentiel de `NullPointerException` lors de la récupération de données en tant que type `java.time.LocalTime` ou `java.time.LocalDate` avec `SQLServerResultSet.getObject()`

Numéro de version : 8.2.0  
Publication : 31 janvier 2020  

### <a name="support-for-jdk-13"></a>Prise en charge du JDK 13

Microsoft JDBC Driver 8.2 pour SQL Server est désormais compatible avec le JDK version 13.0 en plus des versions 11.0 et 1.8.

### <a name="always-encrypted-with-secure-enclaves"></a>Always Encrypted avec enclaves sécurisées

| Changement concernant Always Encrypted | Détails |
| :--------- | :------ |
| Le pilote Microsoft JDBC 8.2 pour SQL Server prend désormais en charge Always Encrypted avec enclaves sécurisées. Vous trouverez les détails ici : Always Encrypted avec enclaves sécurisées. |
| Détails et exemples de code supplémentaires. | Consultez [Always Encrypted avec enclaves sécurisées](../../connect/jdbc/using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver.md). |
| &nbsp; | &nbsp; |

### <a name="performance-improvement-when-retrieving-temporal-datatypes-from-sql-server-sup1sup"></a>Amélioration des performances lors de la récupération de types de données temporels à partir de SQL Server <sup>1</sup>

| Changement concernant les types de données temporels | Détails |
| :---------- | :------ |
| Le pilote Microsoft JDBC 8.2 pour SQL Server améliore les performances lors de la récupération de types de données temporels auprès de SQL Server. | Ce changement évite les conversions de types de données temporels inutiles en supprimant l’utilisation de java.util.Calendar dans la mesure du possible. |
| La liste suivante répertorie les types de données temporels qui ont bénéficié de cette amélioration des performances ; les types sont exprimés au format de type de données SQL Server suivi du mappage Java respectif. | date (java.sql.Date), datetime (java.sql.Timestamp), datetime2 (java.sql.Timestamp), smalldatetime (java.sql.Timestamp) et time (java.sql.Time). |
| &nbsp; | &nbsp; |

<sup>1</sup> En raison du mode de traitement différent des fuseaux horaires par java.util.Calendar ou l’API java.time.LocalDateTime, cette amélioration ne concerne pas les types de données temporels qui sont associés à un objet java.util.Calendar fourni par l’utilisateur, ni les types de données microsoft.sql.DateTimeOffset.

### <a name="deployment-of-mssql-jdbc_auth-version-archdll-previously-sqljdbc_authdll-to-maven-repository"></a>Déploiement de mssql-jdbc_auth-\<version>-\<arch>.dll (précédemment sqljdbc_auth.dll) sur le Référentiel Maven

| Changement concernant sqljdbc_auth.dll | Détails |
| :------------------- | :------ |
| À compter de la version JDBC Driver 8.2 pour SQL Server, le pilote s’appuie sur mssql-jdbc_auth-\<version>-\<arch>.dll au lieu de sqljdbc_auth.dll pour utiliser la fonctionnalité d’authentification d’Azure Active Directory. | &nbsp; |
| La DLL a également été chargée dans le référentiel Maven pour faciliter son accès. | Consultez [cette page](https://search.maven.org/artifact/com.microsoft.sqlserver/mssql-jdbc_auth). |
| &nbsp; | &nbsp; |

### <a name="82-known-issues"></a>Problèmes connus avec 8.2

| Problèmes connus | Détails |
| :----------- | :------ |
| Quand Always Encrypted avec enclaves sécurisées est utilisé avec Java 8. | Les utilisateurs doivent inclure le fournisseur BouncyCastle en tant que dépendance OU mapper/charger un fournisseur de sécurité qui prend en charge l’algorithme de signature RSASSA-PSS. |
| &nbsp; | &nbsp; |

## <a name="a-id74-741"></a><a id="74"> 7.4.1

**[![Télécharger](../../ssms/media/download-icon.png) Télécharger le pilote Microsoft JDBC 7.4.1 pour SQL Server (fichier exécutable à extraction automatique)](https://go.microsoft.com/fwlink/?linkid=2122712)**  
**[![Télécharger](../../ssms/media/download-icon.png) Télécharger le pilote Microsoft JDBC 7.4.1 pour SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122613)**  

Numéro de version : 7.4.1  
Publication : 2 août 2019

Si vous avez besoin de télécharger le pilote dans une langue autre que celle détectée, vous pouvez utiliser ces liens directs.  
Pour télécharger le pilote sous forme de fichier exécutable à extraction automatique : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x40a)  
Pour télécharger le pilote sous forme de fichier tar.gz : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x40a)  

### <a name="74-compliance"></a>Conformité de la version 7.4

| Modification de conformité | Détails |
| :---------------- | :------ |
| Téléchargez les dernières mises à jour du pilote JDBC 7.4. | &bull; &nbsp; [GitHub, 7.4.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v7.4.1)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Entièrement conforme à la spécification de l’API JDBC 4.2. | Les fichiers JAR dans le package 7.4 sont nommés en fonction de la compatibilité avec les versions de Java.<br/><br/>Par exemple, le fichier mssql-jdbc-7.4.1.jre11.jar du package 7.4 doit être utilisé avec Java 11. |
| Compatible avec le kit de développement Java (JDK) versions 12.0, 11.0 et 1.8. | Le pilote JDBC Microsoft 7.4 pour SQL Server est désormais compatible avec JDK (Java Development Kit) version 12.0 en plus de 11.0 et 1.8. |
| &nbsp; | &nbsp; |

### <a name="74-releases"></a>Versions release 7.4

Numéro de version : 7.4.1  
Publication : 2 août 2019  
Problèmes résolus :  

- Restauration des nouvelles implémentations d’API `hashCode()` et `equals()` à partir de `SQLServerDataTable` et `SQLServerDataColumn`, car la modification de l’API a rompu la compatibilité descendante

Numéro de version : 7.4.0  
Publication : 31 juillet 2019  

### <a name="support-for-jdk-12"></a>Prise en charge de JDK 12

Le pilote JDBC Microsoft 7.4 pour SQL Server est désormais compatible avec JDK (Java Development Kit) version 12.0 en plus de 11.0 et 1.8.

### <a name="introduces-ntlm-authentication"></a>Présente l'authentification NTLM.

| Modification de NTLM | Détails |
| :--------- | :------ |
| Prend en charge le mode d’authentification NTLM. | Ce mode d’authentification permet aux clients Windows et non Windows de s’authentifier auprès de SQL Server à l’aide d’utilisateurs de domaine Windows. |
| Plus de détails et un exemple d’application pour utiliser ce mode d’authentification. | Consultez [Se connecter avec l’authentification NTLM](using-ntlm-authentication-to-connect-to-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="introduces-querying-parametermetadata-via-_usefmtonly_"></a>Présente l’interrogation de ParameterMetaData via _useFmtOnly_

| modification de useFmtOnly | Détails |
| :---------- | :------ |
| propriété de connexion **useFmtOnly**. | Cette fonctionnalité permet aux utilisateurs d’interroger éventuellement ParameterMetaData via l’API héritée `SET FMTONLY ON`. Cela est utile pour les scénarios où `sp_describe_undeclared_parameters` ne s’exécute pas comme prévu. |
| Plus de détails et de restrictions. | Voir [Utilisation d’useFmtOnly](using-usefmtonly.md) |
| &nbsp; | &nbsp; |

### <a name="updated-_microsoft-azure-key-vault-sdk-for-java_-version-121"></a>Mise à jour du _kit de développement logiciel (SDK) Microsoft Azure Key Vault pour Java_, version 1.2.1

| Modification du SDK Key Vault | Détails |
| :------------------- | :------ |
| Mise à jour de la dépendance Maven sur le _kit de développement logiciel (SDK) Microsoft Azure Key Vault pour Java_ vers la version 1.2.1. | &nbsp; |
| Supprime _Microsoft Azure SDK pour Key Vault WebKey_ en tant que dépendance Maven. | &nbsp; |
| Détails supplémentaires. | Consultez [Dépendances de fonctionnalité de Microsoft JDBC Driver pour SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="74-known-issues"></a>Problèmes connus avec 7.4

| Problèmes connus | Détails |
| :----------- | :------ |
| Pendant l’utilisation de l’authentification NTLM. | L’activation simultanée de la protection étendue et des connexions chiffrées n’est pas prise en charge actuellement. |
| Pendant l’utilisation d’useFmtOnly. | Il existe des problèmes avec la fonctionnalité, qui sont dus à des lacunes dans la logique d’analyse SQL. Consultez [Utilisation de useFmtOnly](using-usefmtonly.md) pour plus d’informations et pour obtenir des suggestions de contournement. |
| &nbsp; | &nbsp; |

## <a name="a-id72-722"></a><a id="72"> 7.2.2

**[![Télécharger](../../ssms/media/download-icon.png) Télécharger le pilote Microsoft JDBC 7.2.2 pour SQL Server (fichier exécutable à extraction automatique)](https://go.microsoft.com/fwlink/?linkid=2122435)**  
**[![Télécharger](../../ssms/media/download-icon.png) Télécharger le pilote Microsoft JDBC 7.2.2 pour SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122434)**  

Numéro de version : 7.2.2  
Publication : Avril 16, 2019

Si vous avez besoin de télécharger le pilote dans une langue autre que celle détectée, vous pouvez utiliser ces liens directs.  
Pour télécharger le pilote sous forme de fichier exécutable à extraction automatique : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x40a)  
Pour télécharger le pilote sous forme de fichier tar.gz : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x40a)  

### <a name="72-compliance"></a>Conformité de la version 7.2

| Modification de conformité | Détails |
| :---------------- | :------ |
| Téléchargez les dernières mises à jour du pilote JDBC 7.2. | &bull; &nbsp; [GitHub, 7.2.2](https://github.com/Microsoft/mssql-jdbc/releases/tag/v7.2.2)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Entièrement conforme à la spécification de l’API JDBC 4.2. | Les fichiers JAR dans le package 7.2 sont nommés en fonction de la compatibilité des versions de Java.<br/><br/>Par exemple, le fichier mssql-jdbc-7.2.2.jre11.jar du package 7.2 doit être utilisé avec Java 11. |
| Compatible avec JDK (Java Development Kit) version 11.0 en plus de JDK 1.8. | Le pilote JDBC Microsoft 7.2 pour SQL Server est désormais compatible avec JDK (Java Development Kit) version 11.0 en plus de JDK 1.8. |
| &nbsp; | &nbsp; |

### <a name="72-releases"></a>Versions release 7.2

Numéro de version : 7.2.2  
Publication : Avril 16, 2019  
Problèmes résolus :  

- Correction des problèmes de nettoyage des ID d’activité

Numéro de version : 7.2.1  
Publication : 11 février 2019  
Problèmes résolus :  

- Correction des problèmes d’analyse de certaines requêtes paramétrables

Numéro de version : 7.2.0  
Publication : 31 janvier 2019  

### <a name="active-directory-_managed-identity_-msi-authentication"></a>Authentification _Managed Identity_ Azure Active Directory

| Changement MSI | Détails |
| :--------- | :------ |
| Prend en charge le mode d’authentification Managed Identity Active Directory. | Ce mode d’authentification est applicable aux ressources Azure avec support de la fonctionnalité « Identité » activée.<br/><br/>Les deux types d’identités managées sont pris en charge par le pilote pour acquérir **accessToken** afin d’établir une connexion sécurisée. |
| Plus de détails et un exemple d’application pour utiliser ce mode d’authentification. | Consultez [Connexion avec l’authentification Azure Active Directory](connecting-using-azure-active-directory-authentication.md). |
| &nbsp; | &nbsp; |

### <a name="introduces-_open-service-gateway-initiative_-osgi-support"></a>Introduit la prise en charge d’_Open Service Gateway Initiative_ (OSGi)

| Modification de l’OSGi | Détails |
| :---------- | :------ |
| Implémentation **DataSourceFactory** ajoutée. | &bull; &nbsp; `org.osgi.service.jdbc.DataSourceFactory`<br/>&bull; &nbsp; `com.microsoft.sqlserver.jdbc.osgi.SQLServerDataSourceFactory` |
| Implémentation **Activateur** ajoutée. | &bull; &nbsp; `org.osgi.framework.BundleActivator`<br/>&bull; &nbsp; `com.microsoft.sqlserver.jdbc.osgi.Activator` |
| &nbsp; | &nbsp; |

### <a name="introduces-_sqlservererror_-apis"></a>Introduit les API _SQLServerError_

| Modification de l’API d’erreur | Détails |
| :--------------- | :------ |
| Introduction de l’API SQLServerError. | API getter pour récupérer des détails supplémentaires sur l’erreur générée à partir du serveur.<br/><br/>&bull; &nbsp; `SQLServerException.getSQLServerError()`<br/>&bull; &nbsp; `SQLServerError` |
| Détails supplémentaires. | Consultez [Gestion des erreurs](handling-errors.md). |
| &nbsp; | &nbsp; |

### <a name="updated-_microsoft-azure-active-directory-authentication-library-adal4j-for-java_-version-163"></a>Mise à jour de la _bibliothèque d’authentification Microsoft Azure Active Directory (ADAL4J) pour Java_, version 1.6.3

| Modification d’ADAL4J | Détails |
| :------------ | :------ |
| Mise à jour de la dépendance Maven sur ADAL4J vers la version 1.6.3. | &nbsp; |
| Introduit _exécution du Client Java pour AutoRest_ comme une dépendance Maven, version 1.6.5). | &nbsp; |
| Détails supplémentaires. | Consultez [Dépendances de fonctionnalité de Microsoft JDBC Driver pour SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="updated-_microsoft-azure-key-vault-sdk-for-java_-version-120"></a>Mise à jour du _kit de développement logiciel (SDK) Microsoft Azure Key Vault pour Java_, version 1.2.0

| Modification du SDK Key Vault | Détails |
| :------------------- | :------ |
| Mise à jour de la dépendance Maven sur le _kit de développement logiciel (SDK) Microsoft Azure Key Vault pour Java_ vers la version 1.2.0. | &nbsp; |
| Présente _Microsoft Azure SDK pour Key Vault WebKey_ en tant que dépendance Maven, version 1.2.0. | &nbsp; |
| Détails supplémentaires. | Consultez [Dépendances de fonctionnalité de Microsoft JDBC Driver pour SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="72-known-issues"></a>Problèmes connus avec 7.2

| Problèmes connus | Détails |
| :----------- | :------ |
| Requêtes paramétrables, dans certains cas. | v7.2.1, une mise à jour de la version 7.2.0, a été publiée en février 2019 pour résoudre ce problème. |
| Nettoyage d’ActivityId. | v7.2.2, une mise à jour de la version 7.2.1, a été publiée en avril 2019 pour résoudre ce problème. |
| &nbsp; | &nbsp; |

## <a name="70"></a>7.0

**[![Télécharger](../../ssms/media/download-icon.png) Télécharger le pilote Microsoft JDBC 7.0 pour SQL Server (fichier exécutable à extraction automatique)](https://go.microsoft.com/fwlink/?linkid=2122713)**  
**[![Télécharger](../../ssms/media/download-icon.png) Télécharger le pilote Microsoft JDBC 7.0 pour SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122614)**  

Numéro de version : 7.0.0  
Publication : 31 juillet 2018

Si vous avez besoin de télécharger le pilote dans une langue autre que celle détectée, vous pouvez utiliser ces liens directs.  
Pour télécharger le pilote sous forme de fichier exécutable à extraction automatique : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x40a)  
Pour télécharger le pilote sous forme de fichier tar.gz : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x40a)  

Le pilote JDBC 7.0 Microsoft pour SQL Server est parfaitement conforme à la spécification de l’API JDBC 4.2. Les fichiers JAR dans le package 7.0 sont nommés en fonction de la compatibilité avec les versions de Java. Par exemple, le fichier mssql-jdbc-7.0.0.jre10.jar du package 7.0 doit être utilisé avec Java 10.

### <a name="support-for-jdk-10"></a>Prise en charge de JDK 10

Le pilote JDBC Microsoft 7.0 pour SQL Server est désormais compatible avec JDK (Java Development Kit) version 10.0 en plus de JDK 1.8. Cette mise à jour expose également le `Automatic-Module-Name` du pilote en tant que `com.microsoft.sqlserver.jdbc` via son fichier MANIFESTE.

### <a name="support-for-spatial-datatypes"></a>Prise en charge des types de données spatiaux

Le pilote JDBC 7.0 Microsoft pour SQL Server prend désormais en charge les types de données spatiales Géographie et Géométrie de SQL Server. Pour plus d’informations sur les API des types de données spatiales et la façon de les utiliser, consultez [Utilisation des types de données spatiales](use-spatial-datatypes.md).

### <a name="implementation-for-jdbc-43-introduced-javasqlconnection-apis-beginrequest-and-endrequest"></a>Implémentation des API java.sql.Connection beginRequest() et endRequest() introduites dans JDBC 4.3

Le pilote JDBC 7.0 Microsoft pour SQL Server implémente maintenant les API `beginRequest()` et `endRequest()` à partir de la classe `java.sql.Connection`. Ces API ont été introduites avec les spécifications JDBC 4.3 et JDK 9. Pour plus d’informations sur l’implémentation du pilote de ces API, consultez [Conformité de JDBC 4.3 pour le pilote JDBC](jdbc-4-3-compliance-for-the-jdbc-driver.md).

### <a name="support-for-sql-data-discovery-and-classification"></a>Prise en charge de la détection et de la classification des données SQL

Le pilote JDBC 7.0 Microsoft pour SQL Server fournit un support pour la détection et la classification des données SQL avec une base de données cible prenant en charge cette fonctionnalité. Le pilote expose désormais des API `SQLServerResultSet.getSensitivityClassification()` pour extraire ces informations à partir de `ResultSet` récupéré.

Pour plus d'informations sur l'utilisation de cette fonctionnalité avec le pilote JDBC, consultez l'exemple fourni dans [Détection et classification de données SQL](data-discovery-classification-sample.md).

### <a name="added-connection-property-usebulkcopyforbatchinsert"></a>Ajout d’une propriété de connexion : useBulkCopyForBatchInsert

Le pilote JDBC 7.0 Microsoft pour SQL Server introduit une nouvelle propriété de connexion, `useBulkCopyForBatchInsert`. Cette propriété est prise en charge uniquement pour Azure Synapse Analytics.

Elle est désactivée par défaut. Vous pouvez l’activer pour augmenter les performances des applications de l’utilisateur lorsque vous transmettez de grandes quantités de données à Azure Synapse Analytics. L’activation de cette propriété modifie le comportement des opérations d’insertion de lots pour passer à des opérations de copie en bloc avec des données fournies par l’utilisateur. Pour plus d’informations sur cette propriété et ses limitations, consultez [Utilisation de l’API de copie en bloc pour les opérations d’insertion de lots](use-bulk-copy-api-batch-insert-operation.md).

### <a name="added-connection-property-cancelquerytimeout"></a>Ajout d’une propriété de connexion : cancelQueryTimeout

Le pilote JDBC 7.0 Microsoft pour SQL Server introduit une nouvelle propriété de connexion, `cancelQueryTimeout`, pour annuler `queryTimeout` sur les objets `java.sql.Connection` et `java.sql.Statement`.

### <a name="added-azure-key-vault-provider-constructors"></a>Ajout de constructeurs Azure Key Vault Provider

Le pilote JDBC 7.0 Microsoft pour SQL Server réintroduit un constructeur précédemment supprimé pour `SQLServerColumnEncryptionAzureKeyVaultProvider`. Il permettait l’authentification avec une méthode personnalisée implémentée via `SQLServerKeyVaultAuthenticationCallback` pour récupérer un jeton d’accès.

Les nouveaux constructeurs ont la définition suivante :

```java
/* This constructor is added to provide backward compatibility with 6.0
* version of the driver. It is marked deprecated for removal in the next
* stable release.
*/
@Deprecated
public SQLServerColumnEncryptionAzureKeyVaultProvider(
        SQLServerKeyVaultAuthenticationCallback authenticationCallback,
        ExecutorService executorService) throws SQLServerException;

/*New constructor to replace the above constructor*/
public SQLServerColumnEncryptionAzureKeyVaultProvider(
            SQLServerKeyVaultAuthenticationCallback authenticationCallback) throws SQLServerException;
```

### <a name="updated-microsoft-azure-active-directory-authentication-library-adal4j-for-java-version-160"></a>Version mise à jour de la « bibliothèque d’authentification Microsoft Azure Active Directory (ADAL4J) pour Java » : 1.6.0

Le pilote JDBC 7.0 Microsoft pour SQL Server a mis à jour sa dépendance Maven sur la « bibliothèque d'authentification Active Directory Microsoft Azure (ADAL4J) pour Java » vers la version 1.6.0. Pour plus d’informations sur les dépendances, consultez [Dépendances de fonctionnalité de Microsoft JDBC Driver pour SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md).

## <a name="64"></a>6.4

**[![Télécharger](../../ssms/media/download-icon.png) Télécharger le pilote Microsoft JDBC 6.4 pour SQL Server (fichier exécutable à extraction automatique)](https://go.microsoft.com/fwlink/?linkid=2122436)**  
**[![Télécharger](../../ssms/media/download-icon.png) Télécharger le pilote Microsoft JDBC 6.4 pour SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122537)**  

Numéro de version : 6.4.0  
Publication : 27 février 2018

Si vous avez besoin de télécharger le pilote dans une langue autre que celle détectée, vous pouvez utiliser ces liens directs.  
Pour télécharger le pilote sous forme de fichier exécutable à extraction automatique : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x40a)  
Pour télécharger le pilote sous forme de fichier tar.gz : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x40a)  

Le pilote JDBC 6.4 Microsoft pour SQL Server est parfaitement conforme aux spécifications JDBC 4.1. et 4.2. Les fichiers JAR dans le package 6.4 sont nommés en fonction de la compatibilité avec les versions de Java. Par exemple, le fichier mssql-jdbc-6.4.0.jre8.jar du package 6.4 doit être utilisé avec Java 8.

### <a name="support-for-jdk-9"></a>Prise en charge de JDK 9

Le pilote prend en charge JDK 9.0 en plus de JDK 8.0 et 7.0.

### <a name="jdbc-43-compliance"></a>Conformité avec JDBC 4.3

Le pilote prend en charge la spécification de l’API Java Database Connectivity 4.3, en plus des versions 4.1 et 4.2. Les méthodes de l’API JDBC 4.3 sont ajoutés, mais pas encore implémentés. Pour plus d’informations, consultez [Conformité à JDBC 4.3 pour le pilote JDBC](jdbc-4-3-compliance-for-the-jdbc-driver.md).

### <a name="added-connection-property-sslprotocol"></a>Ajout d’une propriété de connexion : sslProtocol

Une nouvelle propriété de connexion permet aux utilisateurs de spécifier le mot clé du protocole TLS. Les valeurs possibles sont les suivantes : « TLS », « TLSv1 », « TLSv1.1 » et « TLSv1.2 ». Pour plus d’informations, consultez [SSLProtocol](https://github.com/Microsoft/mssql-jdbc/wiki/SSLProtocol).

### <a name="deprecated-connection-property-fipsprovider"></a>Propriété de connexion dépréciée : fipsProvider

La propriété de connexion `fipsProvider` est supprimée de la liste des propriétés de connexion acceptées. Pour plus d’informations, consultez la [demande de tirage (pull request) GitHub](https://github.com/Microsoft/mssql-jdbc/pull/460).

### <a name="added-connection-properties-for-specifying-a-custom-trustmanager"></a>Ajout de propriétés de connexion pour spécifier un TrustManager personnalisé

Le pilote prend désormais en charge la spécification d’un TrustManager personnalisé grâce à l’ajout des propriétés de connexion `trustManagerClass` et `trustManagerConstructorArg`. Vous pouvez spécifier dynamiquement un ensemble de certificats approuvés sur une base par connexion sans modifier les paramètres globaux de l’environnement de machine virtuelle (JVM) Java.

### <a name="added-support-for-datetimesmalldatetime-in-table-valued-parameters"></a>Ajout d’une prise en charge pour datetime/smallDatetime dans les paramètres table

Le pilote prend désormais en charge les types de données `datetime` et `smallDatetime` lorsque vous utilisez des paramètres table (TVP).

### <a name="added-support-for-the-sql_variant-datatype"></a>Ajout d’une prise en charge pour le type de données sql_variant

Le pilote JDBC prend désormais en charge les types de données `sql_variant` à utiliser avec SQL Server. Le type de données `sql_variant` est également pris en charge avec des fonctionnalités telles que les paramètres table et de copie en bloc, avec les limitations suivantes :

- **Pour les valeurs de date** :

  Lorsque vous utilisez un paramètre de table (TVP) pour remplir une table qui contient des valeurs `datetime`, `smalldatetime` ou `date` stockées dans une colonne `sql_variant`, l’appel de la méthode `getDateTime()`, `getSmallDateTime()` ou `getDate()` ne fonctionne pas sur le jeu de résultats et lève l’exception suivante :

  `java java.lang.String cannot be cast to java.sql.Timestamp`

  Pour contourner ce problème, utilisez la méthode `getString()` ou `getObject()` à la place.

- **Utilisation d’un paramètre table (TVP) avec sql_variant pour les valeurs null** :
  
  Si vous utilisez un TVP pour remplir une table et envoyez une valeur NULL au type de colonne `sql_variant`, vous rencontrez une exception. L’insertion d’une valeur NULL avec le type de colonne `sql_variant` dans un TVP n’est actuellement pas prise en charge.

### <a name="implemented-prepared-statement-metadata-caching"></a>Implémentation de la mise en cache des métadonnées de l’instruction préparée

Le pilote JDBC a implémenté la mise en cache des métadonnées de l’instruction préparée pour améliorer les performances. Le pilote prend désormais en charge la mise en cache des métadonnées de l’instruction préparée dans le pilote avec les propriétés de connexion `disableStatementPooling` et `statementPoolingCacheSize`. Cette fonctionnalité est désactivée par défaut. Pour plus d’informations, consultez [Mise en cache des métadonnées de l’instruction préparée pour le pilote JDBC](prepared-statement-metadata-caching-for-the-jdbc-driver.md).

### <a name="added-support-for-azure-ad-integrated-authentication-on-linuxmacos"></a>Ajout de la prise en charge de l’authentification intégrée Azure AD sur Linux/macOS

Le pilote JDBC prend maintenant en charge l’authentification intégrée Azure Active Directory (Azure AD) sur tous les systèmes d’exploitation pris en charge (Windows, Linux et macOS) avec Kerberos. Sur les systèmes d’exploitation Windows, les utilisateurs peuvent également s’authentifier avec mssql-jdbc_auth-\<version>-\<arch>.dll.

### <a name="updated-microsoft-azure-active-directory-authentication-library-adal4j-for-java-version-140"></a>Version mise à jour de la « bibliothèque d’authentification Microsoft Azure Active Directory (ADAL4J) pour Java » : 1.4.0

Le pilote JDBC a mis à jour sa dépendance Maven sur la « bibliothèque d'authentification Active Directory Microsoft Azure (ADAL4J) pour Java » vers la version 1.4.0. Pour plus d’informations sur les dépendances, consultez [Dépendances de fonctionnalité de Microsoft JDBC Driver pour SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md).

## <a name="62"></a>6.2

**[![Télécharger](../../ssms/media/download-icon.png) Télécharger le pilote Microsoft JDBC 6.2 pour SQL Server (fichier exécutable à extraction automatique)](https://go.microsoft.com/fwlink/?linkid=2122616)**  
**[![Télécharger](../../ssms/media/download-icon.png) Télécharger le pilote Microsoft JDBC 6.2 pour SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122615)**  

Numéro de version : 6.2.2  
Publication : 29 septembre 2017

Si vous avez besoin de télécharger le pilote dans une langue autre que celle détectée, vous pouvez utiliser ces liens directs.  
Pour télécharger le pilote sous forme de fichier exécutable à extraction automatique : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x40a)  
Pour télécharger le pilote sous forme de fichier tar.gz : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x40a)  

Le pilote JDBC 6.2 Microsoft pour SQL Server est parfaitement conforme aux spécifications JDBC 4.1. et 4.2. Les fichiers JAR dans le package 6.2 sont nommés en fonction de la compatibilité avec les versions de Java. Par exemple, l’utilisation du fichier mssql-jdbc-6.2.2.jre8.jar du package 6.2 est recommandée avec Java 8.

### <a name="62-releases"></a>Versions release 6.2

Numéro de version : 6.2.2  
Publication : 3 octobre 2017  
Problèmes résolus :  

- Mise à jour de la dépendance ADAL4J avec la version 1.2.0 et la dépendance d’Azure Key Vault à la version 1.0.0

Numéro de version : 6.2.1  
Publication : 14 juillet 2017  
Problèmes résolus :  

- Correction d’un problème lors de l’exécution de requêtes sans paramètres à l’aide de `preparedStatement`

Numéro de version : 6.2.0  
Publication : 30 juin 2017  

> [!NOTE]  
> Un problème lié à l’amélioration de la mise en cache de métadonnées a été trouvé dans JDBC 6.2 RTW, publié le 29 juin 2017. L’amélioration a été annulée et de nouveaux fichiers JAR (version 6.2.1) ont été publiés le 17 juillet 2017.
>
> Une autre amélioration mis à niveau la version de bibliothèque dépendante d’Azure Key Vault à 1.0.0 et de nouveaux fichiers JAR (version 6.2.2) ont été publiés sur le 19 octobre 2017.
>
> Téléchargez les dernières mises à jour pour le pilote JDBC 6.2 via les liens ci-dessus ([GitHub](https://github.com/Microsoft/mssql-jdbc/releases/tag/v6.2.2)ou [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver)). Mettez à jour vos projets pour utiliser la version 6.2.2 des fichiers JAR. Pour plus d’informations, consultez les notes de publication pour [6.2.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v6.2.1) et [6.2.2](https://github.com/Microsoft/mssql-jdbc/releases/tag/v6.2.2).

### <a name="azure-ad-support-for-linux"></a>Prise en charge d’Azure AD pour Linux

Connectez vos applications Linux à Azure SQL Database à l’aide de l’authentification Azure AD via les méthodes avec nom d’utilisateur/mot de passe et jeton d’accès.

### <a name="fips-enabled-jvms"></a>JVM compatible FIPS

Le pilote JDBC peut désormais être utilisé sur des machines virtuelles Java exécutées en mode de conformité avec les normes FIPS (Federal Information Processing Standard) 140 afin de respecter les normes fédérales sur la conformité.

### <a name="kerberos-authentication-improvements"></a>Améliorations apportées à l’authentification Kerberos

Le pilote JDBC prend désormais en charge ce qui suit :

- Méthode par principal/mot de passe pour les applications où la configuration de Kerberos ne peut pas être modifiée ou ne peut pas récupérer un jeton ou un fichier Keytab nouveau. Cette méthode peut être utilisée pour l’authentification auprès d’une instance SQL Server autorisant uniquement l’authentification Kerberos.
- Authentification interdomaine qui utilise l’authentification intégrée Kerberos sans définir explicitement le SPN du serveur. Le pilote calcule maintenant automatiquement le domaine, même s’il n’y en a pas.
- Délégation contrainte Kerberos par acceptation d’informations d'identification d’utilisateurs impersonnés en tant qu’objet d’informations d’identification GSS via la source de données. Ces informations d’identification avec emprunt d’identité sont ensuite utilisées pour établir une connexion Kerberos.

### <a name="added-timeouts"></a>Ajout de délais d’expiration

Le pilote JDBC prend désormais en charge les délais d’expiration configurables suivants. Vous pouvez les modifier selon les besoins de votre application.

- Délai d’expiration de requête pour contrôler le nombre de secondes à attendre avant un délai d’expiration lorsque vous exécutez une requête.
- Délai d’expiration de socket pour spécifier le nombre de millisecondes à attendre avant un délai d’expiration d’un socket lu ou accepté.

## <a name="61"></a>6.1

Numéro de version : 6.1.0  
Publication : 17 novembre 2016  

Le pilote JDBC 6.1 Microsoft pour SQL Server est parfaitement conforme aux spécifications JDBC 4.1. et 4.2. Il s’agit de la version open source initiale du pilote JDBC. Le code source se trouve dans l’[étiquette GitHub v6.1.0](https://github.com/microsoft/mssql-jdbc/releases/tag/v6.1.0). Il génère les fichiers mssql-jdbc-6.1.0.jre8.jar et mssql-jdbc-6.1.0.jre7.jar, qui indiquent la compatibilité avec une version de Java.

## <a name="60"></a>6.0

**[![Télécharger](../../ssms/media/download-icon.png) Télécharger le pilote Microsoft JDBC 6.0 pour SQL Server (fichier exécutable à extraction automatique)](https://go.microsoft.com/fwlink/?linkid=2122617)**  
**[![Télécharger](../../ssms/media/download-icon.png) Télécharger le pilote Microsoft JDBC 6.0 pour SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122714)**  

Numéro de version : 6.0.8112  
Publication : 17 janvier 2017

Si vous avez besoin de télécharger le pilote dans une langue autre que celle détectée, vous pouvez utiliser ces liens directs.  
Pour télécharger le pilote sous forme de fichier exécutable à extraction automatique : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x40a)  
Pour télécharger le pilote sous forme de fichier tar.gz : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x40a)  

Le pilote JDBC 6.0 Microsoft pour SQL Server est parfaitement conforme aux spécifications JDBC 4.1. et 4.2. Les fichiers JAR dans le package 6.0 sont nommés en fonction de leur conformité avec la version de l’API JDBC. Par exemple, le fichier sqljdbc42.jar du package 6.0 est compatible avec l’API JDBC 4.2. De même, le fichier sqljdbc41.jar est conforme à l’API JDBC 4.1.

Pour vous assurer d’avoir le bon fichier sqljdbc42.jar ou sqljdbc41.jar, exécutez les lignes de code suivantes. Si la sortie est « version du pilote : 6.0.7507.100 », vous avez le package de pilote JDBC 6.0.

```java
Connection conn = DriverManager.getConnection("jdbc:sqlserver://<server>;user=<user>;password=<password>;");
System.out.println("Driver version: " + conn.getMetaData().getDriverVersion());
```

### <a name="always-encrypted"></a>Always Encrypted

Le pilote prend en charge la fonctionnalité Always Encrypted dans SQL Server 2016. Cette fonctionnalité garantit que les données sensibles ne soient jamais affichées comme texte en clair dans une instance de SQL Server. Always Encrypted chiffre les données de l’application de manière transparente, de sorte que SQL Server manipule uniquement les données chiffrées, et non des valeurs en texte clair. Même si l’instance SQL Server ou l’ordinateur hôte est compromis, la personne malveillante ne pourra récupérer qu’un texte chiffré des données sensibles. Pour plus d’informations, voir [Utiliser Always Encrypted avec le pilote JDBC](using-always-encrypted-with-the-jdbc-driver.md).

### <a name="internationalized-domain-names"></a>Noms de domaine internationaux

Le pilote prend en charge les noms de domaine internationaux (IDN) pour les noms des serveurs. Pour plus d’informations, consultez « Utilisation des noms de domaines internationaux » dans l’article [Fonctionnalités internationales du pilote JDBC](international-features-of-the-jdbc-driver.md).

### <a name="parameterized-queries"></a>Requêtes paramétrables

Le pilote prend maintenant en charge la récupération des métadonnées de paramètres avec des instructions préparées pour les requêtes complexes, comme les sous-requêtes et/ou les jointures. Notez que cette amélioration est disponible uniquement quand vous utilisez SQL Server 2012 et des versions plus récentes.

### <a name="azure-active-directory"></a>Azure Active Directory

L’authentification Azure AD est un mécanisme de connexion à Azure SQL Database v12 à l’aide d’identités dans Azure AD. Utilisez l’authentification Azure AD pour gérer de manière centralisée les identités des utilisateurs de base de données et comme alternative à l’authentification SQL Server.

Vous pouvez utiliser le pilote JDBC 6.0 pour spécifier vos informations d’identification Azure AD dans la chaîne de connexion JDBC afin de vous connecter à Azure SQL Database. Pour plus d’informations, consultez la propriété d’authentification dans l’article [Définition des propriétés de connexion](setting-the-connection-properties.md).

### <a name="table-valued-parameters"></a>Paramètres table

Les paramètres table (TVP) fournissent un moyen simple de marshaler plusieurs lignes de données d’une application cliente vers SQL Server sans avoir recours à plusieurs allers-retours ou à une logique spéciale côté serveur pour traiter les données. Vous pouvez utiliser des TVP pour encapsuler des lignes de données dans une application cliente et envoyer les données au serveur dans une commande paramétrable unique. Les lignes de données entrantes sont stockées dans une variable de table que vous pouvez ensuite utiliser à l’aide de Transact-SQL. Pour plus d’informations, consultez [Utilisation de paramètres table](using-table-valued-parameters.md).

### <a name="always-on-availability-groups"></a>Groupes de disponibilité Always On

Le pilote prend désormais en charge les connexions transparentes aux groupes de disponibilité Always On. Le pilote détecte rapidement la topologie Always On actuelle de votre infrastructure serveur et se connecte au serveur actif de manière transparente.

## <a name="42"></a>4,2

**[![Télécharger](../../ssms/media/download-icon.png) Télécharger le pilote Microsoft JDBC 4.2 pour SQL Server (fichier exécutable à extraction automatique)](https://go.microsoft.com/fwlink/?linkid=2122538)**  
**[![Télécharger](../../ssms/media/download-icon.png) Télécharger le pilote Microsoft JDBC 4.2 pour SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122437)**  

Numéro de version : 4.2.8112  
Publication : 24 août 2015

Si vous avez besoin de télécharger le pilote dans une langue autre que celle détectée, vous pouvez utiliser ces liens directs.  
Pour télécharger le pilote sous forme de fichier exécutable à extraction automatique : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x40a)  
Pour télécharger le pilote sous forme de fichier tar.gz : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x40a)  

Le pilote JDBC 4.2 Microsoft pour SQL Server est parfaitement conforme aux spécifications JDBC 4.1. et 4.2. Les fichiers JAR dans le package 4.2 sont nommés en fonction de leur conformité avec la version de l’API JDBC. Par exemple, le fichier sqljdbc42.jar du package 4.2 est compatible avec l’API JDBC 4.2. De même, le fichier sqljdbc41.jar est conforme à l’API JDBC 4.1.

Pour être certain d’avoir le bon fichier sqljdbc42.jar ou sqljdbc41.jar, exécutez les lignes de code suivantes. Si la sortie est « version du pilote : 4.2.6420.100 », vous avez le package de pilote JDBC 4.2.

```java
Connection conn = DriverManager.getConnection("jdbc:sqlserver://<server>;user=<user>;password=<password>;");
System.out.println("Driver version: " + conn.getMetaData().getDriverVersion());
```

### <a name="support-for-jdk-8"></a>Prise en charge de JDK 8

Le pilote prend en charge JDK 8.0 en plus de JDK 7.0, 6.0 et 5.0.

### <a name="jdbc-41-and-42-compliance"></a>Compatible avec JDBC 4.1 et 4.2

Le pilote prend en charge les spécifications de l’API Java Database Connectivity 4.1 et 4.2, en plus de la version 4.0. Pour plus d’informations, consultez [compatibilité avec JDBC 4.1 pour le pilote JDBC](jdbc-4-1-compliance-for-the-jdbc-driver.md) et [Compatibilité avec JDBC 4.2 pour le pilote JDBC](jdbc-4-2-compliance-for-the-jdbc-driver.md).

### <a name="bulk-copy"></a>Copie en bloc

Vous utilisez la fonctionnalité de copie en bloc pour copier rapidement de grandes quantités de données dans des tables ou des vues de bases de données SQL Server. Pour plus d’informations, consultez [Utilisation de la copie en bloc avec le pilote JDBC](using-bulk-copy-with-the-jdbc-driver.md).

### <a name="xa-transaction-rollback-option"></a>Option de restauration des transactions XA

Le pilote dispose de nouvelles options de délai d’attente pour la restauration automatique existante de transactions non préparées. Pour plus d’informations, consultez [Présentation des transactions XA](understanding-xa-transactions.md).

### <a name="new-kerberos-principal-connection-property"></a>Nouvelle propriété de connexion principale Kerberos

Le pilote utilise une nouvelle propriété de connexion pour faciliter la flexibilité avec les connexions Kerberos. Pour plus d’informations, consultez [Utilisation de l’authentification intégrée Kerberos pour se connecter à SQL Server](using-kerberos-integrated-authentication-to-connect-to-sql-server.md).

## <a name="41"></a>4,1

Numéro de version : 4.1.8112  
Publication : 12 décembre 2014

### <a name="support-for-jdk-7"></a>Prise en charge de JDK 7

Le pilote prend en charge JDK 7.0 en plus de JDK 6.0 et 5.0.

## <a name="itanium-not-supported-for-jdbc-driver-applications"></a>Itanium non pris en charge par les applications JDBC Driver

L’exécution d’applications Microsoft JDBC Driver pour SQL Server n’est pas prise en charge sur les ordinateurs Itanium.

## <a name="see-also"></a>Voir aussi

[Vue d’ensemble du pilote JDBC](overview-of-the-jdbc-driver.md)
