---
description: Error, objet
title: Objet d’erreur | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error
helpviewer_keywords:
- error object [ADO]
ms.assetid: a175d453-fa55-4f49-9ede-a26d83177919
author: rothja
ms.author: jroth
ms.openlocfilehash: eba5b63bc766161556779dd261393b0dfe06822d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171201"
---
# <a name="error-object"></a>Error, objet
Contient des détails sur les erreurs d’accès aux données qui se rapportent à une seule opération impliquant le fournisseur.  
  
## <a name="remarks"></a>Notes  
 Toute opération impliquant des objets ADO peut générer une ou plusieurs erreurs de fournisseur. À mesure que chaque erreur se produit, un ou plusieurs objets **Error** sont placés dans la collection [Errors](../../../ado/reference/ado-api/errors-collection-ado.md) de l’objet [Connection](../../../ado/reference/ado-api/connection-object-ado.md) . Lorsqu’une autre opération ADO génère une erreur, la collection **Errors** est désactivée et le nouvel ensemble d’objets **Error** est placé dans la collection **Errors** .  
  
> [!NOTE]
>  Chaque objet **Error** représente une erreur de fournisseur spécifique, et non une erreur ADO. Les erreurs ADO sont exposées au mécanisme de gestion des exceptions au moment de l’exécution. Par exemple, dans Microsoft Visual Basic, l’occurrence d’une erreur spécifique à ADO déclenchera un événement en cas d' **erreur** et apparaîtra dans l’objet d' **erreur** . Pour obtenir la liste complète des erreurs ADO, consultez la rubrique [ErrorValueEnum](../../../ado/reference/ado-api/errorvalueenum.md) .  
  
 Vous pouvez lire les propriétés d’un objet **Error** pour obtenir des détails spécifiques sur chaque erreur, y compris les éléments suivants :  
  
-   La propriété [Description](../../../ado/reference/ado-api/description-property.md) , qui contient le texte de l’erreur. Il s’agit de la propriété par défaut.  
  
-   La propriété [Number](../../../ado/reference/ado-api/number-property-ado.md) , qui contient la valeur de type entier **long** de la constante d’erreur.  
  
-   Propriété [source](../../../ado/reference/ado-api/source-property-ado-error.md) , qui identifie l’objet qui a déclenché l’erreur. Cela s’avère particulièrement utile lorsque vous avez plusieurs objets d' **erreur** dans la collection d' **Erreurs** suite à une demande adressée à une source de données.  
  
-   Les propriétés [SQLSTATE](../../../ado/reference/ado-api/sqlstate-property.md) et [NativeError](../../../ado/reference/ado-api/nativeerror-property-ado.md) , qui fournissent des informations à partir de sources de données SQL.  
  
 Lorsqu’une erreur de fournisseur se produit, elle est placée dans la collection d' **Erreurs** de l’objet de **connexion** . ADO prend en charge le retour de plusieurs erreurs par une seule opération ADO pour permettre des informations d’erreur spécifiques au fournisseur. Pour obtenir ces informations détaillées sur l’erreur dans un gestionnaire d’erreurs, utilisez les fonctionnalités de recouvrement des erreurs appropriées du langage ou de l’environnement avec lequel vous travaillez, puis utilisez des boucles imbriquées pour énumérer les propriétés de chaque objet d' **erreur** dans la collection d' **Erreurs** .  
  
> [!NOTE]
>  **Utilisateurs Microsoft Visual Basic et VBScript** S’il n’existe aucun objet de **connexion** valide, vous devez récupérer les informations d’erreur à partir de l’objet d' **erreur** .  
  
 Tout comme les fournisseurs, ADO efface l’objet **OLE error info** avant d’effectuer un appel susceptible de générer une nouvelle erreur de fournisseur. Toutefois, la collection d' **Erreurs** sur l’objet de **connexion** est effacée et remplie uniquement lorsque le fournisseur génère une nouvelle erreur ou lorsque la méthode [Clear](../../../ado/reference/ado-api/clear-method-ado.md) est appelée.  
  
 Certaines propriétés et méthodes retournent des avertissements qui s’affichent en tant qu’objets d' **erreur** dans la collection d' **Erreurs** , mais n’arrêtent pas l’exécution d’un programme. Avant d’appeler les méthodes [Resync](../../../ado/reference/ado-api/resync-method.md), [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md)ou [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) sur un objet [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) ; la méthode [Open](../../../ado/reference/ado-api/open-method-ado-connection.md) sur un objet **Connection** ; ou définissez la propriété [Filter](../../../ado/reference/ado-api/filter-property.md) sur un objet **Recordset** , appelez la méthode **Clear** sur la collection **Errors** . De cette façon, vous pouvez lire la propriété [Count](../../../ado/reference/ado-api/count-property-ado.md) de la collection **Errors** pour tester les avertissements retournés.  
  
 L’objet d' **erreur** n’est pas sûr pour l’écriture de scripts.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de l’objet Error](../../../ado/reference/ado-api/error-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Description, HelpContext, HelpFile, NativeError, Number, source et SQLState, exemples de propriétés (VB)](../../../ado/reference/ado-api/description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Description, HelpContext, HelpFile, NativeError, Number, source et SQLState, exemples de propriétés (VC + +)](../../../ado/reference/ado-api/description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [Connection, objet (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)   
 [Errors, collection (ADO)](../../../ado/reference/ado-api/errors-collection-ado.md)   
 [Annexe A : Fournisseurs](../../../ado/guide/appendixes/appendix-a-providers.md)
