---
description: Conventions de la syntaxe Transact-SQL (Transact-SQL)
title: Conventions de la syntaxe Transact-SQL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- sql13.TSQLExpandPortal.f1
dev_langs:
- TSQL
helpviewer_keywords:
- conventions [SQL Server]
- Applies to section in Transact-SQL topics
- code example conventions [SQL Server]
- objects [SQL Server], names
- code [SQL Server], conventions
- multipart names [SQL Server]
- Transact-SQL syntax conventions
- syntax conventions [SQL Server]
- code [SQL Server]
- Transact-SQL
- naming conventions [SQL Server]
- syntax [SQL Server], Transact-SQL
ms.assetid: 35fbcf7f-8b55-46cd-a957-9b8c7b311241
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ed6fee3cc0fae70b0258bc427322a66a32f39eff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340632"
---
# <a name="transact-sql-syntax-conventions-transact-sql"></a>Conventions de la syntaxe Transact-SQL (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Le tableau suivant répertorie et décrit les conventions utilisées dans les diagrammes de syntaxe du Guide de référence [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
|Convention|Utilisé pour|  
|----------------|--------------|  
|MAJUSCULES|Mots clés [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|_italique_|Paramètres de syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] fournis par l'utilisateur.|  
|**gras**|Entrez les noms de bases de données, de tables, de colonnes, d'index, de procédures stockées, d'utilitaires, de types de données et de textes exactement comme ils sont indiqués.|  
|souligné|Indique que l'instruction est omise par la valeur par défaut appliquée lorsque la clause contient la valeur soulignée.|  
|&#124; (barre verticale)|Sépare les éléments de syntaxe placés entre crochets ou entre accolades. Vous ne pouvez utiliser qu'un seul de ces éléments.|  
|`[ ]` (crochets)|Éléments de syntaxe facultatifs. Ne tapez pas les crochets.|  
|{} (accolades)|Éléments de syntaxe obligatoires. Ne tapez pas les accolades.|  
|[ **,** ..._n_]|Indique que l’élément précédent peut se répéter _n_ fois. Les occurrences sont séparées par des virgules.|  
|[..._n_]|Indique que l’élément précédent peut se répéter _n_ fois. Les occurrences sont séparées par des espaces.|  
|;|Terminateur d'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]. Bien que le point-virgule ne soit pas requis pour la plupart des instructions dans cette version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il sera requis dans une version à venir.|  
|\<label> ::=|Nom d'un bloc de syntaxe. Utilisez cette convention pour regrouper et étiqueter des sections de syntaxe longue ou une unité de syntaxe que vous pouvez utiliser à plusieurs emplacements au sein d'une instruction. Tous les emplacements dans lesquels le bloc de syntaxe peut être utilisé sont signalés par une étiquette encadrée de chevrons : \<label>.<br /><br /> Un jeu est une collection d’expressions, par exemple \<grouping set>, tandis qu’une liste est une collection de jeux, par exemple \<composite element list>.|  
  
## <a name="multipart-names"></a>Noms à plusieurs composantes  
Sauf indication contraire, toutes les références [!INCLUDE[tsql](../../includes/tsql-md.md)] au nom d'un objet de base de données peuvent prendre la forme d'un nom à quatre composantes, comme suit :  
  
_server\_name_.[_database\_name_].[_schema\_name_]._object\_name_  
  
| _database\_name_.[_schema\_name_]._object\_name_  
 
| _schema\_name_._object\_name_  
  
| _object\_name_  
  
_server\_name_  
Spécifie un nom de serveur lié ou un nom de serveur distant.  
  
_database\_name_  
Spécifie le nom d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lorsque l'objet réside dans une instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quand l’objet se trouve dans un serveur lié, *database_name* spécifie un catalogue OLE DB.  
  
_schema\_name_  
Spécifie le nom du schéma contenant l'objet si celui-ci réside dans une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lorsque l’objet se trouve dans un serveur lié, *schema_name* spécifie un nom de schéma OLE DB.  
  
_object\_name_  
Fait référence au nom de l'objet.  
  
Lorsque vous faites référence à un objet en particulier, il n'est pas toujours nécessaire de spécifier le serveur, la base de données et le schéma pour que le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] l'identifie. Cependant, si l’objet est introuvable, un message d’erreur est retourné.  
  
> [!NOTE]  
> Pour éviter des erreurs relatives à la résolution du nom, nous vous recommandons de spécifier le nom du schéma chaque fois que vous indiquez un objet correspondant.  
  
Pour omettre des nœuds intermédiaires, utilisez des points pour indiquer ces emplacements. Le tableau suivant répertorie les formats valides des noms d'objets.  
  
|Format de la référence d'objet|Description|  
|-----------------------------|-----------------|  
|_server_._database_._schema_._object_|Nom à quatre composantes.|  
|_server_._database_.._object_|Le nom du schéma est omis.|  
|_server_.._schema_._object_|Le nom de la base de données est omis.|  
|_server_..._object_|Les noms de la base de données et du schéma sont omis.|  
|_database_._schema_._object_|Le nom du serveur est omis.|  
|_database_.._object_|Les noms du serveur et du schéma sont omis.|  
|_schema_._object_|Les noms du serveur et de la base de données sont omis.|  
|_object_|Les noms du serveur, de la base de données et du schéma sont omis.|  
  
## <a name="code-example-conventions"></a>Conventions des exemples de code  
Sauf indication contraire, les exemples fournis dans le Guide de référence [!INCLUDE[tsql](../../includes/tsql-md.md)] ont été testés à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et de ses paramètres par défaut pour les options suivantes :  
  
-   ANSI_NULLS  
-   ANSI_NULL_DFLT_ON  
-   ANSI_PADDING  
-   ANSI_WARNINGS  
-   CONCAT_NULL_YIELDS_NULL  
-   QUOTED_IDENTIFIER  
  
La plupart des exemples de code figurant dans le Guide de référence [!INCLUDE[tsql](../../includes/tsql-md.md)] ont été testés sur des serveurs prenant en charge un ordre de tri respectant la casse. Généralement, les serveurs de test ont exécuté la page de codes ANSI/ISO 1252.  
  
De nombreux exemples de code font précéder les constantes de chaînes de caractères Unicode de la lettre **N**. Sans le préfixe **N**, la chaîne est convertie en page de codes par défaut de la base de données. Cette page risque de ne pas reconnaître certains caractères.  
  
## <a name="applies-to-references"></a>Informations de référence de type « S'applique à »  
Les informations de référence sur [!INCLUDE[tsql](../../includes/tsql-md.md)] comprennent des articles sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)].   

En haut de chaque article se trouve une section indiquant les produits qui prennent en charge le sujet de l’article. Si un produit est omis, alors la fonctionnalité décrite par l’article n’est pas disponible dans ce produit. Par exemple, les groupes de disponibilité ont été introduits dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. L’article **CREATE AVAILABILITY GROUP** indique qu’il s’applique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures) car il ne s’applique pas à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] ou [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
Dans certains cas, le sujet général de l’article peut être utilisé dans un produit, mais tous les arguments ne sont pas pris en charge. Par exemple, les utilisateurs de base de données autonome ont été introduits dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. Utilisez l’instruction **CREATE USER** dans tous les produits [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mais la syntaxe **WITH PASSWORD** ne peut pas être utilisée avec les versions plus anciennes. Des sections **S’applique à** supplémentaires sont insérées dans les descriptions des arguments appropriés dans le corps de l’article.  
  
## <a name="see-also"></a>Voir aussi  
[Référence Transact-SQL &#40;moteur de base de données&#41;](../language-reference.md)    
[Mots clés réservés &#40;Transact-SQL&#41;](../../t-sql/language-elements/reserved-keywords-transact-sql.md)      
[Problèmes de conception de Transact-SQL](/previous-versions/visualstudio/visual-studio-2010/dd193411(v=vs.100))    
[Problèmes de nommage de Transact-SQL](/previous-versions/visualstudio/visual-studio-2010/dd193246(v=vs.100))        
[Problèmes de performances de Transact-SQL](/previous-versions/visualstudio/visual-studio-2010/dd172117(v=vs.100))