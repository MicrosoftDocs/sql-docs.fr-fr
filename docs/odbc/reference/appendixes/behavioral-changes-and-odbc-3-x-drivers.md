---
description: Changements de comportement et pilotes ODBC 3.x
title: Changements de comportement et pilotes ODBC 3. x | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- sql_attr_odbc_version [ODBC]
- backward compatibility [ODBC], behavioral changes
- compatibility [ODBC], behavioral changes
ms.assetid: 88a503cc-bff7-42d9-83ff-8e232109ed06
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a76e950ad8c929d701a3c967b3c5f5422fd3db89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212499"
---
# <a name="behavioral-changes-and-odbc-3x-drivers"></a>Changements de comportement et pilotes ODBC 3.x
L’attribut d’environnement SQL_ATTR_ODBC_VERSION indique au pilote s’il doit présenter un comportement ODBC *2. x* ou un comportement ODBC *3. x* . La façon dont l’attribut d’environnement SQL_ATTR_ODBC_VERSION est défini dépend de l’application. Les applications ODBC *3. x* doivent appeler **SQLSetEnvAttr** pour définir cet attribut après avoir appelé **SQLAllocHandle** pour allouer un handle d’environnement et avant d’appeler **SQLAllocHandle** pour allouer un handle de connexion. Si ce n’est pas le cas, le gestionnaire de pilotes retourne SQLSTATE HY010 (erreur de séquence de fonction) sur le dernier appel à **SQLAllocHandle**.  
  
> [!NOTE]  
>  Pour plus d’informations sur les changements de comportement et sur le fonctionnement d’une application, consultez [changements de comportement](../../../odbc/reference/develop-app/behavioral-changes.md).  
  
 Les applications ODBC *2. x* et les applications ODBC *2. x* recompilées avec les fichiers d’en-tête ODBC *3. x* n’appellent pas **SQLSetEnvAttr**. Toutefois, ils appellent **SQLAllocEnv** au lieu de **SQLAllocHandle** pour allouer un handle d’environnement. Par conséquent, lorsque l’application appelle **SQLAllocEnv** dans le gestionnaire de pilotes, le gestionnaire de pilotes appelle **SQLAllocHandle** et **SQLSetEnvAttr** dans le pilote. Par conséquent, les pilotes ODBC *3. x* peuvent toujours compter sur cet attribut en cours de définition.  
  
 Si une application conforme aux normes compilée avec l’indicateur de compilation ODBC_STD appelle **SQLAllocEnv** (ce qui peut se produire parce que **SQLAllocEnv** n’est pas déconseillé dans ISO), l’appel est mappé à **SQLAllocHandleStd** au moment de la compilation. Au moment de l’exécution, l’application appelle **SQLAllocHandleStd**. Le gestionnaire de pilotes définit l’attribut d’environnement SQL_ATTR_ODBC_VERSION sur SQL_OV_ODBC3. Un appel à **SQLAllocHandleStd** est équivalent à un appel à **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_ENV et un appel à **SQLSetEnvAttr** pour définir SQL_ATTR_ODBC_VERSION à SQL_OV_ODBC3.  
  
 Dans certaines architectures de pilote, le pilote doit apparaître comme un pilote ODBC *2. x* ou un pilote ODBC *3. x* , en fonction de la connexion. Dans ce cas, le pilote peut ne pas être un pilote, mais une couche qui réside entre le gestionnaire de pilotes et un autre pilote. Par exemple, il peut imiter un pilote, tel qu’un espion ODBC. Dans un autre exemple, il peut agir en tant que passerelle, comme EDA/SQL. Pour qu’il apparaisse en tant que pilote ODBC *3. x* , un tel pilote doit pouvoir exporter **SQLAllocHandle** et s’afficher en tant que pilote ODBC *2. x* , doit pouvoir exporter **SQLAllocConnect**, **SQLAllocEnv** et **SQLAllocStmt,**. Lorsqu’un environnement, une connexion ou une instruction doit être allouée, le gestionnaire de pilotes vérifie si ce pilote exporte **SQLAllocHandle**. Depuis le pilote, le gestionnaire de pilotes appelle **SQLAllocHandle** dans le pilote. Si le pilote utilise un pilote ODBC *2. x* , le pilote doit mapper l’appel à **SQLAllocHandle** à **SQLAllocConnect**, **SQLAllocEnv** ou **SQLAllocStmt,**, selon le cas. Elle ne doit également rien faire avec l’appel **SQLSetEnvAttr** lorsqu’elle se comporte comme un pilote ODBC *2. x* .  
  
 Cette section contient les rubriques suivantes :  
  
-   [Type de données datetime](../../../odbc/reference/appendixes/datetime-data-types.md)  
  
-   [Compatibilité descendante des types de données C](../../../odbc/reference/appendixes/backward-compatibility-of-c-data-types.md)  
  
-   [Signets de longueur fixe](../../../odbc/reference/appendixes/fixed-length-bookmarks.md)  
  
-   [SQLGetInfo, prise en charge](../../../odbc/reference/appendixes/sqlgetinfo-support.md)  
  
-   [Retour de SQL_NO_DATA](../../../odbc/reference/appendixes/returning-sql-no-data.md)  
  
-   [Appel de SQLSetPos pour insérer des données](../../../odbc/reference/appendixes/calling-sqlsetpos-to-insert-data.md)  
  
-   [Chargement par ordinal](../../../odbc/reference/appendixes/loading-by-ordinal.md)
