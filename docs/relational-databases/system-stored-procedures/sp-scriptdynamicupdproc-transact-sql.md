---
description: sp_scriptdynamicupdproc (Transact-SQL)
title: sp_scriptdynamicupdproc (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_scriptdynamicupdproc_TSQL
- sp_scriptdynamicupdproc
helpviewer_keywords:
- sp_scriptdynamicupdproc
ms.assetid: b4c18863-ed92-4aa2-a04f-7ed832fc9e07
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ab8af33b865570067301df85355f13e0112979ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189991"
---
# <a name="sp_scriptdynamicupdproc-transact-sql"></a>sp_scriptdynamicupdproc (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Génère l’instruction CREATE PROCEDURE qui crée une procédure stockée de mise à jour dynamique. L'instruction UPDATE dans la procédure stockée personnalisée est créée dynamiquement en fonction de la syntaxe MCALL qui indique les colonnes devant être modifiées. Utilisez cette procédure stockée si le nombre d’index dans la table d’abonnement augmente et si le nombre de colonnes modifiées est limité. Cette procédure stockée est exécutée sur la base de données de publication du serveur de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_scriptdynamicupdproc [ @artid =] artid  
```  
  
## <a name="arguments"></a>Arguments  
`[ @artid = ] artid` Est l’ID de l’article. *artid* est de **type int**, sans valeur par défaut.  
  
## <a name="result-sets"></a>Jeux de résultats  
 Retourne un jeu de résultats qui se compose d’une seule colonne **nvarchar (4000)** . L'ensemble de résultats forme l’instruction CREATE PROCEDURE complète qui permet de créer la procédure stockée personnalisée.  
  
## <a name="remarks"></a>Notes  
 **sp_scriptdynamicupdproc** est utilisé dans la réplication transactionnelle. La logique de script MCALL par défaut inclut toutes les colonnes dans l'instruction UPDATE et utilise une image bitmap pour déterminer les colonnes qui ont changé. Si une colonne n'a pas changé, elle est rétablie, ce qui ne cause généralement aucun problème. Si la colonne est indexée, un traitement supplémentaire intervient. L'approche dynamique inclut uniquement les colonnes qui ont changé, ce qui fournit une chaîne UPDATE optimale. Toutefois, un traitement supplémentaire a lieu pendant la phase d’exécution lors de la génération de l’instruction UPDATE dynamique. Nous vous recommandons de tester les approches dynamique et statique, puis d'opter pour la meilleure solution.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_scriptdynamicupdproc**.  
  
## <a name="examples"></a>Exemples  
 Cet exemple crée un article (avec un *artid* défini sur **1**) sur la table **Authors** de la base de données **pubs** et spécifie que l’instruction Update est la procédure personnalisée à exécuter :  
  
```  
'MCALL sp_mupd_authors'  
```  
  
 Générez les procédures stockées personnalisées, que doit exécuter l'Agent de distribution au niveau de l’Abonné, en exécutant la procédure stockée suivante au niveau du serveur de publication :  
  
```  
EXEC sp_scriptdynamicupdproc @artid = '1'  
  
The statement returns:  
  
CREATE PROCEDURE [sp_mupd_authors]   
  @c1 varchar(11),@c2 varchar(40),@c3 varchar(20),@c4 char(12),@c5 varchar(40),@c6 varchar(20),  
  @c7 char(2),@c8 char(5),@c9 bit,@pkc1 varchar(11),@bitmap binary(2)  
as  
  
declare @stmt nvarchar(4000), @spacer nvarchar(1)  
SELECT @spacer =N''  
SELECT @stmt = N'UPDATE [authors] SET '  
  
if substring(@bitmap,1,1) & 2 = 2  
begin  
  select @stmt = @stmt + @spacer + N'[au_lname]' + N'=@2'  
  select @spacer = N','  
end  
if substring(@bitmap,1,1) & 4 = 4  
begin  
  select @stmt = @stmt + @spacer + N'[au_fname]' + N'=@3'  
  select @spacer = N','  
end  
if substring(@bitmap,1,1) & 8 = 8  
begin  
  select @stmt = @stmt + @spacer + N'[phone]' + N'=@4'  
  select @spacer = N','  
end  
if substring(@bitmap,1,1) & 16 = 16  
begin  
  select @stmt = @stmt + @spacer + N'[address]' + N'=@5'  
  select @spacer = N','  
end  
if substring(@bitmap,1,1) & 32 = 32  
begin  
  select @stmt = @stmt + @spacer + N'[city]' + N'=@6'  
  select @spacer = N','  
end  
if substring(@bitmap,1,1) & 64 = 64  
begin  
  select @stmt = @stmt + @spacer + N'[state]' + N'=@7'  
  select @spacer = N','  
end  
if substring(@bitmap,1,1) & 128 = 128  
begin  
  select @stmt = @stmt + @spacer + N'[zip]' + N'=@8'  
  select @spacer = N','  
end  
if substring(@bitmap,2,1) & 1 = 1  
begin  
  select @stmt = @stmt + @spacer + N'[contract]' + N'=@9'  
  select @spacer = N','  
end  
select @stmt = @stmt + N' where [au_id] = @1'  
exec sp_executesql @stmt, N' @1 varchar(11),@2 varchar(40),@3 varchar(20),@4 char(12),@5 varchar(40),  
                             @6 varchar(20),@7 char(2),@8 char(5),@9 bit',@pkc1,@c2,@c3,@c4,@c5,@c6,@c7,@c8,@c9  
  
if @@rowcount = 0  
   if @@microsoftversion>0x07320000  
      exec sp_MSreplraiserror 20598  
```  
  
 Après avoir exécuté cette procédure stockée, vous pouvez utiliser le script résultant pour créer manuellement la procédure stockée au niveau des Abonnés.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
