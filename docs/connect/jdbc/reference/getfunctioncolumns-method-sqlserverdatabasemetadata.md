---
description: Méthode getFunctionColumns (SQLServerDatabaseMetaData)
title: Méthode getFunctionColumns (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: e2b0e0f7-717c-48e6-bcd2-a325d938a833
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 03a5bbe04c9fdf511659a2abef7e6e4135e87370
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175761"
---
# <a name="getfunctioncolumns-method-sqlserverdatabasemetadata"></a>Méthode getFunctionColumns (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère une description des paramètres de fonctions système ou utilisateur du catalogue spécifié et un type de retour.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public ResultSet getFunctionColumns(java.lang.String catalog,  
                       java.lang.String schemaPattern,  
                       java.lang.String functionNamePattern  
                       java.lang.String columnNamePattern)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *catalog*  
  
 **Chaîne** contenant le nom du catalogue. Si la chaîne est vide « », le résultat inclut les fonctions sans catalogue. Si elle est **null**, le nom du catalogue n’est pas utilisé pour la recherche.  
  
 *schemaPattern*  
  
 **Chaîne** contenant le modèle de nom du schéma. Si la chaîne est vide « », le résultat inclut les fonctions sans schéma. Si elle est **null**, le nom du schéma n’est pas utilisé pour la recherche.  
  
 *functionNamePattern*  
  
 **Chaîne** qui contient le nom d’une fonction.  
  
 *columnNamePattern*  
  
 **Chaîne** qui contient le nom d’un paramètre.  
  
## <a name="return-value"></a>Valeur de retour  
 Objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getFunctionColumns est spécifiée par la méthode getFunctionColumns de l’interface java.sql.DatabaseMetaData.  
  
 Cette méthode retourne uniquement les fonctions et paramètres qui correspondent au nom de schéma, de fonction et de paramètre spécifié avec le catalogue spécifié.  
  
 Chaque ligne du jeu de résultats inclut les colonnes suivantes pour une description de paramètre, une description de colonne ou un type de retour :  
  
|Nom|Type|Description|  
|----------|----------|-----------------|  
|FUNCTION_CAT|**Chaîne**|Nom de la base de données qui contient la fonction.|  
|FUNCTION_SCHEM|**Chaîne**|Schéma pour la fonction.|  
|FUNCTION_NAME|**Chaîne**|Nom de la fonction.|  
|COLUMN_NAME|**Chaîne**|Nom d'un paramètre ou d'une colonne.|  
|COLUMN_TYPE|**short**|**Type de la colonne. Il peut avoir une des valeurs suivantes :**<br /><br /> functionColumnUnknown (0) : Type inconnu.<br /><br /> functionColumnIn (1) : Paramètre d’entrée.<br /><br /> functionColumnInOut (2) : Paramètre d’entrée/sortie.<br /><br /> functionColumnOut (3) : Paramètre de sortie.<br /><br /> functionReturn (4) : Valeur de retour de la fonction.<br /><br /> functionColumnResult (5) : Un paramètre ou une colonne est une colonne dans le jeu de résultats.|  
|DATA_TYPE|**smallint**|Type de données SQL de Java.sql.Types.|  
|TYPE_NAME|**Chaîne**|Nom du type de données.|  
|PRECISION|**int**|Nombre total de chiffres significatifs.|  
|LENGTH|**int**|Longueur des données en octets.|  
|SCALE|**short**|Nombre de chiffres à droite de la virgule décimale.|  
|RADIX|**short**|Base des types numériques.|  
|NULLABLE|**short**|Indique si le paramètre ou la valeur de retour peut contenir une valeur **null**.<br /><br /> **Il peut avoir une des valeurs suivantes :**<br /><br /> functionNoNulls (0) : La valeur Null n’est pas acceptée.<br /><br /> functionNullable (1) : La valeur Null est acceptée.<br /><br /> functionNullableUnknown (2) : Inconnu.|  
|Remarques|**Chaîne**|Commentaires sur une colonne ou un paramètre.|  
|COLUMN_DEF|**Chaîne**|Valeur par défaut de la colonne.<br /><br /> **Remarque :** Ces informations sont disponibles avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et sont propres au pilote JDBC.|  
|SQL_DATA_TYPE|**smallint**|Cette colonne est la même que la colonne **DATA_TYPE**, excepté pour les types de données **datetime** et **interval** ISO.<br /><br /> **Remarque :** Ces informations sont disponibles avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et sont propres au pilote JDBC.|  
|SQL_DATETIME_SUB|**smallint**|Le sous-code **interval** ISO de **datetime** si la valeur de **SQL_DATA_TYPE** est **SQL_DATETIME** ou **SQL_INTERVAL**. Pour les types de données autres que **datetime** et **interval** ISO, cette colonne prend la valeur Null.<br /><br /> **Remarque :** Ces informations sont disponibles avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et sont propres au pilote JDBC.|  
|CHAR_OCTET_LENGTH|**int**|Longueur maximale des paramètres ou colonnes binaires et basés sur des caractères. Pour les autres types de données, il s'agit de la valeur NULL.|  
|ORDINAL_POSITION|**int**|Pour les paramètres d'entrée et de sortie, elle représente la position commençant à 1.<br /><br /> Pour les colonnes de jeux de résultats, il s'agit de la position de la colonne dans le jeu de résultats commençant à 1.<br /><br /> Pour la valeur de retour, il s'agit de la valeur 0.|  
|IS_NULLABLE|**Chaîne**|Détermine l'acceptation des valeurs NULL par un paramètre ou une colonne.<br /><br /> Ce peut être l’une des valeurs suivantes :<br /><br /> **YES** : Le paramètre ou la colonne peut inclure des valeurs Null.<br /><br /> **NO** : Le paramètre ou la colonne ne peut pas inclure de valeurs Null.<br /><br /> Chaîne vide ("") : Inconnu.|  
|SS_TYPE_CATALOG_NAME|**Chaîne**|Nom du catalogue qui contient le type défini par l'utilisateur (UDT).|  
|SS_TYPE_SCHEMA_NAME|**Chaîne**|Nom du schéma qui contient le type défini par l'utilisateur (UDT).|  
|SS_UDT_CATALOG_NAME|**Chaîne**|Type défini par l'utilisateur (UDT) du nom complet.|  
|SS_UDT_SCHEMA_NAME|**Chaîne**|Nom du catalogue dans lequel un nom de collection de schémas XML est défini. Si le nom du catalogue est introuvable, cette variable contient une chaîne vide.|  
|SS_UDT_ASSEMBLY_TYPE_NAME|**Chaîne**|Nom du schéma dans lequel un nom de collection de schémas XML est défini. Si le nom du schéma est introuvable, la chaîne est vide.|  
|SS_XML_SCHEMACOLLECTION_CATALOG_NAME|**Chaîne**|Nom d'une collection de schémas XML. Si le nom est introuvable, la chaîne est vide.|  
|SS_XML_SCHEMACOLLECTION_SCHEMA_NAME|**Chaîne**|Nom du catalogue qui contient le type défini par l'utilisateur (UDT).|  
|SS_XML_SCHEMACOLLECTION_NAME|**Chaîne**|Nom du schéma qui contient le type défini par l'utilisateur (UDT).|  
|SS_DATA_TYPE|**tinyint**|Type de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilisé par les procédures stockées étendues.<br /><br /> **Remarque** : Pour plus d’informations sur les types de données retournés par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez la rubrique « Types de données (Transact-SQL) » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDatabaseMetaData, membres](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
