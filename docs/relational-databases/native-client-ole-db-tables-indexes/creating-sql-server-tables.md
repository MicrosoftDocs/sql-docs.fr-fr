---
title: Créer des tables SQL Server (fournisseur Native Client OLE DB) | Microsoft Docs
description: Découvrez comment le fournisseur de OLE DB SQL Server Native Client expose les fonctions qui permettent aux consommateurs de créer SQL Server tables.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- tables [OLE DB]
- SQL Server Native Client OLE DB provider, tables
- DBCOLUMNDESC usage
- adding tables
- CreateTable function
ms.assetid: a7b8d142-d76a-44d9-a583-86ac5109fbe8
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9943b2c5e05d9cdc90efa8752aedf933ec624db9
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87241539"
---
# <a name="creating-sql-server-native-client-tables"></a>Création de tables SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client expose la fonction **ITableDefinition :: CreateTable** , ce qui permet aux consommateurs de créer des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tables. Les consommateurs utilisent **CreateTable** pour créer des tables permanentes nommées par le consommateur, ainsi que des tables permanentes ou temporaires avec des noms uniques générés par le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client.  
  
 Lorsque le consommateur appelle **ITableDefinition :: CreateTable**, si la valeur de la propriété DBPROP_TBL_TEMPTABLE est VARIANT_TRUE, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client génère un nom de table temporaire pour le consommateur. Le consommateur attribue la valeur NULL au paramètre *pTableID* de la méthode **CreateTable**. Les tables temporaires avec les noms générés par le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client n’apparaissent pas dans l’ensemble de lignes des **tables** , mais sont accessibles via l’interface **IOpenRowset** .  
  
 Lorsque les consommateurs spécifient le nom de la table dans le membre *pwszName* de l’Union *uname* dans le paramètre *pTableID* , le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client crée une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] table portant ce nom. Des contraintes en matière d'affectation de noms aux tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'appliquent et le nom de la table peut indiquer une table permanente ou une table temporaire locale ou globale. Pour plus d’informations, consultez [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md). Le paramètre *ppTableID* peut avoir la valeur NULL.  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client peut générer les noms des tables permanentes ou temporaires. Lorsque le consommateur définit le paramètre *pTableID* sur null et définit *ppTableID* pour qu’il pointe vers un dbid valide \* , le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client retourne le nom généré de la table dans le membre *pwszName* de l’Union *uname* du dbid désigné par la valeur de *ppTableID*. Pour créer une table temporaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native cliente OLE DB fournisseur, le consommateur comprend la propriété de table OLE DB DBPROP_TBL_TEMPTABLE dans un jeu de propriétés de table référencé dans le paramètre *rgPropertySets* . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Les tables temporaires du fournisseur OLE DB Native Client sont locales.  
  
 **CreateTable** retourne DB_E_BADTABLEID si le membre *eKind* du paramètre *pTableID* n’indique pas DBKIND_NAME.  
  
## <a name="dbcolumndesc-usage"></a>Utilisation de la structure DBCOLUMNDESC  
 Le consommateur peut indiquer un type de données de colonne en utilisant soit le membre *pwszTypeName*, soit le membre *wType*. Si le consommateur spécifie le type de données dans *pwszTypeName*, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client ignore la valeur de *wType*.  
  
 S’il utilise le membre *pwszTypeName*, le consommateur spécifie le type de données à l’aide de noms du type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les noms de type de données valides sont les noms retournés dans la colonne TYPE_NAME de l'ensemble de lignes de schéma PROVIDER_TYPES.  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client reconnaît un sous-ensemble de valeurs DbType énumérées OLE DB dans le membre *wType* . Pour plus d'informations, voir [Mappage de types de données dans ITableDefinition](../../relational-databases/native-client-ole-db-data-types/data-type-mapping-in-itabledefinition.md).  
  
> [!NOTE]  
>  **CreateTable** retourne DB_E_BADTYPE si le consommateur choisit le membre *pTypeInfo* ou *pclsid* pour spécifier le type de données de colonne.  
  
 Le consommateur spécifie le nom de colonne dans le membre *pwszName* de l’union *uName* du membre *dbcid* DBCOLUMNDESC. Le nom de colonne est spécifié en tant que chaîne de caractères Unicode. Le membre *eKind* de *dbcid* doit être DBKIND_NAME. **CreateTable** retourne DB_E_BADCOLUMNID si le membre *eKind* n’est pas valide, si *pwszName* possède la valeur NULL ou si la valeur de *pwszName* n’est pas un identificateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide.  
  
 Toutes les propriétés de colonne sont disponibles dans toutes les colonnes définies pour la table. **CreateTable** peut retourner DB_S_ERRORSOCCURRED ou DB_E_ERRORSOCCURRED si les valeurs des propriétés sont en conflit. **CreateTable** retourne une erreur lorsque les paramètres des propriétés de colonne non valides entraîne un échec lors de la création de tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Les propriétés des colonnes dans une structure DBCOLUMNDESC se définissent de la manière suivante.  
  
|ID de propriété|Description|  
|-----------------|-----------------|  
|DBPROP_COL_AUTOINCREMENT|R/W : Lecture/écriture<br /><br /> Valeur par défaut : Description de VARIANT_FALSE : Définit la propriété d’identité sur la colonne créée. Pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la propriété d'identité est valide pour une colonne unique au sein d'une table. La définition de la propriété sur VARIANT_TRUE pour plusieurs colonnes génère une erreur lorsque le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client tente de créer la table sur le serveur.<br /><br /> La propriété d’identité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est uniquement valide pour les types **integer**, **numeric** et **decimal** lorsque l’échelle est définie sur 0. La définition de la propriété sur VARIANT_TRUE sur une colonne de tout autre type de données génère une erreur lorsque le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client tente de créer la table sur le serveur.<br /><br /> Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client retourne DB_S_ERRORSOCCURRED lorsque DBPROP_COL_AUTOINCREMENT et DBPROP_COL_NULLABLE sont tous deux VARIANT_TRUE et que le *valeur dwoption* de DBPROP_COL_NULLABLE n’est pas DBPROPOPTIONS_REQUIRED. DB_E_ERRORSOCCURRED est retourné lorsque DBPROP_COL_AUTOINCREMENT et DBPROP_COL_NULLABLE sont tous les deux définis sur VARIANT_TRUE et quand la valeur *dwOption* de DBPROP_COL_NULLABLE est égale à DBPROPOPTIONS_REQUIRED. La colonne est définie avec la propriété d’identité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le membre *dwStatus* DBPROP_COL_NULLABLE est défini sur DBPROPSTATUS_CONFLICTING.|  
|DBPROP_COL_DEFAULT|R/W : Lecture/écriture<br /><br /> Valeur par défaut : None<br /><br /> Description : Crée une contrainte DEFAULT [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la colonne.<br /><br /> Le membre *vValue* DBPROP peut appartenir aux types suivants. Le membre *vValue.vt* doit spécifier un type compatible avec le type de données de la colonne. Par exemple, le choix de BSTR N/A comme valeur par défaut d'une colonne définie en tant que DBTYPE_WSTR offre une valeur de correspondance compatible. La définition de la même valeur par défaut sur une colonne définie en tant que DBTYPE_R8 génère une erreur lorsque le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client tente de créer la table sur le serveur.|  
|DBPROP_COL_DESCRIPTION|R/W : Lecture/écriture<br /><br /> Valeur par défaut : None<br /><br /> Description : la propriété de colonne DBPROP_COL_DESCRIPTION n’est pas implémentée par le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client.<br /><br /> Le membre *dwStatus* de la structure DBPROP retourne DBPROPSTATUS_NOTSUPPORTED lorsque le consommateur tente d’écrire la valeur de propriété.<br /><br /> La définition de la propriété ne constitue pas une erreur irrécupérable pour le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client. Si toutes les autres valeurs de paramètres sont valides, la table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est créée.|  
|DBPROP_COL_FIXEDLENGTH|R/W : Lecture/écriture<br /><br /> Valeur par défaut : VARIANT_FALSE<br /><br /> Description : le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client utilise DBPROP_COL_FIXEDLENGTH pour déterminer le mappage de type de données lorsque le consommateur définit le type de données d’une colonne à l’aide du membre *WTYPE* du DBCOLUMNDESC. Pour plus d'informations, voir [Mappage de types de données dans ITableDefinition](../../relational-databases/native-client-ole-db-data-types/data-type-mapping-in-itabledefinition.md).|  
|DBPROP_COL_NULLABLE|R/W : Lecture/écriture<br /><br /> Valeur par défaut : None<br /><br /> Description : lors de la création de la table, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client indique si la colonne doit accepter des valeurs NULL si la propriété est définie. Lorsque la propriété n'est pas définie, l'aptitude de la colonne à valider la valeur NULL dépend de l'option de base de données par défaut ANSI_NULLS de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client est un fournisseur conforme à la norme ISO. Les sessions connectées affichent des comportements ISO. Si le consommateur ne définit pas DBPROP_COL_NULLABLE, les colonnes acceptent des valeurs NULL.|  
|DBPROP_COL_PRIMARYKEY|R/W : Lecture/écriture<br /><br /> Valeur par défaut : VARIANT_FALSE Description : lorsque VARIANT_TRUE, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client crée la colonne avec une contrainte de clé primaire.<br /><br /> Si la propriété est définie comme une propriété de colonne, seule une colonne unique peut déterminer la contrainte. La définition de la VARIANT_TRUE de la propriété pour plusieurs colonnes retourne une erreur lorsque le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client tente de créer la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] table.<br /><br /> Remarque : Le consommateur peut faire appel à la fonction **IIndexDefinition::CreateIndex** pour créer une contrainte PRIMARY KEY dans deux ou plusieurs colonnes.<br /><br /> Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client retourne DB_S_ERRORSOCCURRED lorsque DBPROP_COL_PRIMARYKEY et DBPROP_COL_UNIQUE sont tous deux VARIANT_TRUE et que le *valeur dwoption* de DBPROP_COL_UNIQUE n’est pas DBPROPOPTIONS_REQUIRED.<br /><br /> DB_E_ERRORSOCCURRED est retourné lorsque DBPROP_COL_PRIMARYKEY et DBPROP_COL_UNIQUE sont tous les deux définis sur VARIANT_TRUE et quand la valeur *dwOption* de DBPROP_COL_UNIQUE est égale à DBPROPOPTIONS_REQUIRED. La colonne est définie avec la propriété d’identité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le membre *dwStatus* DBPROP_COL_PRIMARYKEY est défini sur DBPROPSTATUS_CONFLICTING.<br /><br /> Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client retourne une erreur lorsque DBPROP_COL_PRIMARYKEY et DBPROP_COL_NULLABLE sont tous les deux VARIANT_TRUE.<br /><br /> Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client retourne une erreur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lorsque le consommateur tente de créer une contrainte de clé primaire sur une colonne de type de données non valide [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous ne pouvez pas définir des contraintes PRIMARY KEY dans des colonnes créées avec les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**bit**, **text**, **ntext** et **image**.|  
|DBPROP_COL_UNIQUE|R/W : Lecture/écriture<br /><br /> Valeur par défaut : Description de VARIANT_FALSE : Applique une contrainte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] UNIQUE à la colonne.<br /><br /> Si la propriété est définie comme une propriété de colonne, la contrainte est appliquée dans une seule colonne uniquement. Le consommateur peut se servir de la fonction **IIndexDefinition::CreateIndex** pour appliquer une contrainte UNIQUE à des valeurs combinées de deux ou plusieurs colonnes.<br /><br /> Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client retourne DB_S_ERRORSOCCURRED lorsque DBPROP_COL_PRIMARYKEY et DBPROP_COL_UNIQUE sont tous deux VARIANT_TRUE et que *valeur dwOption* n’est pas DBPROPOPTIONS_REQUIRED.<br /><br /> DB_E_ERRORSOCCURRED est retourné lorsque DBPROP_COL_PRIMARYKEY et DBPROP_COL_UNIQUE sont tous les deux définis sur VARIANT_TRUE et quand la valeur *dwOption* est égale à DBPROPOPTIONS_REQUIRED. La colonne est définie avec la propriété d’identité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le membre *dwStatus* DBPROP_COL_PRIMARYKEY est défini sur DBPROPSTATUS_CONFLICTING.<br /><br /> Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client retourne DB_S_ERRORSOCCURRED lorsque DBPROP_COL_NULLABLE et DBPROP_COL_UNIQUE sont tous deux VARIANT_TRUE et que *valeur dwOption* n’est pas DBPROPOPTIONS_REQUIRED.<br /><br /> DB_E_ERRORSOCCURRED est retourné lorsque DBPROP_COL_NULLABLE et DBPROP_COL_UNIQUE sont tous les deux définis sur VARIANT_TRUE et quand la valeur *dwOption* est égale à DBPROPOPTIONS_REQUIRED. La colonne est définie avec la propriété d’identité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le membre *dwStatus* DBPROP_COL_NULLABLE est défini sur DBPROPSTATUS_CONFLICTING.<br /><br /> Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client retourne une erreur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lorsque le consommateur tente de créer une contrainte unique sur une colonne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] type de données non valide. Vous ne pouvez pas définir des contraintes UNIQUE sur des colonnes créées avec le type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **bit**.|  
  
 Lorsque le consommateur appelle **ITableDefinition :: CreateTable**, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client interprète les propriétés de la table comme suit.  
  
|ID de propriété|Description|  
|-----------------|-----------------|  
|DBPROP_TBL_TEMPTABLE|R/W : Lecture/écriture<br /><br /> Valeur par défaut : VARIANT_FALSE Description : par défaut, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client crée des tables nommées par le consommateur. Lorsque VARIANT_TRUE, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client génère un nom de table temporaire pour le consommateur. Le consommateur attribue la valeur NULL au paramètre *pTableID* de **CreateTable**. Le paramètre *ppTableID* doit contenir un pointeur valide.|  
  
 Si le consommateur demande qu’un ensemble de lignes soit ouvert sur une table créée avec succès, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client ouvre un ensemble de lignes pris en charge par le curseur. Toutes les propriétés de l'ensemble de lignes peuvent être indiquées dans les jeux de propriétés passés.  
  
 L'exemple ci-dessous crée une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
// This CREATE TABLE statement shows the details of the table created by   
// the following example code.  
//  
// CREATE TABLE OrderDetails  
// (  
//    OrderID      int      NOT NULL  
//    ProductID   int      NOT NULL  
//    CONSTRAINT PK_OrderDetails  
//         PRIMARY KEY CLUSTERED (OrderID, ProductID),  
//    UnitPrice   money      NOT NULL,  
//    Quantity   int      NOT NULL,  
//    Discount   decimal(2,2)   NOT NULL  
//        DEFAULT 0  
// )  
//  
// The PRIMARY KEY constraint is created in an additional example.  
HRESULT CreateTable  
    (  
    ITableDefinition* pITableDefinition  
    )  
    {  
    DBID            dbidTable;  
    const ULONG     nCols = 5;  
    ULONG           nCol;  
    ULONG           nProp;  
    DBCOLUMNDESC    dbcoldesc[nCols];  
  
    HRESULT         hr;  
  
    // Set up column descriptions. First, set default property values for  
    //  the columns.  
    for (nCol = 0; nCol < nCols; nCol++)  
        {  
        dbcoldesc[nCol].pwszTypeName = NULL;  
        dbcoldesc[nCol].pTypeInfo = NULL;  
        dbcoldesc[nCol].rgPropertySets = new DBPROPSET;  
        dbcoldesc[nCol].pclsid = NULL;  
        dbcoldesc[nCol].cPropertySets = 1;  
        dbcoldesc[nCol].ulColumnSize = 0;  
        dbcoldesc[nCol].dbcid.eKind = DBKIND_NAME;  
        dbcoldesc[nCol].wType = DBTYPE_I4;  
        dbcoldesc[nCol].bPrecision = 0;  
        dbcoldesc[nCol].bScale = 0;  
  
        dbcoldesc[nCol].rgPropertySets[0].rgProperties =   
            new DBPROP[NCOLPROPS_MAX];  
        dbcoldesc[nCol].rgPropertySets[0].cProperties = NCOLPROPS_MAX;  
        dbcoldesc[nCol].rgPropertySets[0].guidPropertySet =  
            DBPROPSET_COLUMN;  
  
        for (nProp = 0; nProp < NCOLPROPS_MAX; nProp++)  
            {  
            dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                dwOptions = DBPROPOPTIONS_REQUIRED;  
            dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].colid  
                 = DB_NULLID;  
  
            VariantInit(  
                &(dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                    vValue));  
  
            dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                vValue.vt = VT_BOOL;  
            }  
        }  
  
    // Set the column-specific information.  
    dbcoldesc[0].dbcid.uName.pwszName = L"OrderID";  
    dbcoldesc[0].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[0].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[0].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[1].dbcid.uName.pwszName = L"ProductID";  
    dbcoldesc[1].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[1].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[1].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[2].dbcid.uName.pwszName = L"UnitPrice";  
    dbcoldesc[2].wType = DBTYPE_CY;  
    dbcoldesc[2].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[2].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[2].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[3].dbcid.uName.pwszName = L"Quantity";  
    dbcoldesc[3].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[3].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[3].rgPropertySets[0].cProperties = 1;  
  
    dbcoldesc[4].dbcid.uName.pwszName = L"Discount";  
    dbcoldesc[4].wType = DBTYPE_NUMERIC;  
    dbcoldesc[4].bPrecision = 2;  
    dbcoldesc[4].bScale = 2;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[0].dwPropertyID =   
        DBPROP_COL_NULLABLE;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[0].vValue.boolVal =   
        VARIANT_FALSE;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[1].dwPropertyID =   
        DBPROP_COL_DEFAULT;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[1].vValue.vt = VT_BSTR;  
    dbcoldesc[4].rgPropertySets[0].rgProperties[1].vValue.bstrVal =  
        SysAllocString(L"0");  
    dbcoldesc[4].rgPropertySets[0].cProperties = 2;  
  
    // Set up the dbid for OrderDetails.  
    dbidTable.eKind = DBKIND_NAME;  
    dbidTable.uName.pwszName = L"OrderDetails";  
  
    if (FAILED(hr = pITableDefinition->CreateTable(NULL, &dbidTable,  
        nCols, dbcoldesc, NULL, 0, NULL, NULL, NULL)))  
        {  
        DumpError(pITableDefinition, IID_ITableDefinition);  
        goto SAFE_EXIT;  
        }  
  
SAFE_EXIT:  
    // Clean up dynamic allocation in the property sets.  
    for (nCol = 0; nCol < nCols; nCol++)  
        {  
        for (nProp = 0; nProp < NCOLPROPS_MAX; nProp++)  
            {  
            if (dbcoldesc[nCol].rgPropertySets[0].rgProperties[nProp].  
                vValue.vt == VT_BSTR)  
                {  
                SysFreeString(dbcoldesc[nCol].rgPropertySets[0].  
                    rgProperties[nProp].vValue.bstrVal);  
                }  
            }  
  
        delete [] dbcoldesc[nCol].rgPropertySets[0].rgProperties;  
        delete [] dbcoldesc[nCol].rgPropertySets;  
        }  
  
    return (hr);  
    }  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Tables et index](../../relational-databases/native-client-ole-db-tables-indexes/tables-and-indexes.md)  
  
  
