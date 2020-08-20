---
description: 'ISSCommandWithParameters :: SetParameterProperties en SQL Server Native Client (OLE DB)'
title: ISSCommandWithParameters::SetParameterProperties (OLE DB)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- ISSCommandWithParameters::SetParameterProperties (OLE DB)
apitype: COM
helpviewer_keywords:
- SetParameterProperties method
ms.assetid: 4cd0281a-a2a0-43df-8e46-eb478b64cb4b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ec0621d8967f2fa351e9147815bddf2d80f0e300
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88490746"
---
# <a name="isscommandwithparameterssetparameterproperties-in-sql-server-native-client-ole-db"></a>ISSCommandWithParameters :: SetParameterProperties en SQL Server Native Client (OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Définit les propriétés de paramètre pour chaque paramètre par ordinal ou définit des propriétés de paramètre en bloc en spécifiant un tableau de structures SSPARAMPROPS.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp
HRESULT SetParameterProperties(  
      DB_UPARAMS cParams,   
      SSPARAMPROPS rgParamProperties[]);  
```  
  
## <a name="arguments"></a>Arguments  
 *cParams*[in]  
 Nombre de structures SSPARAMPROPS dans le tableau *rgParamProperties*. Si ce nombre est égal à zéro, **ISSCommandWithParameters::SetParameterProperties** supprime toutes les propriétés qui ont pu être spécifiées pour les paramètres dans la commande.  
  
 *rgParamProperties*[in]  
 Tableau de structures SSPARAMPROPS à définir.  
  
## <a name="return-code-values"></a>Codet de retour  
 La méthode **ISSCommandWithParameters::SetParameterProperties** retourne les mêmes codes d’erreur que la méthode OLE DB **ICommandProperties::SetProperties** principale.  
  
## <a name="remarks"></a>Notes  
 La définition des propriétés de paramètre avec cette méthode est autorisée pour chaque paramètre par ordinal, ou avec un appel unique à **ISSCommandWithParameters::SetParameterProperties**, une fois SSPARAMPROPS créé à partir du tableau de propriétés.  
  
 Vous devez appeler la méthode **SetParameterInfo** avant d’appeler la méthode **ISSCommandWithParameters::SetParameterProperties**. Le fait d'appeler `SetParameterProperties(0, NULL)` efface toutes les propriétés de paramètre spécifiées, tandis que l'appel de `SetParameterInfo(0,NULL,NULL)` efface toutes les informations sur les paramètres y compris toutes les propriétés qui peuvent être associées à un paramètre.  
  
 L’appel de **ISSCommandWithParameters :: SetParameterProperties** pour spécifier des propriétés pour un paramètre qui n’est pas de type DBTYPE_XML ou DBTYPE_UDT retourne DB_E_ERRORSOCCURRED ou DB_S_ERRORSOCCURRED et marque le champ *dwStatus* de tous les DBPROP contenus dans SSPARAMPROPS pour ce paramètre avec DBPROPSTATUS_NOTSET. Le tableau DBPROP de chaque DBPROPSET contenu dans SSPARAMPROPS doit être parcouru pour détecter le paramètre auquel DB_E_ERRORSOCCURRED ou DB_S_ERRORSOCCURRED fait référence.  
  
 Si **ISSCommandWithParameters::SetParameterProperties** est appelé pour spécifier les propriétés de paramètres dont les informations sur les paramètres n’ont pas encore été définies avec **SetParameterInfo**, le fournisseur retourne E_UNEXPECTED avec le message d’erreur suivant :  
  
 La méthode SetParameterProperties ne peut pas être appelée pour les paramètres spécifiés sans appeler auparavant la méthode SetParameterInfo. Vous devez définir les informations de paramètre avant de définir les propriétés de paramètre.  
  
 Si l’appel à **ISSCommandWithParameters::SetParameterProperties** contient des paramètres où les informations sur les paramètres ont été définies et quelques paramètres où les informations sur les paramètres n’ont pas été définies, les propriétés dwStatus dans le DBPROPSET du jeu de propriétés SSPARAMPROPS seront retournées avec DBSTATUS_NOTSET.  
  
 La structure SSPARAMPROPS est défini comme suit :  

```cpp
struct SSPARAMPROPS {
    DBORDINAL iOrdinal;
    ULONG cPropertySets;
    DBPROPSET *rgPropertySets;
};
```

 Les améliorations apportées au moteur de base de données à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] permettent à ISSCommandWithParameters::SetParameterProperties d'obtenir des descriptions plus exactes des résultats attendus. Ces résultats plus exacts peuvent différer des valeurs retournées ISSCommandWithParameters::SetParameterProperties dans les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Découverte des métadonnées](../../relational-databases/native-client/features/metadata-discovery.md).  
  
|Membre|Description|  
|------------|-----------------|  
|*iOrdinal*|Position ordinale du paramètre transmis.|  
|*cPropertySets*|Nombre de structures DBPROPSET dans *rgPropertySets*.|  
|*rgPropertySets*|Pointeur vers la mémoire dans lequel retourner un tableau de structures DBPROPSET.|  
|||

## <a name="see-also"></a>Voir aussi  
 [ISSCommandWithParameters &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  
