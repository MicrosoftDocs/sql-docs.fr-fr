---
description: Méthode getTypeInfo (SQLServerDatabaseMetaData)
title: Méthode getTypeInfo (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getTypeInfo
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 23208f01-c1bf-4235-b29c-9051d3df59a3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 43145d029b4095ab05cd47526deface99dc81eb0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99155644"
---
# <a name="gettypeinfo-method-sqlserverdatabasemetadata"></a>Méthode getTypeInfo (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère une description de tous les types SQL standard pris en charge par la base de données actuelle.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.sql.ResultSet getTypeInfo()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getTypeInfo est spécifiée par la méthode getTypeInfo de l’interface java.sql.DatabaseMetaData.  
  
 Le jeu de résultats retourné par la méthode getTypeInfo contient les informations suivantes :  
  
|Name|Type|Description|  
|----------|----------|-----------------|  
|TYPE_NAME|**Chaîne**|Nom du type de données.|  
|DATA_TYPE|**short**|Type de données SQL de java.sql.Types.|  
|PRECISION|**int**|Nombre total de chiffres significatifs.|  
|LITERAL_PREFIX|**Chaîne**|Caractère(s) utilisé(s) devant une constante.|  
|LITERAL_SUFFIX|**Chaîne**|Caractère(s) utilisé(s) pour terminer une constante.|  
|CREATE_PARAMS|**Chaîne**|Description des paramètres de création du type de données.|  
|NULLABLE|**short**|Indique si la colonne peut contenir une valeur Null. Ce peut être l’une des valeurs suivantes :<br /><br /> typeNoNulls (0)<br /><br /> typeNullable (1)<br /><br /> typeNullableUnknown (2)|  
|CASE_SENSITIVE|**boolean**|Indique si le type de donnée respecte la casse. « **true** » si le type respecte la casse ; sinon, « **false** ».|  
|POSSIBILITÉ DE RECHERCHE|**short**|Indique si la colonne désignée peut être utilisée dans une clause SQL WHERE. Ce peut être l’une des valeurs suivantes :<br /><br /> typePredNone (0)<br /><br /> typePredChar (1)<br /><br /> typePredBasic (2)<br /><br /> typeSeachable (3)|  
|UNSIGNED_ATTRIBUTE|**boolean**|Indique le signe du type de données. « **true** » si le type est non signé ; sinon, « **false** ».|  
|FIXED_PREC_SCALE|**boolean**|Indique que le type de données peut être une valeur monétaire. « **true** » si le type de données est money ; sinon, « **false** ».|  
|AUTO_INCREMENT|**boolean**|Indique que le type de données peut être incrémenté automatiquement. « **true** » si le type ne peut pas être incrémenté automatiquement ; sinon, « **false** ».|  
|LOCAL_TYPE_NAME|**Chaîne**|Nom localisé du type de données.|  
|MINIMUM_SCALE|**short**|Nombre maximal de chiffres situés à droite de la virgule décimale.|  
|MAXIMUM_SCALE|**short**|Nombre minimal de chiffres situés à droite de la virgule décimale.|  
|SQL_DATA_TYPE|**int**|Non pris en charge par le pilote JDBC.|  
|SQL_DATETIME_SUB|**int**|Non pris en charge par le pilote JDBC.|  
|NUM_PREC_RADIX|**int**|Nombre de bits ou de chiffres pour le calcul du nombre maximal pouvant être contenu dans la colonne.|  
|INTERVAL_PRECISION|**smallint**|Valeur de la précision d'en-tête de l'intervalle.|  
|USERTYPE|**smallint**|Valeur **usertype** de la table **systypes**. Pour plus d'informations, consultez la documentation en ligne de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
  
> [!NOTE]  
>  Pour plus d’informations sur les données retournées par la méthode getTypeInfo, consultez « sp_datatype_info (Transact-SQL) » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="example"></a> Exemple  
 L’exemple suivant montre comment utiliser la méthode getTypeInfo pour retourner des informations sur les types de données utilisés dans une base de données [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] (ou ultérieur).  
  
```  
public static void executeGetTypeInfo(Connection con) {  
   try {  
      DatabaseMetaData dbmd = con.getMetaData();  
      ResultSet rs = dbmd.getTypeInfo();  
      ResultSetMetaData rsmd = rs.getMetaData();  
  
      // Display the result set data.  
      int cols = rsmd.getColumnCount();  
      while(rs.next()) {  
         for (int i = 1; i <= cols; i++) {  
            System.out.println(rs.getString(i));  
         }  
      }  
      rs.close();  
   }   
  
   catch (Exception e) {  
      e.printStackTrace();  
   }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDatabaseMetaData, méthodes](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData, membres](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
