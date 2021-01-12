---
description: ALTER ASSEMBLY (Transact-SQL)
title: ALTER ASSEMBLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_ASSEMBLY_TSQL
- ALTER ASSEMBLY
dev_langs:
- TSQL
helpviewer_keywords:
- assemblies [CLR integration], modifying
- refreshing assemblies
- assemblies [CLR integration], versioning
- assemblies [CLR integration], adding files
- modifying assemblies
- adding files
- ALTER ASSEMBLY statement
ms.assetid: 87bca678-4e79-40e1-bb8b-bd5ed8f34853
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: dda1de9a2eb4628765333651c7d074b0c38551db
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100909"
---
# <a name="alter-assembly-transact-sql"></a>ALTER ASSEMBLY (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifie un assembly en changeant les propriétés de catalogue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'un assembly. ALTER ASSEMBLY le réactualise avec la dernière copie des modules [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] qui contiennent son implémentation, et ajoute ou supprime les fichiers qui lui sont associés. Les assemblys sont créés à l’aide de [CREATE ASSEMBLY](../../t-sql/statements/create-assembly-transact-sql.md).  

> [!WARNING]
>  CLR utilise la sécurité d’accès du code (CAS) dans le .NET Framework, qui n’est plus pris en charge comme limite de sécurité. Un assembly CLR créé avec `PERMISSION_SET = SAFE` peut être en mesure d’accéder à des ressources système externes, d’appeler du code non managé et d’acquérir des privilèges sysadmin. À compter de [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)], une option de `sp_configure` appelée `clr strict security` est introduite pour renforcer la sécurité des assemblys CLR. `clr strict security` est activée par défaut et traite les assemblys `SAFE` et `EXTERNAL_ACCESS` comme s’ils étaient marqués `UNSAFE`. L’option `clr strict security` peut être désactivée pour assurer une compatibilité descendante, mais ceci n’est pas recommandé. Microsoft recommande que tous les assemblys soient signés par un certificat ou une clé asymétrique avec une connexion correspondante à laquelle a été accordée l’autorisation `UNSAFE ASSEMBLY` dans la base de données master. Pour plus d’informations, consultez [CLR strict security](../../database-engine/configure-windows/clr-strict-security.md).  

 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER ASSEMBLY assembly_name  
    [ FROM <client_assembly_specifier> | <assembly_bits> ]  
    [ WITH <assembly_option> [ ,...n ] ]  
    [ DROP FILE { file_name [ ,...n ] | ALL } ]  
    [ ADD FILE FROM   
    {   
        client_file_specifier [ AS file_name ]   
      | file_bits AS file_name   
    } [,...n ]   
    ] [ ; ]  
<client_assembly_specifier> :: =  
    '\\computer_name\share-name\[path\]manifest_file_name'  
  | '[local_path\]manifest_file_name'  
  
<assembly_bits> :: =  
    { varbinary_literal | varbinary_expression }  
  
<assembly_option> :: =  
    PERMISSION_SET = { SAFE | EXTERNAL_ACCESS | UNSAFE }   
  | VISIBILITY = { ON | OFF }  
  | UNCHECKED DATA  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *assembly_name*  
 Nom de l'assembly à modifier. *assembly_name* doit déjà exister dans la base de données.  
  
 FROM \<client_assembly_specifier> | \<assembly_bits>  
 Met à jour un assembly avec la dernière copie des modules [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] qui contiennent sa mise en œuvre. Cette option ne peut être utilisée que s'il n'existe aucun fichier associé avec l'assembly spécifié.  
  
 \<client_assembly_specifier> spécifie l’emplacement réseau ou local où se trouve l’assembly actuellement réactualisé. L'emplacement réseau inclut le nom de l'ordinateur, le nom de partage et un chemin d'accès au sein de ce partage. *manifest_file_name* spécifie le nom du fichier qui contient le manifeste de l’assembly.  

> [!IMPORTANT]
> Azure SQL Database ne prend pas en charge le référencement d’un fichier.
  
 \<assembly_bits> est la valeur binaire de l’assembly.  
  
 Des instructions ALTER ASSEMBLY distinctes doivent être émises pour tous les assemblys dépendants qui requièrent aussi une mise à jour.  
  
 PERMISSION_SET = { SAFE | EXTERNAL_ACCESS | UNSAFE }   
> [!IMPORTANT]
>  L’option `PERMISSION_SET` est affectée par l’option `clr strict security`, qui est décrite dans l’avertissement d’ouverture. Quand `clr strict security` est activé, tous les assemblys sont traités en tant que `UNSAFE`.  
>  Spécifie la propriété de l'ensemble d'autorisation du code d'accès [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] de l'assembly. Pour plus d’informations sur cette propriété, consultez [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md).  
> 
> [!NOTE]
>  Les options EXTERNAL_ACCESS et UNSAFE ne sont pas disponibles dans une base de données autonome.  
  
 VISIBILITY = { ON | OFF }  
 Indique si l'assembly est visible pour créer des fonctions, des procédures stockées, des déclencheurs, des types et des fonctions d'agrégation définis par l'utilisateur CLR (Common Language Runtime). Avec la valeur OFF, l'assembly ne peut être appelé que par d'autres assemblys. S'il existe des objets de base de données CLR créés sur l'assembly, sa visibilité ne peut pas être modifiée. Tout assembly référencé par *assembly_name* est chargé par défaut comme n’étant pas visible.  
  
 UNCHECKED DATA  
 Par défaut, ALTER ASSEMBLY échoue si elle doit vérifier la cohérence de lignes individuelles d'une table. Cette option permet de reporter les vérifications à une date ultérieure à l'aide de DBCC CHECKTABLE. Si cette option est spécifiée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécute l'instruction ALTER ASSEMBLY même si des tables de la base de données contiennent les éléments suivants :  
  
-   des colonnes calculées persistantes qui référencent directement ou indirectement des méthodes dans l'assembly, par le biais de fonctions ou de méthodes [!INCLUDE[tsql](../../includes/tsql-md.md)] ;  
  
-   des contraintes CHECK qui référencent directement ou indirectement des méthodes de l'assembly ;  
  
-   Des colonnes d’un type CLR défini par l’utilisateur dépendant de l’assembly, ce type implémentant un format de sérialisation **UserDefined** (non-**Native**).  
  
-   des colonnes d'un type CLR défini par l'utilisateur qui font référence à des vues créées à l'aide de WITH SCHEMABINDING.  
  
 S'il y a des contraintes CHECK, elles sont désactivées et signalées comme étant non approuvées. Les tables qui contiennent des colonnes dépendant de l'assembly sont signalées comme contenant des données non vérifiées jusqu'à ce que ces tables soient explicitement vérifiées.  
  
 Seuls les membres des rôles de base de données fixes **db_owner** et **db_ddlowner** peuvent spécifier cette option.  
  
 Nécessite l’autorisation **ALTER ANY SCHEMA** pour spécifier cette option.  
  
 Pour plus d’informations, consultez [Implémentation de points de terminaison](../../relational-databases/clr-integration/assemblies-implementing.md).  
  
 [ DROP FILE { *file_name*[ **,** _...n_] | ALL } ]  
 Supprime le nom de fichier associé à l'assembly ou tous les fichiers associés à l'assembly, de la base de données. Si DROP FILE est utilisé avec ADD FILE qui suit, il s'exécute en premier. Cela vous permet de remplacer un fichier avec le même nom de fichier.  
  
> [!NOTE]  
>  Cette option n’est disponible ni dans une base de données autonome, ni dans Azure SQL Database.  
  
 [ ADD FILE FROM { *client_file_specifier* [ AS *file_name*] | *file_bits* AS *file_name*}  
 Charge un fichier à associer à l’assembly, par exemple du code source, des fichiers de débogage ou d’autres informations apparentées, sur le serveur et l’affiche dans la vue de catalogue **sys.assembly_files**. *client_file_specifier* spécifie l’emplacement à partir duquel le fichier est chargé. *file_bits* peut être utilisé à la place pour spécifier la liste des valeurs binaires qui constituent le fichier. *file_name* spécifie le nom sous lequel le fichier doit être stocké dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *file_name* doit être spécifié si *file_bits* est spécifié, et est facultatif si *client_file_specifier* est spécifié. Si *file_name* n’est pas spécifié, la partie file_name de *client_file_specifier* est utilisée comme *file_name*.  
  
> [!NOTE]  
>  Cette option n’est disponible ni dans une base de données autonome, ni dans Azure SQL Database.  
  
## <a name="remarks"></a>Notes  
 ALTER ASSEMBLY n'interrompt pas les sessions en cours qui exécutent du code dans l'assembly modifié. Ces sessions se terminent en utilisant les bits non modifiés de l'assembly.  
  
 Si la clause FROM est spécifiée, ALTER ASSEMBLY met à jour l'assembly par rapport aux dernières copies des modules fournis. Comme il peut y avoir dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des fonctions, des procédures stockées, des déclencheurs, des types de données et des fonctions d'agrégation définies par l'utilisateur CLR qui sont déjà définis dans l'assembly, l'instruction ALTER ASSEMBLY les réassocie à la dernière mise en œuvre de l'assembly. Pour cela, les méthodes qui effectuent le mappage avec les fonctions, les procédures stockées et les déclencheurs CLR doivent toujours exister dans l'assembly modifié, avec les mêmes signatures. Les classes qui mettent en œuvre des types et des fonctions d'agrégation CLR définis par l'utilisateur doivent toutefois satisfaire aux exigences inhérentes aux types ou agrégations définis par l'utilisateur.  
  
> [!CAUTION]  
>  Si WITH UNCHECKED DATA n'est pas spécifié, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente d'empêcher ALTER ASSEMBLY de s'exécuter si la nouvelle version de l'assembly affecte des données existantes dans des tables, des index ou dans d'autres sites persistants. Cependant, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne garantit pas la cohérence des colonnes, des vues indexées, des expressions ou des index calculés avec les routines et les types sous-jacents une fois que l'assembly CLR a été mis à jour. Soyez prudent lorsque vous exécutez ALTER ASSEMBLY pour vérifier qu'il n'existe pas de discordance entre le résultat d'une expression et une valeur basée sur la version de cette expression stockée dans l'assembly.  
  
 ALTER ASSEMBLY modifie la version de l'assembly. La culture et le jeton de clé publique de l'assembly restent inchangés.  
  
 L'instruction ALTER ASSEMBLY ne permet pas de modifier :  
  
-   Les signatures des fonctions, fonctions d'agrégation, procédures stockées et déclencheurs CLR d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui font référence à l'assembly. ALTER ASSEMBLY échoue lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas réassocier des objets de base de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec la nouvelle version de l'assembly.  
  
-   Les signatures des méthodes de l'assembly qui sont appelées depuis d'autres assemblys.  
  
-   La liste des assemblys qui dépendent de l’assembly, référencés dans la propriété **DependentList** de l’assembly.  
  
-   La capacité d'indexation d'une méthode, à moins qu'il n'existe pas d'index ni de colonnes calculées persistantes dépendant de cette méthode, que ce soit directement ou indirectement.  
  
-   L’attribut du nom de méthode **FillRow** pour les fonctions table CLR.  
  
-   La signature de méthode **Accumulate** et **Terminate** pour les agrégations définies par l’utilisateur.  
  
-   Assemblys système.  
  
-   Appartenance de l'assembly. Utilisez plutôt [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
 Qui plus est, dans le cas des assemblys qui mettent en œuvre des types définis par l'utilisateur, vous pouvez utiliser ALTER ASSEMBLY pour apporter les modifications suivantes uniquement :  
  
-   Modification des méthodes publiques de la classe du type défini par l'utilisateur, tant que les signatures ou les attributs ne sont pas modifiés.  
  
-   Ajout de nouvelles méthodes publiques.  
  
-   Modification des méthodes privées de quelque manière que ce soit.  
  
 Les champs qui se trouvent dans un type défini par l'utilisateur sérialisé de façon native (notamment les membres de données ou les classes de base) ne peuvent pas être modifiés par le biais de ALTER ASSEMBLY. Aucune autre modification n'est prise en charge.  
  
 Si ADD FILE FROM n'est pas spécifié, ALTER ASSEMBLY supprime tous les fichiers associés à l'assembly.  
  
 Si l'instruction ALTER ASSEMBLY est exécutée sans la clause de données UNCHECKED, des vérifications sont effectuées pour s'assurer que la nouvelle version de l'assembly n'affecte pas les données existantes dans les tables. Selon la quantité de données à vérifier, cela peut affecter les performances.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER sur l'assembly. Il y a d'autres exigences :  
  
-   Pour modifier un assembly dont le jeu d’autorisations existant est EXTERNAL_ACCESS, vous devez avoir l’autorisation **EXTERNAL ACCESS ASSEMBLY** sur le serveur.  
  
-   Pour modifier un assembly dont le jeu d’autorisations existant est UNSAFE, vous devez avoir l’autorisation **UNSAFE ASSEMBLY** sur le serveur.  
  
-   Pour changer le jeu d’autorisations d’un assembly et le définir sur EXTERNAL_ACCESS, vous devez avoir l’autorisation **EXTERNAL ACCESS ASSEMBLY** sur le serveur.  
  
-   Pour changer le jeu d’autorisations d’un assembly et le définir sur UNSAFE, vous devez avoir l’autorisation **UNSAFE ASSEMBLY** sur le serveur.  
  
-   La spécification de la clause WITH UNCHECKED DATA nécessite l’autorisation **ALTER ANY SCHEMA**.  


### <a name="permissions-with-clr-strict-security"></a>Autorisations avec sécurité CLR stricte    
Les autorisations suivantes sont nécessaires pour modifier un assembly CLR quand `CLR strict security` est activée :

- L’utilisateur doit avoir l’autorisation `ALTER ASSEMBLY`.  
- Et une des conditions suivantes doit également être remplie :  
  - L’assembly est signé avec un certificat ou une clé asymétrique qui a une connexion correspondante avec l’autorisation `UNSAFE ASSEMBLY` sur le serveur. Signer l’assembly est recommandé.  
  - La base de données a la propriété `TRUSTWORTHY` définie sur `ON`, et elle est détenue par une connexion qui a l’autorisation `UNSAFE ASSEMBLY` sur le serveur. Cette option n’est pas recommandée.  
  
  
 Pour plus d’informations sur les jeux d’autorisations des assemblys, consultez [Conception d’assemblys](../../relational-databases/clr-integration/assemblies-designing.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-refreshing-an-assembly"></a>R. Actualisation d'un assembly  
 L'exemple suivant met à jour l'assembly `ComplexNumber` avec la dernière copie des modules [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] qui conservent son implémentation.  
  
> [!NOTE]  
>  L'assembly `ComplexNumber` peut être créé en exécutant les scripts d'exemple UserDefinedDataType. Pour plus d’informations, consultez [Type défini par l’utilisateur](/previous-versions/sql/sql-server-2016/ms131078(v=sql.130)).  
  
 ```sql
 ALTER ASSEMBLY ComplexNumber 
 FROM 'C:\Program Files\Microsoft SQL Server\130\Tools\Samples\1033\Engine\Programmability\CLR\UserDefinedDataType\CS\ComplexNumber\obj\Debug\ComplexNumber.dll' 
  ```

> [!IMPORTANT]
> Azure SQL Database ne prend pas en charge le référencement d’un fichier.

### <a name="b-adding-a-file-to-associate-with-an-assembly"></a>B. Ajout d'un fichier à associer à un assembly  
 L'exemple suivant télécharge le fichier du code source `Class1.cs` à associer à l'assembly `MyClass`. Cet exemple suppose que l'assembly `MyClass` est déjà créé dans la base de données.  
  
```sql  
ALTER ASSEMBLY MyClass   
ADD FILE FROM 'C:\MyClassProject\Class1.cs';  
```  

> [!IMPORTANT]
> Azure SQL Database ne prend pas en charge le référencement d’un fichier.

### <a name="c-changing-the-permissions-of-an-assembly"></a>C. Modification des autorisations d'un assembly  
 L'exemple suivant change l'ensemble d'autorisations de l'assembly `ComplexNumber` pour passer de SAFE à `EXTERNAL ACCESS`.  
  
```sql  
ALTER ASSEMBLY ComplexNumber WITH PERMISSION_SET = EXTERNAL_ACCESS;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)   
 [DROP ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-assembly-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
