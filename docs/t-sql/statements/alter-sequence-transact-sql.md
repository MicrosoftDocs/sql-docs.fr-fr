---
description: ALTER SEQUENCE (Transact-SQL)
title: ALTER SEQUENCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_SEQUENCE_TSQL
- ALTER SEQUENCE
dev_langs:
- TSQL
helpviewer_keywords:
- sequence number object, altering
- ALTER SEQUENCE statement
ms.assetid: decc0760-029e-4baf-96c9-4a64073df1c2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c32f4e9a6a020df9f7be879eb6e4327bbb9d51f2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094673"
---
# <a name="alter-sequence-transact-sql"></a>ALTER SEQUENCE (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Modifie les arguments d'un objet séquence existant. Si la séquence a été créée avec l’option **CACHE**, la modification de la séquence recrée le cache.  
  
 Les objets séquences sont créés à l’aide de l’instruction [CREATE SEQUENCE](../../t-sql/statements/create-sequence-transact-sql.md). Les séquences sont des valeurs entières et peuvent être de tout type de données qui retourne un entier. Le type de données ne peut pas être modifié à l'aide de l'instruction ALTER SEQUENCE. Pour modifier le type de données, supprimez et créez l'objet séquence.  
  
 Une séquence est un objet lié au schéma défini par l'utilisateur qui génère une séquence de valeurs numériques d'après une spécification. De nouvelles valeurs sont générées à partir d'une séquence par appel de la fonction NEXT VALUE FOR. Utilisez **sp_sequence_get_range** pour obtenir plusieurs numéros séquentiels à la fois. Pour obtenir des informations et des scénarios qui utilisent à la fois CREATE SEQUENCE, **sp_sequence_get_range** et la fonction NEXT VALUE FOR, consultez [Numéros séquentiels](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER SEQUENCE [schema_name. ] sequence_name  
    [ RESTART [ WITH <constant> ] ]  
    [ INCREMENT BY <constant> ]  
    [ { MINVALUE <constant> } | { NO MINVALUE } ]  
    [ { MAXVALUE <constant> } | { NO MAXVALUE } ]  
    [ CYCLE | { NO CYCLE } ]  
    [ { CACHE [ <constant> ] } | { NO CACHE } ]  
    [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *sequence_name*  
 Spécifie le nom unique sous lequel la séquence est connue dans la base de données. Le type est **sysname**.  
  
 RESTART [ WITH \<constant> ]  
 Valeur suivante qui sera retournée par l'objet séquence. Si elle est fournie, la valeur RESTART WITH doit être un entier inférieur ou égal à la valeur maximale et supérieur ou égal à la valeur minimale de l'objet séquence. Si la valeur WITH est omise, la numérotation de séquence redémarre selon les options CREATE SEQUENCE d'origine.  
  
 INCREMENT BY \<constant>  
 Valeur utilisée pour incrémenter (ou décrémenter en cas de valeurs négatives) la valeur de base de l’objet séquence pour chaque appel à la fonction NEXT VALUE FOR. Si l'incrément est une valeur négative, l'objet séquence décroît ; sinon, il augmente. L'incrément ne peut pas être égal à 0.  
  
 [ MINVALUE \<constant> | NO MINVALUE ]  
 Spécifie les limites de l'objet séquence. Si NO MINVALUE est spécifié, la valeur minimale possible du type de données de séquence est utilisée.  
  
 [ MAXVALUE \<constant> | NO MAXVALUE  
 Spécifie les limites de l'objet séquence. Si NO MAXVALUE est spécifié, la valeur maximale possible du type de données de séquence est utilisée.  
  
 [ CYCLE | NO CYCLE ]  
 Cette propriété spécifie si l'objet séquence doit redémarrer à partir de la valeur minimale (ou de la valeur maximale pour les objets séquences décroissants) ou lever une exception lorsque sa valeur minimale ou maximale est dépassée.  
  
> [!NOTE]  
>  À l'issue du cycle, la valeur suivante correspond à la valeur minimale ou à la valeur maximale, et non à la valeur START VALUE de la séquence.  
  
 [ CACHE [\<constant> ] | NO CACHE ]  
 Augmente les performances pour les applications qui utilisent des objets séquences en réduisant le nombre d'entrées/sorties requises pour rendre des valeurs générées persistantes dans les tables système.  
  
 Pour plus d’informations sur le comportement du cache, consultez [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md).  
  
## <a name="remarks"></a>Notes  
 Pour plus d’informations sur la façon dont les séquences sont créées et dont le cache de séquence est géré, consultez [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md).  
  
 La valeur MINVALUE pour les séquences croissantes et la valeur MAXVALUE pour les séquences décroissantes ne peuvent pas être modifiées par une valeur qui n'autorise pas la valeur START WITH de la séquence. Pour modifier la valeur MINVALUE d'une séquence croissante par un nombre plus grand que la valeur START WITH ou pour modifier la valeur MAXVALUE d'une séquence décroissante par un nombre plus petit que la valeur START WITH, incluez l'argument RESTART WITH pour redémarrer la séquence à un point de votre choix compris dans la plage minimale et maximale.  
  
## <a name="metadata"></a>Métadonnées  
 Pour plus d’informations sur les séquences, interrogez [sys.sequences](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md).  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Exige l’autorisation **ALTER** sur la séquence ou l’autorisation **ALTER** sur le schéma. Pour accorder l’autorisation **ALTER** sur la séquence, utilisez **ALTER ON OBJECT** dans le format suivant :  
  
```sql  
GRANT ALTER ON OBJECT::Test.TinySeq TO [AdventureWorks\Larry]  
```  
  
 La propriété d’un objet séquence peut être transférée à l’aide de l’instruction **ALTER AUTHORIZATION**.  
  
### <a name="audit"></a>Audit  
 Pour auditer **ALTER SEQUENCE**, surveillez **SCHEMA_OBJECT_CHANGE_GROUP**.  
  
## <a name="examples"></a>Exemples  
 Pour obtenir des exemples de création de séquences et d’utilisation de la fonction **NEXT VALUE FOR** pour générer des numéros séquentiels, consultez [Numéros séquentiels](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
### <a name="a-altering-a-sequence"></a>R. Modification d'une séquence  
 L’exemple suivant crée un schéma nommé Test et une séquence nommée TestSeq à l’aide du type de données **int**, avec une plage comprise entre 100 et 200. La séquence démarre à 125 et est incrémentée de 25 chaque fois qu'un nombre est généré. Étant donné que la séquence est configurée pour se répéter, lorsque la valeur dépasse la valeur maximale de 200, la séquence redémarre à la valeur minimale de 100.  
  
```sql  
CREATE SCHEMA Test ;  
GO  
  
CREATE SEQUENCE Test.TestSeq  
    AS int   
    START WITH 125  
    INCREMENT BY 25  
    MINVALUE 100  
    MAXVALUE 200  
    CYCLE  
    CACHE 3  
;  
GO  
```  
  
 L’exemple suivant modifie la séquence TestSeq pour que la plage soit comprise entre 50 et 200. La séquence redémarre la série de numérotation à 100 et est incrémentée de 50 chaque fois qu'un nombre est généré.  
  
```sql  
ALTER SEQUENCE Test. TestSeq  
    RESTART WITH 100  
    INCREMENT BY 50  
    MINVALUE 50  
    MAXVALUE 200  
    NO CYCLE  
    NO CACHE  
;  
GO  
```  
  
 Étant donné que la séquence ne se répète pas, la fonction **NEXT VALUE FOR** génère une erreur quand la séquence dépasse 200.  
  
### <a name="b-restarting-a-sequence"></a>B. Redémarrage d'une séquence  
 L’exemple suivant crée une séquence nommée CountBy1. La séquence utilise les valeurs par défaut.  
  
```sql  
CREATE SEQUENCE Test.CountBy1 ;  
```  
  
 Pour générer une valeur de séquence, le propriétaire exécute ensuite l'instruction suivante :  
  
```sql  
SELECT NEXT VALUE FOR Test.CountBy1  
```  
  
 La valeur retournée -9 223 372 036 854 775 808 est la plus petite valeur possible pour le type de données **bigint**. Le propriétaire se rend compte qu’il souhaitait que la séquence démarre à 1, mais qu’il n’a pas indiqué la clause **START WITH** au moment de la création de la séquence. Pour corriger cette erreur, le propriétaire exécute l'instruction suivante.  
  
```sql  
ALTER SEQUENCE Test.CountBy1 RESTART WITH 1 ;  
```  
  
 Puis le propriétaire exécute une nouvelle fois l'instruction suivante pour générer un numéro séquentiel.  
  
```sql  
SELECT NEXT VALUE FOR Test.CountBy1;  
```  
  
 Le nombre correspond maintenant à 1, comme attendu.  
  
 La séquence CountBy1 a été créée à l’aide de la valeur par défaut NO CYCLE ; elle cesse donc de fonctionner après avoir généré le nombre 9 223 372 036 854 775 807. Les appels suivants à l'objet séquence retourneront l'erreur 11728. L'instruction suivante modifie l'objet séquence en vue d'une répétition et définit un cache de 20.  
  
```sql  
ALTER SEQUENCE Test.CountBy1  
    CYCLE  
    CACHE 20 ; 
```  
  
 Maintenant lorsque l'objet séquence atteint 9 223 372 036 854 775 807, il entame un cycle de répétition et le nombre suivant à l'issue du cycle correspond à la valeur minimale du type de données, à savoir -9 223 372 036 854 775 808.  
  
 Le propriétaire s’est rendu compte que le type de données **bigint** utilise 8 octets chaque fois qu’il est utilisé. Le type de données **int** qui utilise 4 octets est suffisant. Toutefois, le type de données d'un objet séquence ne peut pas être modifié. Pour passer à un type de données **int**, le propriétaire doit supprimer l’objet séquence et recréer l’objet avec le type de données approprié.  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md)   
 [DROP SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-sequence-transact-sql.md)   
 [NEXT VALUE FOR &#40;Transact-SQL&#41;](../../t-sql/functions/next-value-for-transact-sql.md)   
 [Numéros séquentiels](../../relational-databases/sequence-numbers/sequence-numbers.md)   
 [sp_sequence_get_range &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md)  
  
  
