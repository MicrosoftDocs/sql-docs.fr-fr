---
description: MSSQLSERVER_137
title: MSSQLSERVER_137 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 137 (Database Engine error)
ms.assetid: 47fb4212-2165-4fec-bc41-6d548465d7be
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c50c997d96029ee91673f8145e20d174c967a8fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197039"
---
# <a name="mssqlserver_137"></a>MSSQLSERVER_137
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|137|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|P_SCALAR_VAR_NOTFOUND|  
|Texte du message|La variable scalaire « %.*ls » doit être déclarée.|  
  
## <a name="explanation"></a>Explication  
Cette erreur se produit lorsqu'une variable utilisée dans un script SQL n'a pas été préalablement déclarée. L’exemple suivant retourne l’erreur 137 pour les instructions SET et SELECT parce que **\@mycol** n’a pas été déclarée.  
  
```sql
SET @mycol = 'ContactName';  
  
SELECT @mycol; 
```
  
L'une des causes les plus compliquées de cette erreur est l'utilisation d'une variable qui a été déclarée en dehors de l'instruction EXECUTE. Par exemple, la variable **\@mycol** spécifiée dans l’instruction SELECT est locale par rapport à cette instruction SELECT ; elle se situe donc en dehors de l’instruction EXECUTE.  
  
```sql
USE AdventureWorks2012;  
  
GO  
  
DECLARE @mycol nvarchar(20);  
  
SET @mycol = 'Name';  
  
EXECUTE ('SELECT @mycol FROM Production.Product;'); 
```
  
## <a name="user-action"></a>Action de l'utilisateur  
Vérifiez que toutes variables utilisées dans un script SQL ont été déclarées avant d'être utilisées par ailleurs dans le script.  
  
Réécrivez le script afin qu'il ne fasse pas référence aux variables de l'instruction EXECUTE déclarées en dehors. Par exemple :  
  
```sql
USE AdventureWorks2012;  
  
GO  
  
DECLARE @mycol nvarchar(20) ;  
  
SET @mycol = 'Name';  
  
EXECUTE ('SELECT ' + @mycol + ' FROM Production.Product';) ;
```
  
## <a name="see-also"></a>Voir aussi  
[EXECUTE &#40;Transact-SQL&#41;](~/t-sql/language-elements/execute-transact-sql.md)  
[Instructions SET &#40;Transact-SQL&#41;](~/t-sql/statements/set-statements-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](~/t-sql/language-elements/declare-local-variable-transact-sql.md)  
  
