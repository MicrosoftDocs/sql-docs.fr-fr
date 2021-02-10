---
description: Informations de référence sur les erreurs ADO
title: Référence des erreurs ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO], number reference
- errors [ADO], ErrorValueEnum
- ErrorValueEnum enumeration [ADO]
ms.assetid: f653393e-d4b0-4c34-ad5f-2bdf56bc1305
author: rothja
ms.author: jroth
ms.openlocfilehash: cfb6a2ede665c939aa5824810c692bdbe02df6c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028371"
---
# <a name="ado-errors"></a>Erreurs ADO
La constante **ErrorValueEnum** décrit les valeurs d’erreur ADO. Pour obtenir la liste complète de ces constantes énumérées, y compris les valeurs, consultez l' [annexe B : erreurs ADO](../appendixes/appendix-b-ado-errors.md). Cette section examine certaines des erreurs les plus intéressantes et explique certaines situations spécifiques qui peuvent les déclencher, ou des solutions pour résoudre le problème. La constante **ErrorValueEnum** et le nombre décimal positif bref sont répertoriés.

|Nombre|ErrorValueEnum, constante|Description/causes possibles|
|------------|-----------------------------|----------------------------------|
|**3000**|**adErrProviderFailed**|Le fournisseur n’a pas pu effectuer l’opération demandée.|
|**3001**|**adErrInvalidArgument**|Les arguments sont de type incorrect, sont en dehors de la plage acceptable ou sont en conflit les uns avec les autres. Cette erreur est souvent due à une erreur typographique dans une instruction SQL SELECT. Par exemple, un nom de champ ou un nom de table mal orthographié peut générer cette erreur. Cette erreur peut également se produire lorsqu’un champ ou une table nommée dans une instruction SELECT n’existe pas dans le magasin de données.|
|**3002**|**adErrOpeningFile**|Impossible d’ouvrir le fichier. Un nom de fichier mal orthographié a été spécifié ou un fichier a été déplacé, renommé ou supprimé. Sur un réseau, le lecteur peut être temporairement indisponible ou le trafic réseau peut empêcher une connexion.|
|**3003**|**adErrReadFile**|Impossible de lire le fichier. Le nom du fichier est spécifié de manière incorrecte, le fichier a peut-être été déplacé ou supprimé, ou le fichier a peut-être été endommagé.|
|**3004**|**adErrWriteFile**|Échec de l’écriture dans le fichier. Vous avez peut-être fermé un fichier, puis essayé d’écrire dans celui-ci, ou le fichier est peut-être endommagé. Si le fichier se trouve sur un lecteur réseau, des conditions de réseau transitoires peuvent empêcher l’écriture sur un lecteur réseau.|
|**3021**|**adErrNoCurrentRecord**|**BOF** ou **EOF** a la valeur true ou l’enregistrement en cours a été supprimé. L’opération demandée requiert un enregistrement en cours.<br /><br /> Une tentative de mise à jour des enregistrements a été effectuée à l’aide de **Find** ou **Seek** pour déplacer le pointeur d’enregistrement vers l’enregistrement souhaité. Si l’enregistrement est introuvable, **EOF** prend la valeur true. Cette erreur peut également se produire après l’échec de **AddNew** ou **Delete** , car il n’y a pas d’enregistrement en cours lorsque ces méthodes échouent.|
|**3219**|**adErrIllegalOperation**|L’opération n’est pas autorisée dans ce contexte.|
|**3220**|**adErrCantChangeProvider**|Le fournisseur fourni est différent de celui déjà utilisé.|
|**3246**|**adErrInTransaction**|L’objet de **connexion** ne peut pas être explicitement fermé dans une transaction. Impossible de fermer un objet **Recordset** ou **Connection** qui participe actuellement à une transaction. Appelez **RollbackTrans** ou **CommitTrans** avant de fermer l’objet.|
|**3251**|**adErrFeatureNotAvailable**|L’objet ou le fournisseur ne peut pas effectuer l’opération demandée. Certaines opérations dépendent d’une version de fournisseur particulière.|
|**3265**|**adErrItemNotFound**|L’élément est introuvable dans la collection correspondant au nom ou à l’ordinal demandé. Un champ ou un nom de table incorrect a été spécifié.|
|**3367**|**adErrObjectInCollection**|L’objet est déjà dans la collection. Impossible d’ajouter. Un objet ne peut pas être ajouté à la même collection à deux reprises.|
|**3420**|**adErrObjectNotSet**|L’objet n’est plus valide.|
|**3421**|**adErrDataConversion**|L’application utilise une valeur de type incorrect pour l’opération en cours. Vous avez peut-être fourni une chaîne à une opération qui attend un flux, par exemple.|
|**3704**|**adErrObjectClosed**|L’opération n’est pas autorisée lorsque l’objet est fermé. La **connexion** ou l’ensemble **d’enregistrements** a été fermé. Par exemple, une autre routine peut avoir fermé un objet global. Vous pouvez éviter cette erreur en vérifiant la propriété **State** avant de tenter une opération.|
|**3705**|**adErrObjectOpen**|L’opération n’est pas autorisée lorsque l’objet est ouvert. Impossible d’ouvrir un objet ouvert. Les champs ne peuvent pas être ajoutés à un **Recordset** ouvert.|
|**3706**|**adErrProviderNotFound**|Le fournisseur est introuvable. Il n’est peut-être pas installé correctement.<br /><br /> Le nom du fournisseur peut être spécifié de manière incorrecte, le fournisseur spécifié n’est peut-être pas installé sur l’ordinateur sur lequel votre code est en cours d’exécution, ou l’installation a peut-être été endommagée.|
|**3707**|**adErrBoundToCommand**|La propriété **ActiveConnection** d’un objet **Recordset** , qui a un objet **Command** comme source, ne peut pas être modifiée. L’application a tenté d’assigner un nouvel objet de **connexion** à un **jeu d’enregistrements** dont la source est un objet de **commande** .|
|**3708**|**adErrInvalidParamInfo**|L’objet de **paramètre** n’est pas correctement défini. Des informations incohérentes ou incomplètes ont été fournies.|
|**3709**|**adErrInvalidConnection**|La connexion ne peut pas être utilisée pour effectuer cette opération. Il est fermé ou non valide dans ce contexte.|
|**3710**|**adErrNotReentrant**|Impossible d’effectuer l’opération lors du traitement de l’événement. Une opération ne peut pas être effectuée dans un gestionnaire d’événements qui provoque le déclenchement de l’événement. Par exemple, les méthodes de navigation ne doivent pas être appelées à partir d’un gestionnaire d’événements **WillMove** .|
|**3711**|**adErrStillExecuting**|Impossible d’effectuer l’opération lors de l’exécution de manière asynchrone.|
|**3712**|**adErrOperationCancelled**|L’opération a été annulée par l’utilisateur. L’application a appelé la méthode **CancelUpdate** ou **CancelBatch** et l’opération en cours a été annulée.|
|**3713**|**adErrStillConnecting**|Impossible d’effectuer l’opération lors de la connexion asynchrone.|
|**3714**|**adErrInvalidTransaction**|La coordination de la transaction n’est pas valide ou n’a pas démarré.|
|**3715**|**adErrNotExecuting**|Impossible d’effectuer l’opération pendant qu’elle n’est pas exécutée.|
|**3716**|**adErrUnsafeOperation**|Les paramètres de sécurité de cet ordinateur interdisent l’accès à une source de données d’un autre domaine.|
|**3717**|**adWrnSecurityDialog**|À usage interne uniquement. N’utilisez pas. (L’entrée a été incluse pour des raisons d’exhaustivité. Cette erreur ne doit pas apparaître dans votre code.)|
|**3718**|**adWrnSecurityDialogHeader**|À usage interne uniquement. N’utilisez pas. (Entrée incluse pour des raisons d’exhaustivité. Cette erreur ne doit pas apparaître dans votre code.)|
|**3719**|**adErrIntegrityViolation**|La valeur des données est en conflit avec les contraintes d’intégrité du champ. Une nouvelle valeur pour un **champ** provoquerait une clé dupliquée. Une valeur qui forme un côté d’une relation entre deux enregistrements peut ne pas pouvoir être mise à jour.|
|**3720**|**adErrPermissionDenied**|Les autorisations insuffisantes empêchent l’écriture dans le champ. L’utilisateur nommé dans la chaîne de connexion ne dispose pas des autorisations appropriées pour écrire dans un **champ**.|
|**3721**|**adErrDataOverflow**|La valeur des données est trop grande pour être représentée par le type de données du champ. Une valeur numérique qui est trop grande pour le champ prévu a été assignée. Par exemple, une valeur d’entier long a été assignée à un champ de type entier Short.|
|**3722**|**adErrSchemaViolation**|La valeur des données est en conflit avec le type de données ou les contraintes du champ. Le magasin de données a des contraintes de validation qui diffèrent de la valeur de **champ** .|
|**3723**|**adErrSignMismatch**|Échec de la conversion car la valeur des données était signée et le type de données du champ utilisé par le fournisseur n’était pas signé.|
|**3724**|**adErrCantConvertvalue**|La valeur des données ne peut pas être convertie pour des raisons autres que l’incompatibilité de signe ou le dépassement de données. Par exemple, la conversion aurait des données tronquées.|
|**3725**|**adErrCantCreate**|Impossible de définir ou de récupérer la valeur des données, car le type de données du champ est inconnu ou le fournisseur ne dispose pas de ressources suffisantes pour effectuer l’opération.|
|**3726**|**adErrColumnNotOnThisRow**|L’enregistrement ne contient pas ce champ. Un nom de champ incorrect a été spécifié ou un champ qui n’est pas dans la collection de **champs** de l’enregistrement en cours a été référencé.|
|**3727**|**adErrURLDoesNotExist**|L’URL source ou le parent de l’URL de destination n’existe pas. Il y a une erreur typographique dans l’URL source ou de destination. Vous avez peut-être besoin à la `https://mysite/photo/myphoto.jpg` `https://mysite/photos/myphoto.jpg` place de. L’erreur typographique dans l’URL parente (dans ce cas, *photo* au lieu de *photos*) a provoqué l’erreur.|
|**3728**|**adErrTreePermissionDenied**|Les autorisations sont insuffisantes pour accéder à l’arborescence ou à la sous-arborescence. L’utilisateur nommé dans la chaîne de connexion ne dispose pas des autorisations appropriées.|
|**3729**|**adErrInvalidURL**|L’URL contient des caractères non valides. Assurez-vous que l’URL est correctement tapée. L’URL suit le schéma inscrit au fournisseur actuel (par exemple, le fournisseur de publication Internet est inscrit pour http).|
|**3730**|**adErrResourceLocked**|L’objet représenté par l’URL spécifiée est verrouillé par un ou plusieurs autres processus. Attendez que le processus soit terminé, puis recommencez l’opération. L’objet auquel vous essayez d’accéder a été verrouillé par un autre utilisateur ou par un autre processus dans votre application. Cela peut se produire dans un environnement multi-utilisateur.|
|**3731**|**adErrResourceExists**|Impossible d’effectuer l’opération de copie. L’objet nommé par l’URL de destination existe déjà. Spécifiez **adCopyOverwrite** pour remplacer l’objet. Si vous ne spécifiez pas **adCopyOverwrite** lors de la copie des fichiers dans un répertoire, la copie échoue quand vous essayez de copier un élément qui existe déjà dans l’emplacement de destination.|
|**3732**|**adErrCannotComplete**|Le serveur ne peut pas terminer l’opération. Cela peut être dû au fait que le serveur est occupé par d’autres opérations ou que des ressources sont insuffisantes.|
|**3733**|**adErrVolumeNotFound**|Le fournisseur ne peut pas localiser le périphérique de stockage indiqué par l’URL. Assurez-vous que l’URL est correctement tapée. L’URL du périphérique de stockage est peut-être incorrecte, mais cette erreur peut se produire pour d’autres raisons. L’appareil peut être hors connexion ou un volume important de trafic réseau peut empêcher la connexion d’être établie.|
|**3734**|**adErrOutOfSpace**|Impossible d’effectuer l’opération. Le fournisseur ne peut pas obtenir suffisamment d’espace de stockage. Il se peut qu’il n’y ait pas suffisamment de RAM ou d’espace disque dur pour les fichiers temporaires sur le serveur.|
|**3735**|**adErrResourceOutOfScope**|L’URL source ou de destination est en dehors de l’étendue de l’enregistrement en cours.|
|**3736**|**adErrUnavailable**|L’opération a échoué et l’État n’est pas disponible. Le champ n’est peut-être pas disponible ou l’opération n’a pas été tentée. Un autre utilisateur a peut-être modifié ou supprimé le champ auquel vous essayez d’accéder.|
|**3737**|**adErrURLNamedRowDoesNotExist**|L’enregistrement nommé par cette URL n’existe pas. Lors de la tentative d’ouverture d’un fichier à l’aide d’un objet **Record** , le nom de fichier ou le chemin d’accès au fichier a été mal orthographié.|
|**3738**|**adErrDelResOutOfScope**|L’URL de l’objet à supprimer est en dehors de l’étendue de l’enregistrement en cours.|
|**3747**|**adErrCatalogNotSet**|L’opération requiert un **ParentCatalog** valide.|
|**3748**|**adErrCantChangeConnection**|La connexion a été refusée. La nouvelle connexion que vous avez demandée présente des caractéristiques différentes de celles déjà utilisées.|
|**3749**|**adErrFieldsUpdateFailed**|Échec de la mise à jour des champs. Pour plus d’informations, examinez la propriété **Status** des objets Field individuels. Cette erreur peut se produire dans deux situations : lors de la modification de la valeur d’un objet **champ** dans le processus de modification ou d’ajout d’un enregistrement à la base de données. et lors de la modification des propriétés de l’objet de **champ** lui-même.<br /><br /> Échec de la mise à jour de l' **enregistrement** ou de l’ensemble **d’enregistrements** en raison d’un problème avec l’un des champs de l’enregistrement en cours. Énumérez la collection de **champs** et vérifiez la propriété **Status** de chaque champ pour déterminer la cause du problème.|
|**3750**|**adErrDenyNotSupported**|Le fournisseur ne prend pas en charge les restrictions de partage. Une tentative a été effectuée pour restreindre le partage de fichiers et votre fournisseur ne prend pas en charge le concept.|
|**3751**|**adErrDenyTypeNotSupported**|Le fournisseur ne prend pas en charge le type de restriction de partage demandé. Une tentative a été effectuée pour établir un type particulier de restriction de partage de fichiers qui n’est pas prise en charge par votre fournisseur. Consultez la documentation du fournisseur pour déterminer quelles sont les restrictions de partage de fichiers prises en charge.|