---
title: Always Encrypted | Microsoft Docs
description: Vue d’ensemble d’Always Encrypted qui prend en charge le chiffrement transparent côté client, et le calcul confidentiel dans SQL Server et Azure SQL Database
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], Always Encrypted
- Always Encrypted
- TCE Always Encrypted
- Always Encrypted, about
- SQL13.SWB.COLUMNMASTERKEY.CLEANUP.F1
ms.assetid: 54757c91-615b-468f-814b-87e5376a960f
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 534d7238316fe2037ea0ce43e2b4aeeb11e6eea2
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171331"
---
# <a name="always-encrypted"></a>Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

  ![Always Encrypted](../../../relational-databases/security/encryption/media/always-encrypted.png "Always Encrypted")  
  
 Always Encrypted est une fonctionnalité conçue pour protéger les données sensibles, telles que les numéros de carte de crédit ou les numéros nationaux d’identification (par exemple, les numéros de sécurité sociale aux États-Unis), qui sont stockées dans des bases de données [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)] ou [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Always Encrypted permet aux clients de chiffrer des données sensibles dans des applications clientes et de ne jamais révéler les clés de chiffrement au [!INCLUDE[ssDE](../../../includes/ssde-md.md)] ([!INCLUDE[ssSDS](../../../includes/sssds-md.md)] ou [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]). Ainsi, Always Encrypted fournit une séparation entre ceux qui détiennent les données et peuvent les voir, et ceux qui les gèrent mais ne doivent pas y avoir accès. En empêchant les administrateurs de base de données locales, les opérateurs de base de données cloud ou d’autres utilisateurs avec des privilèges élevés, mais non autorisés, d’accéder aux données chiffrées, Always Encrypted permet aux clients de stocker des données sensibles de manière sécurisée en dehors de leur contrôle direct. De cette manière, les organisations peuvent stocker leurs données dans Azure et activer la délégation de l’administration de base de données locale à des tiers, ou réduire les contraintes d’attestation de sécurité pour leur propre personnel d’administration de base de données.

 Always Encrypted fournit des fonctionnalités de calcul confidentiel en permettant au [!INCLUDE[ssDE](../../../includes/ssde-md.md)] de traiter certaines requêtes sur les données chiffrées, tout en préservant la confidentialité des données et en offrant les avantages de sécurité ci-dessus. Dans [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)], [!INCLUDE[sssSQLv14](../../../includes/sssqlv14-md.md)] et dans [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)], Always Encrypted prend en charge la comparaison d’égalité par le biais du chiffrement déterministe. Consultez [Sélection d’un chiffrement déterministe ou aléatoire](#selecting--deterministic-or-randomized-encryption). 

  > [!NOTE] 
  > Dans [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)], les enclaves sécurisées étendent substantiellement les fonctionnalités de calcul confidentiel d’Always Encrypted avec des critères spéciaux, d’autres opérateurs de comparaison et le chiffrement sur place. Consultez [Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md).

 Always Encrypted rend le chiffrement transparent pour les applications. À cette fin, un pilote Always Encrypted installé sur l’ordinateur client chiffre et déchiffre automatiquement les données sensibles dans l’application cliente. Le pilote chiffre les données dans les colonnes sensibles avant de les transmettre au [!INCLUDE[ssDE](../../../includes/ssde-md.md)]et il réécrit automatiquement les requêtes pour que la sémantique de l’application soit conservée. De même, le pilote déchiffre de manière transparente les données stockées dans les colonnes de base de données chiffrées contenues dans les résultats de requête.  
  
 Always Encrypted est disponible dans toutes les éditions de [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)], à partir de [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)] et de tous les niveaux de service de [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]. (Pour les versions avant [!INCLUDE[ssSQL15_md](../../../includes/sssql16-md.md)] SP1, Always Encrypted était limité à l’édition Enterprise.) Pour visualiser une présentation Channel 9 qui aborde Always Encrypted, consultez [Keeping Sensitive Data Secure with Always Encrypted](https://channel9.msdn.com/events/DataDriven/SQLServer2016/AlwaysEncrypted)(Sécurisation des données sensibles avec Always Encrypted).  

  
## <a name="typical-scenarios"></a>Scénarios typiques  
  
### <a name="client-and-data-on-premises"></a>Client et données locaux  
 Un client dispose d’une application cliente et de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui s’exécutent localement, au sein de son entreprise. Le client souhaite embaucher un fournisseur externe pour administrer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Afin de protéger les données sensibles stockées dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], le client utilise toujours Always Encrypted pour garantir la séparation des responsabilités entre les administrateurs de base de données et les administrateurs d’application. Le client stocke les valeurs en texte clair des clés Always Encrypted dans un magasin de clés approuvé accessible à l’application cliente. Les administrateurs[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] n’ont pas accès aux clés et, de ce fait, ne peuvent pas déchiffrer les données sensibles stockées dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
### <a name="client-on-premises-with-data-in-azure"></a>Client local avec des données dans Azure  
 Un client dispose d’une application cliente locale au sein de son entreprise. L’application manipule des données sensibles stockées dans une base de données hébergée dans Azure ([!INCLUDE[ssSDS](../../../includes/sssds-md.md)] ou [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s’exécutant dans une machine virtuelle sur Microsoft Azure). Le client utilise Always Encrypted et stocke les clés Always Encrypted dans un magasin de clés approuvé hébergé localement, pour s’assurer que les administrateurs de cloud [!INCLUDE[msCoName](../../../includes/msconame-md.md)] n’aient pas accès aux données sensibles.  
  
### <a name="client-and-data-in-azure"></a>Client et données dans Azure  
 Un client dispose d’une application cliente, hébergée dans Microsoft Azure (par exemple, dans un rôle de travail ou un rôle web), qui manipule des données sensibles également stockées dans une base de données hébergée dans Azure (SQL Database ou SQL Server s’exécutant sur une machine virtuelle Microsoft Azure). Même si Always Encrypted n’isole pas complètement les données des administrateurs de cloud, puisque les données et les clés sont exposées aux administrateurs de cloud de la plateforme qui héberge la couche cliente, le client tire quand même profit de la réduction de la surface d’attaque (les données sont toujours chiffrées dans la base de données).  
 
## <a name="how-it-works"></a>Fonctionnement

Vous pouvez configurer Always Encrypted pour les colonnes de base de données contenant des données sensibles. Quand vous configurez le chiffrement pour une colonne, vous spécifiez les informations sur l’algorithme de chiffrement et les clés de chiffrement utilisées pour protéger les données de la colonne. Always Encrypted utilise deux types de clés : des clés de chiffrement de colonne et des clés principales de colonne. Une clé de chiffrement de colonne sert à chiffrer les données qui se trouvent dans une colonne chiffrée. Une clé principale de colonne est une clé de protection de clé qui chiffre une ou plusieurs clés de chiffrement de colonne. 

Le moteur de base de données stocke la configuration de chiffrement pour chaque colonne dans des métadonnées de base de données. Notez toutefois que le moteur de base de données ne stocke et n’utilise jamais ces clés en texte clair. Il stocke uniquement des valeurs chiffrées des clés de chiffrement de colonne et les informations sur l’emplacement des clés principales de colonne, qui sont stockées dans des magasins de clés approuvés externes, tel qu’Azure Key Vault, le magasin de certificats Windows sur un ordinateur client ou un module de sécurité matériel.

Pour accéder aux données stockées dans une colonne chiffrée en texte clair, une application doit utiliser un pilote client compatible avec Always Encrypted. Quand une application émet une requête paramétrée, le pilote collabore en toute transparence avec le moteur de base de données pour déterminer quels paramètres ciblent des colonnes chiffrées et, par conséquent, doivent être chiffrés. Pour chaque paramètre qui doit être chiffré, le pilote obtient les informations sur l’algorithme de chiffrement et la valeur chiffrée de la clé de chiffrement de colonne pour la colonne, les cibles de paramètres, ainsi que l’emplacement de la clé principale de colonne correspondante.

Ensuite, le pilote contacte le magasin de clés, qui contient la clé principale de colonne, afin de déchiffrer la valeur de la clé de chiffrement de colonne chiffrée, puis il utilise la clé de chiffrement de colonne en texte clair pour chiffrer le paramètre. La clé de chiffrement de colonne en texte clair résultante est mise en cache pour réduire le nombre d’allers-retours vers le magasin de clés lors des utilisations ultérieures de la même clé de chiffrement de colonne. Le pilote remplace les valeurs de texte en clair des paramètres ciblant des colonnes chiffrées par leurs valeurs chiffrées, et il envoie la requête au serveur pour traitement.

Le serveur calcule le jeu de résultats et, pour toutes les colonnes chiffrées dans le jeu de résultats, le pilote attache les métadonnées de chiffrement pour la colonne, notamment les informations sur l’algorithme de chiffrement et les clés correspondantes. Le pilote tente d’abord de trouver la clé de chiffrement de colonne en texte clair dans le cache local, et il accède à la clé principale de colonne uniquement s’il ne trouve pas la clé dans le cache. Ensuite, le pilote déchiffre les résultats et retourne des valeurs en texte clair à l’application.

 Un pilote client interagit avec un magasin de clés, qui contient une clé principale de colonne, à l’aide d’un fournisseur de magasins de clé principale de colonne, qui est un composant logiciel côté client qui encapsule un magasin de clé contenant la clé principale de colonne. Les fournisseurs des types courants de magasins de clés sont disponibles dans des bibliothèques de pilotes côté client chez Microsoft ou sous forme de téléchargements autonomes. Vous pouvez également implémenter votre propre fournisseur. Les fonctionnalités d’Always Encrypted, notamment les fournisseurs de magasins de clés principales de colonne intégrés, varient en fonction de la bibliothèque de pilotes et de sa version. 

Pour plus d’informations sur le développement des applications à l’aide d’Always Encrypted avec des pilotes clients particuliers, consultez [Développer des applications à l’aide d’Always Encrypted](always-encrypted-client-development.md).

## <a name="remarks"></a>Notes 

Le chiffrement et le déchiffrement sont effectués par le biais du pilote client. Cela signifie que certaines actions qui se produisent uniquement côté serveur ne fonctionneront pas en utilisant Always Encrypted. Citons par exemple la copie de données d’une colonne vers une autre à l’aide d’une instruction UPDATE, BULK INSERT(T-SQL), SELECT INTO, INSERT..SELECT. 

Voici un exemple d’une instruction UPDATE qui tente de déplacer des données d’une colonne chiffrée vers une colonne non chiffrée sans retourner de jeu de résultats au client : 

```sql
update dbo.Patients set testssn = SSN
```

Si SSN est une colonne chiffrée à l’aide d’Always Encrypted, l’instruction de mise à jour ci-dessus échoue avec une erreur similaire à :

```
Msg 206, Level 16, State 2, Line 89
Operand type clash: char(11) encrypted with (encryption_type = 'DETERMINISTIC', encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_1', column_encryption_key_database_name = 'ssn') collation_name = 'Latin1_General_BIN2' is incompatible with char
```

Pour mettre à jour correctement la colonne, effectuez les étapes suivantes :

1. Sélectionnez les données de la colonne SSN et stockez-les comme jeu de résultats dans l’application. Cela permettra à l’application (*pilote* client) de déchiffrer la colonne.
2. Insérez les données du jeu de résultats dans SQL Server. 

 >[!IMPORTANT]
 > Dans ce scénario, les données sont non chiffrées quand elles sont renvoyées au serveur, car la colonne de destination est un type varchar standard qui n’accepte pas les données chiffrées. 
  
## <a name="selecting-deterministic-or-randomized-encryption"></a><a name="selecting--deterministic-or-randomized-encryption"></a> Sélection d’un chiffrement déterministe ou aléatoire  
 Le moteur de base de données n’opère jamais sur des données en texte clair stockées dans des colonnes chiffrées, mais il prend quand même en charge les requêtes sur des données chiffrées, en fonction du type de chiffrement de la colonne. Always Encrypted prend en charge deux types de chiffrement : le chiffrement aléatoire et le chiffrement déterministe.  
  
- Le chiffrement déterministe génère toujours la même valeur chiffrée pour une valeur en texte clair donnée. Le chiffrement déterministe permet d’effectuer des recherches de point, des jointures d’égalité, ainsi que des regroupements et une indexation sur les colonnes chiffrées. Toutefois, il peut également permettre aux utilisateurs non autorisés de deviner des informations sur des valeurs chiffrées en examinant les modèles dans la colonne chiffrée, en particulier en cas de petit ensemble de valeurs chiffrées possibles, par exemple, Vrai/Faux ou la région Nord/Sud/Est/Ouest. Le chiffrement déterministe doit utiliser un classement de colonne avec un ordre de tri binaire 2 pour les colonnes de type caractère.

- Le chiffrement aléatoire utilise une méthode qui chiffre les données de manière moins prévisible. Le chiffrement aléatoire est plus sécurisé, mais il empêche la recherche, le regroupement, l’indexation et la jointure sur des colonnes chiffrées.

Utilisez le chiffrement déterministe pour les colonnes que vous utilisez comme paramètres de recherche ou de regroupement. Par exemple, un numéro d’identité officiel. Utilisez le chiffrement aléatoire pour des données (par exemple, des commentaires d’enquête confidentielle) qui ne sont pas regroupées avec d’autres enregistrements et ne sont pas utilisées pour joindre des tables.
Pour plus d’informations sur les algorithmes de chiffrement Always Encrypted, consultez [Chiffrement Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-cryptography.md).

## <a name="configuring-always-encrypted"></a>Configuration d’Always Encrypted

 L’installation initiale d’Always Encrypted dans une base de données implique la génération de clés Always Encrypted, la création de métadonnées de clé, la configuration des propriétés de chiffrement des colonnes de base de données sélectionnées et/ou le chiffrement des données qui existent déjà dans les colonnes qui doivent être chiffrées. Notez que certaines de ces tâches ne sont pas prise en charge dans Transact-SQL et nécessitent l’utilisation d’outils côté client. Les clés Always Encrypted et les données sensibles protégées n’étant jamais révélées en texte clair au serveur, le moteur de base de données ne peut pas participer au provisionnement des clés ni effectuer d’opérations de chiffrement ou de déchiffrement des données. Vous pouvez utiliser SQL Server Management Studio ou PowerShell pour accomplir ces tâches. 

|Tâche|SSMS|PowerShell|T-SQL|
|:---|:---|:---|:---
|Mise en service des clés principales de colonne, des clés de chiffrement de colonne et des clés de chiffrement de colonne chiffrées avec leurs clés principales de colonne correspondantes|Oui|Oui|Non |
|Création des métadonnées de clé dans la base de données|Oui|Oui|Oui|
|Création de tables avec des colonnes chiffrées|Oui|Oui|Oui|
|Chiffrement de données existantes dans les colonnes de base de données sélectionnées|Oui|Oui|Non |

> [!NOTE]
> [Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md), introduit dans [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)], prend en charge le chiffrement des données existantes à l’aide de Transact-SQL. Vous n’avez alors plus besoin de déplacer les données hors de la base de données pour les opérations de chiffrement.

> [!NOTE]
> Veillez à exécuter les outils de chiffrement de données ou de mise en service des clés dans un environnement sécurisé, sur un ordinateur différent de celui qui héberge votre base de données. Dans le cas contraire, il pourrait y avoir une fuite des données sensibles ou des clés dans l’environnement serveur, ce qui réduirait les avantages liés à l’utilisation d’Always Encrypted.  

Pour plus d’informations sur la configuration d’Always Encrypted, consultez :
- [Configurer Always Encrypted à l’aide de SQL Server Management Studio](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)
- [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)

## <a name="getting-started-with-always-encrypted"></a>Prise en main d’Always Encrypted

Utilisez l’ [Assistant Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-wizard.md) pour une prise en main rapide d’Always Encrypted. L’Assistant met en service les clés nécessaires et configure le chiffrement pour les colonnes sélectionnées. Si les colonnes pour lesquelles vous définissez le chiffrement contiennent déjà des données, l’Assistant chiffre ces données. L’exemple suivant illustre le processus de chiffrement d’une colonne.

> [!NOTE]  
>  Pour visionner une vidéo qui inclut l’utilisation de l’Assistant, consultez [Getting Started with Always Encrypted with SSMS](https://channel9.msdn.com/Shows/Data-Exposed/Getting-Started-with-Always-Encrypted-with-SSMS)(Prise en main d’Always Encrypted avec SSMS).

1.  Connectez-vous à une base de données qui contient des tables avec des colonnes que vous souhaitez chiffrer à l’aide de l’ **Explorateur d’objets** de Management Studio, ou créez une base de données, créez une ou plusieurs tables avec des colonnes à chiffrer et connectez-vous à cette base de données.
2.  Cliquez avec le bouton droit sur votre base de données, pointez sur **Tâches**, puis cliquez sur **Chiffrer les colonnes** pour ouvrir l’**Assistant Always Encrypted**.
3.  Examinez la page **Introduction** , puis cliquez sur **Suivant**.
4.  Dans la page **Sélectionner les colonnes** , développez les tables et sélectionnez les colonnes que vous souhaitez chiffrer.
5.  Pour chaque colonne sélectionnée pour le chiffrement, sélectionnez **Déterministe** ou *Aléatoire* comme *Type de chiffrement*.
6.  Pour chaque colonne sélectionnée pour le chiffrement, sélectionnez une **Clé de chiffrement**. Si vous n’avez encore jamais créé de clé de chiffrement pour cette base de données, sélectionnez l’option par défaut de génération automatique d’une nouvelle clé, puis cliquez sur **Suivant**.
7.  Dans la page **Configuration de la clé principale** , sélectionnez un emplacement pour stocker la nouvelle clé et une source de clé principale, puis cliquez sur **Suivant**.
8.  Dans la page **Validation** , indiquez si vous souhaitez exécuter le script immédiatement ou créer un script PowerShell, puis cliquez sur **Suivant**.
9.  Dans la page **Récapitulatif**, passez en revue les options que vous avez sélectionnées, puis cliquez sur **Terminer**. Fermez l’Assistant à la fin.

  
## <a name="feature-details"></a>Détails sur les fonctionnalités  
  
-   Les requêtes peuvent effectuer une comparaison d’égalité sur des colonnes chiffrées à l’aide du chiffrement déterministe, mais aucune autre opération (par exemple, comparaison de supériorité ou d’infériorité, correspondance de modèle à l’aide de l’opérateur LIKE ou opérations arithmétiques).  
  
-   Les requêtes sur les colonnes chiffrées à l’aide du chiffrement aléatoire ne peuvent pas effectuer d’opérations sur ces colonnes. L’indexation des colonnes chiffrées à l’aide du chiffrement aléatoire n’est pas prise en charge.  
 > [!NOTE] 
 > [Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md), introduit dans [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)], résout la limitation ci-dessus en activant des critères spéciaux, des opérateurs de comparaison et l’indexation sur les colonnes qui utilisent le chiffrement aléatoire.

-   Une clé de chiffrement de colonne peut avoir jusqu’à deux valeurs chiffrées différentes, chacune chiffrée avec une clé principale de colonne différente. Cela facilite la permutation des clés principales de colonne.  
  
-   Dans le chiffrement déterministe, une colonne doit avoir l’un des [classements *BIN2*](../../../relational-databases/collations/collation-and-unicode-support.md).  

-   Après avoir modifié la définition d’un objet chiffré, exécutez [sp_refresh_parameter_encryption](../../../relational-databases/system-stored-procedures/sp-refresh-parameter-encryption-transact-sql.md) pour mettre à jour les métadonnées Always Encrypted de l’objet.
  
Always Encrypted n’est pas pris en charge pour les colonnes avec les caractéristiques ci-dessous. Par exemple, si l’une des conditions suivantes s’applique à la colonne, la clause `ENCRYPTED WITH` ne peut pas être utilisée dans `CREATE TABLE/ALTER TABLE` pour une colonne :  
  
-   Colonnes utilisant l’un des types de données suivants : `xml`, `timestamp`/`rowversion`, `image`, `ntext`, `text`, `sql_variant`, `hierarchyid`, `geography`, `geometry`, alias, types définis par l’utilisateur.  
- Colonnes `FILESTREAM`  
- Colonnes avec la propriété `IDENTITY`.  
- Colonnes avec la propriété `ROWGUIDCOL`.  
- Chaîne (`varchar`, `char`, etc.) avec des classements non-bin2.  
- Colonnes qui font office de clés pour les index en cluster et non cluster lors de l’utilisation du chiffrement aléatoire (le chiffrement déterministe est pris en charge).
- Colonnes incluses dans les index de recherche en texte intégral (Always Encrypted ne prend pas en charge la [recherche en texte intégral](../../../relational-databases/search/full-text-search.md)).  
- Colonnes calculées
- Colonnes référencées par des colonnes calculées (quand l’expression effectue des opérations non prises en charge pour Always Encrypted).  
- Jeu de colonnes éparses.  
- Colonnes référencées par des statistiques lors de l’utilisation du chiffrement aléatoire (le chiffrement déterministe est pris en charge).  
- Colonnes utilisant des types d’alias.  
- Colonnes de partitionnement.  
- Colonnes avec des contraintes par défaut.  
- Colonnes référencées par des contraintes uniques en cas d’utilisation du chiffrement aléatoire (le chiffrement déterministe est pris en charge).  
- Colonnes de clé primaire en cas d’utilisation du chiffrement aléatoire (le chiffrement déterministe est pris en charge).  
- Colonnes de référence dans les contraintes de clé étrangère en cas d’utilisation du chiffrement aléatoire ou déterministe, si les colonnes de référence et référencées utilisent des clés ou algorithmes différents.  
- Colonnes référencées par des contraintes de validation.  
- Colonnes capturées/suivies à l’aide de la capture des données modifiées.  
- Colonnes de clé primaire sur des tables avec suivi des changements.  
- Colonnes masquées (à l’aide de Dynamic Data Masking).  
- Colonnes dans les tables Stretch Database. (Les tables comportant des colonnes chiffrées avec Always Encrypted peuvent être activées pour l’extension.)  
- Colonnes de tables externes (PolyBase) (remarque : l’utilisation de tables externes et de tables avec des colonnes chiffrées dans la même requête est prise en charge).  
- Les paramètres tabulaires ciblant des colonnes chiffrées ne sont pas pris en charge.  

Vous ne pouvez pas utiliser les clauses suivantes pour les colonnes chiffrées :

- `FOR XML`
- `FOR JSON PATH`

Les fonctionnalités suivantes ne fonctionnent pas sur les colonnes chiffrées :

- Réplication transactionnelle ou de fusion
- Requêtes distribuées (serveurs liés, `OPENROWSET`(T-SQL), `OPENDATASOURCE`(T-SQL))

Spécifications des outils

- SQL Server Management Studio version 18 ou ultérieure est recommandé pour exécuter des requêtes qui déchiffrent les résultats récupérés dans les colonnes chiffrées ou qui insèrent, mettent à jour ou filtrent des colonnes chiffrées. 
- Nécessite `sqlcmd` version 13.1 ou ultérieure, disponible dans le [Centre de téléchargement](https://go.microsoft.com/fwlink/?LinkID=825643).

  
## <a name="database-permissions"></a>Autorisations de base de données  
 Il existe quatre autorisations pour Always Encrypted :  
  
*  `ALTER ANY COLUMN MASTER KEY` (Obligatoire pour créer et supprimer une clé principale de colonne.)  
  
*  `ALTER ANY COLUMN ENCRYPTION KEY` (Obligatoire pour créer et supprimer une clé de chiffrement de colonne.) 
  
*  `VIEW ANY COLUMN MASTER KEY DEFINITION` (Obligatoire pour accéder et lire les métadonnées des clés principales de colonne pour gérer les clés ou interroger des colonnes chiffrées.)  
  
*  `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` (Obligatoire pour accéder et lire les métadonnées de la clé de chiffrement de colonne pour gérer les clés ou interroger des colonnes chiffrées.)  
  
 Le tableau suivant récapitule les autorisations nécessaires pour effectuer des actions courantes.  
  
|Scénario|<code>ALTER ANY COLUMN MASTER KEY</code>|<code>ALTER ANY COLUMN ENCRYPTION KEY</code>|<code>VIEW ANY COLUMN MASTER KEY DEFINITION</code>|<code>VIEW ANY COLUMN ENCRYPTION KEY DEFINITION</code>|  
|--------------|-----------------------------------|---------------------------------------|---------------------------------------------|-------------------------------------------------|  
|Gestion de clés (création/modification/examen des métadonnées de clé dans la base de données)|X|X|X|X|  
|Interrogation de colonnes chiffrées|||X|X|  
  
 **Remarques importantes :**  
  
-   Les autorisations s’appliquent aux actions à l’aide de [!INCLUDE[tsql](../../../includes/tsql-md.md)], [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] (boîtes de dialogue et Assistant) ou de PowerShell.  
  
-   Les deux autorisations *VIEW* sont obligatoires quand l’utilisateur sélectionne des colonnes chiffrées, même s’il n’est pas autorisé à déchiffrer les colonnes.  
  
-   Dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], les deux autorisations *VIEW* sont accordées par défaut au rôle de base de données fixe `public` . Un administrateur de base de données peut choisir de révoquer (ou refuser) les autorisations *VIEW* pour le rôle `public` , et de les accorder à des rôles ou des utilisateurs spécifiques pour implémenter un contrôle plus restreint.  
  
-   Dans [!INCLUDE[ssSDS](../../../includes/sssds-md.md)], les autorisations *VIEW* ne sont pas accordées par défaut au rôle de base de données fixe `public`. Cela permet à certains outils hérités (utilisant des versions antérieures de DacFx) de fonctionner correctement. Ainsi, pour travailler avec des colonnes chiffrées (même si vous ne les déchiffrez pas), un administrateur de base de données doit accorder explicitement les deux autorisations *VIEW* .  

  
## <a name="example"></a> Exemple  
 Le code [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivant crée des métadonnées de clé principale de colonne, des métadonnées de clé de chiffrement de colonne et une table comportant des colonnes chiffrées. Pour plus d’informations sur la façon de créer les clés référencées dans les métadonnées, consultez :
- [Configurer Always Encrypted à l’aide de SQL Server Management Studio](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)
- [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)

  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
     KEY_STORE_PROVIDER_NAME = 'MSSQL_CERTIFICATE_STORE',   
     KEY_PATH = 'Current User/Personal/f2260f28d909d21c642a3d8e0b45a830e79a1420'  
   );  
---------------------------------------------  
CREATE COLUMN ENCRYPTION KEY MyCEK   
WITH VALUES  
(  
    COLUMN_MASTER_KEY = MyCMK,   
    ALGORITHM = 'RSA_OAEP',   
    ENCRYPTED_VALUE = 0x01700000016C006F00630061006C006D0061006300680069006E0065002F006D0079002F003200660061006600640038003100320031003400340034006500620031006100320065003000360039003300340038006100350064003400300032003300380065006600620063006300610031006300284FC4316518CF3328A6D9304F65DD2CE387B79D95D077B4156E9ED8683FC0E09FA848275C685373228762B02DF2522AFF6D661782607B4A2275F2F922A5324B392C9D498E4ECFC61B79F0553EE8FB2E5A8635C4DBC0224D5A7F1B136C182DCDE32A00451F1A7AC6B4492067FD0FAC7D3D6F4AB7FC0E86614455DBB2AB37013E0A5B8B5089B180CA36D8B06CDB15E95A7D06E25AACB645D42C85B0B7EA2962BD3080B9A7CDB805C6279FE7DD6941E7EA4C2139E0D4101D8D7891076E70D433A214E82D9030CF1F40C503103075DEEB3D64537D15D244F503C2750CF940B71967F51095BFA51A85D2F764C78704CAB6F015EA87753355367C5C9F66E465C0C66BADEDFDF76FB7E5C21A0D89A2FCCA8595471F8918B1387E055FA0B816E74201CD5C50129D29C015895CD073925B6EA87CAF4A4FAF018C06A3856F5DFB724F42807543F777D82B809232B465D983E6F19DFB572BEA7B61C50154605452A891190FB5A0C4E464862CF5EFAD5E7D91F7D65AA1A78F688E69A1EB098AB42E95C674E234173CD7E0925541AD5AE7CED9A3D12FDFE6EB8EA4F8AAD2629D4F5A18BA3DDCC9CF7F352A892D4BEBDC4A1303F9C683DACD51A237E34B045EBE579A381E26B40DCFBF49EFFA6F65D17F37C6DBA54AA99A65D5573D4EB5BA038E024910A4D36B79A1D4E3C70349DADFF08FD8B4DEE77FDB57F01CB276ED5E676F1EC973154F86  
);  
---------------------------------------------  
CREATE TABLE Customers (  
    CustName nvarchar(60)   
        COLLATE  Latin1_General_BIN2 ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = MyCEK,  
        ENCRYPTION_TYPE = RANDOMIZED,  
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'),   
    SSN varchar(11)   
        COLLATE  Latin1_General_BIN2 ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = MyCEK,  
        ENCRYPTION_TYPE = DETERMINISTIC ,  
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'),   
    Age int NULL  
);  
GO  
  
```  
## <a name="see-also"></a> Voir aussi  
- [Configurer Always Encrypted à l’aide de SQL Server Management Studio](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)   
- [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)   
- [Développer des applications avec Always Encrypted](always-encrypted-client-development.md) 
- [Configurer le chiffrement de colonne à l’aide de l’Assistant Always Encrypted](always-encrypted-wizard.md)
- [Chiffrement Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-cryptography.md)   
- [CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-column-master-key-transact-sql.md)   
- [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)   
- [CREATE TABLE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-table-transact-sql.md)   
- [column_definition &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-table-column-definition-transact-sql.md)   
- [sys.column_encryption_keys  &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)  
- [sys.column_encryption_key_values &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md)   
- [sys.column_master_keys &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)   
- [sys.columns &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
- [sp_refresh_parameter_encryption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-refresh-parameter-encryption-transact-sql.md)   
  
  
