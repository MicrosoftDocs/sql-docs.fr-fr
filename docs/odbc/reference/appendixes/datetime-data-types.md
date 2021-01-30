---
description: Type de données datetime
title: Types de données DateTime | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- time data type [ODBC]
- datetime data types [ODBC]
- date data type [ODBC]
- data types [ODBC], date
- backward compatibility [ODBC], datetime data types
- timestamp data type [ODBC]
- data types [ODBC], timestamp
- data types [ODBC], backward compatibility
- compatibility [ODBC], datetime data types
- data types [ODBC], time
ms.assetid: 6b9363c9-04bf-4492-a210-7aa15dea4af8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b2bee41df18a0a05c7b7812c0a23321feec13ed7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194915"
---
# <a name="datetime-data-types"></a>Type de données datetime
Dans ODBC *3. x*, les identificateurs pour les types de données SQL date, Time et TIMESTAMP sont modifiés par rapport à SQL_DATE, SQL_TIME et SQL_TIMESTAMP (avec des instances de **#define** dans le fichier d’en-tête 9, 10 et 11) à SQL_TYPE_DATE, SQL_TYPE_TIME et SQL_TYPE_TIMESTAMP (avec des instances de **#define** dans le fichier d’en-tête 91, 92 et 93), respectivement. Les identificateurs de type C correspondants ont été modifiés de SQL_C_DATE, SQL_C_TIME et SQL_C_TIMESTAMP en SQL_C_TYPE_DATE, SQL_C_TYPE_TIME et SQL_C_TYPE_TIMESTAMP, respectivement, et les instances de **#define** ont changé en conséquence.  
  
 La taille de colonne et les chiffres décimaux renvoyés pour les types de données DateTime SQL dans ODBC *3. x* sont les mêmes que la précision et l’échelle retournées pour eux dans ODBC *2. x*. Ces valeurs sont différentes de celles des champs de descripteur SQL_DESC_PRECISION et SQL_DESC_SCALE. (Pour plus d’informations, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) dans l’annexe D : types de données.)  
  
 Ces modifications affectent **SQLDescribeCol**, **SQLDescribeParam** et **SQLColAttributes**; **SQLBindCol**, **SQLBindParameter** et **SQLGetData**; et **SQLColumns**, **SQLGetTypeInfo**, **SQLProcedureColumns**, **SQLStatistics** et **SQLSpecialColumns**.  
  
 Un pilote ODBC *3. x* traite les appels de fonction listés dans le paragraphe précédent en fonction du paramètre de l’attribut d’environnement SQL_ATTR_ODBC_VERSION. Pour **SQLColumns**, **SQLGetTypeInfo**, **SQLProcedureColumns**, **SQLSpecialColumns** et **SQLStatistics**, si SQL_ATTR_ODBC_VERSION est défini sur SQL_OV_ODBC3, les fonctions retournent SQL_TYPE_DATE, SQL_TYPE_TIME et SQL_TYPE_TIMESTAMP dans le champ data_type. La colonne COLUMN_SIZE (dans le jeu de résultats retourné par **SQLColumns**, **SQLGetTypeInfo**, **SQLProcedureColumns** et **SQLSpecialColumns**) contient la précision binaire pour le type numérique approximatif. La colonne NUM_PREC_RADIX (dans le jeu de résultats retourné par **SQLColumns**, **SQLGetTypeInfo** et **SQLProcedureColumns**) contient la valeur 2. Si SQL_ATTR_ODBC_VERSION a la valeur SQL_OV_ODBC2, les fonctions retournent SQL_DATE, SQL_TIME et SQL_TIMESTAMP dans le champ DATA_TYPE, la colonne COLUMN_SIZE contient la précision décimale pour le type numérique approximatif et la colonne NUM_PREC_RADIX contient la valeur 10.  
  
 Lorsque tous les types de données sont demandés dans un appel à **SQLGetTypeInfo**, le jeu de résultats retourné par la fonction contient à la fois SQL_TYPE_DATE, SQL_TYPE_TIME et SQL_TYPE_TIMESTAMP comme défini dans ODBC *3. x*, et SQL_DATE, SQL_TIME et SQL_TIMESTAMP comme défini dans ODBC *2. x*.  
  
 En raison de la façon dont le gestionnaire de pilotes ODBC *3. x* effectue le mappage des types de données date, Time et timestamp, ODBC *3. x* les pilotes doivent uniquement reconnaître les **#defines** de 91, 92 et 93 pour les types de données date, Time et timestamp C entrés dans les arguments *TargetType* de **SQLBindCol** et **SQLGetData** ou l’argument *ValueType* de **SQLBindParameter**, et doivent uniquement reconnaître **#defines** de 91, 92 et 93 pour les types de données SQL date, Time et timestamp entrés dans l’argument *ParameterType* de **SQLBindParameter** ou l’argument *DataType* de **SQLGetTypeInfo**. Pour plus d’informations, consultez [modifications des types de données DateTime](../../../odbc/reference/develop-app/datetime-data-type-changes.md).
