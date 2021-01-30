---
description: SET PATH, commande
title: SET PATH, commande | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET PATH command [ODBC]
ms.assetid: db488d1e-0963-4f45-8c76-a23b9bde9e9d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a97ac5a074116fcaad0477625ea683226abaf76e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203923"
---
# <a name="set-path-command"></a>SET PATH, commande
Spécifie un chemin d’accès pour les recherches de fichiers. Pour obtenir des informations spécifiques au pilote, consultez les notes.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET PATH TO [Path]  
```  
  
## <a name="arguments"></a>Arguments  
 À [ *chemin*]  
 Spécifie les répertoires dans lesquels vous souhaitez que Visual FoxPro recherche. Utilisez des virgules ou des points-virgules pour séparer les répertoires.  
  
## <a name="remarks"></a>Notes  
 SET PATH vous permet de spécifier des chemins de recherche pour d’autres programmes Visual FoxPro qui peuvent être appelés dans des procédures stockées. SET PATH ne modifie pas le chemin d’accès de la source de données que vous avez spécifiée pour la connexion.  
  
 Problème de définition du chemin d’accès à sans *chemin* d’accès pour restaurer le chemin d’accès au répertoire ou au dossier par défaut.  
  
## <a name="driver-remarks"></a>Remarques sur le pilote  
 Si vous émettez SET PATH dans une procédure stockée, il sera ignoré par les fonctions et les commandes suivantes :  
  
-   Les fonctions de catalogue telles que [SQLTables](../../odbc/microsoft/sqltables-visual-foxpro-odbc-driver.md) et [SQLColumns](../../odbc/microsoft/sqlcolumns-visual-foxpro-odbc-driver.md) ignorent le nouveau chemin d’accès et continuent à référencer le chemin d’accès spécifié par la source de données dans [SQLPrepare](../../odbc/microsoft/sqlprepare-visual-foxpro-odbc-driver.md) ou [SQLExecDirect](../../odbc/microsoft/sqlexecdirect-visual-foxpro-odbc-driver.md).  
  
-   Les commandes telles que SELECT, INSERT, UPDATE, DELETE et CREATE TABLE ignorent le nouveau chemin d’accès et continuent à référencer le chemin d’accès spécifié par la source de données dans **SQLPrepare** ou **SQLExecDirect**.  
  
 Si vous émettez SET PATH dans une procédure stockée et que vous ne réaffectez pas par la suite le chemin d’accès à son état d’origine, les autres connexions à la base de données utiliseront le nouveau chemin d’accès (car SET PATH n’est pas étendu aux sessions de données).  
  
 Si vous souhaitez créer, sélectionner ou mettre à jour des tables dans un répertoire autre que celui spécifié par la source de données, spécifiez le chemin d’accès complet du fichier avec votre commande.  
  
## <a name="see-also"></a>Voir aussi  
 [Boîte de dialogue installation de ODBC pour Visual FoxPro](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md)   
 [SQLColumns (pilote ODBC Visual FoxPro)](../../odbc/microsoft/sqlcolumns-visual-foxpro-odbc-driver.md)   
 [SQLDriverConnect (pilote ODBC Visual FoxPro)](../../odbc/microsoft/sqldriverconnect-visual-foxpro-odbc-driver.md)   
 [SQLTables (pilote ODBC Visual FoxPro)](../../odbc/microsoft/sqltables-visual-foxpro-odbc-driver.md)
