---
description: DECLARE CURSOR (Transact-SQL)
title: DECLARE CURSOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DECLARE_CURSOR_TSQL
- CURSOR_TSQL
- DECLARE CURSOR
- CURSOR
dev_langs:
- TSQL
helpviewer_keywords:
- DECLARE CURSOR statement
- cursors [SQL Server], attributes
- local cursors [SQL Server]
- nesting cursors
- Transact-SQL cursors, attributes
- global cursors [SQL Server]
ms.assetid: 5a3a27aa-03e8-4c98-a27e-809282379b21
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5fcd8a9ca3239d368b25f7444c11c311e4b1d306
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102479"
---
# <a name="declare-cursor-transact-sql"></a>DECLARE CURSOR (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Définit les attributs d'un curseur [!INCLUDE[tsql](../../includes/tsql-md.md)], notamment s'il permet ou non le défilement, et la requête utilisée pour créer le jeu de résultats sur lequel le curseur opère. La syntaxe d’une instruction `DECLARE CURSOR` peut utiliser à la fois la syntaxe ISO et un jeu d’extensions [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ISO Syntax  
DECLARE cursor_name [ INSENSITIVE ] [ SCROLL ] CURSOR   
     FOR select_statement   
     [ FOR { READ ONLY | UPDATE [ OF column_name [ ,...n ] ] } ]  
[;]  
Transact-SQL Extended Syntax  
DECLARE cursor_name CURSOR [ LOCAL | GLOBAL ]   
     [ FORWARD_ONLY | SCROLL ]   
     [ STATIC | KEYSET | DYNAMIC | FAST_FORWARD ]   
     [ READ_ONLY | SCROLL_LOCKS | OPTIMISTIC ]   
     [ TYPE_WARNING ]   
     FOR select_statement   
     [ FOR UPDATE [ OF column_name [ ,...n ] ] ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *cursor_name*  
 Nom du curseur côté serveur [!INCLUDE[tsql](../../includes/tsql-md.md)] défini. *cursor_name* doit suivre les règles applicables aux identificateurs.  
  
 INSENSITIVE  
 Définit un curseur qui effectue une copie temporaire des données qu'il doit utiliser. Toutes les réponses aux requêtes destinées au curseur sont effectuées à partir de cette table temporaire dans **tempdb**. Par conséquent, les modifications apportées aux tables de base de données ne sont pas répercutées dans les données retournées par les extractions de ce curseur, et ce dernier n’accepte pas de modifications. Si, lors de l’utilisation de la syntaxe ISO, l’option `INSENSITIVE` est omise, les suppressions et les mises à jour validées effectuées (par n’importe quel utilisateur) dans les tables sous-jacentes sont reflétées dans les extractions ultérieures.  
  
 SCROLL  
 Spécifie que toutes les fonctions d’extraction (`FIRST`, `LAST`, `PRIOR`, `NEXT`, `RELATIVE`, `ABSOLUTE`) sont disponibles. Si `SCROLL` n’est pas spécifié dans une instruction `DECLARE CURSOR` ISO, seule l’option d’extraction `NEXT` est prise en charge. `SCROLL` ne peut pas être spécifié si `FAST_FORWARD` est également spécifié. Si `SCROLL` n’est pas spécifié, seule l’option d’extraction `NEXT` est disponible et le curseur devient `FORWARD_ONLY`.
  
 *select_statement*  
 Instruction `SELECT` standard qui définit le jeu de résultats du curseur. Les mots clés `FOR BROWSE` et `INTO` ne sont pas autorisés dans l’instruction *select_statement* d’une déclaration de curseur.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convertit implicitement le curseur en un autre type si les clauses de l’instruction *select_statement* sont incompatibles avec la fonctionnalité du type de curseur demandé.  
  
 READ ONLY  
 Interdit les mises à jour par l'intermédiaire de ce curseur. Le curseur ne peut pas être référencé dans une clause `WHERE CURRENT OF` d’une instruction `UPDATE` ou `DELETE`. Cette option remplace la possibilité par défaut de mise à jour d'un curseur.  
  
 UPDATE [OF *column_name* [**,**...*n*]]  
 Définit les colonnes qui peuvent être mises à jour par le curseur. Si OF <column_name> [, <... n>] est spécifié, seules les colonnes listées autorisent les modifications. Si vous spécifiez `UPDATE` sans liste de colonnes, toutes les colonnes peuvent être mises à jour.  
  
*cursor_name*  
Nom du curseur côté serveur [!INCLUDE[tsql](../../includes/tsql-md.md)] défini. *cursor_name* doit suivre les règles applicables aux identificateurs.  
  
LOCAL  
Spécifie que l'étendue du curseur est locale pour le traitement d'instructions, la procédure stockée ou le déclencheur dans lequel il a été créé. Le nom du curseur n’est valide que dans cette étendue. Le curseur peut être référencé par des variables de curseur locales dans le traitement, la procédure stockée ou le déclencheur, ou bien par un paramètre `OUTPUT` d’une procédure stockée. Un paramètre `OUTPUT` est utilisé pour passer en retour le curseur local au traitement par lot, à la procédure stockée ou au déclencheur appelant, qui peut affecter le paramètre à une variable de curseur pour référencer le curseur à la fin de la procédure stockée. Le curseur est désalloué implicitement à la fin du traitement par lot, de la procédure stockée ou du déclencheur, à moins d’avoir été passé en retour dans un paramètre `OUTPUT`. S’il a été passé en retour dans un paramètre `OUTPUT`, le curseur est désalloué quand la dernière variable qui y fait référence est désallouée, ou quand il est en dehors de l’étendue.  
  
GLOBAL  
Spécifie que l'étendue du curseur est globale pour la connexion. Toute procédure stockée ou tout lot exécuté par la connexion peut faire référence au nom du curseur. Le curseur n'est désalloué implicitement qu'au moment de la déconnexion.  
  
> [!NOTE]  
>  Si ni `GLOBAL` ni `LOCAL` ne sont spécifiés, la valeur par défaut est contrôlée par la valeur de l’option de base de données **default to local cursor**.  
  
FORWARD_ONLY  
Spécifie que le curseur peut seulement aller vers l’avant et défiler de la première à la dernière ligne. `FETCH NEXT` est la seule fonction d’extraction prise en charge. Toutes les instructions d’insertion, de mise à jour et de suppression émises par l’utilisateur actuel ou validées par d’autres utilisateurs et ayant une incidence sur les lignes du jeu de résultats sont visibles au fur et à mesure que les lignes sont extraites. Cependant, étant donné que le curseur ne permet pas le défilement arrière, les modifications apportées aux lignes de la base de données après l’extraction d’une ligne ne sont pas visibles par le biais du curseur. Les curseurs avant uniquement sont dynamiques par défaut, ce qui signifie que toutes les modifications sont détectées lors du traitement de la ligne active. Cela accélère l’ouverture du curseur et permet au jeu de résultats d’afficher les mises à jour apportées aux tables sous-jacentes. Bien que les curseurs avant uniquement ne prennent pas en charge le défilement vers l’arrière, les applications peuvent retourner au début du jeu de résultats en fermant et en rouvrant le curseur. Si vous spécifiez `FORWARD_ONLY` sans les mots clés `STATIC`, `KEYSET` ou `DYNAMIC`, le curseur fonctionne comme un curseur dynamique. Quand ni `FORWARD_ONLY` ni `SCROLL` ne sont spécifiés, `FORWARD_ONLY` est la valeur par défaut, sauf si les mots clés `STATIC`, `KEYSET` ou `DYNAMIC` sont spécifiés. Les curseurs `STATIC`, `KEYSET` et `DYNAMIC` sont par défaut de type `SCROLL`. Contrairement aux API de base de données comme ODBC et ADO, `FORWARD_ONLY` est pris en charge avec les curseurs `STATIC`, `KEYSET` et `DYNAMIC` de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
   
 STATIC  
Spécifie que le curseur affiche toujours le jeu de résultats tel qu’il était quand le curseur a été ouvert pour la première fois, et effectue une copie temporaire des données à utiliser par le curseur. Toutes les requêtes au curseur reçoivent une réponse à partir de cette table temporaire dans **tempdb**. Par conséquent, les insertions, mises à jour et suppressions effectuées sur des tables de base ne sont pas répercutées dans les données retournées par les extractions de ce curseur, et celui-ci ne détecte pas les modifications apportées à l’appartenance, à l’ordre ou aux valeurs du jeu de résultats après l’ouverture du curseur. Les curseurs statiques peuvent détecter leurs propres mises à jour, suppressions et insertions, bien qu’ils ne soient pas obligés de le faire. Par exemple, supposez qu’un curseur statique extrait une ligne et qu’une autre application met ensuite à jour cette ligne. Si l’application réextrait la ligne à partir du curseur statique, les valeurs qu’elle voit sont inchangées, malgré les modifications apportées par l’autre application. Tous les types de défilement sont pris en charge. 
  
KEYSET  
Spécifie que l'appartenance au curseur et l'ordre des lignes sont fixés lors de l'ouverture du curseur. L’ensemble des clés qui identifient de manière unique les lignes est créé dans une table **tempdb** nommée **keyset**. Ce curseur fournit une fonctionnalité entre un curseur statique et un curseur dynamique grâce à sa capacité à détecter les modifications. Comme un curseur statique, il ne détecte pas toujours les modifications apportées à l’appartenance et à l’ordre du jeu de résultats. Comme un curseur dynamique, il détecte les modifications apportées aux valeurs des lignes dans le jeu de résultats. Les curseurs de jeux de clés sont gérés par un ensemble d’identificateurs uniques (clés) appelé jeu de clés. Les clés sont créées à partir d'un ensemble de colonnes qui identifient uniquement les lignes de l'ensemble de résultats. Le jeu de clés est l’ensemble des valeurs de clés de toutes les lignes retournées par l’instruction de requête. Avec les curseurs de jeux de clés, une clé est générée et enregistrée pour chaque ligne du curseur et stockée sur la station de travail cliente ou sur le serveur. Quand vous accédez à chaque ligne, la clé stockée est utilisée pour extraire les valeurs de données actuelles de la source de données. Dans un curseur de jeux de clés, l’appartenance au jeu de résultats est figée quand le jeu de clés est plein. Par la suite, les ajouts ou mises à jour affectant l’appartenance ne font partie du jeu de résultats qu’une fois celui-ci rouvert. Les modifications apportées aux valeurs de données (par le propriétaire du jeu de clés ou par d’autres processus) sont visibles à mesure que l’utilisateur fait défiler le jeu de résultats :
-  Si une ligne est supprimée, une tentative d’extraction de la ligne retourne un `@@FETCH_STATUS` de -2, car la ligne supprimée apparaît sous forme d’espace dans le jeu de résultats. La clé pour la ligne existe dans le jeu de clés, mais la ligne n’existe plus dans le jeu de résultats. 
-  Les insertions effectuées en dehors du curseur (par d’autres processus) sont visibles uniquement si le curseur est fermé puis rouvert. Les insertions effectuées à partir de l’intérieur du curseur sont visibles à la fin du jeu de résultats.
-  Les mises à jour de valeurs de clés effectuées hors du curseur sont semblables à la suppression de l'ancienne ligne suivie de l'insertion d'une nouvelle. La ligne avec les nouvelles valeurs n’est pas visible et les tentatives d’extraction avec les anciennes valeurs retournent la valeur -2 pour `@@FETCH_STATUS`. Les nouvelles valeurs sont visibles si la mise à jour est effectuée via le curseur en spécifiant la clause `WHERE CURRENT OF`. 

> [!NOTE]  
> Si la requête fait référence à au moins une table sans index unique, le curseur de jeu de clés est converti en curseur statique.  
  
DYNAMIC  
Définit un curseur qui reflète toutes les modifications de données apportées aux lignes dans son jeu de résultats à mesure que vous faites défiler le curseur et que vous extrayez un nouvel enregistrement, indépendamment du fait que les modifications se produisent à partir de l’intérieur du curseur ou qu’elles soient effectuées par d’autres utilisateurs en dehors du curseur. Par conséquent, toutes les instructions d’insertion, de mise à jour et de suppression émises par l’ensemble des utilisateurs sont visibles à l’aide du curseur. Les valeurs des données, l'ordre et l'appartenance aux lignes peuvent changer à chaque extraction. L’option d’extraction `ABSOLUTE` n’est pas prise en charge par les curseurs dynamiques. Les mises à jour effectuées en dehors du curseur ne sont pas visibles tant qu’elles n’ont pas été validées (sauf si le niveau d’isolation de la transaction du curseur a la valeur `UNCOMMITTED`). Par exemple, supposez qu’un curseur dynamique extrait deux lignes et qu’une autre application met ensuite à jour l’une de ces lignes et supprime l’autre. Si le curseur dynamique extrait alors ces lignes, il ne trouvera pas la ligne supprimée, mais il affichera les nouvelles valeurs pour la ligne mise à jour. 
  
FAST_FORWARD  
Spécifie un curseur `FORWARD_ONLY`, `READ_ONLY` pour lequel les optimisations de performances sont activées. `FAST_FORWARD` ne peut pas être spécifié si `SCROLL` ou `FOR_UPDATE` est également spécifié. Ce type de curseur n’autorise pas les modifications de données à partir de l’intérieur du curseur.  
  
> [!NOTE]  
> `FAST_FORWARD` et `FORWARD_ONLY` peuvent être utilisés dans une même instruction `DECLARE CURSOR`.  
  
READ_ONLY  
Interdit les mises à jour par l'intermédiaire de ce curseur. Le curseur ne peut pas être référencé dans une clause `WHERE CURRENT OF` d’une instruction `UPDATE` ou `DELETE`. Cette option remplace la possibilité par défaut de mise à jour d'un curseur.  
  
SCROLL_LOCKS  
Spécifie que la réussite des mises à jour ou des suppressions positionnées effectuées via le curseur est garantie. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verrouille les lignes lorsqu’elles sont lues dans le curseur pour vérifier leur disponibilité lors des modifications ultérieures. `SCROLL_LOCKS` ne peut pas être spécifié si `FAST_FORWARD` ou `STATIC` est également spécifié.  
  
OPTIMISTIC  
Spécifie que les mises à jour ou les suppressions positionnées effectuées via le curseur ne réussissent pas si la ligne a été mise à jour depuis qu'elle a été lue dans le curseur. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne verrouille pas les lignes à mesure qu'elles sont lues dans le curseur. Il utilise à la place des comparaisons des valeurs de la colonne **timestamp**, ou une valeur de somme de contrôle si la table n’a pas de colonne **timestamp**, pour déterminer si la ligne a été modifiée après avoir été lue dans le curseur. Si la ligne a été modifiée, la mise à jour ou la suppression positionnée que vous avez tentée échoue. `OPTIMISTIC` ne peut pas être spécifié si `FAST_FORWARD` est également spécifié.  
  
 TYPE_WARNING  
 Indique qu'un message d'avertissement est envoyé au client lorsque le curseur est converti implicitement du type demandé vers un autre type.  
  
 *select_statement*  
 Instruction SELECT standard qui définit le jeu de résultats du curseur. Les mots clés `COMPUTE`, `COMPUTE BY`, `FOR BROWSE` et `INTO` ne sont pas autorisés dans l’instruction *select_statement* d’une déclaration de curseur.  
  
> [!NOTE]  
> Vous pouvez utiliser un indicateur de requête dans une déclaration de curseur ; cependant, si vous utilisez également la clause `FOR UPDATE OF`, spécifiez `OPTION (<query_hint>)` après `FOR UPDATE OF`.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convertit implicitement le curseur en un autre type si les clauses de l’instruction *select_statement* sont incompatibles avec la fonctionnalité du type de curseur demandé. Pour plus d'informations, consultez la rubrique Conversions implicites de curseur.  
  
FOR UPDATE [OF *column_name* [**,**...*n*]]  
Définit les colonnes qui peuvent être mises à jour par le curseur. Si `OF <column_name> [, <... n>]` est fourni, seules les colonnes listées permettent les modifications. Si vous spécifiez `UPDATE`sans liste de colonnes, toutes les colonnes peuvent être mises à jour, sauf si l’option de concurrence `READ_ONLY` a été spécifiée.  
  
## <a name="remarks"></a>Notes  
`DECLARE CURSOR` définit les attributs d’un curseur de serveur [!INCLUDE[tsql](../../includes/tsql-md.md)], notamment son comportement quant au défilement et la requête utilisée pour créer le jeu de résultats sur lequel le curseur opère. L’instruction `OPEN` remplit le jeu de résultats, et l’instruction `FETCH` retourne une ligne du jeu de résultats. L’instruction `CLOSE` libère le jeu de résultats actuel associé au curseur. L’instruction `DEALLOCATE` libère les ressources utilisées par le curseur.  
  
Le premier format de l’instruction `DECLARE CURSOR` utilise la syntaxe ISO pour déclarer les comportements du curseur. Le second format de `DECLARE CURSOR` utilise les extensions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui permettent de définir les curseurs en utilisant les mêmes types de curseur que ceux des fonctions de curseur API de base de données ODBC ou ADO.  
  
Vous ne pouvez pas utiliser les deux formats simultanément. Si vous spécifiez les mots clés `SCROLL` ou `INSENSITIVE` avant le mot clé `CURSOR`, vous ne pouvez pas insérer de mot clé entre les mots clés `CURSOR` et `FOR <select_statement>`. Si vous spécifiez des mots clés entre les mots clés `CURSOR` et `FOR <select_statement>`, vous ne pouvez pas spécifier `SCROLL` ou `INSENSITIVE` avant le mot clé `CURSOR`.  
  
Si un `DECLARE CURSOR` utilisant la syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] ne spécifie pas `READ_ONLY`, `OPTIMISTIC`, ou `SCROLL_LOCKS`, la valeur par défaut est la suivante :  
  
-   Si l’instruction `SELECT` ne prend pas en charge les mises à jour (autorisations insuffisantes, accès à des tables distantes ne prenant pas en charge les mises à jour, etc.), le curseur est `READ_ONLY`.  
  
-   Les curseurs `STATIC` et `FAST_FORWARD` sont par défaut de type `READ_ONLY`.  
  
-   Les curseurs `DYNAMIC` et `KEYSET` sont par défaut de type `OPTIMISTIC`.  
  
Les noms de curseurs peuvent uniquement être référencés par d'autres instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ils ne peuvent pas être référencés par des fonctions API de base de données. Ainsi, après la déclaration d'un curseur, le nom du curseur ne peut pas être référencé par des fonctions ou méthodes OLE DB, ODBC ou ADO. Les lignes du curseur ne peuvent pas être extraites à l'aide des fonctions ou méthodes d'extraction des interfaces de programmation d'application (API) mais uniquement au moyen d'instructions FETCH [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
Une fois un curseur déclaré, les procédures stockées système suivantes peuvent être utilisées pour déterminer ses caractéristiques.  
  
|Procédures stockées système|Description|  
|------------------------------|-----------------|  
|**sp_cursor_list**|Renvoie une liste des curseurs actuellement visibles par la connexion et leurs attributs.|  
|**sp_describe_cursor**|Décrit les attributs d'un curseur. Indique par exemple s'il s'agit d'un curseur de défilement avant uniquement ou d'un curseur de défilement.|  
|**sp_describe_cursor_columns**|Décrit les attributs des colonnes contenues dans le jeu de résultats du curseur.|  
|**sp_describe_cursor_tables**|Décrit les tables de base auxquelles accède le curseur.|  
  
 Les variables peuvent être utilisées dans l’instruction *select_statement* qui déclare un curseur. Les valeurs de variable de curseur ne changent pas après la déclaration d'un curseur.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations `DECLARE CURSOR` sont octroyées par défaut à tout utilisateur qui a des autorisations `SELECT` sur les vues, les tables et les colonnes utilisées par le curseur.
 
## <a name="limitations-and-restrictions"></a>Limitations et restrictions

Vous ne pouvez pas utiliser de curseurs ou de déclencheurs sur une table avec un index cluster columnstore. Cette restriction ne s’applique pas aux index columnstore non-cluster. En effet, vous pouvez utiliser des curseurs et des déclencheurs sur une table avec un index columnstore non-cluster. 
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-simple-cursor-and-syntax"></a>R. Utilisation d'un curseur et d'une syntaxe simples  

Le jeu de résultats généré à l'ouverture du curseur ci-après contient toutes les lignes et toutes les colonnes de la table. Ce curseur peut être mis à jour, et toutes les mises à jour et les suppressions sont représentées sous la forme d'extractions effectuées sur ce curseur. `FETCH NEXT` est la seule extraction disponible car l'option `SCROLL` n'a pas été spécifiée.  
 
```sql  
DECLARE vend_cursor CURSOR  
    FOR SELECT * FROM Purchasing.Vendor  
OPEN vend_cursor  
FETCH NEXT FROM vend_cursor;  
```  
  
### <a name="b-using-nested-cursors-to-produce-report-output"></a>B. Utilisation de curseurs imbriqués pour la production d'un rapport  
 L'exemple suivant montre comment les curseurs peuvent être imbriqués pour produire des rapports complexes. Le curseur interne est déclaré pour chaque fournisseur.  
  
```sql  
SET NOCOUNT ON;  
  
DECLARE @vendor_id INT, @vendor_name NVARCHAR(50),  
    @message VARCHAR(80), @product NVARCHAR(50);  
  
PRINT '-------- Vendor Products Report --------';  
  
DECLARE vendor_cursor CURSOR FOR   
SELECT VendorID, Name  
FROM Purchasing.Vendor  
WHERE PreferredVendorStatus = 1  
ORDER BY VendorID;  
  
OPEN vendor_cursor  
  
FETCH NEXT FROM vendor_cursor   
INTO @vendor_id, @vendor_name  
  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    PRINT ' '  
    SELECT @message = '----- Products From Vendor: ' +   
        @vendor_name  
  
    PRINT @message  
  
    -- Declare an inner cursor based     
    -- on vendor_id from the outer cursor.  
  
    DECLARE product_cursor CURSOR FOR   
    SELECT v.Name  
    FROM Purchasing.ProductVendor pv, Production.Product v  
    WHERE pv.ProductID = v.ProductID AND  
    pv.VendorID = @vendor_id  -- Variable value from the outer cursor  
  
    OPEN product_cursor  
    FETCH NEXT FROM product_cursor INTO @product  
  
    IF @@FETCH_STATUS <> 0   
        PRINT '         <<None>>'       
  
    WHILE @@FETCH_STATUS = 0  
    BEGIN  
  
        SELECT @message = '         ' + @product  
        PRINT @message  
        FETCH NEXT FROM product_cursor INTO @product  
        END  
  
    CLOSE product_cursor  
    DEALLOCATE product_cursor  
        -- Get the next vendor.  
    FETCH NEXT FROM vendor_cursor   
    INTO @vendor_id, @vendor_name  
END   
CLOSE vendor_cursor;  
DEALLOCATE vendor_cursor;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [@@FETCH_STATUS &#40;Transact-SQL&#41;](../../t-sql/functions/fetch-status-transact-sql.md)   
 [CLOSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/close-transact-sql.md)   
 [Curseurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DEALLOCATE &#40;Transact-SQL&#41;](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [FETCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/fetch-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
