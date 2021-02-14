---
title: Vue d’ensemble de l’intégration du CLR | Microsoft Docs
description: Microsoft SQL Server hébergeant le CLR est appelé intégration du CLR. La création dans du code managé peut améliorer les performances. SQL Server utilise les autorités de certification pour aider à sécuriser le code managé.
ms.custom: ''
ms.date: 04/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- common language runtime [SQL Server], about CLR integration
- extended stored procedures [SQL Server], vs. managed code
- objects [CLR integration]
- Transact-SQL vs. managed code
- managed code [SQL Server], vs. Transact-SQL
- managed code [SQL Server], vs. extended stored procedures
- execution at client vs. execution at server [CLR integration]
ms.assetid: 5aa176da-3652-4afa-a742-4c40c77ce5c3
author: rothja
ms.author: jroth
ms.openlocfilehash: 667ded44c653f71902893f160b6231ad8b751789
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100341452"
---
# <a name="clr-integration---overview"></a>Intégration du CLR - Vue d’ensemble
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Le CLR (Common Language Runtime) est le cœur de Microsoft .NET Framework et fournit l'environnement d'exécution de la totalité du code .NET Framework. Le code qui s'exécute au sein du CLR est désigné sous le nom de code managé. Le CLR fournit divers fonctions et services requis pour l'exécution du programme, y compris la compilation juste-à-temps (JIT), l'allocation et la gestion de la mémoire, la mise en application de la cohérence des types, la gestion des exceptions, la gestion des threads et la sécurité.  Pour plus d'informations, consultez la documentation du Kit de développement (SDK) .NET Framework.  
  
 Avec le CLR hébergé dans Microsoft SQL Server (intégration du CLR), vous pouvez créer des procédures stockées, des déclencheurs, des fonctions définies par l'utilisateur, des types définis par l'utilisateur et des agrégats définis par l'utilisateur en code managé. Comme le code managé est compilé en code natif avant l'exécution, vous pouvez obtenir une amélioration significative des performances dans certains scénarios.  
  
 Le code managé utilise la sécurité d'accès du code pour empêcher les assemblys d'effectuer certaines opérations. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise la sécurité d'accès du code pour aider à sécuriser le code managé et empêcher que le système d'exploitation ou le serveur de base de données ne soit menacé.  
  
## <a name="advantages-of-clr-integration"></a>Avantages de l'intégration du CLR  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] est conçu spécifiquement pour l'accès direct aux données et les manipulations de la base de données. Même si [!INCLUDE[tsql](../../includes/tsql-md.md)] convient parfaitement pour accéder aux données et les gérer, ce n'est pas un langage de programmation à part entière. Par exemple, [!INCLUDE[tsql](../../includes/tsql-md.md)] ne prend pas en charge les tableaux, les collections, les boucles for-each, le décalage de bits ou les classes. Bien que certaines de ces constructions puissent être simulées dans [!INCLUDE[tsql](../../includes/tsql-md.md)], le code managé a intégré leur prise en charge. Selon le scénario, ces fonctionnalités peuvent fournir une raison attrayante d'implémenter certaines fonctionnalités de base de données en code managé.  
  
 Microsoft Visual Basic .NET et Microsoft Visual C# offrent des fonctions orientées objet telles que l'encapsulation, l'héritage et le polymorphisme. Le code connexe peut désormais être aisément organisé en classes et en espaces de noms. Lorsque vous travaillez avec un code serveur volumineux, cela vous permet de l'organiser et de le gérer plus facilement.  
  
 Le code managé est mieux adapté que [!INCLUDE[tsql](../../includes/tsql-md.md)] pour les calculs et les logiques d'exécution élaborées, et des fonctionnalités étendues prennent en charge nombre de tâches complexes, comme la manipulation des chaînes et des expressions régulières. Avec les fonctionnalités proposées dans la bibliothèque .NET Framework, vous avez accès aux milliers de classes et routines prégénérées. Il est possible d'accéder simplement à celles-ci à partir d'une procédure stockée, d'un déclencheur ou d'une fonction définie par l'utilisateur. La bibliothèque des classes de base (BCL, Base Class Library) inclut les classes qui fournissent les fonctionnalités de manipulation de chaînes, d'opérations mathématiques avancées, d'accès aux fichiers, de chiffrement, etc.  
  
> [!NOTE]  
>  Alors que la plupart de ces classes sont disponibles pour être utilisées au sein du code CLR de SQL Server, celles qui ne sont pas adaptées à une utilisation côté serveur (par exemple, les classes de fenêtrage) ne sont pas disponibles. Pour plus d’informations, consultez [.NET Framework les bibliothèques prises en charge](../../relational-databases/clr-integration/database-objects/supported-net-framework-libraries.md).  
  
 L'un des avantages du code managé est la cohérence des types, ou l'assurance que le code n'accède aux types que de façon bien définie et autorisée. Avant que le code managé ne soit exécuté, le CLR vérifie que le code ne présente pas de risque. Par exemple, il est procédé à un contrôle du code pour s'assurer qu'aucune mémoire ne soit lue qui n'ait été préalablement écrite. Le CLR peut également aider à vérifier que le code ne manipule pas une mémoire non managée.  
  
 L'intégration du CLR offre la possibilité de meilleures performances. Pour plus d’informations, consultez [performances de l’intégration du CLR](../../relational-databases/clr-integration/clr-integration-architecture-performance.md).  
 
> [!WARNING]
>  CLR utilise la sécurité d’accès du code (CAS) dans le .NET Framework, qui n’est plus pris en charge comme limite de sécurité. Un assembly CLR créé avec `PERMISSION_SET = SAFE` peut être en mesure d’accéder à des ressources système externes, d’appeler du code non managé et d’acquérir des privilèges sysadmin. À compter de [!INCLUDE[sssql17](../../includes/sssql17-md.md)], une option de `sp_configure` appelée `clr strict security` est introduite pour renforcer la sécurité des assemblys CLR. `clr strict security` est activée par défaut et traite les assemblys `SAFE` et `EXTERNAL_ACCESS` comme s’ils étaient marqués `UNSAFE`. L’option `clr strict security` peut être désactivée pour assurer une compatibilité descendante, mais ceci n’est pas recommandé. Microsoft recommande que tous les assemblys soient signés par un certificat ou une clé asymétrique avec une connexion correspondante à laquelle a été accordée l’autorisation `UNSAFE ASSEMBLY` dans la base de données master. Pour plus d’informations, consultez [CLR strict security](../../database-engine/configure-windows/clr-strict-security.md). 
  
## <a name="choosing-between-transact-sql-and-managed-code"></a>Choix entre Transact-SQL et le code managé  
 Lors de l'écriture des procédures stockées, des déclencheurs et des fonctions définies par l'utilisateur, vous devez choisir entre l'utilisation du langage [!INCLUDE[tsql](../../includes/tsql-md.md)] traditionnel et celle d'un langage .NET Framework, tel que Visual Basic .NET ou Visual C#. Utilisez [!INCLUDE[tsql](../../includes/tsql-md.md)] lorsque le code effectue principalement un accès aux données avec une logique procédurale nulle ou minime. Utilisez le code managé pour les fonctions gourmandes en ressources processeur et les procédures qui affichent une logique complexe, ou lorsque vous souhaitez utiliser la bibliothèque de classes de base du .NET Framework.  
  
### <a name="choosing-between-execution-in-the-server-and-execution-in-the-client"></a>Choix entre exécution dans le serveur et exécution dans le client  
 Autre facteur de votre décision relative au choix de [!INCLUDE[tsql](../../includes/tsql-md.md)] ou du code managé, l'emplacement où vous souhaitez que votre code réside, à savoir sur le serveur ou sur l'ordinateur client. [!INCLUDE[tsql](../../includes/tsql-md.md)] et le code managé peuvent tous deux être exécutés sur le serveur. Le code et les données se retrouvent ensemble, et vous permettent de tirer parti de la puissance de traitement du serveur. En revanche, vous pouvez souhaiter éviter de placer les tâches intensives du processeur sur votre serveur de base de données. Aujourd'hui, la plupart des ordinateurs clients sont très puissants et il se peut que vous souhaitiez tirer parti de cette puissance de traitement en plaçant autant de code que possible sur le client. Le code managé peut s'exécuter sur un ordinateur client, contrairement à [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="choosing-between-extended-stored-procedures-and-managed-code"></a>Choix entre procédures stockées étendues et code managé  
 Les procédures stockées étendues peuvent être créées pour remplir une fonctionnalité inaccessible via les procédures stockées [!INCLUDE[tsql](../../includes/tsql-md.md)]. Toutefois, les procédures stockées étendues peuvent compromettre l'intégrité du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], contrairement au code managé qui est soumis à une vérification pour être certain qu'il ne présente aucun risque. En outre, la gestion de la mémoire, la planification des threads et des fibres, et les services de synchronisation sont plus profondément intégrés entre le code managé du CLR et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Avec l'intégration du CLR, vous avez une solution plus sécurisée que les procédures stockées étendues pour écrire les procédures stockées qui vous sont nécessaires pour exécuter des tâches qui ne peuvent l'être dans [!INCLUDE[tsql](../../includes/tsql-md.md)]. Pour plus d’informations sur l’intégration du CLR et les procédures stockées étendues, consultez [performances de l’intégration du CLR](../../relational-databases/clr-integration/clr-integration-architecture-performance.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Installation du .NET Framework](https://technet.microsoft.com/library/ms166014\(v=SQL.105\).aspx)   
 [Architecture de l’intégration du CLR](./clr-integration-architecture-clr-hosted-environment.md)   
 [Accès aux données à partir des objets de base de données CLR](../../relational-databases/clr-integration/data-access/data-access-from-clr-database-objects.md)   
 [Mise en route avec l'intégration du CLR](../../relational-databases/clr-integration/database-objects/getting-started-with-clr-integration.md)  
  
