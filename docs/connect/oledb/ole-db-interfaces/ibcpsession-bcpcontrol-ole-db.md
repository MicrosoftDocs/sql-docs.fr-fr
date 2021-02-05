---
title: IBCPSession::BCPControl (pilote OLE DB) | Microsoft Docs
description: Découvrez comment IBCPSession::BCPControl définit les options pour une opération de copie en bloc dans OLE DB Driver pour SQL Server.
ms.custom: ''
ms.date: 07/03/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- IBCPSession::BCPControl (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPControl method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cedb68aa9157f89df053b1ab4c150736835b37b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186270"
---
# <a name="ibcpsessionbcpcontrol-ole-db"></a>IBCPSession::BCPControl (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Définit les options d'une opération de copie en bloc.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HRESULT BCPControl(   
      int eOption,  
      void *iValue);  
```  
  
## <a name="remarks"></a>Notes  
 La méthode **BCPControl** définit différents paramètres de contrôle pour les opérations de copie en bloc, y compris le nombre d'erreurs autorisées avant l'annulation de la copie en bloc, les numéros des première et dernière lignes à copier à partir d'un fichier de données, et la taille du lot.  
  
 Cette méthode est également utilisée pour spécifier l'instruction SELECT à utiliser lors de la copie en bloc de données à partir de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Vous pouvez affecter à l'argument **eOption** la valeur BCP_OPTION_HINTS et à l'argument **iValue** un pointeur vers une chaîne de caractères larges contenant l'instruction SELECT.  
  
 Les valeurs possibles pour *eOption* sont :  
  
|Option|Description|  
|------------|-----------------|  
|BCP_OPTION_ABORT|Arrête une opération de copie en bloc déjà en cours. Vous pouvez appeler la méthode **BCPControl** avec un argument *eOption* de valeur BCP_OPTION_ABORT à partir d'un autre thread pour arrêter une opération de copie en bloc en cours d'exécution. L'argument *iValue* est ignoré.|  
|BCP_OPTION_BATCH|Nombre de lignes traitées par lot. La valeur par défaut est 0, ce qui indique toutes les lignes d'une table lorsque les données sont extraites, ou toutes les lignes dans le fichier de données utilisateur lorsque les données sont copiées vers [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Une valeur inférieure à 1 rétablit la valeur par défaut de BCP_OPTION_BATCH.|  
|BCP_OPTION_DELAYREADFMT|Une valeur booléenne définie sur true entraîne la lecture d’[IBCPSession::BCPReadFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpreadfmt-ole-db.md) au moment de l’exécution. Si la valeur False est définie (valeur par défaut), IBCPSession::BCPReadFmt lira immédiatement le fichier de format. Une erreur de séquence se produit si **BCP_OPTION_DELAYREADFMT** a la valeur true et que vous appelez IBCPSession::BCPColumns ou IBCPSession::BCPColFmt.<br /><br /> Une erreur de séquence se produira également si vous appelez `IBCPSession::BCPControl(BCPDELAYREADFMT, (void *)FALSE))` après avoir appelé `IBCPSession::BCPControl(BCPDELAYREADFMT, (void *)TRUE)` et IBCPSession::BCPWriteFmt.<br /><br /> Pour plus d’informations, consultez [Découverte des métadonnées](../../oledb/features/metadata-discovery.md).|  
|BCP_OPTION_FILECP|L'argument *iValue* contient le numéro de la page de codes pour le fichier de données. Vous pouvez spécifier le numéro de la page de codes, tel que 1252 ou 850, ou l'une des valeurs suivantes :<br /><br /> BCP_FILECP_ACP : les données dans le fichier figurent dans la page de codes Microsoft Windows� du client.<br /><br /> BCP_FILECP_OEMCP : les données dans le fichier figurent dans la page de codes OEM du client (valeur par défaut).<br /><br /> BCP_FILECP_RAW : les données dans le fichier figurent dans la page de codes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|BCP_OPTION_FILEFMT|Numéro de version du format de fichier de données. Il peut s'agir de 80 ([!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)]), 90 ([!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]), 100 ([!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] ou [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)]) ou de 110 ([!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]). 110 est la valeur par défaut. Cela s'avère utile pour exporter et importer des données dans des formats pris en charge par une version antérieure du serveur.  Par exemple, pour importer des données obtenues à partir d’une colonne de texte sur un serveur [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] dans une colonne **varchar(max)** sur un serveur [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] ou version ultérieure, vous devez spécifier 80. De la même façon, si vous spécifiez 80 quand vous exportez des données à partir d’une colonne **varchar(max)** , celles-ci sont enregistrées de la même façon que les colonnes de texte (au format [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)]) et peuvent être importées dans une colonne de texte d’un serveur [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)].|  
|BCP_OPTION_FIRST|Première ligne de données du fichier ou de la table à copier. La valeur par défaut est 1 ; une valeur inférieure à 1 rétablit la valeur par défaut de cette option.|  
|BCP_OPTION_FIRSTEX|Pour les opérations bcp out, spécifie la première ligne de la table de base de données à copier dans le fichier de données.<br /><br /> Pour les opérations bcp in, spécifie la première ligne du fichier de données à copier dans la table de base de données.<br /><br /> Le paramètre *iValue* est supposé être l'adresse d'un entier 64 bits signé contenant la valeur. La valeur maximale qui peut être passée à BCPFIRSTEX est 2^63-1.|  
|BCP_OPTION_FMTXML|Permet de spécifier que le fichier de format généré doit être au format XML. Par défaut, cette option est désactivée et les fichiers de format sont enregistrés en tant que fichiers texte. Le fichier de format XML fournit une plus grande souplesse, mais aussi quelques contraintes supplémentaires. Par exemple, vous ne pouvez pas spécifier simultanément le préfixe et la terminaison d'un champ, ce qui est possible dans les fichiers de format plus anciens.<br /><br /> Remarque : Les fichiers de format XML ne sont pris en charge que si les outils [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sont installés parallèlement à OLE DB Driver pour SQL Server.|  
|BCP_OPTION_HINTS|L'argument *iValue* contient un pointeur de chaîne de caractères larges. La chaîne adressée spécifie des indicateurs de traitement de copie en bloc [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou une instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] qui retourne un jeu de résultats. Si une instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] est spécifiée qui retourne plusieurs jeux de résultats, tous les jeux de résultats après le premier sont ignorés.|  
|BCP_OPTION_KEEPIDENTITY|Quand l’argument *iValue* a la valeur true, cette option spécifie que les méthodes de copie en bloc insèrent des valeurs de données fournies pour les colonnes [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] définies avec une contrainte d’identité. Le fichier d'entrée doit fournir des valeurs pour les colonnes d'identité. Si cela n'est pas défini, de nouvelles valeurs d'identités sont générées pour les lignes insérées. Toutes les données présentes dans le fichier pour les colonnes d'identité sont ignorées.|  
|BCP_OPTION_KEEPNULLS|Spécifie si les valeurs de données vides dans le fichier sont converties en valeurs NULL dans la table [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Quand l’argument *iValue* a la valeur TRUE, les valeurs vides sont converties en valeurs NULL dans la table [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. L'option par défaut consiste à convertir les valeurs vides en une valeur par défaut pour la colonne dans la table [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] si une valeur par défaut existe.|  
|BCP_OPTION_LAST|Dernière ligne à copier. La valeur par défaut indique de copier toutes les lignes. Une valeur inférieure à 1 rétablit la valeur par défaut de cette option.|  
|BCP_OPTION_LASTEX|Pour les opérations bcp out, spécifie la dernière ligne de la table de base de données à copier dans le fichier de données.<br /><br /> Pour les opérations bcp in, spécifie la dernière ligne du fichier de données à copier dans la table de base de données.<br /><br /> Le paramètre *iValue* est supposé être l'adresse d'un entier 64 bits signé contenant la valeur. La valeur maximale qui peut être passée à BCPLASTEX est 2^63-1.|  
|BCP_OPTION_MAXERRS|Nombre d'erreurs autorisées avant l'échec de l'opération de copie en bloc. La valeur par défaut est de 10. Une valeur inférieure à 1 rétablit la valeur par défaut de cette option. La copie en bloc impose 65 535 erreurs au maximum. Toute tentative d'attribution d'une valeur supérieure à 65 535 à cette option entraîne l'attribution de la valeur 65 535 à l'option.|  
|BCP_OPTION_ROWCOUNT|Retourne le nombre de lignes affectées par l'opération BCP en cours (ou la dernière).|  
|BCP_OPTION_TEXTFILE|Le fichier de données n'est pas un fichier binaire, mais un fichier texte. BCP détecte si le fichier texte est un fichier Unicode ou non en vérifiant le marqueur d'octet Unicode dans les 2 premiers octets du fichier de données.|  
|BCP_OPTION_UNICODEFILE|Lorsque cette option a pour valeur TRUE, elle spécifie que le fichier d'entrée est un format de fichier Unicode.|  
  
## <a name="arguments"></a>Arguments  
 *eOption*[in]  
 Spécifiez l'une des options répertoriées dans la section Notes ci-dessus.  
  
 *iValue*[in]  
 Valeur pour le paramètre *eOption* spécifié. L'argument *iValue* est un cast de valeur entière à un pointeur void permettant d'autoriser l'expansion future vers des valeurs 64 bits.  
  
## <a name="return-code-values"></a>Codet de retour  
 S_OK  
 S_OK  
  
 E_FAIL  
 Une erreur spécifique au fournisseur s’est produite. Pour obtenir des informations détaillées, utilisez l’interface [ISQLServerErrorInfo](./isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 E_UNEXPECTED  
 L'appel à la méthode était inattendu. Par exemple, la méthode [IBCPSession::BCPInit](../../oledb/ole-db-interfaces/ibcpsession-bcpinit-ole-db.md) n’a pas été appelée avant l’appel de cette fonction.  
  
 E_OUTOFMEMORY  
 Erreur de mémoire insuffisante.  
  
## <a name="see-also"></a>Voir aussi  
 [IBCPSession &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md)   
 [Exécution d'opérations de copie en bloc](../../oledb/features/performing-bulk-copy-operations.md)  
  
