---
title: Page d’accueil pour la programmation client SQL | Microsoft Docs
description: Page contenant des liens annotés vers des téléchargements et une documentation pour différents langages et de systèmes d’exploitation, pour la connexion à SQL Server ou à Azure SQL Database.
author: David-Engel
ms.date: 11/07/2018
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: v-daveng
ms.author: v-daenge
ms.openlocfilehash: 10d04306621722abd9c9a170028ba632a8d95c9e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045499"
---
# <a name="homepage-for-client-programming-to-microsoft-sql-server"></a>Page d’accueil pour la programmation client sur Microsoft SQL Server


Bienvenue dans notre page d’accueil sur la programmation client pour interagir avec Microsoft SQL Server et avec Azure SQL Database dans le cloud. Cet article fournit les informations suivantes :

- Répertorie et décrit les combinaisons de langages et de pilotes disponibles.
  - Des informations sont fournies pour les systèmes d’exploitation Linux (Ubuntu et autres), macOS et Windows.
- Fournit des liens vers la documentation détaillée de chaque combinaison.
- Affiche les zones et sous-zones de la documentation hiérarchique pour certains langages, le cas échéant.


#### <a name="azure-sql-database"></a>Azure SQL Database

Dans tous les langages, le code qui se connecte à SQL Server est presque identique au code de connexion à Azure SQL Database.

Pour plus d’informations sur les chaînes de connexion à Azure SQL Database, consultez :

- [Utiliser .NET Core (C#) pour interroger une base de données Azure SQL](/azure/sql-database/sql-database-connect-query-dotnet-core).
- Autres articles sur les bases de données Azure SQL Database proches de l’article précédent dans la table des matières, sous à propos des autres langages. Par exemple, consultez [Utiliser PHP pour interroger une base de données Azure SQL](/azure/sql-database/sql-database-connect-query-php).


#### <a name="build-an-app-webpages"></a>Pages web Créer une application

Nos pages web *Créer une application* présentent des exemples de code ainsi que des informations de configuration dans un autre format. Pour plus d’informations, consultez plus loin dans cet article la [section intitulée *site web Créer une application*](#an-204-aka-ms-sqldev).



<a name="an-050-languages-clients" />

## <a name="languages-and-drivers-for-client-programs"></a>Langages et pilotes pour les programmes clients


Dans le tableau suivant, chaque image de langage est un lien vers des détails sur l’utilisation du langage avec SQL Server. Chaque lien permet d’accéder à une section ultérieure de cet article.

:::row:::
    :::column:::
        [![Logo C#][image-ref-320-csharp]](#an-110-ado-net-docu)  

        [![Loge Node.js][image-ref-340-node]](#an-140-node-js-docu)  

        [![Logo Python][image-ref-370-python]](#an-180-python-docu)  
    :::column-end:::
    :::column:::
        [![ORM Entity Framework, de .NET Framework][image-ref-333-ef]](#an-116-csharp-ef-orm)  

        [**`ODBC for C++`**](#an-160-odbc-cpp-docu)<br/>[![cpp-big-plus][image-ref-322-cpp]](#an-160-odbc-cpp-docu)  

        [![Logo Ruby][image-ref-380-ruby]](#an-190-ruby-docu)  
    :::column-end:::
    :::column:::
        [![Logo Java][image-ref-330-java]](#an-130-jdbc-docu)  

        [![Logo PHP][image-ref-360-php]](#an-170-php-docu)  
    :::column-end:::
:::row-end:::

#### <a name="downloads-and-installs"></a>Téléchargements et installations

L’article suivant est consacré au téléchargement et à l’installation de divers pilotes de connexion SQL pour une utilisation par les langages de programmation :

- [Pilotes SQL Server](./sql-connection-libraries.md)



<a name="an-110-ado-net-docu" />

## <a name="c-logoimage-ref-320-csharp-c-using-adonet"></a>![Logo C#][image-ref-320-csharp] C# utilisant ADO.NET

Les langages managés .NET, tels que C# et Visual Basic, sont les utilisateurs les plus courants d’ADO.net. *ADO.NET* est le nom informel d’un sous-ensemble de classes .NET Framework.

#### <a name="code-examples"></a>Exemples de code

| Exemple | Description |
| :-- | :-- |
| [Preuve de concept pour se connecter à SQL à l’aide d’ADO.NET](./ado-net/step-3-connect-sql-ado-net.md) | Un petit exemple de code axé sur la connexion et l’interrogation de SQL Server. |
| [Connexion résiliente à SQL avec ADO.NET](./ado-net/step-4-connect-resiliently-sql-ado-net.md) | Logique de nouvelle tentative dans un exemple de code, car les connexions peuvent parfois rencontrer des pertes de connectivité.<br /><br />La logique de nouvelle tentative s’applique bien aux connexions maintenues avec n’importe quelle base de données, telle qu’Azure SQL Database, via Internet. |
| [Azure SQL Database : Démonstration de l’utilisation de .NET Core sur Windows/Linux/macOS pour créer un programme C#, se connecter et interroger](/azure/sql-database/sql-database-connect-query-dotnet-core) | Exemple de base de données Azure SQL. |
| [Créer une application : C#, ADO.NET, Windows](https://www.microsoft.com/sql-server/developer-get-started/csharp/win/) | Informations de configuration ainsi qu’exemples de code. |
| &nbsp; | <br /> |

#### <a name="documentation"></a>Documentation

| Domaine | Description |
| :-- | :-- |
| [C# utilisant ADO.NET](./ado-net/microsoft-ado-net-sql-server.md)| Racine de notre documentation. |
| [Espace de noms : System.Data](/dotnet/api/system.data) | Ensemble de classes utilisé pour ADO.NET. |
| [Espace de noms : Microsoft.Data.SqlClient](/dotnet/api/microsoft.data.SqlClient) | Ensemble de classes utilisé pour le fournisseur de données Microsoft .NET pour SQL Server |
| &nbsp; | <br /> |



<a name="an-116-csharp-ef-orm" />

## <a name="entity-framework-logoimage-ref-333-ef-entity-framework-ef-with-cx23"></a>![Logo Entity Framework][image-ref-333-ef] Entity Framework (EF) avec C&#x23;

Entity Framework (EF) fournit un mappage objet-relationnel (ORM). Avec ORM, votre code source de programmation orientée objet (OOP) peut manipuler plus facilement les données récupérées à partir d’une base de données SQL relationnelle.

EF a des relations directes ou indirectes avec les technologies suivantes :

- .NET Framework
- [LINQ to SQL](/dotnet/framework/data/adonet/sql/linq/) ou [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities)
- Améliorations de la syntaxe du langage, telles que l’opérateur **=>** dans C#.
- Programmes pratiques générant du code source pour les classes qui mappent aux tables de votre base de données SQL. Par exemple, [EdmGen.exe](/dotnet/framework/data/adonet/ef/edm-generator-edmgen-exe).


#### <a name="original-ef-and-new-ef"></a>EF d’origine et nouvel EF

La [page de démarrage pour Entity Framework](/ef/) présente EF avec une description similaire à ce qui suit :

- Entity Framework est un mappeur objet-relationnel qui permet aux développeurs .NET de travailler avec une base de données à l’aide d’objets .NET. Il élimine le recours à la plupart du code source d’accès aux données que les développeurs doivent généralement écrire.

*Entity Framework* est un nom partagé par deux branches de code source distinctes. Une branche EF est plus ancienne, et son code source peut désormais être géré par le public. L’autre EF est nouveau. Les deux EF sont décrits ci-après :

| Version | Description |
| :-- | :-- |
| [EF 6.x](/ef/ef6/) | Microsoft a lancé EF en août 2008. En mars 2015, Microsoft a annoncé qu’EF 6.x était la version finale que Microsoft développerait. Microsoft a publié le code source pour le domaine public.<br /><br />Initialement, EF faisait partie de .NET Framework. Mais EF 6.x a été supprimé de .NET Framework.<br /><br />[Code source EF 6.x sur GitHub, dans le référentiel *aspnet/EntityFramework6*](https://github.com/aspnet/EntityFramework6) |
| [EF Core](/ef/core/) | Microsoft a publié le nouveau développement d’EF Core en juin 2016. EF Core est conçu pour améliorer la flexibilité et la portabilité. EF Core peut s’exécuter sur des systèmes d’exploitation autres que Microsoft Windows. Et EF Core peut interagir avec les bases de données autres que Microsoft SQL Server ainsi qu’avec d’autres bases de données relationnelles.<br /><br />**Exemples de code C&#x23;**<br />[Prise en main d’Entity Framework Core](/ef/core/get-started/index)<br />[Prise en main d’EF Core sur .NET Framework avec une base de données existante](/ef/core/get-started/full-dotnet/existing-db) |
| &nbsp; | <br /> |

EF et les technologies associées sont puissantes, et un développeur qui souhaite maîtriser l’ensemble du domaine a beaucoup à apprendre.

&nbsp;



<a name="an-130-jdbc-docu" />

## <a name="java-logoimage-ref-330-java-java-and-jdbc"></a>![Logo Java][image-ref-330-java] Java et JDBC

Microsoft fournit un pilote JDBC (Java Database Connectivity) pouvant être utilisé avec SQL Server (ou avec Azure SQL Database). Il s’agit d’un pilote JDBC de type 4 offrant une connectivité de base de données par le biais des interfaces de programmation d’applications (API) JDBC standard.

#### <a name="code-examples"></a>Exemples de code

| Exemple | Description |
| :-- | :-- |
| [Exemples de code](./jdbc/sample-jdbc-driver-applications.md) | Exemples de code qui renseignent sur les types de données, les jeux de résultats et les données volumineuses. |
| [Exemple d’URL de connexion](./jdbc/connection-url-sample.md) | Décrit comment utiliser une URL de connexion pour se connecter à SQL Server. Utilisez-la ensuite pour récupérer des données avec une instruction SQL. |
| [Exemple de source de données](./jdbc/data-source-sample.md) | Décrit comment utiliser une source de données pour se connecter à SQL Server. Utilisez ensuite une procédure stockée pour récupérer des données. |
| [Utiliser Java pour interroger une base de données Azure SQL Database](/azure/sql-database/sql-database-connect-query-java) | Exemple de base de données Azure SQL. |
| [Créer des applications Java avec SQL Server sur Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/) | Informations de configuration ainsi qu’exemples de code. |
| &nbsp; | <br /> |

#### <a name="documentation"></a>Documentation

La documentation JDBC inclut les principaux domaines suivants :

| Domaine | Description |
| :-- | :-- |
| [Java Database Connectivity (JDBC)](./jdbc/microsoft-jdbc-driver-for-sql-server.md) | Racine de notre documentation JDBC. |
| [Référence](./jdbc/reference/jdbc-driver-api-reference.md) | Interfaces, classes et membres. |
| [Guide de programmation pour le pilote JDBC SQL](./jdbc/programming-guide-for-jdbc-sql-driver.md) | Informations de configuration ainsi qu’exemples de code. |
| &nbsp; | <br /> |



<a name="an-140-node-js-docu" />

## <a name="nodejs-logoimage-ref-340-node-nodejs"></a>![Logo Node.js][image-ref-340-node] Node.js

Avec Node.js, vous pouvez vous connecter à SQL Server à partir de Windows, Linux ou macOS. La racine de notre documentation Node.js est [ici](./node-js/node-js-driver-for-sql-server.md).

Le pilote de connexion Node.js pour SQL Server est implémenté en JavaScript. Le pilote utilise le protocole TDS, qui est pris en charge par toutes les versions récentes de SQL Server. Le pilote est un projet open source, [disponible sur GitHub](https://tediousjs.github.io/tedious/).

#### <a name="code-examples"></a>Exemples de code

| Exemple | Description |
| :-- | :-- |
| [Preuve de concept pour se connecter à SQL à l’aide de Node.js](./node-js/step-3-proof-of-concept-connecting-to-sql-using-node-js.md) | Code source simple pour se connecter à SQL Server et exécuter une requête. |
| [Base de données Azure SQL : utiliser Node.js pour interroger](/azure/sql-database/sql-database-connect-query-nodejs) | Exemple pour Azure SQL Database dans le cloud. |
| [Créer des applications Node.js pour utiliser SQL Server sur macOS](https://www.microsoft.com/sql-server/developer-get-started/node/mac/) | Informations de configuration ainsi qu’exemples de code. |
| &nbsp; | <br /> |



<a name="an-160-odbc-cpp-docu" />

## <a name="odbc-for-c"></a>ODBC pour C++

![Logo ODBC][image-ref-350-odbc] ![cpp-big-plus][image-ref-322-cpp]

Open Database Connectivity (ODBC) a été développé au cours des années 90 et est antérieur à .NET Framework. ODBC est conçu pour être indépendant de tout système de base de données particulier, quel que soit le système d’exploitation.

Au fil des années, de nombreux pilotes ODBC ont été créés et publiés par des groupes au sein et en dehors de Microsoft. La gamme de pilotes implique plusieurs langages de programmation client. La liste des cibles de données va bien au-delà de SQL Server.

D’autres pilotes de connectivité utilisent ODBC en interne.

#### <a name="code-example"></a>Exemple de code

- [Exemple de code C++, utilisant ODBC](../odbc/reference/sample-odbc-program.md)

#### <a name="documentation-outline"></a>Structure de la documentation

Le contenu ODBC de cette section se concentre sur l’accès à SQL Server ou à Azure SQL Database à partir de C++. Le tableau suivant répertorie une structure approximative de la documentation principale pour ODBC.


| Domaine | Sous-domaine | Description |
| :--- | :------ | :---------- |
| [ODBC pour C++](./odbc/microsoft-odbc-driver-for-sql-server.md) | Racine de notre documentation. |
| [Linux-macOS](./odbc/linux-mac/system-requirements.md) | &nbsp; | Informations sur l’utilisation d’ODBC sur les systèmes d’exploitation Linux ou macOS. |
| [Windows](./odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows.md)     | &nbsp; | Informations sur l’utilisation d’ODBC sur le système d’exploitation Windows. |
| [Administration](../odbc/admin/odbc-data-source-administrator.md) | &nbsp; | Outil d’administration pour la gestion des sources de données ODBC. |
| [Microsoft](../odbc/microsoft/microsoft-supplied-odbc-drivers.md)  | &nbsp; | Différents pilotes ODBC qui sont créés et fournis par Microsoft. |
| [Conceptuel et référence](../odbc/reference/introduction-to-odbc.md) | &nbsp; | Informations conceptuelles sur l’interface ODBC en plus de la référence traditionnelle. |
| &nbsp; " | [Annexes](../odbc/reference/appendixes/odbc-appendixes.md)    | Tables de transition d’état, bibliothèque de curseurs ODBC, etc. |
| &nbsp; " | [Développer une application](../odbc/reference/develop-app/checking-feature-support-and-variability.md)  | Fonctions, descripteurs, etc. |
| &nbsp; " | [Développer des pilotes](../odbc/reference/develop-driver/developing-an-odbc-driver.md) | Comment développer votre propre pilote ODBC si vous avez une source de données spécialisée. |
| &nbsp; " | [Installer](../odbc/reference/install/odbc-subkey.md) | Installation d’ODBC, sous-clés, etc. |
| &nbsp; " | [Syntaxe](../odbc/reference/syntax/odbc-reference.md)   | API pour la configuration, programme d'installation, traduction et accès aux données. |
| &nbsp; | &nbsp; | <br /> |



<a name="an-170-php-docu" />

## <a name="php-logoimage-ref-360-php-php"></a>![Logo PHP][image-ref-360-php] PHP

Vous pouvez utiliser PHP pour interagir avec SQL Server. La racine de notre documentation PHP est [ici](./php/microsoft-php-driver-for-sql-server.md).

#### <a name="code-examples"></a>Exemples de code

| Exemple | Description |
| :-- | :-- |
| [Preuve de concept pour se connecter à SQL à l’aide de PHP](./php/step-3-proof-of-concept-connecting-to-sql-using-php.md) | Un petit exemple de code axé sur la connexion et l’interrogation de SQL Server. |
| [Connexion résiliente à SQL avec PHP](./php/step-4-connect-resiliently-to-sql-with-php.md) | Logique de nouvelle tentative dans un exemple de code, car les connexions via Internet et le cloud peuvent parfois rencontrer des pertes de connectivité. |
| [Base de données Azure SQL : utiliser PHP pour interroger](/azure/sql-database/sql-database-connect-query-php) | Exemple de base de données Azure SQL. |
| [Créer des applications PHP pour utiliser SQL Server sur RHEL](https://www.microsoft.com/sql-server/developer-get-started/php/rhel/) | Informations de configuration ainsi qu’exemples de code. |
| &nbsp; | <br /> |



<a name="an-180-python-docu" />

## <a name="python-logoimage-ref-370-python-python"></a>![Logo Python][image-ref-370-python] Python


Vous pouvez utiliser Python pour interagir avec SQL Server.

#### <a name="code-examples"></a>Exemples de code

| Exemple | Description |
| :-- | :-- |
| [Preuve de concept pour se connecter à SQL avec Python à l’aide de pyodbc](./python/pyodbc/step-3-proof-of-concept-connecting-to-sql-using-pyodbc.md) | Un petit exemple de code axé sur la connexion et l’interrogation de SQL Server. |
| [Base de données Azure SQL : utiliser Python pour interroger](/azure/sql-database/sql-database-connect-query-python) | Exemple de base de données Azure SQL. |
| [Créer des applications PHP pour utiliser SQL Server sur SLES](https://www.microsoft.com/sql-server/developer-get-started/python/sles/) | Informations de configuration ainsi qu’exemples de code. |
| &nbsp; | <br /> |

#### <a name="documentation"></a>Documentation

| Domaine | Description |
| :--- | :---------- |
| [Python vers SQL Server](./python/python-driver-for-sql-server.md) | Racine de notre documentation. |
| [pymssql driver](./python/pymssql/python-sql-driver-pymssql.md) | Microsoft ne gère pas et ne teste pas le pilote pymssql.<br /><br />Le pilote de connexion pymssql est une interface simple avec les bases de données SQL à utiliser dans les programmes Python. Les builds pymssql basés sur FreeTDS pour fournir une interface Python DB-API (PEP-249) à Microsoft SQL Server. |
| [pilote pyodbc](./python/pyodbc/python-sql-driver-pyodbc.md)   | Le pilote de connexion pyodbc est un module Python open source qui facilite l’accès aux bases de données ODBC. Il implémente la spécification de base de données API 2.0, mais est compressé avec encore plus de commodité Python. |
| &nbsp; | <br /> |


<a name="an-190-ruby-docu" />

## <a name="ruby-logoimage-ref-380-ruby-ruby"></a>![Logo Ruby][image-ref-380-ruby] Ruby

Vous pouvez utiliser Ruby pour interagir avec SQL Server. La racine de notre documentation Ruby est [ici](./ruby/ruby-driver-for-sql-server.md).

#### <a name="code-examples"></a>Exemples de code

| Exemple | Description |
| :-- | :-- |
| [Preuve de concept pour la connexion à SQL avec Ruby](./ruby/step-3-proof-of-concept-connecting-to-sql-using-ruby.md) | Un petit exemple de code axé sur la connexion et l’interrogation de SQL Server. |
| [Base de données Azure SQL : utiliser Ruby pour interroger](/azure/sql-database/sql-database-connect-query-ruby) | Exemple de base de données Azure SQL. |
| [Créer des applications Ruby pour utiliser SQL Server sur macOS](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/) | Informations de configuration ainsi qu’exemples de code. |
| &nbsp; | <br /> |



<a name="an-204-aka-ms-sqldev" />

## <a name="build-an-app-website-for-sql-client-development"></a>[Créer un site web d’application pour développer des clients SQL](https://www.microsoft.com/sql-server/developer-get-started/)


Sur nos sites web [*Créer une application*](https://www.microsoft.com/sql-server/developer-get-started/), vous pouvez choisir dans une longue liste de langages de programmation pour la connexion à SQL Server. Et votre programme client peut exécuter une variété de systèmes d'exploitation.

*Créer une application* met en avant la simplicité et l’exhaustivité du développeur qui est en cours de prise en main. Les étapes mentionnées expliquent les tâches suivantes :

1. Guide pratique pour installer Microsoft SQL Server
2. Comment télécharger et installer des outils et des pilotes.
3. Comment effectuer les configurations nécessaires en fonction du système d’exploitation choisi.
4. Comment compiler le code source fourni.
5. Comment exécuter le programme.

Voici quelques contours approximatifs des détails fournis sur le site web :

#### <a name="java-on-ubuntu"></a>Java sur Ubuntu

1. Configurer votre environnement
    - Étape 1.1 : installer SQL Server
    - Étape 1.2 : installer Java
    - Étape 1.3 : installer le kit de développement Java (JDK)
    - Étape 1.4 : installer Maven
2. Créez une application Java avec SQL Server
    - Étape 2.1 : créer une application Java qui se connecte à SQL Server et exécute des requêtes
    - Étape 2.2 : créer une application Java qui se connecte à SQL Server à l’aide du framework populaire Hibernate
3. Rendez votre application Java jusqu’à 100 x plus rapide
    - Étape 3.1 : créer une application Java pour faire la démonstration d’index ColumnStore

#### <a name="python-on-windows"></a>Python sur Windows

1. Configurer votre environnement
    - Étape 1.1 : installer SQL Server
    - Étape 1.2 : installer Python
    - Étape 1.3 : installer le pilote ODBC et l’utilitaire de ligne de commande SQL pour SQL Server
2. Créez une application Python avec SQL Server
    - Étape 2.1 : installer le pilote Python pour SQL Server
    - Étape 2.2 : créer une base de données pour votre application
    - Étape 2.3 : créer une application Python qui se connecte à SQL Server et exécute des requêtes
3. Rendez votre application Python jusqu’à 100 x plus rapide
    - Étape 3.1 : créer une nouvelle table avec 5 millions à l’aide de sqlcmd
    - Étape 3.2 : créer une application Python qui interroge cette table et mesure le temps nécessaire
    - Étape 3.3 : mesurer le temps nécessaire à l’exécution de la requête
    - Étape 3.4 : ajouter un index ColumnStore à votre table
    - Étape 3.5 : mesurer le temps nécessaire à l’exécution de la requête avec un index columnStore

Les captures d’écran suivantes vous donnent une idée de ce à quoi ressemble notre site web de documentation de développement SQL.

#### <a name="choose-a-language"></a>Choisir une langue

![Site web de développement SQL, prise en main][image-ref-390-aka-ms-sqldev-choose-language]

&nbsp;

#### <a name="choose-an-operating-system"></a>Choisir un système d'exploitation

![Site web SQL Dev, Java Ubuntu][image-ref-400-aka-ms-sqldev-java-ubuntu]

&nbsp;



## <a name="other-development"></a>Autre développement


Cette section fournit des liens vers d’autres options de développement. Il s’agit notamment de l’utilisation de ces mêmes langages pour le développement Azure en général. Les informations vont au-delà du simple ciblage d’Azure SQL Database et de Microsoft SQL Server.

#### <a name="developer-hub-for-azure"></a>Hub de développeur pour Azure

- [Hub de développeur pour Azure](/azure/)
- [Azure for .NET developers](/dotnet/azure/) (Azure pour les développeurs .NET)
- [Azure for Java developers](/java/azure/) (Azure pour les développeurs Java)
- [Azure for Node.js developers](/nodejs/azure/) (Azure pour les développeurs Node.js)
- [Azure for Python developers](/python/azure/) (Azure pour les développeurs Python)
- [Créer une application web PHP dans Azure](/azure/app-service-web/app-service-web-get-started-php)

#### <a name="other-languages"></a>Autres langages

- [Créer des applications Go avec SQL Server sur Windows](https://www.microsoft.com/sql-server/developer-get-started/go/windows/)



<!-- Image references. -->

[image-ref-322-cpp]: ./media/homepage-sql-connection-drivers/gm-cpp-4point-p61f.png
[image-ref-320-csharp]: ./media/homepage-sql-connection-drivers/gm-csharp-c10c.png
[image-ref-333-ef]: ./media/homepage-sql-connection-drivers/gm-entity-framework-ef20d.png
[image-ref-330-java]: ./media/homepage-sql-connection-drivers/gm-java-j18c.png
[image-ref-340-node]: ./media/homepage-sql-connection-drivers/gm-node-n30.png
[image-ref-350-odbc]: ./media/homepage-sql-connection-drivers/gm-odbc-ic55826-o35.png
[image-ref-360-php]: ./media/homepage-sql-connection-drivers/gm-php-php60.png
[image-ref-370-python]: ./media/homepage-sql-connection-drivers/gm-python-py72.png
[image-ref-380-ruby]: ./media/homepage-sql-connection-drivers/gm-ruby-un-r82.png
[image-ref-390-aka-ms-sqldev-choose-language]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-choose-language-g21.png
[image-ref-400-aka-ms-sqldev-java-ubuntu]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-java-ubuntu-c31.png