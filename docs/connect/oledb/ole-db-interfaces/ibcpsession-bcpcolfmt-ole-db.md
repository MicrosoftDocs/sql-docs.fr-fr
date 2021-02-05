---
title: IBCPSession::BCPColFmt (pilote OLE DB) | Microsoft Docs
description: Découvrez comment la méthode IBCPSession::BCPColFmt crée une liaison entre des variables de programme et des colonnes SQL Server dans OLE DB Driver pour SQL Server.
ms.custom: ''
ms.date: 05/25/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- IBCPSession::BCPColFmt (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPColFmt method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4806671e534faf347837333b0c667c396751aeef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186290"
---
# <a name="ibcpsessionbcpcolfmt-ole-db"></a>IBCPSession::BCPColFmt (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Crée une liaison entre des variables de programme et des colonnes [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HRESULT BCPColFmt(   
      DBORDINAL idxUserDataCol,  
      int eUserDataType,  
      int cbIndicator,  
      int cbUserData,  
      BYTE *pbUserDataTerm,  
      int cbUserDataTerm,  
      DBORDINAL idxServerCol);  
```  
  
## <a name="remarks"></a>Notes  
 La méthode **BCPColFmt** permet de créer une liaison entre des champs de fichier de données BCP et des colonnes [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Il accepte la longueur, le type, la marque de fin et la longueur de préfixe d'une colonne en tant que paramètres et définit chacune de ces propriétés pour les champs individuels.  
  
 Si l'utilisateur choisit le mode interactif, cette méthode est appelée deux fois : une fois pour définir le format de colonne en fonction des valeurs par défaut (qui varient selon le type de la colonne serveur), et une fois pour définir le format en fonction du type de colonne du choix du client choisi lors du mode interactif pour chaque colonne.  
  
 Dans les modes non interactifs, elle est appelée uniquement une fois par colonne pour attribuer le type natif ou caractère à chaque colonne, et pour définir les marques de fin de colonne et de ligne.  
  
 La méthode **BCPColFmt** vous permet de spécifier le format de fichier utilisateur pour les copies en bloc. Pour la copie en bloc, un format se compose des éléments suivants :  
  
-   un mappage des champs de fichier utilisateur aux colonnes de base de données ;  
  
-   le type de données de chaque champ de fichier utilisateur ;  
  
-   la longueur de l'indicateur facultatif pour chaque champ ;  
  
-   la longueur maximale des données par champ de fichier utilisateur ;  
  
-   la séquence d'octets de fin facultative pour chaque champ <a href="#terminator_note"><sup>**1**</sup></a> ;  
  
-   la longueur de la séquence d'octets de fin facultative <a href="#terminator_note"><sup>**1**</sup></a>.  
  

> [!IMPORTANT]
> <b id="terminator_note">[1] :</b> L’utilisation de la séquence de fin dans les scénarios où la page de codes du fichier de données est définie sur UTF-8 n’est pas prise en charge. Dans de tels scénarios, **pbUserDataTerm** doit être défini sur `nullptr` et **cbUserDataTerm** doit être défini sur `0`.

 Chaque appel à **BCPColFmt** spécifie le format pour un champ de fichier utilisateur. Par exemple, pour modifier les paramètres par défaut pour trois champs dans un fichier de données utilisateur de cinq champs, appelez tout d'abord `BCPColumns(5)`, puis **BCPColFmt** cinq fois, trois de ces appels définissant votre format personnalisé. Pour les deux appels restants, attribuez BCP_TYPE_DEFAULT à *eUserDataType* et, respectivement, 0, BCP_VARIABLE_LENGTH et 0 à *cbIndicator*, *cbUserData* et *cbUserDataTerm* . Cette procédure copie les cinq colonnes, trois avec votre format personnalisé et deux avec le format par défaut.  
  
> [!NOTE]  
>  La méthode [IBCPSession::BCPColumns](../../oledb/ole-db-interfaces/ibcpsession-bcpcolumns-ole-db.md) doit être appelée avant tout appel à **BCPColFmt**. Vous devez appeler **BCPColFmt** une fois pour chaque colonne dans le fichier utilisateur. Le fait d'appeler **BCPColFmt** plus d'une fois pour toute colonne de fichier utilisateur génère une erreur.  
  
 Vous n'êtes pas obligé de copier toutes les données dans un fichier utilisateur vers une table [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour ignorer une colonne, spécifiez le format des données pour la colonne en attribuant la valeur 0 au paramètre idxServerCol. Pour ignorer un champ, vous avez encore besoin de toutes les informations pour que la méthode fonctionne correctement.  
  
 **Remarque** La fonction [IBCPSession::BCPWriteFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpwritefmt-ole-db.md) peut être utilisée pour assurer la persistance de la spécification de format fournie par le biais de **BCPColFmt**.  
  
## <a name="arguments"></a>Arguments  
 *idxUserDataCol*[in]  
 Index de champ dans le fichier de données de l'utilisateur.  
  
 *eUserDataType*[in]  
 Type de données de champ dans le fichier de données de l'utilisateur. Les types de données disponibles sont répertoriés dans le fichier d'en-tête OLE DB Driver pour SQL Server (msoledbsql.h) au format BCP_TYPE_XXX ; par exemple, BCP_TYPE_SQLINT4. Si la valeur BCP_TYPE_DEFAULT est spécifiée, le fournisseur essaie d'utiliser le même type que la table ou la colonne de vue. Pour les opérations de copie en bloc de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vers un fichier lorsque l’argument **eUserDataType** est BCP_TYPE_SQLDECIMAL ou BCP_TYPE_SQLNUMERIC :  
  
-   Si la colonne source n'est pas décimale ou numérique, la précision et l'échelle par défaut sont utilisées.  
  
-   Si la colonne source est décimale ou numérique, la précision et l'échelle de la colonne source sont utilisées.  
  
 *cbIndicator*[in]  
 Longueur de préfixe pour le champ. La valeur par défaut est BCP_PREFIX_DEFAULT. Les longueurs valides pour le préfixe sont 0, 1, 2, 4 et 8. La taille de préfixe 8 est la plus souvent utilisée pour indiquer que le champ est segmenté. Elle permet d'effectuer des copies en bloc efficaces de colonnes de type valeur volumineuses.  
  
 *cbUserData*[in]  
 Longueur maximale, en octets, des données de ce champ dans le fichier utilisateur, sans compter la longueur de tout indicateur de longueur ou marque de fin.  
  
 Le fait d'attribuer BCP_LENGTH_NULL à **cbUserData** indique que toutes les valeurs dans les champs de fichier de données sont ou doivent être NULL. Le fait d'attribuer BCP_LENGTH_VARIABLE à **cbUserData** indique que le système doit déterminer la longueur des données pour chaque champ. Pour certains champs, cela peut signifier qu'un indicateur de longueur/null est généré pour précéder les données sur une copie à partir de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], ou que l'indicateur est attendu dans les données copiées vers [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Pour les types de données caractères et binaires [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], **cbUserData** peut être BCP_LENGTH_VARIABLE, BCP_LENGTH_NULL, 0 ou une valeur positive. Si **cbUserData** est BCP_LENGTH_VARIABLE, le système utilise l'indicateur de longueur, s'il est présent, ou une séquence de marque de fin pour déterminer la longueur des données. Si un indicateur de longueur et une séquence de terminaison sont fournis, la copie en bloc utilise celui qui implique le volume de données à copier le plus faible. Si **cbUserData** est BCP_LENGTH_VARIABLE, le type de données est un type caractère ou binaire [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si aucun indicateur de longueur ni aucune séquence de terminateur ne sont spécifiés, le système retourne un message d’erreur.  
  
 Si **cbUserData** a la valeur 0 ou une valeur positive, le système utilise **cbUserData** comme longueur de données maximale. Toutefois, si un indicateur de longueur ou une séquence de terminaison est fournie en plus d'une valeur **cbUserData** positive, le système détermine la longueur de données en utilisant la méthode qui entraîne la plus petite quantité de données à copier.  
  
 La valeur **cbUserData** représente le nombre d'octets de données. Si des données caractères sont représentées par des caractères Unicode étendus, une valeur de paramètre **cbUserData** positive représente le nombre de caractères multiplié par la taille, en octets, de chaque caractère.  
  
 *pbUserDataTerm*[size_is][in]  
 Séquence de marque de fin à utiliser pour le champ. Ce paramètre est utile surtout pour les types de données de caractères puisque tous les autres types sont de longueur fixe ou, dans le cas des données binaires, nécessitent un indicateur de longueur pour enregistrer avec précision le nombre d'octets présents.  
  
 Pour éviter de terminer des données extraites ou pour indiquer que les données dans un fichier utilisateur ne sont pas terminées, attribuez la valeur NULL à ce paramètre.  
  
 Si plusieurs méthodes sont employées pour définir la longueur des colonnes du fichier utilisateur (par exemple, un terminateur et un indicateur de longueur, ou un terminateur et une longueur de colonne maximale), la copie en bloc choisit celle qui implique le volume de données à copier le moins élevé.  
  
 L'interface de programmation d'applications (API) de la copie en bloc procède à la conversion des caractères Unicode vers MBCS en fonction des besoins. Prenez soin de définir comme il se doit la chaîne d'octets de terminaison et la longueur de cette même chaîne. Consultez la section [Notes](#remarks) ci-dessus pour connaître les limitations d’encodage UTF-8.

 *cbUserDataTerm*[in]  
 Longueur, en octets, de la séquence de marque de fin à utiliser pour la colonne. Si aucune marque de fin n'est présente ou désirée dans les données, attribuez 0 à cette valeur. Consultez la section [Notes](#remarks) ci-dessus pour connaître les limitations d’encodage UTF-8.

 *idxServerCol*[in]  
 Position ordinale de la colonne dans la table de base de données. Le premier numéro de colonne est 1. La position ordinale d'une colonne est signalée par **IColumnsInfo::GetColumnInfo** ou des méthodes semblables. Si cette valeur est 0, la copie en bloc ignore le champ dans le fichier de données.  
  
## <a name="return-code-values"></a>Codet de retour  
 S_OK  
 S_OK  
  
 E_FAIL  
 Une erreur spécifique au fournisseur s’est produite. Pour obtenir des informations détaillées, utilisez l’interface [ISQLServerErrorInfo](./isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 E_UNEXPECTED  
 L'appel à la méthode était inattendu. Par exemple, la méthode [IBCPSession::BCPInit](../../oledb/ole-db-interfaces/ibcpsession-bcpinit-ole-db.md) n’a pas été appelée avant d’appeler cette méthode.  
  
 E_INVALIDARG  
 L'argument n'était pas valide.  
  
 E_OUTOFMEMORY  
 Erreur de mémoire insuffisante.  
  
## <a name="see-also"></a>Voir aussi  
 [IBCPSession &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md)   
 [Exécution d'opérations de copie en bloc](../../oledb/features/performing-bulk-copy-operations.md)  
  
