---
description: ErrorValueEnum
title: ErrorValueEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ErrorValueEnum
helpviewer_keywords:
- ErrorValueEnum enumeration [ADO]
ms.assetid: 9469ba3a-5e4f-4a10-bbb8-a51a6c9660ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 238c8bf3d0c21625b9317690f70adbe16bd5075f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171168"
---
# <a name="errorvalueenum"></a>ErrorValueEnum
Spécifie le type d’erreur d’exécution ADO.  
  
 Trois formes du numéro d’erreur sont répertoriées :  
  
-   Decimal positif : deux octets de poids faible au format décimal. Ce nombre est affiché dans la boîte de dialogue par défaut Visual Basic message d’erreur. Par exemple, erreur d’exécution' 3707 '.  
  
-   Décimale négative-traduction décimale du numéro d’erreur complet.  
  
-   Hexadecimal-The représentation hexadécimale du numéro d’erreur complet. Le code de l’installation Windows se trouve dans le quatrième chiffre. Le code d’installation pour les numéros d’erreur ADO est *un*. Par exemple : 0x800 ***A** _0E7B.  
  
> [!NOTE]
>  OLE DB erreurs peuvent être transmises à votre application ADO. En règle générale, elles peuvent être identifiées par un code d’installation Windows de _4 *. Par exemple, 0x800 * * * 4* *_.  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|_ *adErrBoundToCommand**|3707-2146824581 0x800A0E7B|Impossible de modifier la propriété **ActiveConnection** d’un objet **Recordset** qui possède un objet **Command** comme source.|  
|**adErrCannotComplete**|3732-2146824556 0x800A0E94|Le serveur ne peut pas terminer l’opération.|  
|**adErrCantChangeConnection**|3748-2146824540 0x800A0EA4|La connexion a été refusée. La nouvelle connexion que vous avez demandée présente des caractéristiques différentes de celles déjà utilisées.|  
|**adErrCantChangeProvider**|3220-2146825068 0X800A0C94|Le fournisseur fourni est différent de celui déjà utilisé.|  
|**adErrCantConvertvalue**|3724-2146824564 0x800A0E8C|La valeur des données ne peut pas être convertie pour des raisons autres que l’incompatibilité de signe ou le dépassement de données. Par exemple, la conversion aurait des données tronquées.|  
|**adErrCantCreate**|3725-2146824563 0x800A0E8D|Impossible de définir ou de récupérer la valeur des données, car le type de données du champ est inconnu ou le fournisseur ne dispose pas de ressources suffisantes pour effectuer l’opération.|  
|**adErrCatalogNotSet**|3747-2146824541 0x800A0EA3|L’opération requiert un **ParentCatalog** valide.|  
|**adErrColumnNotOnThisRow**|3726-2146824562 0x800A0E8E|L’enregistrement ne contient pas ce champ.|  
|**adErrDataConversion**|3421-2146824867 0x800A0D5D|L’application utilise une valeur de type incorrect pour l’opération en cours.|  
|**adErrDataOverflow**|3721-2146824567 0x800A0E89|La valeur des données est trop grande pour être représentée par le type de données du champ.|  
|**adErrDelResOutOfScope**|3738-2146824550 0x800A0E9A|L’URL de l’objet à supprimer est en dehors de l’étendue de l’enregistrement en cours.|  
|**adErrDenyNotSupported**|3750-2146824538 0x800A0EA6|Le fournisseur ne prend pas en charge les restrictions de partage.|  
|**adErrDenyTypeNotSupported**|3751-2146824537 0x800A0EA7|Le fournisseur ne prend pas en charge le type de restriction de partage demandé.|  
|**adErrFeatureNotAvailable**|3251-2146825037 0x800A0CB3|L’objet ou le fournisseur n’est pas en mesure d’effectuer l’opération demandée.|  
|**adErrFieldsUpdateFailed**|3749-2146824539 0x800A0EA5|Échec de la mise à jour des champs. Pour plus d’informations, consultez la propriété **Status** des objets Field individuels.|  
|**adErrIllegalOperation**|3219-2146825069 0x800A0C93|L’opération n’est pas autorisée dans ce contexte.|  
|**adErrIntegrityViolation**|3719-2146824569 0x800A0E87|La valeur des données est en conflit avec les contraintes d’intégrité du champ.|  
|**adErrInTransaction**|3246-2146825042 0x800A0CAE|L’objet de **connexion** ne peut pas être explicitement fermé dans une transaction.|  
|**adErrInvalidArgument**|3001-2146825287 0x800A0BB9|Les arguments sont de type incorrect, sont en dehors de la plage acceptable ou sont en conflit les uns avec les autres.|  
|**adErrInvalidConnection**|3709-2146824579 0x800A0E7D|La connexion ne peut pas être utilisée pour effectuer cette opération. Il est fermé ou non valide dans ce contexte.|  
|**adErrInvalidParamInfo**|3708-2146824580 0x800A0E7C|L’objet de **paramètre** n’est pas défini correctement. Des informations incohérentes ou incomplètes ont été fournies.|  
|**adErrInvalidTransaction**|3714-2146824574 0x800A0E82|La coordination de la transaction n’est pas valide ou n’a pas démarré.|  
|**adErrInvalidURL**|3729-2146824559 0x800A0E91|L’URL contient des caractères non valides. Assurez-vous que l’URL est correctement tapée.|  
|**adErrItemNotFound**|3265-2146825023 0x800A0CC1|L’élément est introuvable dans la collection qui correspond au nom ou à l’ordinal demandé.|  
|**adErrNoCurrentRecord**|3021-2146825267 0x800A0BCD|**BOF** ou **EOF** a la valeur true ou l’enregistrement en cours a été supprimé. L’opération demandée requiert un enregistrement en cours.|  
|**adErrNotExecuting**|3715-2146824573 0x800A0E83|Impossible d’effectuer l’opération pendant qu’elle n’est pas exécutée.|  
|**adErrNotReentrant**|3710-2146824578 0x800A0E7E|Impossible d’effectuer l’opération lors du traitement de l’événement.|  
|**adErrObjectClosed**|3704-2146824584 0x800A0E78|L’opération n’est pas autorisée lorsque l’objet est fermé.|  
|**adErrObjectInCollection**|3367-2146824921 0x800A0D27|L’objet est déjà dans la collection. Impossible d’ajouter.|  
|**adErrObjectNotSet**|3420-2146824868 0x800A0D5C|L’objet n’est plus valide.|  
|**adErrObjectOpen**|3705-2146824583 0x800A0E79|L’opération n’est pas autorisée lorsque l’objet est ouvert.|  
|**adErrOpeningFile**|3002-2146825286 0x800A0BBA|Impossible d’ouvrir le fichier.|  
|**adErrOperationCancelled**|3712-2146824576 0x800A0E80|L’opération a été annulée par l’utilisateur.|  
|**adErrOutOfSpace**|3734-2146824554 0x800A0E96|Impossible d’effectuer l’opération. Le fournisseur ne peut pas obtenir suffisamment d’espace de stockage.|  
|**adErrPermissionDenied**|3720-2146824568 0x800A0E88|Les autorisations insuffisantes empêchent l’écriture dans le champ.|  
|**adErrProviderFailed**|3000-2146825288 0x800A0BB8|Le fournisseur n’a pas effectué l’opération demandée.|  
|**adErrProviderNotFound**|3706-2146824582 0x800A0E7A|Le fournisseur est introuvable. Il est possible qu’il ne soit pas correctement installé.|  
|**adErrReadFile**|3003-2146825285 0x800A0BBB|Impossible de lire le fichier.|  
|**adErrResourceExists**|3731-2146824557 0x800A0E93|Impossible d’effectuer l’opération de copie. L’objet nommé par l’URL de destination existe déjà. Spécifiez **adCopyOverwrite** pour remplacer l’objet.|  
|**adErrResourceLocked**|3730-2146824558 0x800A0E92|L’objet représenté par l’URL spécifiée est verrouillé par un ou plusieurs autres processus. Attendez que le processus soit terminé, puis recommencez l’opération.|  
|**adErrResourceOutOfScope**|3735-2146824553 0x800A0E97|L’URL source ou de destination est en dehors de l’étendue de l’enregistrement en cours.|  
|**adErrSchemaViolation**|3722-2146824566 0x800A0E8A|La valeur des données est en conflit avec le type de données ou les contraintes du champ.|  
|**adErrSignMismatch**|3723-2146824565 0x800A0E8B|Échec de la conversion car la valeur des données était signée et le type de données du champ utilisé par le fournisseur n’était pas signé.|  
|**adErrStillConnecting**|3713-2146824575 0x800A0E81|Impossible d’effectuer l’opération lors de la connexion asynchrone.|  
|**adErrStillExecuting**|3711-2146824577 0x800A0E7F|Impossible d’effectuer l’opération lors de l’exécution de manière asynchrone.|  
|**adErrTreePermissionDenied**|3728-2146824560 0x800A0E90|Les autorisations sont insuffisantes pour accéder à l’arborescence ou à la sous-arborescence.|  
|**adErrUnavailable**|3736-2146824552 0x800A0E98|L’opération ne s’est pas terminée et l’État n’est pas disponible. Le champ n’est peut-être pas disponible ou l’opération n’a pas été tentée.|  
|**adErrUnsafeOperation**|3716-2146824572 0x800A0E84|Les paramètres de sécurité de cet ordinateur empêchent l’accès à une source de données sur un autre domaine.|  
|**adErrURLDoesNotExist**|3727-2146824561 0x800A0E8F|L’URL source ou le parent de l’URL de destination n’existe pas.|  
|**adErrURLNamedRowDoesNotExist**|3737-2146824551 0x800A0E99|L’enregistrement nommé par cette URL n’existe pas.|  
|**adErrVolumeNotFound**|3733-2146824555 0x800A0E95|Le fournisseur ne peut pas localiser le périphérique de stockage indiqué par l’URL. Assurez-vous que l’URL est correctement tapée.|  
|**adErrWriteFile**|3004-2146825284 0x800A0BBC|Échec de l’écriture dans le fichier.|  
|**adWrnSecurityDialog**|3717-2146824571 0x800A0E85|À usage interne uniquement. Ne pas utiliser.|  
|**adWrnSecurityDialogHeader**|3718-2146824570 0x800A0E86|À usage interne uniquement. Ne pas utiliser.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Package : **com. ms. wfc. Data**  
  
 Seuls les sous-ensembles suivants d’équivalents ADO/WFC sont définis.  
  
|Constante|  
|--------------|  
|AdoEnums.ErrorValue.BOUNDTOCOMMAND|  
|AdoEnums.ErrorValue.DATACONVERSION|  
|AdoEnums.ErrorValue.FEATURENOTAVAILABLE|  
|AdoEnums. ErrorValue. ILLEGALOPERATION|  
|AdoEnums.ErrorValue.INTRANSACTION|  
|AdoEnums.ErrorValue.INVALIDARGUMENT|  
|AdoEnums.ErrorValue.INVALIDCONNECTION|  
|AdoEnums.ErrorValue.INVALIDPARAMINFO|  
|AdoEnums.ErrorValue.ITEMNOTFOUND|  
|AdoEnums.ErrorValue.NOCURRENTRECORD|  
|AdoEnums.ErrorValue.NOTEXECUTING|  
|AdoEnums.ErrorValue.NOTREENTRANT|  
|AdoEnums.ErrorValue.OBJECTCLOSED|  
|AdoEnums.ErrorValue.OBJECTINCOLLECTION|  
|AdoEnums.ErrorValue.OBJECTNOTSET|  
|AdoEnums. ErrorValue. OBJECTOPEN|  
|AdoEnums.ErrorValue.OPERATIONCANCELLED|  
|AdoEnums.ErrorValue.PROVIDERNOTFOUND|  
|AdoEnums.ErrorValue.STILLCONNECTING|  
|AdoEnums. ErrorValue. STILLEXECUTING|  
|AdoEnums.ErrorValue.UNSAFEOPERATION|  
  
## <a name="applies-to"></a>S'applique à  
 [Number, propriété (ADO)](../../../ado/reference/ado-api/number-property-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Codes d’erreur ADO](../../../ado/guide/appendixes/ado-error-codes.md)
