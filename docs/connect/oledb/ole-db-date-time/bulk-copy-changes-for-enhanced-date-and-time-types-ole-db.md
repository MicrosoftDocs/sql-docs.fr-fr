---
title: Modifications de la copie en bloc pour les types de date et d’heure améliorés (OLE DB) | Microsoft Docs
description: Découvrez les améliorations apportées à la date/heure permettant de prendre en charge les fonctionnalités de copie en block dans OLE DB Driver pour SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB, bulk copy operations
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 808454606f24ab397f5423edf901393ab3f7ccfa
ms.sourcegitcommit: c95f3ef5734dec753de09e07752a5d15884125e2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88860234"
---
# <a name="bulk-copy-changes-for-enhanced-date-and-time-types-ole-db"></a>Modifications de la copie en bloc pour les types de date et d’heure améliorés (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Cet article décrit les améliorations de date/heure permettant de prendre en charge les fonctionnalités dans OLE DB Driver pour SQL Server.  
  
## <a name="format-files"></a>Fichiers de format  
 Lors de la génération interactive de fichiers de format, le tableau ci-dessous décrit l'entrée utilisée pour spécifier des types date/heure et les noms de type de données de fichier hôte correspondants.  
  
|type de stockage de fichier|Type de données du fichier hôte|Réponse à l'invite : « Entrer le type de stockage de fichier du champs <nom_du_champ> [\<default>] : »|  
|-----------------------|-------------------------|-----------------------------------------------------------------------------------------------------|  
|Datetime|SQLDATETIME|d|  
|Smalldatetime|SQLDATETIM4|D|  
|Date|SQLDATE|de|  
|Temps|SQLTIME|te|  
|Datetime2|SQLDATETIME2|d2|  
|Datetimeoffset|SQLDATETIMEOFFSET|do|  
  
 Le fichier au format XML XSD possédera les ajouts suivants :  
  
```  
<xs:complexType name="SQLDATETIME2">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
<xs:complexType name="SQLDATETIMEOFFSET">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
<xs:complexType name="SQLDATE">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
<xs:complexType name="SQLTIME">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
```  
  
## <a name="character-data-files"></a>Fichiers de données de type caractère  
 Dans les fichiers de données de type caractère, les valeurs de date et d'heure sont représentées comme décrit dans la section « Formats de données : Chaînes et littéraux » de [Prise en charge des types de données pour les améliorations de date et heure OLE DB](../../oledb/ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md) pour OLE DB.  
  
 Dans les fichiers de données natifs, les valeurs de date et d’heure pour les quatre nouveaux types sont sous la forme de leurs représentations TDS avec une échelle de 7 (car ceci est le maximum pris en charge par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et les fichiers de données bcp ne stockent pas l’échelle de ces colonnes). Aucune modification n’est apportée au stockage des types **datetime** et **smalldatetime** existants, ni à leurs représentations de flux de données tabulaires (TDS).  
  
 Les tailles de stockage pour les différents types de stockage sont les suivantes pour OLE DB :  
  
|type de stockage de fichier|Taille de stockage en octets|  
|-----------------------|---------------------------|  
|DATETIME|8|  
|smalldatetime|4|  
|Date|3|  
|time|6|  
|datetime2|9|  
|datetimeoffset|11|  
 
  
## <a name="bcp-types-in-msoledbsqlh"></a>Types BCP dans msoledbsql.h  
 Les types suivants sont définis dans msoledbsql.h. Ces types sont passés avec le paramètre *eUserDataType* de IBCPSession::BCPColFmt dans OLE DB.  
  
|type de stockage de fichier|Type de données du fichier hôte|Saisissez msoledbsql.h pour l’utiliser avec IBCPSession::BCPColFmt|Valeur|  
|-----------------------|-------------------------|-----------------------------------------------------------|-----------|  
|Datetime|SQLDATETIME|BCP_TYPE_SQLDATETIME|0x3d|  
|Smalldatetime|SQLDATETIM4|BCP_TYPE_SQLDATETIM4|0x3a|  
|Date|SQLDATE|BCP_TYPE_SQLDATE|0x28|  
|Temps|SQLTIME|BCP_TYPE_SQLTIME|0x29|  
|Datetime2|SQLDATETIME2|BCP_TYPE_SQLDATETIME2|0x2a|  
|Datetimeoffset|SQLDATETIMEOFFSET|BCP_TYPE_SQLDATETIMEOFFSET|0x2b|  
  
## <a name="bcp-data-type-conversions"></a>Conversions des types de données BCP  
 Les tableaux ci-dessous fournissent des informations de conversion.  
  
 **Remarque pour OLE DB** : Les conversions suivantes sont effectuées par IBCPSession. IRowsetFastLoad utilise des conversions OLE DB telles que définies dans [Conversions effectuées de client à serveur](../../oledb/ole-db-date-time/conversions-performed-from-client-to-server.md). Notez que les valeurs datetime sont arrondies au 1/300e de seconde et que les secondes des valeurs smalldatetime sont mises à zéro après l'exécution des conversions clientes décrites ci-dessous. L'arrondi des valeurs datetime est propagé aux heures et aux minutes, mais pas à la date.  
  
|À --><br /><br /> À partir|Date|time|smalldatetime|DATETIME|datetime2|datetimeoffset|char|wchar|  
|------------------------|----------|----------|-------------------|--------------|---------------|--------------------|----------|-----------|  
|Date|1|-|1, 6|1, 6|1, 6|1, 5, 6|1, 3|1, 3|  
|Temps|N/A|1, 10|1, 7, 10|1, 7, 10|1, 7, 10|1, 5, 7, 10|1, 3|1, 3|  
|Smalldatetime|1, 2|1, 4, 10|1|1|1, 10|1, 5, 10|1, 11|1, 11|  
|Datetime|1, 2|1, 4, 10|1, 12|1|1, 10|1, 5, 10|1, 11|1, 11|  
|Datetime2|1, 2|1, 4, 10|1, 12|1, 10|1, 10|1, 5, 10|1, 3|1, 3|  
|Datetimeoffset|1, 2, 8|1, 4, 8, 10|1, 8, 10|1, 8, 10|1, 8, 10|1, 10|1, 3|1, 3|  
|Char/wchar (date)|9|-|9, 6, 12|9, 6, 12|9, 6|9, 5, 6|N/A|N/A|  
|Char/wchar (heure)|-|9, 10|9, 7, 10, 12|9, 7, 10, 12|9, 7, 10|9, 5, 7, 10|N/A|N/A|  
|Char/wchar (datetime)|9, 2|9, 4, 10|9, 10, 12|9, 10, 12|9, 10|9, 5, 10|N/A|N/A|  
|Char/wchar (datetimeoffset)|9, 2, 8|9, 4, 8, 10|9, 8, 10, 12|9, 8, 10, 12|9, 8, 10|9, 10|N/A|N/A|  
  
#### <a name="key-to-symbols"></a>Liste des symboles  
  
|Symbole|Signification|  
|------------|-------------|  
|-|Aucune conversion n'est prise en charge.<br />|  
|1|Si les données fournies ne sont pas valides, une erreur est publiée. Pour les valeurs datetimeoffset, la partie heure doit se situer dans la plage après la conversion au format UTC, même si aucune conversion au format UTC n'est demandée. Ceci tient au fait que TDS et le serveur normalisent toujours l'heure dans les valeurs datetimeoffset pour UTC. Par conséquent, le client doit vérifier que les composants heure se situent dans la plage prise en charge après la conversion au format UTC.|  
|2|Le composant heure est ignoré.|  
|3|S’il se produit une troncation avec perte de données, une erreur est publiée. Pour datetime2, le nombre de chiffres des fractions de seconde (l'échelle) est déterminé à partir de la taille de la colonne de destination, conformément au tableau suivant. Pour les tailles de colonne supérieures à la plage du tableau, une échelle de 9 est nécessaire. Cette conversion accepte jusqu'à neuf chiffres de fractions de seconde, valeur maximale autorisée par OLE DB.<br /><br /> **Type :** DBTIME2<br /><br /> **Échelle impliquée 0**8<br /><br /> **Échelle impliquée 1..9** 1..9<br /><br /> <br /><br /> **Type :** DBTIMESTAMP<br /><br /> **Échelle impliquée 0 :** 19<br /><br /> **Échelle impliquée 1..9 :** 21..29<br /><br /> <br /><br /> **Type :** DBTIMESTAMPOFFSET<br /><br /> **Échelle impliquée 0 :** 26<br /><br /> **Échelle impliquée 1..9 :** 28..36|  
|4|Le composant date est ignoré.|  
|5|Le composant fuseau horaire est défini au format UTC (par exemple, 00:00).|  
|6|L'heure est définie avec la valeur zéro.|  
|7|La date est définie avec la valeur 1900-01-01.|  
|8|Le décalage horaire est ignoré.|  
|9|La chaîne est analysée et convertie en valeur date, datetime, datetimeoffset ou time, selon le premier caractère de ponctuation rencontré et la présence de composants restants. La chaîne est ensuite convertie en type cible, selon les règles fournies dans le tableau à la fin de cet article pour le type source découvert par ce processus. Si les données fournies ne peuvent pas être analysées sans erreur, ou si un composant quelconque est en dehors de la plage autorisée, ou s'il n'y a aucune conversion du type de littéral au type cible, une erreur est publiée. Pour les paramètres datetime et smalldatetime, si l’année est en dehors de la plage prise en charge par ces types, une erreur est publiée.<br /><br /> Pour datetimeoffset, la valeur doit se situer dans la plage après la conversion au format UTC, même si aucune conversion au format UTC n'est demandée. Cela tient au fait que TDS et le serveur normalisent toujours l'heure des valeurs datetimeoffset pour UTC, si bien que le client doit vérifier que les composants heure se situent dans la plage prise en charge après la conversion au format UTC. Si la valeur n’est pas comprise dans la plage UTC prise en charge, une erreur est publiée.|  
|10|Pour les conversions de client à serveur, s'il se produit une troncation avec perte de données, une erreur est publiée. Cette erreur se produit également si la valeur est située en dehors de la plage qui peut être représentée par la plage UTC utilisée par le serveur. S'il se produit une troncation de secondes ou de fractions de seconde lors d'une conversion de serveur à client, seul un avertissement est émis.|  
|11|Pour les conversions de client à serveur, s'il se produit une troncation avec perte de données, une erreur est publiée.|
|12|Les secondes sont réinitialisées et les fractions de seconde sont ignorées. Aucune erreur de troncation n'est possible.|  
|N/A|Le comportement existant et antérieur de [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] est conservé.|  
  
## <a name="see-also"></a>Voir aussi     
 [Améliorations des types de données de date et d’heure &#40;OLE DB&#41;](../../oledb/ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  
