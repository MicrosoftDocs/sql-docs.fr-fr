---
title: 'IBCPSession :: BCPInit (fournisseur Native Client OLE DB) | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- IBCPSession::BCPInit (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPInit method
ms.assetid: 583096d7-da34-49be-87fd-31210aac81aa
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f23247e09c47bc41aa7a12c89df4be6fdb5791bb
ms.sourcegitcommit: e8f6c51d4702c0046aec1394109bc0503ca182f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941101"
---
# <a name="ibcpsessionbcpinit-native-client-ole-db-provider"></a>IBCPSession :: BCPInit (fournisseur Native Client OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Initialise la structure de copie en bloc, effectue une vérification des erreurs, vérifie que les données et les noms de fichiers de format sont corrects, puis les ouvre.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HRESULT BCPInit(   
      const wchar_t *pwszTable,  
      const wchar_t *pwszDataFile,  
      const wchar_t *pwszErrorFile,  
      int eDirection);  
```  
  
## <a name="remarks"></a>Notes  
 La méthode **BCPInit** doit être appelée avant toute autre méthode de copie en bloc. La méthode **BCPInit** effectue les initialisations nécessaires pour une copie en bloc de données entre la station de travail et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 La méthode **BCPInit** examine la structure de la source de base de données ou de la table cible, et non le fichier de données. Elle spécifie des valeurs de format de données pour le fichier de données en fonction de chaque colonne dans la table de base de données, la vue ou le jeu de résultats SELECT. Cette spécification inclut le type de données de chaque colonne, la présence ou l'absence d'un indicateur de longueur ou null et des chaînes d'octet de terminateur dans les données, et la largeur des types de données de longueur fixe. La méthode **BCPInit** définit ces valeurs de la manière suivante :  
  
-   Le type de données spécifié est le type de données de la colonne dans la table de base de données, la vue ou le jeu de résultats SELECT. Le type de données est énuméré par les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] types de données natifs spécifiés dans le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fichier d’en-tête Native Client (sqlncli. h). Leurs valeurs suivent le modèle BCP_TYPE_XXX. Les données sont représentées dans leur forme informatique. Autrement dit, les données d'une colonne de type de données integer sont représentées par une séquence de quatre octets de poids fort ou de poids faible selon l'ordinateur qui a créé le fichier de données.  
  
-   Si un type de données de base de données est de longueur fixe, les données du fichier de données sont également de longueur fixe. Les méthodes de copie en bloc qui traitent les données (par exemple, [IBCPSession::BCPExec](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpexec-ole-db.md)) analysent les lignes de données en prévoyant que la longueur des données du fichier de données sera identique à la longueur des données de la table de base de données, de la vue ou de la liste de colonnes SELECT. Par exemple, les données d'une colonne de base de données définie comme `char(13)` doivent être représentées par 13 caractères pour chaque ligne de données du fichier. Les données de longueur fixe peuvent être préfixées avec un indicateur null si la colonne de base de données autorise les valeurs NULL.  
  
-   Lors de la copie de données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le fichier de données doit avoir les données de chaque colonne de la table de base de données. Lors de la copie de données depuis [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les données de toutes les colonnes de la table de base de données, de la vue ou du jeu de résultats SELECT, sont copiées vers le fichier de données.  
  
-   Lors de la copie de données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la position ordinale d'une colonne du fichier de données doit être identique à la position ordinale de la colonne de la table de base de données. Lors de la copie de données à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la méthode **BCPExec** place les données selon la position ordinale de la colonne dans la table de base de données.  
  
-   Si un type de données de base de données est variable en longueur (par exemple, `varbinary(22)`) ou si une colonne de base de données peut contenir des valeurs NULL, les données du fichier de données sont préfixées par un indicateur de longueur/null. La largeur de l'indicateur varie selon le type de données et la version de la copie en bloc. L’option BCP_OPTION_FILEFMT de la méthode [IBCPSession::BCPControl](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpcontrol-ole-db.md) fournit la compatibilité entre les fichiers de données des copies en bloc antérieurs et les serveurs exécutant des versions ultérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en signalant lorsque la largeur des indicateurs des données est plus étroite que prévu.  
  
> [!NOTE]  
>  Pour modifier les valeurs de format de données spécifiées pour un fichier de données, utilisez les méthodes [IBCPSession::BCPColumns](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpcolumns-ole-db.md) et [IBCPSession::BCPColFmt](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md).  
  
 Les copies en bloc vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent être optimisées pour les tables qui ne contiennent pas d’index en définissant l’option de base de données **select into/bulkcopy**.  
  
## <a name="arguments"></a>Arguments  
 *pwszTable*[in]  
 Nom de la table de base de données depuis ou vers laquelle s’effectue la copie. Le nom peut inclure le nom de la base de données ou le nom du propriétaire. Par exemple, « pubs.username.titles », « pubs..titles », « username.titles ».  
  
 Si l'argument eDirection a la valeur BCP_DIRECTION_OUT, l'argument pwszTable peut être le nom d'une vue de base de données.  
  
 Si l’argument eDirection a la valeur BCP_DIRECTION_OUT et qu’une instruction SELECT est spécifiée à l’aide de la méthode **BCPControl** avant l’appel de la méthode **BCPExec**, l’argument *pwszTable* doit avoir la valeur NULL.  
  
 *pwszDataFile*[in]  
 Nom du fichier utilisateur depuis ou vers lequel s’effectue la copie.  
  
 *pwszErrorFile*[in]  
 Nom du fichier d'erreurs à remplir avec les messages de progression, les messages d'erreur et les copies des lignes qui n'ont pas pu être copiées d'un fichier utilisateur vers une table. Si l'argument *pwszErrorFile* a la valeur NULL, aucun fichier d'erreurs n'est utilisé.  
  
 *eDirection*[in]  
 Direction de l'opération de copie, BCP_DIRECTION_IN ou _OUT BCP_DIRECTION. BCP_DIRECTION_IN indique une copie d'un fichier utilisateur vers une table de base de données ; BCP_DIRECTION_OUT indique une copie d'une table de base de données vers un fichier utilisateur.  
  
## <a name="return-code-values"></a>Codet de retour  
 S_OK  
 S_OK  
  
 E_FAIL  
 Une erreur spécifique au fournisseur s’est produite. pour obtenir des informations détaillées, utilisez l’interface [ISQLServerErrorInfo](https://docs.microsoft.com/sql/connect/oledb/ole-db-interfaces/isqlservererrorinfo-geterrorinfo-ole-db?view=sql-server-ver15) .  
  
 E_OUTOFMEMORY  
 Erreur de mémoire insuffisante.  
  
 E_INVALIDARG  
 Au moins un des arguments n'a pas été spécifié correctement. Par exemple, un nom de fichier non valide a été donné.  
  
## <a name="see-also"></a>Voir aussi  
 [IBCPSession &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-ole-db.md)   
 [Exécution d'opérations de copie en bloc](../../relational-databases/native-client/features/performing-bulk-copy-operations.md)  
  
  
