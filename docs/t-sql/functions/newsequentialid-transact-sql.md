---
description: NEWSEQUENTIALID (Transact-SQL)
title: NEWSEQUENTIALID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- NEWSEQUENTIALID
- NEWSEQUENTIALID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- NEWSEQUENTIALID function
- GUIDs [SQL Server]
ms.assetid: e06d2cab-f1ff-42f1-8550-6aaec57be36f
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 4fe754848be676d40387d4887a5a7e519da697f7
ms.sourcegitcommit: cc23d8646041336d119b74bf239a6ac305ff3d31
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2020
ms.locfileid: "91111055"
---
# <a name="newsequentialid-transact-sql"></a>NEWSEQUENTIALID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crée une valeur GUID supérieure à celle d'un GUID précédemment généré par cette fonction sur un ordinateur donné depuis le démarrage de Windows. Après le redémarrage de Windows, le GUID peut redémarrer à partir d'une plage inférieure, mais est toujours globalement unique. Lorsqu'une colonne GUID est utilisée comme identificateur de ligne, l'utilisation de NEWSEQUENTIALID peut être plus rapide que celle de la fonction NEWID. Cela est dû au fait que la fonction NEWID génère une activité aléatoire et utilise moins de pages de données mises en cache. L'utilisation de NEWSEQUENTIALID vous aide également à remplir complètement les pages de données et d'index.  
  
> [!IMPORTANT]  
>  Si la confidentialité des données pose un problème, n'utilisez pas cette fonction. Il est possible de deviner la valeur du GUID généré suivant, et donc d'accéder aux données qui lui sont associées.  
  
 NEWSEQUENTIALID est un wrapper sur la fonction Windows [UuidCreateSequential](https://go.microsoft.com/fwlink/?LinkId=164027), avec des [octets appliqués aléatoirement](https://blogs.msdn.microsoft.com/dbrowne/2012/07/03/how-to-generate-sequential-guids-for-sql-server-in-net/).
  
> [!WARNING]  
>  La fonction UuidCreateSequential a des dépendances matérielles. Sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], des clusters de valeurs séquentielles peuvent se développer lors du déplacement de bases de données (par exemple, des bases de données autonomes) vers d’autres ordinateurs. Lorsque vous utilisez Always On sur [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)], des clusters de valeurs séquentielles peuvent se développer si la base de données bascule vers un autre ordinateur.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
NEWSEQUENTIALID ( )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]


## <a name="return-type"></a>Type de retour  
 **uniqueidentifier**  
  
## <a name="remarks"></a>Remarques  
 NEWSEQUENTIALID() peut être utilisé uniquement avec des contraintes DEFAULT sur des colonnes de table de type **uniqueidentifier**. Par exemple :  
  
```sql  
CREATE TABLE myTable (ColumnA uniqueidentifier DEFAULT NEWSEQUENTIALID());   
```  
  
 Lorsque vous utilisez la fonction NEWSEQUENTIALID() dans des expressions DEFAULT, vous ne pouvez pas la combiner avec d'autres opérateurs scalaires. Par exemple, vous ne pouvez pas exécuter ce qui suit :  
  
```sql 
CREATE TABLE myTable (ColumnA uniqueidentifier DEFAULT dbo.myfunction(NEWSEQUENTIALID()));  
```  
  
 Dans l'exemple précédent, `myfunction()` est une fonction scalaire définie par l'utilisateur qui accepte et renvoie une valeur de type `uniqueidentifier`.  
  
 La fonction NEWSEQUENTIALID ne peut pas être référencée dans les requêtes.  
  
 Vous pouvez utiliser NEWSEQUENTIALID pour générer des GUID afin de réduire les fractionnements de pages et l'E/S aléatoire au niveau feuille des index.  
  
 Chaque GUID généré à l'aide de NEWSEQUENTIALID est unique sur cet ordinateur. Les GUID générés à l'aide de la fonction NEWSEQUENTIALID sont uniques sur plusieurs ordinateurs seulement si l'ordinateur source possède une carte réseau.  
  
## <a name="see-also"></a>Voir aussi  
 [NEWID &#40;Transact-SQL&#41;](../../t-sql/functions/newid-transact-sql.md)   
 [Opérateurs de comparaison &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md)  
  
  
