---
title: Questions fréquentes (FAQ) sur le pilote JDBC
description: Cette page fournit des réponses aux questions fréquemment posées sur le pilote Microsoft JDBC Driver pour SQL Server.
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: cbc0e397-ecf2-4494-87b2-a492609bceae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 71c5e88db1bbc0c1b8d6698930c503a1134bac7d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837099"
---
# <a name="frequently-asked-questions-faq-for-jdbc-driver"></a>Questions fréquentes (FAQ) sur le pilote JDBC

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Cette page fournit des réponses aux questions fréquemment posées sur le pilote Microsoft JDBC Driver pour SQL Server.

## <a name="frequently-asked-questions"></a>Forum aux questions

**Comment puis-je améliorer le pilote JDBC ?**  
Le pilote JDBC est open source. Vous trouverez le code source sur [GitHub](https://github.com/microsoft/mssql-jdbc). Vous pouvez aider à l’améliorer en soumettant des problèmes et en contribuant au codebase.

**Quelles sont les versions de SQL Server et de Java prises en charge par le pilote ?**  
Pour obtenir des détails, consultez la page [Matrice de prise en charge de Microsoft JDBC Driver pour SQL Server](../../connect/jdbc/microsoft-jdbc-driver-for-sql-server-support-matrix.md).

**Quelle est la différence entre les packages de pilotes JDBC disponibles dans la [page des téléchargements Microsoft](download-microsoft-jdbc-driver-for-sql-server.md) et le pilote JDBC disponible sur GitHub ?**  
Les fichiers de pilotes JDBC disponibles sur le référentiel GitHub pour Microsoft JDBC Driver sont au cœur du pilote JDBC et sous la licence open source indiquée dans le référentiel. Les packages de pilotes de la [page des téléchargements Microsoft](download-microsoft-jdbc-driver-for-sql-server.md) incluent des bibliothèques supplémentaires pour l’authentification Windows intégrée et les transactions XA avec le pilote JDBC, sous la licence incluse avec le package téléchargeable.

**Que faut-il savoir avant la mise à niveau du pilote ?**  
Microsoft JDBC Driver 9.2 prend en charge les spécifications JDBC 4.2 et 4.3 (partiellement) et inclut trois bibliothèques de classes JAR dans le package d’installation, comme suit :

| JAR                        | Spécification JDBC            | Version JDK |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-9.2.1.jre15.jar | JDBC 4.3 (partiellement) et 4.2 | JDK 15.0    |
| mssql-jdbc-9.2.1.jre11.jar | JDBC 4.3 (partiellement) et 4.2 | JDK 11.0    |
| mssql-jdbc-9.2.1.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 8.4 prend en charge les spécifications JDBC 4.2 et 4.3 (partiellement) et inclut trois bibliothèques de classes JAR dans le package d’installation comme suit :

| JAR                        | Spécification JDBC            | Version JDK |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-8.4.1.jre14.jar | JDBC 4.3 (partiellement) et 4.2 | JDK 14.0    |
| mssql-jdbc-8.4.1.jre11.jar | JDBC 4.3 (partiellement) et 4.2 | JDK 11.0    |
| mssql-jdbc-8.4.1.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 8.2 prend en charge les spécifications JDBC 4.2 et 4.3 (partiellement). Le package d’installation comprend les trois bibliothèques de classes JAR suivantes :

| JAR                        | Spécification JDBC            | Version JDK |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-8.2.2.jre13.jar | JDBC 4.3 (partiellement) et 4.2 | JDK 13.0    |
| mssql-jdbc-8.2.2.jre11.jar | JDBC 4.3 (partiellement) et 4.2 | JDK 11.0    |
| mssql-jdbc-8.2.2.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 7.4 prend en charge les spécifications JDBC 4.2 et 4.3 (partiellement). Le package d’installation comprend les trois bibliothèques de classes JAR suivantes :

| JAR                        | Spécification JDBC            | Version JDK |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-7.4.1.jre12.jar | JDBC 4.3 (partiellement) et 4.2 | JDK 12.0    |
| mssql-jdbc-7.4.1.jre11.jar | JDBC 4.3 (partiellement) et 4.2 | JDK 11.0    |
| mssql-jdbc-7.4.1.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 7.2 prend en charge les spécifications JDBC 4.2 et 4.3 (partiellement). Le package d’installation comprend les deux bibliothèques de classes JAR suivantes :

| JAR                        | Spécification JDBC            | Version JDK |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-7.2.2.jre11.jar | JDBC 4.3 (partiellement) et 4.2 | JDK 11.0    |
| mssql-jdbc-7.2.2.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 7.0 prend en charge les spécifications JDBC 4.2 et 4.3 (partiellement). Le package d’installation comprend les deux bibliothèques de classes JAR suivantes :

| JAR                        | Spécification JDBC            | Version JDK |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-7.0.0.jre10.jar | JDBC 4.3 (partiellement) et 4.2 | JDK 10.0    |
| mssql-jdbc-7.0.0.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 6.4 prend en charge les spécifications JDBC 4.1, 4.2 et 4.3 (partiellement). Le package d’installation comprend les trois bibliothèques de classes JAR suivantes :

| JAR                       | Spécification JDBC                 | Version JDK |
| ------------------------- | ---------------------------------- | ----------- |
| mssql-jdbc-6.4.0.jre9.jar | JDBC 4.3 (partiellement), 4.2 et 4.1 | JDK 9.0     |
| mssql-jdbc-6.4.0.jre8.jar | JDBC 4.2 et 4.1                  | JDK 8.0     |
| mssql-jdbc-6.4.0.jre7.jar | JDBC 4.1                           | JDK 7.0     |
| &nbsp;                    | &nbsp;                             | &nbsp;      |

Microsoft JDBC Driver 6.2 prend en charge les spécifications JDBC 4.0, 4.1 et 4.2. Le package d’installation comprend les deux bibliothèques de classes JAR suivantes :

| JAR                       | Spécification JDBC     | Version JDK |
| ------------------------- | ---------------------- | ----------- |
| mssql-jdbc-6.2.2.jre8.jar | JDBC 4.2, 4.1 et 4.0 | JDK 8.0     |
| mssql-jdbc-6.2.2.jre7.jar | JDBC 4.1 et 4.0       | JDK 7.0     |
| &nbsp;                    | &nbsp;                 | &nbsp;      |

Microsoft JDBC Driver 6.0 et 4.2 pour SQL Server prennent en charge les spécifications JDBC 4.0, 4.1 et 4.2. Le package d’installation comprend les deux bibliothèques de classes JAR suivantes :

| JAR           | Spécification JDBC     | Version JDK |
| ------------- | ---------------------- | ----------- |
| sqljdbc42.jar | JDBC 4.2, 4.1 et 4.0 | JDK 8.0     |
| sqljdbc41.jar | JDBC 4.1 et 4.0       | JDK 7.0     |
| &nbsp;        | &nbsp;                 | &nbsp;      |

Microsoft JDBC Driver 4.1 pour SQL Server prend en charge la spécification JDBC 4.0. Le package d’installation comprend la bibliothèque de classes JAR suivante :

| JAR           | Spécification JDBC | Version JDK     |
| ------------- | ------------------ | --------------- |
| sqljdbc41.jar | JDBC 4.0           | JDK 7.0 et 6.0 |
| &nbsp;        | &nbsp;             | &nbsp;      |

**Dois-je modifier le code de mon application pour pouvoir utiliser le pilote le plus récent avec ma version existante de SQL Server ?**  
En règle générale, le pilote est conçu pour assurer une compatibilité descendante. Il est donc inutile de modifier les applications existantes quand vous mettez à niveau le pilote. Lorsqu’une nouvelle version du pilote introduit un changement cassant, la section [Notes de publication du pilote JDBC](../../connect/jdbc/release-notes-for-the-jdbc-driver.md) donne des informations claires sur cette modification et son impact sur les applications existantes. Vous pouvez également consulter les notes de publication fournies avec le pilote pour obtenir la liste des bogues corrigés dans cette version et les problèmes connus.

**Combien coûte le pilote ?**  
Le pilote Microsoft JDBC Driver pour SQL Server est disponible gratuitement.

**Puis-je redistribuer le pilote ?**  
Les pilotes JDBC 6.0, 6.2, 6.4 et 7.0 sont redistribuables. Lisez la clause « Code distribuable » des contrats de licence.

**Puis-je utiliser le pilote pour accéder à Microsoft SQL Server à partir d’un ordinateur Linux ?**  
Oui. Le pilote vous permet d’accéder à SQL Server à partir de Linux, d’Unix et d’autres plateformes non-Windows. Pour plus d’informations, voir [Matrice de prise en charge de Microsoft JDBC Driver pour SQL Server](../../connect/jdbc/microsoft-jdbc-driver-for-sql-server-support-matrix.md) .

**Le pilote prend-il en charge le chiffrement TLS ?**  
À partir de la version 1.2, le pilote prend en charge le chiffrement TLS (Transport Layer Security), anciennement SSL (Secure Sockets Layer). Pour plus d’informations, consultez [Utilisation du chiffrement](../../connect/jdbc/using-ssl-encryption.md).

**Quels sont les types d’authentification pris en charge par le pilote Microsoft JDBC Driver pour SQL Server ?**  
Le tableau ci-dessous répertorie les options d’authentification disponibles. Une authentification Kerberos en Java pur est disponible à compter de la version 4.0 du pilote.

| Plateforme    | Authentification                        |
| ----------- | ------------------------------------- |
| Non-Windows | Kerberos en Java pur                    |
| Non-Windows | SQL Server                            |
| Non-Windows | Authentification Azure Active Directory |
| Non-Windows | NTLM                                  |
| Windows     | Kerberos en Java pur                    |
| Windows     | SQL Server                            |
| Windows     | Kerberos avec sauvegarde NTLM             |
| Windows     | NTLM                                  |
| Windows     | Authentification Azure Active Directory |
| &nbsp;      | &nbsp;                                |

**Le pilote prend-il en charge les adresses IPv6 (Internet Protocol version 6) ?**  
Oui. Le pilote prend en charge les adresses IPv6. Utilisez la collection de propriétés de connexion et la propriété de chaîne de connexion serverName. Pour plus d’informations, consultez [Création de l’URL de connexion](../../connect/jdbc/building-the-connection-url.md).

**Qu’est-ce que la mise en mémoire tampon adaptative ?**  
La mise en mémoire tampon adaptative a été introduite dans Microsoft SQL Server 2005 JDBC Driver version 1.2. Elle est conçue pour récupérer tout type de données de valeur élevée sans la charge mémoire associée aux curseurs côté serveur. La fonctionnalité de mise en mémoire tampon adaptative du pilote JDBC Driver pour Microsoft SQL Server fournit une propriété de chaîne de connexion, responseBuffering, qui peut avoir la valeur « adaptive » ou « full ». Dans la version 1.2, le mode de mise en mémoire tampon est « full » par défaut ; par ailleurs, l’application doit définir explicitement le mode de mise en mémoire tampon adaptative. À compter de la version 2.0 du pilote JDBC Driver, le comportement par défaut du pilote est « adaptive ». Par conséquent, votre application n’a pas besoin de requérir explicitement le comportement de la mise en mémoire tampon adaptative pour l’obtenir. Pour plus d’informations, consultez [Utilisation de la mise en mémoire tampon adaptative](../../connect/jdbc/using-adaptive-buffering.md) et le blog qui indique [ce qu’est la mise en mémoire tampon adaptative et pourquoi l’utiliser](/archive/blogs/jdbcteam/).

**Le pilote prend-il en charge le regroupement de connexions ?**  
Le pilote prend en charge le regroupement de connexions Java EE 5 (Java Platform, Enterprise Edition 5). Le pilote implémente les interfaces JDBC 3.0 nécessaires pour qu’il puisse participer aux implémentations de regroupements de connexions fournies par des fournisseurs de serveurs d’applications intergicielles (middleware). Le pilote participe à des connexions regroupées dans ces environnements. Pour plus d’informations, consultez [Utilisation d’un regroupement de connexions](../../connect/jdbc/using-connection-pooling.md). Le pilote ne fournit pas sa propre implémentation de regroupement, mais il s’appuie sur des serveurs d’applications Java tiers.

**Le pilote bénéficie-t-il des services du support technique ?**  
Plusieurs options de support technique sont disponibles. Vous pouvez publier votre question ou votre problème dans notre [référentiel GitHub](https://github.com/microsoft/mssql-jdbc), contrôlé par Microsoft. Les [forums](https://go.microsoft.com/fwlink/?LinkID=246673) sont gérés par Microsoft, les MVP et la communauté. Vous pouvez aussi contacter le support technique Microsoft. L’équipe de développement peut vous demander de reproduire le problème en dehors de tout serveur d’applications tiers. Si le problème ne peut pas être reproduit en dehors de l’environnement conteneur Java hôte, vous devez impliquer le tiers associé pour que l’équipe puisse continuer à vous aider. L’équipe pourra éventuellement vous demander de reproduire votre problème sur un système d’exploitation comme Windows pour mieux vous aider.

**Le pilote est-il certifié dans le cadre d’une utilisation avec des serveurs d’applications tiers ?**  
Le pilote a été testé sur divers serveurs d’applications, notamment IBM WebSphere et SAP Netweaver.

**Comment activer le suivi ?**  
Le pilote JDBC Driver prend en charge l’utilisation du suivi (ou journalisation) pour faciliter la résolution des problèmes liés à son utilisation dans votre application. Pour activer l’utilisation du suivi JAR côté client, le pilote JDBC Driver utilise les API de journalisation dans java.util.logging. Pour plus d’informations, consultez [Suivi du fonctionnement du pilote](../../connect/jdbc/tracing-driver-operation.md). Pour le suivi XA côté serveur, consultez [Suivi de l’accès aux données dans SQL Server](/previous-versions/sql/sql-server-2012/hh880086(v=msdn.10)).

**Où puis-je télécharger les anciennes versions du pilote, notamment le pilote JDBC pour SQL Server 2000, 2005 ou encore les versions 1.0, 1.1 ou 1.2 du pilote ?**  
Ces versions du pilote ne sont pas disponibles en téléchargement, car elles ne sont plus prises en charge. Nous améliorons constamment la prise en charge de la connectivité Java. Nous vous recommandons donc vivement d’utiliser la dernière version du pilote Microsoft JDBC Driver.

**J’utilise JRE 1.4. Quel est le pilote compatible avec JRE 1.4 ?**  
Pour les clients qui utilisent des produits SAP et réclame le support de JRE 1.4, contactez la [Place de marché SAP Service](https://service.sap.com/) pour obtenir la version 1.2 de Microsoft JDBC Driver.

**Le pilote peut-il communiquer à l’aide d’algorithmes validés par FIPS ?**  
Le pilote Microsoft JDBC Driver ne contient aucun algorithme de chiffrement. Si un client exploite des algorithmes du système d’exploitation, d’applications et JVM jugés conformes aux normes FIPS (Federal Information Processing Standards) et qu’il configure le pilote de telle sorte qu’il utilise ces algorithmes, le pilote utilise uniquement les algorithmes désignés pour la communication.

## <a name="see-also"></a>Voir aussi

[Présentation du pilote JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)