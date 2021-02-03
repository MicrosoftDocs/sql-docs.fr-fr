---
description: CREATE SEQUENCE (Transact-SQL)
title: CREATE SEQUENCE (Transact-SQL)
ms.custom: ''
ms.date: 04/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SEQUENCE
- CREATE SEQUENCE
- SEQUENCE_TSQL
- CREATE_SEQUENCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE SEQUENCE statement
- sequence number object, creating
- sequence object
- number, sequence
ms.assetid: 419f907b-8a72-4d6c-80cb-301df44c24c1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0fb687f42e65bb1f1ec74046927282dc90563ccd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188498"
---
# <a name="create-sequence-transact-sql"></a>CREATE SEQUENCE (Transact-SQL)

[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Crée un objet séquence et spécifie ses propriétés. Une séquence est un objet lié par schéma défini par l'utilisateur qui génère une séquence de valeurs numériques d'après la spécification avec laquelle la séquence a été créée. La séquence de valeurs numériques est générée dans un ordre croissant ou décroissant à un intervalle défini et peut être configurée pour redémarrer (cycle) lorsque épuisée. Les séquences, contrairement aux colonnes d'identité, ne sont pas associées aux tables spécifiques. Les applications font référence à un objet séquence pour extraire sa valeur suivante. La relation entre les séquences et les tables est contrôlée par l'application. Les applications utilisateur peuvent référencer un objet séquence et coordonner les valeurs sur plusieurs lignes et tables.  
  
 Contrairement aux valeurs de colonnes d’identité générées lors de l’insertion de lignes, une application peut obtenir le numéro séquentiel suivant sans insérer la ligne en appelant la [fonction NEXT VALUE FOR](../../t-sql/functions/next-value-for-transact-sql.md). Utilisez [sp_sequence_get_range](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md) pour obtenir plusieurs numéros séquentiels à la fois.  
  
 Pour obtenir des informations et des scénarios qui utilisent à la fois **CREATE SEQUENCE** et la fonction **NEXT VALUE FOR** , consultez [Numéros de séquence](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE SEQUENCE [schema_name . ] sequence_name  
    [ AS [ built_in_integer_type | user-defined_integer_type ] ]  
    [ START WITH <constant> ]  
    [ INCREMENT BY <constant> ]  
    [ { MINVALUE [ <constant> ] } | { NO MINVALUE } ]  
    [ { MAXVALUE [ <constant> ] } | { NO MAXVALUE } ]  
    [ CYCLE | { NO CYCLE } ]  
    [ { CACHE [ <constant> ] } | { NO CACHE } ]  
    [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*sequence_name*  
Spécifie le nom unique sous lequel la séquence est connue dans la base de données. Le type est **sysname**.  
  
[ built_in_integer_type | user-defined_integer_type  
Une séquence peut être définie comme tout type entier. Les types suivants sont autorisés.  
  
-   **tinyint** : plage comprise entre 0 et 255  
-   **smallint** : plage comprise entre -32 768 et 32 767  
-   **int** : plage comprise entre -2 147 483 648 et 2 147 483 647  
-   **bigint** : plage comprise entre -9 223 372 036 854 775,808 et 9 223 372 036 854 775 807  
-   **décimal** et **numérique** avec une échelle de 0.  
-   Tout type de données défini par l'utilisateur (type d'alias) basé sur l'un des types autorisés.  
  
Si aucun type de données n’est fourni, le type de données **bigint** est utilisé comme valeur par défaut.  
  
START WITH \<constant>  
Première valeur retournée par l'objet séquence. La valeur **START** doit être une valeur inférieure ou égale à la valeur maximale et supérieure ou égale à la valeur minimale de l’objet séquence. La valeur de début par défaut d'un nouvel objet séquence correspond à la valeur minimale pour un objet séquence croissant et à la valeur maximale pour un objet séquence décroissant.  
  
INCREMENT BY \<constant>  
Valeur utilisée pour incrémenter (ou décrémenter si la valeur est négative) la valeur de l’objet séquence pour chaque appel à la fonction **NEXT VALUE FOR**. Si l'incrément est une valeur négative, l'objet séquence décroît ; sinon, il croît. L'incrément ne peut pas avoir la valeur 0. L'incrément par défaut pour un nouvel objet séquence est 1.  
  
[ MINVALUE \<constant> | **NO MINVALUE** ]  
Spécifie les limites de l'objet séquence. La valeur minimale par défaut d'un nouvel objet séquence correspond à la valeur minimale du type de données de l'objet séquence. Il s’agit de zéro pour le type de données **tinyint** et d’un nombre négatif pour tous les autres types de données.  
  
[ MAXVALUE \<constant> | **NO MAXVALUE**  
Spécifie les limites de l'objet séquence. La valeur maximale par défaut d'un nouvel objet séquence correspond à la valeur maximale du type de données de l'objet séquence.  
  
[ CYCLE | **NO CYCLE** ]  
Propriété qui spécifie si l'objet séquence doit redémarrer de la valeur minimale (ou maximale pour les objets de séquence décroissante) ou générer une exception lorsque sa valeur minimale ou maximale est dépassée. L'option de cycle par défaut pour les nouveaux objets séquences est NO CYCLE.  
  
> [!NOTE]
> L’exécution d’un cycle de SEQUENCE redémarre à partir de la valeur minimale ou maximale, et non de la valeur de début.  
  
[ **CACHE** [\<constant> ] | NO CACHE ]  
Augmente la performance des applications qui utilisent des objets séquences en réduisant le nombre d'E/S sur le disque requises pour générer des numéros séquentiels. Paramètres par défaut sur CACHE.  
  
Par exemple, si une taille de cache de 50 est choisie, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne conserve pas 50 valeurs individuelles en cache. Il met uniquement en cache la valeur actuelle et le nombre de valeurs restées dans le cache. Cela signifie que la quantité de mémoire requise pour stocker le cache correspond toujours à deux instances du type de données de l'objet séquence.  
  
> [!NOTE]  
> Si l'option de cache est activée sans spécifier une taille du cache, le moteur de base de données en sélectionne une automatiquement. Toutefois, les utilisateurs ne doivent pas compter sur une sélection cohérente. [!INCLUDE[msCoName](../../includes/msconame-md.md)] peut modifier la méthode de calcul de la taille du cache sans préavis.  
  
En cas de création avec l’option **CACHE**, un arrêt inattendu (tel qu’une panne de courant) peut provoquer la perte des numéros séquentiels qui restent dans le cache.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Les numéros séquentiels sont générés à l'extérieur de l'étendue de la transaction actuelle. Ils sont utilisés si la transaction à l'aide du numéro séquentiel est validée ou restaurée. La validation des doublons ne se produit qu’une fois qu’un enregistrement est entièrement rempli. Dans certains cas, il peut arriver que le même numéro soit utilisé pour plusieurs enregistrements durant la création, d’où son identification en tant que doublon. Si cela se produit et que d’autres valeurs de numérotation automatique ont été appliquées aux enregistrements suivants, il peut en résulter un écart entre les valeurs de numérotation automatique et le comportement attendu.
  
### <a name="cache-management"></a>Gestion du cache  
 Pour améliorer les performances, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pré-alloue le nombre de numéros séquentiels spécifié par l’argument **CACHE**.  
  
 Par exemple, une nouvelle séquence est créée avec une valeur de départ de 1 et une taille du cache de 15. Lorsque la première valeur est exigée, les valeurs 1 à 15 sont rendues disponibles à partir de la mémoire. La dernière valeur mise en cache (15) est écrite dans les tables système sur le disque. Lorsque les 15 nombres sont utilisés, la demande suivante (pour le nombre 16) entraînera une nouvelle allocation du cache. La nouvelle et dernière valeur mise en cache (30) sera écrite dans les tables système.  
  
 Si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] est arrêté après que vous avez utilisé 22 numéros, le numéro séquentiel prévu suivant en mémoire (23) est écrit dans les tables système, remplaçant ainsi le nombre précédemment stocké.  
  
 Après le redémarrage de SQL Server, un numéro séquentiel est exigé ; le nombre initial est lu à partir des tables système (23). Le montant de cache de 15 nombres (23-38) est alloué à la mémoire et le numéro de non-cache suivant (39) est écrit dans les tables système.  
  
 Si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] s’arrête de manière anormale suite à un événement tel qu’une panne de courant, la séquence redémarre avec le numéro lu à partir des tables système (39). Tous les numéros séquentiels alloués à la mémoire (mais jamais demandés par un utilisateur ou une application) sont perdus. Cette fonctionnalité peut laisser des espaces vides, mais garantit que la même valeur ne sera jamais émise deux fois pour un objet séquence unique, sauf s’il est défini comme **CYCLE** ou qu’il est redémarré manuellement.  
  
 Le cache est maintenu en mémoire en suivant la valeur actuelle (la dernière valeur émise) et le nombre de valeurs restées dans le cache. Par conséquent, la quantité de mémoire utilisée par le cache correspond toujours à deux instances du type de données de l'objet séquence.  
  
 La définition de l’argument de cache sur **NO CACHE** écrit la valeur de séquence actuelle dans les tables système chaque fois qu’une séquence est utilisée. Cela peut ralentir les performances en augmentant l'accès au disque, mais réduit les risques d'espaces vides non désirés. Des espaces vides peuvent encore se produire si les nombres sont demandés à l’aide des fonctions **NEXT VALUE FOR** ou **sp_sequence_get_range** ; en revanche, les nombres ne sont pas utilisés du tout ou sont utilisés dans des transactions non validées.  
  
 Quand un objet séquence utilise l’option **CACHE**, si vous redémarrez l’objet séquence ou modifiez la valeur **INCREMENT**, **CYCLE**, **MINVALUE**, **MAXVALUE** ou les propriétés de taille du cache, le cache est écrit dans les tables système avant que la modification ait lieu. Puis, le cache est rechargé en commençant par la valeur actuelle (autrement dit, aucun nombre n'est ignoré). Toute modification apportée à la taille du cache prend effet immédiatement.  
  
 **Option CACHE quand les valeurs mises en cache sont disponibles**  
  
 Le processus suivant se produit chaque fois qu’un objet séquence est demandé pour générer la valeur suivante de l’option **CACHE** si des valeurs inutilisées sont disponibles dans le cache interne pour l’objet séquence.  
  
1.  La valeur suivante de l'objet séquence est calculée.  
  
2.  La nouvelle valeur actuelle de l'objet séquence est mise à jour en mémoire.  
  
3.  La valeur calculée est retournée à l'instruction appelante.  
  
**Option CACHE quand le cache est épuisé**  
  
 Le processus suivant se produit chaque fois qu’un objet séquence est demandé pour générer la valeur suivante de l’option **CACHE** si le cache est épuisé :  
  
1.  La valeur suivante de l'objet séquence est calculée.  
  
2.  La dernière valeur du nouveau cache est calculée.  
  
3.  La ligne de table système pour l'objet séquence est verrouillée, et la valeur calculée à l'étape 2 (dernière valeur) est écrite dans la table système. Un xevent de cache épuisé est déclenché pour notifier l'utilisateur de la nouvelle valeur rendue persistante.  
  
**Option NO CACHE**  
  
 Le processus suivant se produit chaque fois qu’un objet séquence est demandé pour générer la valeur suivante de l’option **NON CACHE** :  
  
1.  La valeur suivante de l'objet séquence est calculée.  
  
2.  La nouvelle valeur actuelle de l'objet séquence est écrite dans la table système.  
  
3.  La valeur calculée est retournée à l'instruction appelante.  
  
## <a name="metadata"></a>Métadonnées  
 Pour plus d’informations sur les séquences, interrogez [sys.sequences](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md).  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **CREATE SEQUENCE**, **ALTER** ou **CONTROL** sur le SCHEMA.  
  
-   Les membres des rôles de base de données fixe db_owner et db_ddladmin peuvent créer, modifier et supprimer des objets séquences.  
  
-   Les membres des rôles de base de données fixe db_owner et db_datawriter peuvent mettre à jour des objets séquences en leur faisant générer des nombres.  
  
 L’exemple suivant accorde à l’utilisateur AdventureWorks\Larry l’autorisation de créer des séquences dans le schéma Test.  
  
```sql  
GRANT CREATE SEQUENCE ON SCHEMA::Test TO [AdventureWorks\Larry]  
```  
  
 La propriété d’un objet séquence peut être transférée à l’aide de l’instruction **ALTER AUTHORIZATION**.  
  
 Si une séquence utilise un type de données défini par l'utilisateur, le créateur de la séquence doit avoir l'autorisation REFERENCES sur le type.  
  
### <a name="audit"></a>Audit  
 Pour auditer **CREATE SEQUENCE**, surveillez **SCHEMA_OBJECT_CHANGE_GROUP**.  
  
## <a name="examples"></a>Exemples  
 Pour obtenir des exemples de création de séquences et d’utilisation de la fonction **NEXT VALUE FOR** pour générer des numéros séquentiels, consultez [Numéros séquentiels](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 La plupart des exemples suivants créent des objets séquences dans un schéma nommé Test.  
  
 Pour créer le schéma Test, exécutez l’instruction suivante.  
  
```sql  
CREATE SCHEMA Test ;  
GO  
```  
  
### <a name="a-creating-a-sequence-that-increases-by-1"></a>R. Création d'une séquence qui augmente de 1  
 Dans l’exemple suivant, Thierry crée une séquence nommée CountBy1 qui augmente de une unité chaque fois qu’elle est utilisée.  
  
```sql  
CREATE SEQUENCE Test.CountBy1  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
```  
  
### <a name="b-creating-a-sequence-that-decreases-by-1"></a>B. Création d'une séquence qui décroît de 1  
 L'exemple suivant démarre à 0 et décroît d'une valeur négative chaque fois qu'il est utilisé.  
  
```sql  
CREATE SEQUENCE Test.CountByNeg1  
    START WITH 0  
    INCREMENT BY -1 ;  
GO  
```  
  
### <a name="c-creating-a-sequence-that-increases-by-5"></a>C. Création d'une séquence qui augmente de 5  
 L'exemple suivant crée une séquence qui augmente de 5 chaque fois qu'elle est utilisée.  
  
```sql  
CREATE SEQUENCE Test.CountBy1  
    START WITH 5  
    INCREMENT BY 5 ;  
GO  
```  
  
### <a name="d-creating-a-sequence-that-starts-with-a-designated-number"></a>D. Création d'une séquence qui démarre avec un nombre désigné  
 Après avoir importé une table, Thierry remarque que le numéro d'ID le plus élevé utilisé est 24 328. Thierry a besoin d'une séquence qui générera des nombres qui démarrent à 24 329. Le code suivant crée une séquence qui démarre à 24 329 et est incrémentée de 1.  
  
```sql  
CREATE SEQUENCE Test.ID_Seq  
    START WITH 24329  
    INCREMENT BY 1 ;  
GO  
```  
  
### <a name="e-creating-a-sequence-using-default-values"></a>E. Création d'une séquence à l'aide de valeurs par défaut  
 L'exemple suivant crée une séquence à l'aide des valeurs par défaut.  
  
```sql  
CREATE SEQUENCE Test.TestSequence ;  
```  
  
 Exécutez l'instruction suivante pour consulter les propriétés de la séquence.  
  
```sql  
SELECT * FROM sys.sequences WHERE name = 'TestSequence' ;  
```  
  
 Une liste partielle de la sortie montre les valeurs par défaut.  
  
| Output | Valeur par défaut|  
|-|-|  
|`start_value`|`-9223372036854775808`|  
|`increment`|`1`|  
|`mimimum_value`|`-9223372036854775808`|  
|`maximum_value`|`9223372036854775807`|  
|`is_cycling`|`0`|  
|`is_cached`|`1`|  
|`current_value`|`-9223372036854775808`|  
  
### <a name="f-creating-a-sequence-with-a-specific-data-type"></a>F. Création d'une séquence avec un type de données spécifique

L’exemple suivant crée une séquence à l’aide du type de données **smallint**, avec une plage comprise entre -32 768 et 32 767.  
  
```sql  
CREATE SEQUENCE SmallSeq 
    AS smallint ;  
```  
  
### <a name="g-creating-a-sequence-using-all-arguments"></a>G. Création d'une séquence à l'aide de tous les arguments  
 L’exemple suivant crée une séquence nommée DecSeq à l’aide du type de données **decimal**, avec une plage comprise entre 0 et 255. La séquence démarre à 125 et est incrémentée de 25 chaque fois qu'un nombre est généré. Étant donné que la séquence est configurée pour se répéter lorsque la valeur dépasse la valeur maximale de 200, la séquence redémarre à la valeur minimale de 100.  
  
```sql  
CREATE SEQUENCE Test.DecSeq  
    AS decimal(3,0)   
    START WITH 125  
    INCREMENT BY 25  
    MINVALUE 100  
    MAXVALUE 200  
    CYCLE  
    CACHE 3  
;  
```  
  
 Exécutez l'instruction suivante pour consulter la première valeur ; l'option `START WITH` de 125.  
  
```sql  
SELECT NEXT VALUE FOR Test.DecSeq;  
```  
  
 Exécutez l'instruction trois autres fois pour retourner 150, 175 et 200.  
  
 Exécutez une nouvelle fois l'instruction pour voir comment la valeur de début revient en arrière à l'option `MINVALUE` de 100.  
  
 Exécutez le code suivant pour confirmer la taille du cache et consulter la valeur actuelle.  
  
```sql  
SELECT cache_size, current_value   
FROM sys.sequences  
WHERE name = 'DecSeq' ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [DROP SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-sequence-transact-sql.md)   
 [NEXT VALUE FOR &#40;Transact-SQL&#41;](../../t-sql/functions/next-value-for-transact-sql.md)   
 [Numéros de séquence](../../relational-databases/sequence-numbers/sequence-numbers.md)  
  
  
