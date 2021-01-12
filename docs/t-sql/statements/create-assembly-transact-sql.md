---
description: CREATE ASSEMBLY (Transact-SQL)
title: CREATE ASSEMBLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ASSEMBLY
- CREATE ASSEMBLY
- CREATE_ASSEMBLY_TSQL
- ASSEMBLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- assemblies [CLR integration], validating
- validating assemblies
- CREATE ASSEMBLY statement
- assemblies [CLR integration], creating
ms.assetid: d8d1d245-c2c3-4325-be52-4fc1122c2079
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 13b639ace60f98ce9ab55cc0280130131f6851ca
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098405"
---
# <a name="create-assembly-transact-sql"></a>CREATE ASSEMBLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  Crée un module d'application managée qui contient des métadonnées de classe et du code managé sous forme d'objet dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En référençant ce module, il est possible de créer dans la base de données des fonctions CLR (Common Language Runtime), des procédures stockées, des déclencheurs, des agrégats et des types définis par l'utilisateur.  
  
> [!WARNING]
>  CLR utilise la sécurité d’accès du code (CAS) dans le .NET Framework, qui n’est plus pris en charge comme limite de sécurité. Un assembly CLR créé avec `PERMISSION_SET = SAFE` peut être en mesure d’accéder à des ressources système externes, d’appeler du code non managé et d’acquérir des privilèges sysadmin. À compter de [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)], une option de `sp_configure` appelée `clr strict security` est introduite pour renforcer la sécurité des assemblys CLR. `clr strict security` est activée par défaut et traite les assemblys `SAFE` et `EXTERNAL_ACCESS` comme s’ils étaient marqués `UNSAFE`. L’option `clr strict security` peut être désactivée pour assurer une compatibilité descendante, mais ceci n’est pas recommandé. Microsoft recommande que tous les assemblys soient signés par un certificat ou une clé asymétrique avec une connexion correspondante à laquelle a été accordée l’autorisation `UNSAFE ASSEMBLY` dans la base de données master. Pour plus d’informations, consultez [CLR strict security](../../database-engine/configure-windows/clr-strict-security.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE ASSEMBLY assembly_name  
[ AUTHORIZATION owner_name ]  
FROM { <client_assembly_specifier> | <assembly_bits> [ ,...n ] }  
[ WITH PERMISSION_SET = { SAFE | EXTERNAL_ACCESS | UNSAFE } ]  
[ ; ]  
<client_assembly_specifier> :: =  
        '[\\computer_name\]share_name\[path\]manifest_file_name'  
  | '[local_path\]manifest_file_name'  
  
<assembly_bits> :: =  
{ varbinary_literal | varbinary_expression }  
```  
  
## <a name="arguments"></a>Arguments  
 *assembly_name*  
 Nom de l'assembly. Ce nom doit être unique dans la base de données et correspondre à un [identificateur](../../relational-databases/databases/database-identifiers.md) valide.  
  
 AUTHORIZATION *owner_name*  
 Spécifie le nom d'un utilisateur ou d'un rôle comme propriétaire de l'assembly. *owner_name* doit être le nom d’un rôle dont l’utilisateur actuel est membre, ou l’utilisateur actuel doit disposer de l’autorisation IMPERSONATE sur *owner_name*. En l'absence de spécification, la propriété revient à l'utilisateur actuel.  
  
 \<client_assembly_specifier>  
Spécifie le chemin d'accès local ou l'emplacement sur le réseau où se trouve l'assembly en cours de téléchargement, et également le nom du fichier de manifeste qui correspond à l'assembly.  \<client_assembly_specifier> peut s’exprimer sous forme de chaîne de caractères fixe ou d’une expression qui correspond à une chaîne de caractères fixe, avec des variables. CREATE ASSEMBLY ne prend pas en charge le chargement d'assemblys multimodules. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recherche au même emplacement les assemblys dépendants de cet assembly et les télécharge avec le même propriétaire en tant qu'assembly au niveau racine. Si ces assemblys dépendants sont introuvables et s'ils ne sont pas déjà chargés dans la base de données active, l'instruction CREATE ASSEMBLY échoue. Si les assemblys dépendants sont déjà chargés dans la base de données active, leur propriétaire doit être identique à celui de l'assembly créé.

> [!IMPORTANT]
> Azure SQL Database ne prend pas en charge la création d’un assembly à partir d’un fichier.
  
 Vous ne pouvez pas spécifier \<client_assembly_specifier> si l’identité de l’utilisateur connecté a été empruntée.  
  
 \<assembly_bits>  
 Liste des valeurs binaires qui composent l'assembly et ses assemblys dépendants. La première valeur de la liste est considérée comme l'assembly de niveau racine. Les valeurs correspondant aux assemblys dépendants peuvent être fournies dans n'importe quel ordre. Toutes les valeurs qui ne correspondent pas à des dépendances de l'assembly racine sont ignorées.  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données autonome.  
  
 *varbinary_literal*  
 Est un littéral **varbinary**.  
  
 *varbinary_expression*  
 Est une expression de type **varbinary**.  
  
 PERMISSION_SET { **SAFE** | EXTERNAL_ACCESS | UNSAFE }  
> [!IMPORTANT]
>  L’option `PERMISSION_SET` est affectée par l’option `clr strict security`, qui est décrite dans l’avertissement d’ouverture. Quand `clr strict security` est activé, tous les assemblys sont traités en tant que `UNSAFE`.
 
 Spécifie un ensemble d'autorisations d'accès accordées à l'assembly lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y accède. Si cet argument n'est pas défini, la valeur SAFE est appliquée par défaut.  
  
 Il est recommandé d'utiliser SAFE. Il s'agit de l'ensemble d'autorisations le plus restrictif. Le code exécuté par un assembly avec les autorisations SAFE ne peut pas accéder aux ressources système externes telles que les fichiers, le réseau, les variables d'environnement ou le Registre.  
  
 EXTERNAL_ACCESS permet à des assemblys d'accéder à certaines ressources système externes telles que les fichiers, les réseaux, les variables d'environnement et le Registre.  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données autonome.  
  
 UNSAFE permet à des assemblys d'accéder sans restrictions aux ressources, à l'intérieur ou à l'extérieur d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le code qui s'exécute dans un assembly UNSAFE peut appeler du code non managé.  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données autonome.  
  
> [!IMPORTANT]  
>  SAFE est le paramètre recommandé pour les autorisations des assemblys qui effectuent des calculs et des tâches de gestion des données sans accéder à des ressources extérieures à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
>   
>  Il est recommandé d'utiliser EXTERNAL_ACCESS pour les assemblys qui accèdent à des ressources extérieures à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les assemblys EXTERNAL_ACCESS sont dotés des protections de fiabilité et d'évolutivité des assemblys SAFE, mais sont identiques aux assemblys UNSAFE du point de vue sécurité. Cela est dû au fait que le code des assemblys EXTERNAL_ACCESS s'exécute par défaut sous le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et accède aux ressources externes sous ce compte, excepté si le compte emprunte explicitement l'identité de l'appelant. Par conséquent, l'autorisation de créer des assemblys EXTERNAL_ACCESS doit être accordée uniquement aux connexions approuvées pour exécuter du code sous le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur l’emprunt d’identité, consultez [Sécurité de l’intégration du CLR](../../relational-databases/clr-integration/security/clr-integration-security.md).  
>   
>  UNSAFE permet au code de l'assembly d'effectuer sans restriction des opérations dans l'espace du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ce qui peut nuire potentiellement à la fiabilité de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les assemblys UNSAFE peuvent également corrompre le système de sécurité de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou de CLR (Common Language Runtime). Les autorisations UNSAFE doivent être accordées uniquement aux assemblys dont le niveau d'approbation est très élevé. Seuls les membres du rôle serveur fixe **sysadmin** peuvent créer et modifier les assemblys UNSAFE.  
  
 Pour plus d’informations sur les jeux d’autorisations des assemblys, consultez [Conception d’assemblys](../../relational-databases/clr-integration/assemblies-designing.md).  
  
## <a name="remarks"></a>Remarques  
 CREATE ASSEMBLY télécharge un assembly préalablement compilé sous forme d'un fichier .dll à partir de code managé pour l'utilisation dans une instance de SQL Server.  
 
Quand elle est activée, l’option `PERMISSION_SET` dans les instructions `CREATE ASSEMBLY` et `ALTER ASSEMBLY` est ignorée au moment de l’exécution, mais les options `PERMISSION_SET` sont conservées dans les métadonnées. Ignorer cette option réduit les risques de rupture des instructions de code existantes.
 
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'autorise pas l'inscription de différentes versions d'un assembly avec le même nom, la même culture et la même clé publique.  
  
Lors d’une tentative d’accès à l’assembly spécifié dans \<client_assembly_specifier>, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] emprunte l’identité du contexte de sécurité de la connexion Windows active. Si \<client_assembly_specifier> spécifie un emplacement réseau (chemin UNC), l’emprunt d’identité de la connexion active n’est pas transféré à l’emplacement réseau en raison des restrictions de délégation. Dans ce cas, l'accès a lieu en utilisant le contexte de sécurité du compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Informations d’identification &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md).
  
 Outre l’assembly racine spécifié par *assembly_name*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente de charger les assemblys référencés par l’assembly racine en cours de chargement. Si un assembly référencé est déjà téléchargé dans une base de données du fait d'une précédente instruction CREATE ASSEMBLY, cet assembly n'est pas téléchargé, mais il est disponible pour l'assembly racine. Si un assembly dépendant n'a pas été téléchargé auparavant et si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne trouve pas son fichier de manifeste dans le répertoire source, CREATE ASSEMBLY renvoie une erreur.  
  
 Si des assemblys dépendants référencés par l'assembly racine ne se trouvent pas déjà dans la base de données et sont chargés implicitement avec ce dernier, ils ont le même ensemble d'autorisations que l'assembly de niveau racine. Si les assemblys dépendants doivent être créés en utilisant un autre ensemble d'autorisations que celui de l'assembly racine, ils doivent être téléchargés explicitement avant l'assembly de niveau racine avec l'ensemble d'autorisations approprié.  
  
## <a name="assembly-validation"></a>Validation des assemblys  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vérifie les fichiers assembly binaires téléchargés au moyen de l'instruction CREATE ASSEMBLY pour garantir les conditions suivantes :  
  
-   Le fichier assembly binaire se compose de métadonnées et de segments de code valides et ces segments de code contiennent des instructions MSIL (Microsoft Intermediate language) valides.  
  
-   Le jeu d'assemblys système qu'il référence est l'un des assemblys suivants pris en charge dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : Microsoft.Visualbasic.dll, Mscorlib.dll, System.Data.dll, System.dll, System.Xml.dll, Microsoft.Visualc.dll, Custommarshallers.dll, System.Security.dll, System.Web.Services.dll, System.Data.SqlXml.dll, System.Core.dll, et System.Xml.Linq.dll. D'autres assemblys système peuvent être référencés, mais ils doivent être inscrits explicitement dans la base de données.  
  
-   Pour les assemblys créés à l'aide des ensembles d'autorisations SAFE ou EXTERNAL ACCESS :  
  
    -   Le type du code assembly doit être sécurisé. La sécurité s'établit en exécutant l'outil de vérification CLR (Common Language Runtime) sur l'assembly.  
  
    -   L'assembly ne doit pas contenir de membres de données statiques dans ses classes, excepté s'ils sont marqués en lecture seule.  
  
    -   Les classes de l'assembly ne peuvent pas contenir de finaliseur.  
  
    -   Les classes ou méthodes de l'assembly doivent être annotées uniquement avec des attributs de code autorisés. Pour plus d’informations, consultez [Attributs personnalisés pour les routines CLR](../../relational-databases/clr-integration/database-objects/clr-integration-custom-attributes-for-clr-routines.md)  
  
 Outre les vérifications précédentes effectuées pendant l'exécution de CREATE ASSEMBLY, d'autres vérifications ont lieu au moment de l'exécution du code dans l'assembly :  
  
-   L’appel à certaines API [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] qui nécessitent une autorisation d’accès au code risque d’échouer si l’ensemble d’autorisations de l’assembly ne comprend pas cette autorisation.  
  
-   Pour les assemblys SAFE et EXTERNAL_ACCESS, tout appel aux API [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] annotées avec certains attributs HostProtectionAttributes échoue.  
  
 Pour plus d’informations, consultez [Conception d’assemblys](../../relational-databases/clr-integration/assemblies-designing.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation CREATE ASSEMBLY.  
  
 Si PERMISSION_SET = EXTERNAL_ACCESS est spécifié, nécessite l’autorisation **EXTERNAL ACCESS ASSEMBLY** sur le serveur. Si PERMISSION_SET = UNSAFE est spécifié, nécessite l’autorisation **UNSAFE ASSEMBLY** sur le serveur.  
  
 L'utilisateur doit être propriétaire des assemblys référencés par l'assembly qui doivent être téléchargés s'ils existent déjà dans la base de données. Pour charger un assembly à l’aide d’un chemin de fichier, l’utilisateur actuel doit utiliser une connexion Windows authentifiée ou être membre du rôle serveur fixe **sysadmin**. La connexion Windows de l'utilisateur qui exécute l'instruction CREATE ASSEMBLY doit avoir l'autorisation de lecture sur le partage et sur les fichiers chargés dans l'instruction.  

### <a name="permissions-with-clr-strict-security"></a>Autorisations avec sécurité CLR stricte    
Les autorisations suivantes sont requises pour créer un assembly CLR quand `CLR strict security` est activée :

- L’utilisateur doit avoir l’autorisation `CREATE ASSEMBLY`.  
- Et une des conditions suivantes doit également être remplie :  
  - L’assembly est signé avec un certificat ou une clé asymétrique qui a une connexion correspondante avec l’autorisation `UNSAFE ASSEMBLY` sur le serveur. Signer l’assembly est recommandé.  
  - La base de données a la propriété `TRUSTWORTHY` définie sur `ON`, et elle est détenue par une connexion qui a l’autorisation `UNSAFE ASSEMBLY` sur le serveur. Cette option n’est pas recommandée.  
  
 Pour plus d’informations sur les jeux d’autorisations des assemblys, consultez [Conception d’assemblys](../../relational-databases/clr-integration/assemblies-designing.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="example-a-creating-an-assembly-from-a-dll"></a>Exemple A : Création d’un assembly à partir d’une dll  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 L’exemple suivant suppose que les exemples du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] sont installés à l’emplacement par défaut de l’ordinateur local et que l’exemple d’application HelloWorld.csproj est compilé. Pour plus d’informations, consultez [Exemple Hello World](/previous-versions/sql/sql-server-2016/ff878250(v=sql.130)).  
  
```sql  
CREATE ASSEMBLY HelloWorld   
FROM '<system_drive>:\Program Files\Microsoft SQL Server\100\Samples\HelloWorld\CS\HelloWorld\bin\debug\HelloWorld.dll'  
WITH PERMISSION_SET = SAFE;  
```  

> [!IMPORTANT]
> Azure SQL Database ne prend pas en charge la création d’un assembly à partir d’un fichier.
  
### <a name="example-b-creating-an-assembly-from-assembly-bits"></a>Exemple B : Création d’un assembly à partir de bits d’assembly  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Remplacez les exemples de bits (qui ne sont pas terminés ou valides) par vos bits d’assembly.  
  
```sql  
CREATE ASSEMBLY HelloWorld  
    FROM 0x4D5A900000000000  
WITH PERMISSION_SET = SAFE;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-assembly-transact-sql.md)   
 [DROP ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-assembly-transact-sql.md)   
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)   
 [CREATE AGGREGATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-aggregate-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Scénarios et exemples d’utilisation pour l’intégration du CLR &#40;Common Language Runtime&#41;](/previous-versions/sql/sql-server-2016/ms131078(v=sql.130))  
  
