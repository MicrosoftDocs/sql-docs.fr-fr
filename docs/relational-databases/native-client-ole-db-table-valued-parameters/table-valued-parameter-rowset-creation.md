---
title: Création d’un ensemble de lignes de paramètre table (fournisseur Native Client OLE DB)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters, rowset creation
ms.assetid: ffe213ca-cc0e-465e-b31c-a8272324c4fe
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c7847c9119e1f67963414a0e22dd9241466b1e1c
ms.sourcegitcommit: 75f767c7b1ead31f33a870fddab6bef52f99906b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87331944"
---
# <a name="table-valued-parameter-rowset-creation-in-sql-server-native-client"></a>Création d’un ensemble de lignes de paramètre table dans SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Bien que les consommateurs puissent fournir n'importe quel objet d'ensemble de lignes pour les paramètres table, les objets d'ensemble de lignes communs sont implémentés dans des banques de données principales, ce qui limite leurs performances. C'est pourquoi le fournisseur OLE DB de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client permet aux consommateurs de créer un objet d'ensemble de lignes spécialisé par-dessus les données en mémoire. Cet objet d’ensemble de lignes spécial en mémoire est un nouvel objet COM appelé ensemble de lignes de paramètre table. Il fournit des fonctionnalités similaires aux jeux de paramètres.  
  
 Les objets d'ensemble de lignes de paramètre table sont créés explicitement par le consommateur pour les paramètres d'entrée via plusieurs interfaces de niveau session. Il existe une instance d'un objet d'ensemble de lignes de paramètre table par paramètre table. Le consommateur peut créer les objets d'ensemble de lignes de paramètre table soit en fournissant des informations de métadonnées qui sont déjà connues (scénario statique), soit en révélant ces informations par le biais des interfaces du fournisseur (scénario dynamique). Les sections qui suivent décrivent ces deux scénarios :  
  
## <a name="static-scenario"></a>Scénario statique  
 Lorsque les informations de type sont connues, le consommateur utilise ITableDefinitionWithConstraints::CreateTableWithConstraints pour instancier un objet d'ensemble de lignes de paramètre table qui correspond à un paramètre table.  
  
 Le champ *guid* (paramètre *pTableID*) contient le GUID spécial (CLSID_ROWSET_TVP). Le membre *pwszName* contient le nom du type de paramètre table que le consommateur souhaite instancier. Le champ *eKind* sera défini sur DBKIND_GUID_NAME. Ce nom est requis lorsque l'instruction est une instruction SQL ad hoc ; le nom est facultatif s'il s'agit d'un appel de procédure.  
  
 Pour l'agrégation, le consommateur passe le paramètre *pUnkOuter* avec l'IUnknown de contrôle.  
  
 Les propriétés d’objet d’ensemble de lignes de paramètre table étant en lecture seule, le consommateur n’est pas censé définir des propriétés dans *rgPropertySets*.  
  
 Pour le membre *rgPropertySets* de chaque structure DBCOLUMNDESC, le consommateur peut spécifier des propriétés supplémentaires pour chaque colonne. Ces propriétés appartiennent au jeu de propriétés DBPROPSET_SQLSERVERCOLUMN. Elles vous permettent de spécifier les paramètres calculés et les paramètres par défaut de chaque colonne. Elles prennent également en charge les propriétés de colonnes existantes, telles que la possibilité de valeur Null et l'identité.  
  
 Pour récupérer les informations correspondantes d’un objet d’ensemble de lignes de paramètre table, le consommateur utilise IRowsetInfo::GetProperties.  
  
 Pour récupérer les informations sur l’État null, unique, calculé et mis à jour de chaque colonne, le consommateur utilise IColumnsRowset :: GetColumnsRowset ou IColumnsInfo :: GetColumnInfo. Ces méthodes fournissent des informations détaillées sur chaque colonne de l'ensemble de lignes du paramètre table.  
  
 Le consommateur spécifie le type de chaque colonne du paramètre table. Cela est identique à la spécification de colonnes lors de la création d'une table dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le consommateur obtient un objet d’ensemble de lignes de paramètre table à partir du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client via le paramètre de sortie *ppRowset* .  
  
## <a name="dynamic-scenario"></a>Scénario Dynamique  
 Lorsque le consommateur n’a pas d’informations de type, il doit utiliser IOpenRowset :: OpenRowset pour instancier des objets d’ensemble de lignes de paramètre table. Tout ce que le consommateur doit fournir au fournisseur est le nom du type.  
  
 Dans ce scénario, le fournisseur obtient les informations de type d'un objet d'ensemble de lignes de paramètre table à partir du serveur au nom du consommateur.  
  
 Les paramètres *pTableID* et *pUnkOuter* doivent être définis comme dans le scénario statique. Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client obtient ensuite les informations de type (informations de colonne et contraintes) à partir du serveur et retourne un objet d’ensemble de lignes de paramètre table par le biais du paramètre *ppRowset* . Cette opération requiert une communication avec le serveur, et par conséquent ne fonctionne pas aussi bien que le scénario statique. Le scénario dynamique fonctionne uniquement avec des appels de procédure paramétrables.  
  
## <a name="see-also"></a>Voir aussi  
 [Paramètres table &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [Utiliser les paramètres table &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
