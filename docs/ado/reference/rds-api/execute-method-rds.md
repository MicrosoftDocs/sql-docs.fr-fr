---
description: Execute, méthode (RDS)
title: Execute, méthode (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Execute method [ADO]
ms.assetid: 2d9c30e9-ab5b-4920-91b8-48454c2fb5d8
author: rothja
ms.author: jroth
ms.openlocfilehash: 862ded514855d6ac0cd3c18bcb014463eecf1e76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049479"
---
# <a name="execute-method-rds"></a>Execute, méthode (RDS)
Exécute la demande et crée un jeu d’enregistrements ADO à utiliser dans ADO 2,5 et versions ultérieures.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.Execute(ConnectionString As String, HandlerString As String, QueryString As String, lFetchOptions As Long, Properties, TableId, lExecuteOptions As Long, pParameters, [lcid As Long], [pInformation])  
```  
  
#### <a name="parameters"></a>Paramètres  
 *ConnectionString*  
 Chaîne utilisée pour se connecter au fournisseur OLE DB dans lequel la demande sera envoyée pour exécution. Si un gestionnaire est spécifié à l’aide de *HandlerString* , il peut modifier ou remplacer la chaîne de connexion.  
  
 *HandlerString*  
 Chaîne en deux parties qui identifie le gestionnaire à utiliser avec cette exécution. La chaîne contient deux parties. La première partie contient le nom (ProgID) du gestionnaire à utiliser. La deuxième partie contient les arguments à passer au gestionnaire. Les détails de la façon dont la chaîne d’arguments est interprétée sont spécifiques à chaque gestionnaire. Les deux parties sont séparées par la première instance d’une virgule dans la chaîne. La chaîne d’arguments peut contenir des virgules supplémentaires. Les arguments sont facultatifs.  
  
 *QueryString*  
 Commande dans le langage de commande prise en charge par le fournisseur de OLE DB identifié dans la chaîne de connexion. Pour les fournisseurs basés sur SQL, *QueryString* peut contenir une instruction de commande Transact-SQL, mais pour les fournisseurs non-SQL (par exemple, MSDataShape) il ne s’agit pas d’une [!INCLUDE[tsql](../../../includes/tsql-md.md)] instruction de requête.  
  
 Si un gestionnaire est utilisé, le gestionnaire peut modifier ou remplacer la valeur spécifiée ici. Par exemple, le gestionnaire remplace généralement *QueryString* par une chaîne de requête de son fichier. ini. Par défaut, le fichier Msdfmap.ini est utilisé.  
  
 *lFetchOptions*  
 Indique le type d’extraction asynchrone.  
  
 Pour plus d’informations, consultez la [propriété FetchOptions (RDS)](./fetchoptions-property-rds.md).  
  
 *TableID*  
 **Variante** de type VT_EMPTY ou VT_BSTR. Si cette valeur est de type VT_EMPTY, elle est ignorée. S’il est de type VT_BSTR, le jeu d’enregistrements est créé à l’aide de **adCmdTableDirect** et la valeur spécifiée ici et le paramètre *QueryString* est ignoré.  
  
 *lExecuteOptions*  
 Masque de bits des options d’exécution :  
  
 1 =*ReadOnly* l’objet Recordset sera ouvert à l’aide de **adLockReadOnly**.  
  
 2 =*Nobatch* , le jeu d’enregistrements est ouvert à l’aide de **adLockOptimistic**.  
  
 4 =*AllParamInfoSupplied* l’appelant garantit que les informations de paramètre pour tous les paramètres sont fournies dans *pParameters*.  
  
 8 = les informations de paramètres de *GetInfo* pour la requête seront obtenues à partir du fournisseur OLE DB et retournées dans le paramètre *pParameters* . La requête n’est pas exécutée et aucun Recordset n’est retourné.  
  
 16 =*GetHiddenColumns* l’objet Recordset est ouvert à l’aide de **adLockBatchOptimistic** et toutes les colonnes masquées sont incluses dans le Recordset.  
  
 Les options *ReadOnly*, *nobatch* et *GetHiddenColumns* s’excluent mutuellement ; Toutefois, il ne génère pas d’erreur pour en définir plusieurs. Si plusieurs options sont définies, *GetHiddenColumns* est prioritaire sur tous les autres, suivi de *ReadOnly*. Si aucune option n’est spécifiée, par défaut, le jeu d’enregistrements est ouvert à l’aide de la valeur **adLockBatchOptimistic** et les colonnes masquées ne sont pas incluses dans le jeu d’enregistrements.  
  
 *pParameters*  
 **Variant** qui contient un tableau sécurisé de définitions de paramètres. Si l’option *GetInfo* a été spécifiée dans *lExecuteOptions*, ce paramètre est utilisé pour retourner les définitions de paramètres obtenues à partir du fournisseur OLE DB. Dans le cas contraire, ce paramètre peut être vide.  
  
 *lcid*  
 LCID utilisé pour générer les erreurs retournées dans *pInformation*.  
  
 *pInformation*  
 Pointeur vers une erreur d’informations retournée par l’instruction EXECUTE. Si la valeur est NULL, aucune information d’erreur n’est retournée.  
  
## <a name="remarks"></a>Notes  
 Le paramètre *HandlerString* peut avoir la valeur null. Ce qui se passe dans ce cas dépend de la configuration du serveur RDS. La chaîne de gestionnaire « MSDFMAP. Handler » indique que le gestionnaire fourni par Microsoft (Msdfmap.dll) doit être utilisé. Une chaîne de gestionnaire « MASDFMAP. Handler, sample.ini » indique que le gestionnaire de Msdfmap.dll doit être utilisé et que l’argument « sample.ini » doit être passé au gestionnaire. MSDFMAP.dll interprète l’argument comme une direction pour utiliser la sample.ini pour vérifier la connexion et les chaînes de requête.  
  
## <a name="applies-to"></a>S'applique à  
 [DataFactory, objet (RDSServer)](./datafactory-object-rdsserver.md)