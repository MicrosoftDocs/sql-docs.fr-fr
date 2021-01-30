---
description: sp_server_info (Transact-SQL)
title: sp_server_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_server_info
- sp_server_info_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_server_info
ms.assetid: 2dc2c262-3cfa-4a84-8127-3632ba583543
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ec0ddfe23b45f0ff0cca4db65b616f99db080c69
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190790"
---
# <a name="sp_server_info-transact-sql"></a>sp_server_info (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne une liste de noms d'attributs et de valeurs correspondantes pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la passerelle de base de données ou la source de données sous-jacente.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_server_info [[@attribute_id = ] 'attribute_id']  
```  
  
## <a name="arguments"></a>Arguments  
`[ @attribute_id = ] 'attribute_id'` Est l’ID d’entier de l’attribut. *attribute_id* est de **type int**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**ATTRIBUTE_ID**|**int**|Numéro d'identification de l'attribut.|  
|**ATTRIBUTE_NAME**|**varchar (** 60 **)**|Nom de l'attribut.|  
|**ATTRIBUTE_VALUE**|**varchar (** 255 **)**|Valeur actuelle de l'attribut.|  
  
 Le tableau suivant décrit ces attributs. [!INCLUDE[msCoName](../../includes/msconame-md.md)] Les bibliothèques clientes ODBC utilisent actuellement les attributs **1**, **2**, **18**, **22** et **500** au moment de la connexion.  
  
|ATTRIBUTE_ID|Description de ATTRIBUTE_NAME|ATTRIBUTE_VALUE|  
|-------------------|---------------------------------|----------------------|  
|**1**|DBMS_NAME|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|**2**|DBMS_VER|Microsoft [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] *xxxx*  -  *x. xx. xxxx*<br/><br> Par exemple : `Microsoft SQL Server 2017 - 14.0.3257.3`|  
|**10**|OWNER_TERM|propriétaire|  
|**11**|TABLE_TERM|table|  
|**12**|MAX_OWNER_NAME_LENGTH|128|  
|**13**|TABLE_LENGTH<br /><br /> Spécifie le nombre maximum de caractères pour un nom de table.|128|  
|**14**|MAX_QUAL_LENGTH<br /><br /> Spécifie la longueur maximale du nom d'un qualificateur de table. C'est la première partie d'un nom de table en trois parties.|128|  
|**15**|COLUMN_LENGTH<br /><br /> Spécifie le nombre maximal de caractères pour un nom de colonne.|128|  
|**16**|IDENTIFIER_CASE<br /><br /> Spécifie les noms définis par l'utilisateur (noms de table, noms de colonne, noms de procédure stockée) dans la base de données (cas des objets des catalogues système).|SENSITIVE|  
|**17**|TX_ISOLATION<br /><br /> Spécifie le niveau initial d'isolement de la transaction assuré par le serveur, ce qui correspond à un niveau d'isolement défini dans SQL-92.|2|  
|**19**|COLLATION_SEQ<br /><br /> Spécifie l'ordre du jeu de caractères de ce serveur.|charset=iso_1 sort_order=dictionary_iso charset_num=1 sort_order_num=51|  
|**19**|SAVEPOINT_SUPPORT<br /><br /> Spécifie si le SGBD sous-jacent prend en charge les points d'enregistrement nommés.|O|  
|**20**|MULTI_RESULT_SETS<br /><br /> Spécifie si la base de données sous-jacente ou la passerelle elle-même gère les jeux de résultats multiples (plusieurs instructions peuvent être envoyées par l'intermédiaire de la passerelle et plusieurs jeux de résultats peuvent être retournés au client).|O|  
|**22**|ACCESSIBLE_TABLES<br /><br /> Spécifie si, dans **sp_tables**, la passerelle retourne uniquement les tables, les vues, etc., accessibles par l’utilisateur actuel (autrement dit, l’utilisateur qui a au moins des autorisations SELECT pour la table).|O|  
|**100**|USERID_LENGTH<br /><br /> Spécifie le nombre maximal de caractères pour un nom d'utilisateur.|128|  
|**101**|QUALIFIER_TERM<br /><br /> Spécifie le terme utilisé par le fournisseur du SGBD pour désigner un qualificateur de table (première partie d'un nom en trois parties).|database|  
|**102**|NAMED_TRANSACTIONS<br /><br /> Spécifie si le SGBD sous-jacent prend en charge les transactions nommées.|O|  
|**103**|SPROC_AS_LANGUAGE<br /><br /> Spécifie si les procédures stockées peuvent être exécutées comme événements de langage.|O|  
|**104**|ACCESSIBLE_SPROC<br /><br /> Spécifie si, dans **sp_stored_procedures**, la passerelle retourne uniquement les procédures stockées qui sont exécutables par l’utilisateur actuel.|O|  
|**105**|MAX_INDEX_COLS<br /><br /> Spécifie le nombre maximal de colonnes dans un index pour le SGBD.|16|  
|**106**|RENAME_TABLE<br /><br /> Spécifie si les tables peuvent être renommées.|O|  
|**107**|RENAME_COLUMN<br /><br /> Spécifie si les colonnes peuvent être renommées.|O|  
|**108**|DROP_COLUMN<br /><br /> Spécifie si des colonnes peuvent être supprimées.|O|  
|**109**|INCREASE_COLUMN_LENGTH<br /><br /> Spécifie s'il est possible d'augmenter la taille des colonnes.|O|  
|**110**|DDL_IN_TRANSACTION<br /><br /> Spécifie si des instructions DDL peuvent apparaître dans des transactions.|O|  
|**111**|DESCENDING_INDEXES<br /><br /> Spécifie si des index décroissants sont gérés.|O|  
|**112**|SP_RENAME<br /><br /> Spécifie s'il est possible de renommer une procédure stockée.|O|  
|**113**|REMOTE_SPROC<br /><br /> Spécifie si les procédures stockées peuvent être exécutées par des fonctions de procédures stockées distantes figurant dans la bibliothèque de bases de données.|O|  
|**500**|SYS_SPROC_VERSION<br /><br /> Spécifie la version actuelle des procédures stockées de catalogue.|Numéro de version actuelle|  
  
## <a name="remarks"></a>Notes  
 **sp_server_info** retourne un sous-ensemble des informations fournies par **SQLGetInfo** dans ODBC.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de catalogue &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
