---
description: Vue d'ensemble (objets SMO)
title: Vue d’ensemble (SMO) | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
ms.assetid: e988f9e8-6801-41d1-8069-726f487244d5
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8b3a9c15979d162ca345a0d440f7093c4bd15ad9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439833"
---
# <a name="overview-smo"></a>Vue d'ensemble (objets SMO)
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Les objets SMO (Management Objects) sont des objets conçus pour la gestion par programme de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez les utiliser pour créer des applications de gestion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] personnalisées. Même si [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est une application puissante et étendue de gestion de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], une application SMO donnera dans certains cas de meilleurs résultats.  
  
 Par exemple, il est possible que les applications utilisateur qui contrôlent les tâches de gestion de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent être simplifiées pour répondre aux besoins de nouveaux utilisateurs et pour réduire les coûts de formation. Vous devez peut-être créer des bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] personnalisées ou créer une application de création et de contrôle du rendement des index. Une application SMO peut également être utilisée pour inclure de façon transparente le matériel ou les logiciels tiers dans l'application de gestion de base de données.  
  
 Dans la mesure où SMO est compatible avec [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures, il vous est facile de gérer un environnement multiversion.  
  
 Les fonctionnalités de SMO sont les suivantes :  
  
-   Modèle objet mis en cache et création d'instance d'objet optimisée. Les objets sont chargés uniquement lorsqu'ils sont spécifiquement référencés. Les propriétés de l'objet ne sont que partiellement chargées à la création de l'objet. Les objets et propriétés restants sont chargées lorsqu'ils sont directement référencés.  
  
-   Exécution groupée des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] . Les instructions sont groupées pour améliorer les performances réseau.  
  
-   Capture des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] . Autorise la capture de toute opération dans un script. [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] utilise cette fonctionnalité pour écrire une opération au lieu de l'exécuter immédiatement.  
  
-   Gestion des services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec le fournisseur WMI. Les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent être démarrés, arrêtés et suspendus par programme.  
  
-   Écriture de scripts avancés. Des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] peuvent être générés pour recréer des objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui décrivent des relations à d'autres objets sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Utilisation de noms de ressource uniques (URN). Un URN vous permet de créer des instances d'objets SMO et de les référencer.  
  
 SMO représente également de nombreux composants et fonctionnalités introduits dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]sous la forme de nouveaux objets ou propriétés. Ces nouveaux composants et fonctionnalités sont les suivants :  
  
-   Partitionnement d'index et de table pour le stockage des données sur un schéma de partition. Pour plus d’informations, consultez [Tables et index partitionnés](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
-   Points de terminaison HTTP pour la gestions des requêtes SOAP. Pour plus d'informations, consultez [Implementing Endpoints](../../relational-databases/server-management-objects-smo/tasks/implementing-endpoints.md).  
  
-   Isolement d'instantané et contrôle de version de ligne pour plus de concurrence. Pour plus d’informations, consultez [Utilisation du niveau d’isolement d’instantané](../../relational-databases/native-client/features/working-with-snapshot-isolation.md).  
  
-   La collection de schémas XML, les index XML et le type de données XML permettent la validation et le stockage des données XML. Pour plus d’informations, consultez [collections de schémas xml &#40;SQL Server&#41;](../../relational-databases/xml/xml-schema-collections-sql-server.md) et [utilisation de schémas XML](../../relational-databases/server-management-objects-smo/tasks/using-xml-schemas.md).  
  
-   Bases de données d'instantanés pour la création de copies en lecture seule de bases de données.  
  
-   Prise en charge de la communication basée sur des messages par [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Pour plus d'informations, consultez [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md).  
  
-   Prise en charge des synonymes pour les noms d'objets multiples de la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [synonymes &#40;Moteur de base de données&#41;](../../relational-databases/synonyms/synonyms-database-engine.md).  
  
-   La gestion de la messagerie de base de données vous permet de créer des serveurs, des profils et des comptes de messagerie électronique dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md).  
  
-   Prise en charge des serveurs inscrits pour l'inscription des informations de connexion. Pour plus d'informations, consultez [Register Servers](../../ssms/register-servers/register-servers.md).  
  
-   Traçage et relecture des événements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d'informations, consultez [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md), [SQL Trace](../../relational-databases/sql-trace/sql-trace.md), [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)et [Extended Events](../../relational-databases/extended-events/extended-events.md).  
  
-   Prise en charge des certificats et des clés pour le contrôle de sécurité. Pour plus d'informations, consultez [Encryption Hierarchy](../../relational-databases/security/encryption/encryption-hierarchy.md).  
  
-   Déclencheurs DDL pour l'ajout de fonctionnalités lorsque des événements DDL se produisent. Pour plus d'informations, consultez [DDL Triggers](../../relational-databases/triggers/ddl-triggers.md).  
  
 L'espace de noms SMO est <xref:Microsoft.SqlServer.Management.Smo>. SMO est implémenté en tant qu'assembly [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Cela signifie que le CLR (Common Language Runtime) de la version 2.0 de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] doit être installée avant d'utiliser les objets SMO. Les assembly SMO sont installés par défaut dans le Global Assembly Cache (GAC) avec l'option de Kit de développement logiciel (SDK) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les assemblys se trouvent dans C:\Program Files\Microsoft SQL Server\130\SDK\Assemblies\. Pour plus d’informations, consultez la documentation de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
## <a name="smo-classes"></a>Classes SMO  
 Les classes SMO incluent deux catégories : les classes d'instance et les classes utilitaires.  
  
 **Classes d'instance**  
  
 Les classes d'instance représentent des objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tels que des serveurs, des bases de données, des tables, des déclencheurs et des procédures stockées. La classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> est utilisée pour établir une connexion à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et contrôler le mode de capture des commandes qui lui sont envoyées.  
  
 Les objets d'instance SMO forment une hiérarchie qui représente la hiérarchie d'un serveur de base de données. Au sommet de cette hiérarchie se situent les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], suivies des bases de données, puis des tables, des colonnes, des déclencheurs et ainsi de suite. S'il est logique qu'il existe une relation un-à-plusieurs entre un parent et ses enfants, comme une table constituée d'une ou plusieurs colonnes, il est alors logique que l'enfant soit représenté par une collection d'objets. Sinon, l'enfant est simplement représenté par un objet.  
  
 **Classes utilitaires**  
  
 Les classes utilitaires représentent un groupe d'objets créés explicitement pour effectuer des tâches spécifiques. Elles ont été divisées en différentes hiérarchies d'objets selon leur fonction :  
  
-   Classe de transfert. Utilisée pour transférer un schéma et des données vers une autre base de données.  
  
-   Classes de sauvegarde et de restauration. Utilisées pour sauvegarder et restaurer des bases de données.  
  
-   Classe de générateur de script. Utilisée pour créer des fichiers de script pour la régénération des objets et de leurs dépendances.  
  
## <a name="smo-features"></a>Fonctionnalités SMO  
 **Optimisation des performances**  
  
 L’architecture SMO est efficace en termes de mémoire, car les objets ne sont instanciés que partiellement dans un premier temps, et les informations de propriété minimales sont demandées à partir du serveur. L'instanciation complète des objets est différée jusqu'à ce que l'objet soit explicitement référencé. Un objet est entièrement instancié lorsqu'une propriété demandée ne figure pas dans le jeu de propriétés extrait en premier ou lorsqu'une méthode nécessitant cette propriété est appelée. La transition entre les objets partiellement instanciés et les objets entièrement instanciés est transparente pour l'utilisateur. En outre, certaines propriétés qui utilisent beaucoup de mémoire ne sont jamais récupérées, à moins que la propriété soit explicitement référencée. Ceci est illustré par la propriété <xref:Microsoft.SqlServer.Management.Smo.Database.Size%2A> de la propriété d'objet <xref:Microsoft.SqlServer.Management.Smo.Database>. Toutefois, l'instanciation partielle requiert davantage d'allers-retours sur le réseau et ne constitue peut-être pas la meilleure option pour votre application.  
  
 Vous pouvez contrôler l'instanciation en fonction de l'environnement système. L'instanciation différée réduit la quantité de mémoire requise par l'application, bien que cela puisse déclencher de nombreuses demandes serveur lorsque les propriétés sont référencées.  
  
 Des classes d'instance, des objets qui représentent des objets de base de données réels, peuvent exister dans trois niveaux d'instanciation. instanciation minimale (seules les propriétés requises minimales sont lues dans un bloc), instanciation partielle (toutes les propriétés qui utilisent une quantité de mémoire relativement grande sont lues dans un bloc) et instanciation complète. Les états non instancié et instanciation complète sont les plus courants. L'état d'instanciation partielle accroît l'efficacité car un objet partiellement instancé ne contient pas de valeurs pour le jeu complet de propriétés de l'objet. L'instanciation partielle est l'état par défaut d'un objet qui n'est pas directement référencé. Lorsque l'une de ces propriétés est référencée, une erreur est générée qui invite à l'instanciation complète de l'objet.  
  
 **Exécution par capture**  
  
 L'exécution directe est la méthode habituelle d'exécution. Les instructions sont envoyées à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dès qu'elles sont soumises. L'exécution par capture est une autre possibilité d'exécution.  
  
 L'exécution par capture vous permet de capturer des lots [!INCLUDE[tsql](../../includes/tsql-md.md)] qui devraient être exécutés. Le programmeur SMO peut ainsi différer le script, le stocker en vue de l'exécuter ultérieurement ou fournir un aperçu à l'utilisateur final. Par exemple, les instructions **create database**, **create table** et **create index** peuvent être envoyées dans un lot, puis être exécutées en trois étapes séquentielles. Cette fonctionnalité est contrôlée par l'utilisateur à l'aide de l'objet <xref:Microsoft.SqlServer.Management.Smo.Server.%23ctor%2A>.  
  
 **Fournisseur WMI**  
  
 Les objets du fournisseur WMI sont encapsulés par SMO. Le programmeur SMO dispose ainsi d'un modèle objet simple fort similaire aux classes SMO, sans qu'il ait besoin de comprendre le modèle de programmation représenté par l'espace de noms et les détails du fournisseur WMI de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le fournisseur WMI vous permet de configurer les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , les alias, de même que les bibliothèques réseau client et serveur.  
  
 **Création de scripts**  
  
 Dans SMO, la création de scripts a été améliorée et déplacée dans la classe **Scripter** . La classe **Scripter** peut découvrir les dépendances, comprendre les relations entre des objets et permettre la manipulation de la hiérarchie de dépendances. Le principal objet de script est l'objet **Scripter** . Il existe également plusieurs objets de support qui gèrent les dépendances et répondent aux événements de progression et d'erreur.  
  
 L'objet **Scripter** prend en charge les options de script avancées suivantes :  
  
-   Script simple en 1 phase (crée le script en une étape)  
  
-   Script avancé en 3 phases (crée le script en trois étapes : découverte des dépendances, génération de listes, génération de scripts)  
  
-   Découverte de dépendances bidirectionnelle (permet la découverte des dépendances ou des dépendants)  
  
-   Réponse aux événements de progression  
  
-   Réponse aux événements d'erreur  
  
 **Noms de ressource uniques**  
  
 Un concept clé dans l'utilisation de la bibliothèque d'objets SMO est le nom de ressource unique (URN). L'URN utilise une syntaxe semblable à XPath. XPath est un chemin d'accès hiérarchique utilisé pour spécifier un objet dans lequel chaque niveau a des qualificateurs et des fonctions. Dans SMO, l'URN a deux éléments, le chemin d'accès et le nom d'attribut qui a des fonctionnalités limitées. Le chemin d'accès est utilisé pour spécifier l'emplacement de l'objet, tandis que le nom d'attribut permet un certain degré de filtrage.  
  
 Exemple d'URN pour une base de données :  
  
```  
/Server/Database[@Name='Adventureworks2012']  
```  
  
 L'URN d'un objet peut être récupéré en référençant sa propriété URN. L'objet Scripter utilise également des URN comme paramètres qui passent des références d'objet à la méthode de l'objet **Scripter** . En outre, un URN peut être spécifié pour la méthode **GetSmoObject** de l'objet **Server** . Cela permet de créer une instance de l'objet SMO.  
  
## <a name="sql-server-features-represented-in-smo"></a>Fonctionnalités SQL Serveres représentées dans SMO  
 **Partitionnement des tables et des index**  
  
 Le partitionnement des tables et des index vous permet de gérer les données dispersées dans des tables et des index de groupes de fichiers. Cette nouvelle fonctionnalité est représentée par les objets SMO.  
  
 **Terminaison**  
  
 Les demandes SOAP et les demandes de mise en miroir de base de données sont gérées par des points de terminaison à l'aide de l'objet <xref:Microsoft.SqlServer.Management.Smo.Endpoint>.  
  
 **Isolement d'instantané/Contrôle de version de ligne**  
  
 L'isolement d'instantané (contrôle de version de ligne) est représenté par les nouvelles propriétés de l'objet <xref:Microsoft.SqlServer.Management.Smo.Database>.  
  
 **Espace de noms de schéma XML, index XML et type de données XML**  
  
 Les expaces de noms de schéma XML sont représentés dans SMO par une collection d'objets. Les index XML sont représentés dans SMO par une propriété d'objet **Index** .  
  
 **Améliorations apportées à la recherche en texte intégral**  
  
 De nouveaux objets sont fournis dans SMO pour représenter les améliorations apportées à la recherche en texte intégral.  
  
 **Vérification de page**  
  
 L'objet <xref:Microsoft.SqlServer.Management.Smo.DatabaseOptions.PageVerify%2A> représente les options de vérification de page de bases de données.  
  
 **Bases de données d'instantanés**  
  
 Une base de données d'instantanés est une copie en lecture seule d'une base de données spécifiée à un point spécifique dans le temps. Une base de données d'instantanés peut être spécifiée à l'aide de la propriété <xref:Microsoft.SqlServer.Management.Smo.Database.IsDatabaseSnapshot%2A> de l'objet <xref:Microsoft.SqlServer.Management.Smo.Database>.  
  
 **Service Broker**  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] et ses fonctionnalités sont représentés par un groupe d'objets  
  
 **Améliorations des index**  
  
 Les améliorations des index [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont représentées par de nouvelles propriétés dans l'objet <xref:Microsoft.SqlServer.Management.Smo.Index>.  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts liés à Replication Management Objects](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)  
  
