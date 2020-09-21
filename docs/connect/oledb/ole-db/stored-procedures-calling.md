---
title: Appel d’une procédure stockée (OLE DB) | Microsoft Docs
description: Découvrez comment appeler une procédure stockée dans OLE DB Driver pour SQL Server, notamment comment transmettre des valeurs de paramètre.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- calling stored procedures
- RPC escape sequence
- OLE DB, stored procedures
- parameters [OLE DB Driver for SQL Server], OLE DB
- ODBC CALL escape sequence
- stored procedures [OLE DB], calling
- OLE DB Driver for SQL Server, stored procedures
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3df02e0bee4a0e0f83851a913e0e09961e6320c6
ms.sourcegitcommit: c95f3ef5734dec753de09e07752a5d15884125e2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88859112"
---
# <a name="stored-procedures---calling"></a>Procédures stockées - Appel
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Une procédure stockée peut avoir entre zéro et plusieurs paramètres. Elle peut également retourner une valeur. Lors de l'utilisation d’OLE DB Driver pour SQL Server, les paramètres d'une procédure stockée peuvent être passés de plusieurs façons :  
  
-   via un codage effectué de manière irréversible de la valeur de données ;  
  
-   via un marqueur de paramètre (?) pour spécifier les paramètres, lier une variable de programme au marqueur de paramètre, puis placer la valeur de données dans la variable de programme.  
  
> [!NOTE]  
>  Lors de l’appel des procédures stockées [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l’aide de paramètres nommés avec OLE DB, les noms de paramètres doivent commencer par le caractère « \@ ». Il s'agit d'une restriction spécifique à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le fournisseur OLE DB Driver pour SQL Server applique cette restriction plus strictement que MDAC.  
  
 Pour prendre en charge les paramètres, l’interface **ICommandWithParameters** est exposée sur l’objet de commande. Pour utiliser les paramètres, le consommateur les décrit au préalable au fournisseur en appelant la méthode **ICommandWithParameters::SetParameterInfo** (ou prépare éventuellement une instruction qui appelle la méthode **GetParameterInfo**). Le consommateur crée ensuite un accesseur qui spécifie la structure d'une mémoire tampon dans laquelle il place des valeurs de paramètre. Enfin, il passe le handle de l’accesseur et un pointeur vers la mémoire tampon à **Execute**. Lors des appels ultérieurs à **Execute**, le consommateur place de nouvelles valeurs de paramètre en mémoire tampon et appelle **Execute** avec le handle d’accesseur et le pointeur de la mémoire tampon.  
  
 Une commande qui appelle une procédure stockée temporaire à l’aide de paramètres doit appeler au préalable **ICommandWithParameters::SetParameterInfo** pour définir les informations de paramètre, de sorte que la commande puisse être préparée avec succès. En effet, le nom interne d’une procédure stockée temporaire diffère du nom externe utilisé par un client. Par ailleurs, MSOLEDBSQL ne peut pas interroger les tables système afin de déterminer les informations de paramètre pour une procédure stockée temporaire.  
  
 Voici les étapes du processus de liaison des paramètres :  
  
1.  Indiquez les informations de paramètre dans un tableau de structures DBPARAMBINDINFO, notamment le nom du paramètre, le nom spécifique au fournisseur pour le type de données du paramètre ou un nom de type de données standard, etc. Chaque structure du tableau décrit un paramètre. Ce tableau est ensuite passé à la méthode **SetParameterInfo**.  
  
2.  Appelez la méthode **ICommandWithParameters::SetParameterInfo** pour décrire les paramètres au fournisseur. **SetParameterInfo** spécifie le type de données natif de chaque paramètre. Les arguments de **SetParameterInfo** sont :  
  
    -   le nombre de paramètres pour lesquels définir des informations de type ;  
  
    -   un tableau d'ordinaux de paramètres pour lesquels définir des informations de type ;  
  
    -   un tableau de structures DBPARAMBINDINFO.  
  
3.  Créez un accesseur de paramètre en utilisant la commande **IAccessor::CreateAccessor**. L'accesseur spécifie la structure d'une mémoire tampon dans laquelle il place les valeurs de paramètre. La commande **CreateAccessor** crée un accesseur à partir d’un jeu de liaisons. Ces liaisons sont décrites par le consommateur via un tableau de structures DBBINDING. Chaque liaison associe un paramètre unique à la mémoire tampon du consommateur et contient les informations suivantes :  
  
    -   ordinal du paramètre auquel la liaison s'applique ;  
  
    -   élément lié (valeur de données, longueur et état) ;  
  
    -   offset en mémoire tampon pour chacun de ces composants ;  
  
    -   longueur et type de la valeur de données dans la mémoire tampon du consommateur.  
  
     Un accesseur est identifié par son handle, qui est de type HACCESSOR. Ce handle est retourné par la méthode **CreateAccessor**. Chaque fois que le consommateur finit d’utiliser un accesseur, il doit appeler la méthode **ReleaseAccessor** pour libérer la mémoire détenue.  
  
     Quand le consommateur appelle une méthode, par exemple **ICommand::Execute**, il passe lui-même le handle à un accesseur et un pointeur vers une mémoire tampon. Le fournisseur utilise cet accesseur pour déterminer comment transférer les données contenues en mémoire tampon.  
  
4.  Remplissez la structure DBPARAMS. Les variables de consommateur à partir desquelles les valeurs de paramètre d’entrée sont récupérées et dans lesquelles les valeurs de paramètre de sortie sont écrites sont passées au moment de l’exécution à **ICommand::Execute** dans la structure DBPARAMS. La structure DBPARAMS inclut trois éléments :  
  
    -   un pointeur vers la mémoire tampon à partir de laquelle le fournisseur récupère les données de paramètre d'entrée et dans laquelle il retourne les données de paramètre de sortie, en fonction des liaisons spécifiées par le handle d'accesseur ;  
  
    -   le nombre de jeux de paramètres en mémoire tampon ;  
  
    -   le handle d'accesseur créé à l'étape 3.  
  
5.  Exécutez la commande en utilisant **ICommand::Execute**.  
  
## <a name="methods-of-calling-a-stored-procedure"></a>Méthodes d'appel d'une procédure stockée  
 Lors de l'exécution d'une procédure stockée dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], le fournisseur OLE DB Driver pour SQL Server prend en charge les éléments suivants :  
  
-   séquence d'échappement ODBC CALL ;  
  
-   séquence d'échappement d'appel de procédure distante (RPC, Remote Procedure Call) ;  
  
-   instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] EXECUTE.  
  
### <a name="odbc-call-escape-sequence"></a>Séquence d'échappement ODBC CALL  
 Si vous connaissez les informations de paramètre, appelez la méthode **ICommandWithParameters::SetParameterInfo** pour décrire les paramètres au fournisseur. Sinon, lorsque la syntaxe ODBC CALL est utilisée pour appeler une procédure stockée, le fournisseur appelle une fonction d'assistance afin de rechercher les informations de paramètre de la procédure stockée.  
  
 Si vous n'êtes pas sûr des informations de paramètre (métadonnées de paramètre), la syntaxe ODBC CALL est recommandée.  
  
 La syntaxe générale pour l'appel d'une procédure à l'aide de la séquence d'échappement ODBC CALL est :  
  
 {[ **? =** ]**appeler**_procédure\_nom_[ **(** [*paramètre*] [ **,** [_paramètre_]]... **)** ]}  
  
 Par exemple :  
  
```  
{call SalesByCategory('Produce', '1995')}  
```  
  
### <a name="rpc-escape-sequence"></a>Séquence d'échappement RPC  
 La séquence d'échappement d'appel de procédure distante (RPC, Remote Procedure Call) est semblable à la syntaxe ODBC CALL de l'appel d'une procédure stockée. Si vous devez appeler la procédure plusieurs fois, la séquence d'échappement RPC offre les meilleures performances parmi les trois méthodes d'appel d'une procédure stockée.  
  
 Lorsque la séquence d'échappement RPC est utilisée pour exécuter une procédure stockée, le fournisseur n'appelle pas de fonction d'assistance pour déterminer les informations de paramètre (comme il le fait dans le cas de la syntaxe ODBC CALL). La syntaxe RPC est plus simple que la syntaxe ODBC CALL ; par conséquent, la commande est analysée plus rapidement, ce qui améliore les performances. Dans ce cas, vous devez fournir les informations de paramètre en exécutant **ICommandWithParameters::SetParameterInfo**.  
  
 La séquence d'échappement RPC nécessite que vous disposiez d'une valeur de retour. Si la procédure stockée ne retourne pas de valeur, le serveur retourne 0 par défaut. De plus, vous ne pouvez pas ouvrir de curseur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur la procédure stockée. La procédure stockée est préparée implicitement et l’appel à **ICommandPrepare::Prepare** échoue. En raison de l'incapacité de préparer un appel RPC, vous ne pouvez pas interroger les métadonnées des colonnes ; IColumnsInfo::GetColumnInfo et IColumnsRowset::GetColumnsRowset retourneront DB_E_NOTPREPARED.  
  
 Si vous connaissez toutes les métadonnées de paramètre, la séquence d'échappement RPC est la méthode recommandée pour exécuter les procédures stockées.  
  
 Voici un exemple de séquence d'échappement RPC pour l'appel d'une procédure stockée :  
  
```  
{rpc SalesByCategory}  
```  
  
 Pour obtenir un exemple d’application qui illustre une séquence d’échappement RPC, consultez [Exécuter une procédure stockée (avec la syntaxe RPC) et traiter les codes de retour et les paramètres de sortie &#40;OLE DB&#41;](../../oledb/ole-db-how-to/results/execute-stored-procedure-with-rpc-and-process-output.md).  
  
### <a name="transact-sql-execute-statement"></a>Instruction Transact-SQL EXECUTE  
 La séquence d’échappement ODBC CALL et la séquence d’échappement RPC sont les méthodes recommandées pour l’appel d’une procédure stockée, plutôt que l’instruction [EXECUTE](../../../t-sql/language-elements/execute-transact-sql.md). Le pilote OLE DB pour SQL Server utilise le mécanisme d’appel de procédure distante de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour optimiser le traitement des commandes. Ce protocole RPC augmente les performances en supprimant une bonne partie du traitement des paramètres et de l'analyse des instructions sur le serveur.  
  
 Voici un exemple d’instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] **EXECUTE** :  
  
```  
EXECUTE SalesByCategory 'Produce', '1995'  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées](../../oledb/ole-db/stored-procedures.md)  
  
  
