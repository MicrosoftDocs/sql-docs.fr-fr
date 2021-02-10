---
description: Constantes énumérées ADO
title: Constantes énumérées ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- enumerated constants [ADO]
ms.assetid: c97ed131-1a93-463c-9e61-22f029b0c474
author: rothja
ms.author: jroth
ms.openlocfilehash: a0295fd9f1feec8db54734a5dfc02eba483e1615
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031281"
---
# <a name="ado-enumerated-constants"></a>Constantes énumérées ADO
Pour faciliter le débogage, les énumérations ADO répertorient une valeur pour chaque constante. Toutefois, cette valeur est purement indicative et peut passer d’une version d’ADO à une autre. Votre code ne doit dépendre que du nom, et non de la valeur réelle, de chaque constante énumérée.  
  
|Constante|Description|  
|-|-|  
|[ADCPROP_ASYNCTHREADPRIORITY_ENUM](./adcprop-asyncthreadpriority-enum.md)|Pour un objet **Recordset** RDS, spécifie la priorité d’exécution du thread asynchrone qui récupère les données.|  
|[ADCPROP_AUTORECALC_ENUM](./adcprop-autorecalc-enum.md)|Spécifie quand le fournisseur **MSDataShape** recalcule les colonnes agrégées et calculées dans un **jeu d’enregistrements** hiérarchique.|  
|[ADCPROP_UPDATECRITERIA_ENUM](./adcprop-updatecriteria-enum.md)|Spécifie les champs qui peuvent être utilisés pour détecter les conflits pendant une mise à jour optimiste d’une ligne de la source de données avec un objet **Recordset** .|  
|[ADCPROP_UPDATERESYNC_ENUM](./adcprop-updateresync-enum.md)|Spécifie si la méthode **UpdateBatch** est suivie d’une opération de méthode de **resynchronisation** implicite et, le cas échéant, de l’étendue de cette opération.|  
|[AffectEnum](./affectenum.md)|Spécifie les enregistrements qui sont affectés par une opération.|  
|[BookmarkEnum](./bookmarkenum.md)|Spécifie un signet qui indique où l’opération doit commencer.|  
|[CommandTypeEnum](./commandtypeenum.md)|Spécifie comment un argument de commande doit être interprété.|  
|[CompareEnum](./compareenum.md)|Spécifie la position relative de deux enregistrements représentés par leurs signets.|  
|[ConnectModeEnum](./connectmodeenum.md)|Spécifie les autorisations disponibles pour la modification des données dans une **connexion**, l’ouverture d’un **enregistrement** ou la spécification de valeurs pour la propriété **mode** des objets **Record** et **Stream** .|  
|[ConnectOptionEnum](./connectoptionenum.md)|Spécifie si la méthode **Open** d’un objet **Connection** doit être retournée après (de façon synchrone) ou avant (de façon asynchrone) la connexion est établie.|  
|[ConnectPromptEnum](./connectpromptenum.md)|Spécifie si une boîte de dialogue doit être affichée pour demander des paramètres manquants lors de l’ouverture d’une connexion à une source de données ODBC.|  
|[CopyRecordOptionsEnum](./copyrecordoptionsenum.md)|Spécifie le comportement de la méthode **CopyRecord** .|  
|[CursorLocationEnum](./cursorlocationenum.md)|Spécifie l’emplacement du moteur de curseur.|  
|[CursorOptionEnum](./cursoroptionenum.md)|Spécifie les fonctionnalités que la méthode de **prise en charge** doit tester.|  
|[CursorTypeEnum](./cursortypeenum.md)|Spécifie le type de curseur utilisé dans un objet **Recordset** .|  
|[DataTypeEnum](./datatypeenum.md)|Spécifie le type de données d’un **champ**, d’un **paramètre** ou d’une **propriété**.|  
|[EditModeEnum](./editmodeenum.md)|Spécifie l’état de modification d’un enregistrement.|  
|[ErrorValueEnum](./errorvalueenum.md)|Spécifie le type d’erreur d’exécution ADO.|  
|[EventReasonEnum](./eventreasonenum.md)|Spécifie la raison pour laquelle un événement a eu lieu.|  
|[EventStatusEnum](./eventstatusenum.md)|Spécifie l’état actuel de l’exécution d’un événement.|  
|[ExecuteOptionEnum](./executeoptionenum.md)|Spécifie comment un fournisseur doit exécuter une commande.|  
|[FieldEnum](./fieldenum.md)|Spécifie les champs spéciaux référencés dans la collection **Fields** d’un objet **Record** .|  
|[FieldAttributeEnum](./fieldattributeenum.md)|Spécifie un ou plusieurs attributs d’un objet de **champ** .|  
|[FieldStatusEnum](./fieldstatusenum.md)|Spécifie l’état d’un objet de **champ** .|  
|[FilterGroupEnum](./filtergroupenum.md)|Spécifie le groupe d’enregistrements à filtrer à partir d’un **jeu d’enregistrements**.|  
|[GetRowsOptionEnum](./getrowsoptionenum.md)|Spécifie le nombre d’enregistrements à récupérer à partir d’un **jeu d’enregistrements**.|  
|[IsolationLevelEnum](./isolationlevelenum.md)|Spécifie le niveau d’isolation des transactions pour un objet de **connexion** .|  
|[LineSeparatorsEnum](./lineseparatorsenum.md)|Spécifie le caractère utilisé comme séparateur de lignes dans les objets de **flux** de texte.|  
|[LockTypeEnum](./locktypeenum.md)|Spécifie le type de verrou placé sur les enregistrements lors de la modification.|  
|[MarshalOptionsEnum](./marshaloptionsenum.md)|Spécifie les enregistrements qui doivent être renvoyés au serveur.|  
|[MoveRecordOptionsEnum](./moverecordoptionsenum.md)|Spécifie le comportement de la méthode **MoveRecord** de l’objet **Record** .|  
|[ObjectStateEnum](./objectstateenum.md)|Spécifie si un objet est ouvert ou fermé, s’il se connecte à une source de données, exécute une commande ou extrait des données.|  
|[ParameterAttributesEnum](./parameterattributesenum.md)|Spécifie les attributs d’un objet de **paramètre** .|  
|[ParameterDirectionEnum](./parameterdirectionenum.md)|Spécifie si le **paramètre** représente un paramètre d’entrée, un paramètre de sortie, ou les deux, ou si le paramètre est la valeur de retour d’une procédure stockée.|  
|[PersistFormatEnum](./persistformatenum.md)|Spécifie le format d’enregistrement d’un **jeu d’enregistrements**.|  
|[PositionEnum](./positionenum.md)|Spécifie la position actuelle du pointeur d’enregistrement dans un **Recordset**.|  
|[PropertyAttributesEnum](./propertyattributesenum.md)|Spécifie les attributs d’un objet de **propriété** .|  
|[RecordCreateOptionsEnum](./recordcreateoptionsenum.md)|Spécifie la méthode d' **ouverture** de l’objet d' **enregistrement** si un **enregistrement** existant doit être ouvert ou si un nouvel **enregistrement** doit être créé.|  
|[RecordOpenOptionsEnum](./recordopenoptionsenum.md)|Spécifie les options d’ouverture d’un **enregistrement**. Ces valeurs peuvent être combinées à l’aide d’un opérateur OR.|  
|[RecordStatusEnum](./recordstatusenum.md)|Spécifie l’état d’un enregistrement en ce qui concerne les mises à jour par lots et les autres opérations en bloc.|  
|[RecordTypeEnum](./recordtypeenum.md)|Spécifie le type d’objet d' **enregistrement** .|  
|[ResyncEnum](./resyncenum.md)|Spécifie si les valeurs sous-jacentes sont remplacées par un appel à **Resync**.|  
|[SaveOptionsEnum](./saveoptionsenum.md)|Spécifie si un fichier doit être créé ou remplacé lors de l’enregistrement à partir d’un objet de **flux** . Les valeurs peuvent être combinées avec un opérateur AND.|  
|[SchemaEnum](./schemaenum.md)|Spécifie le type de **jeu d’enregistrements** de schéma que la méthode **OpenSchema** récupère. Spécifie la direction d’une recherche d’enregistrement dans un **Recordset**.|  
|[SearchDirectionEnum](./searchdirectionenum.md)|Spécifie la direction d’une recherche d’enregistrement dans un **Recordset**. Spécifie le type de **recherche** à exécuter.|  
|[SeekEnum](./seekenum.md)|Spécifie le type de **recherche** à exécuter. Spécifie les options d’ouverture d’un objet de **flux** . Les valeurs peuvent être combinées avec un opérateur AND.|  
|[StreamOpenOptionsEnum](./streamopenoptionsenum.md)|Spécifie les options d’ouverture d’un objet de **flux** . Les valeurs peuvent être combinées avec un opérateur AND. Spécifie si l’intégralité du flux ou la ligne suivante doit être lue à partir d’un objet de **flux** .|  
|[StreamReadEnum](./streamreadenum.md)|Spécifie si l’intégralité du flux ou la ligne suivante doit être lue à partir d’un objet de **flux** . Spécifie le type de données stockées dans un objet de **flux** .|  
|[StreamTypeEnum](./streamtypeenum.md)|Spécifie le type de données stockées dans un objet de **flux** . Spécifie si un séparateur de ligne est ajouté à la chaîne écrite dans un objet de **flux** .|  
|[StreamWriteEnum](./streamwriteenum.md)|Spécifie si un séparateur de ligne est ajouté à la chaîne écrite dans un objet de **flux** . Spécifie le format lors de la récupération d’un **jeu d’enregistrements** sous forme de chaîne.|  
|[StringFormatEnum](./stringformatenum.md)|Spécifie le format lors de la récupération d’un **jeu d’enregistrements** sous forme de chaîne. Spécifie les attributs de transaction d’un objet de **connexion** .|  
|[XactAttributeEnum](./xactattributeenum.md)|Spécifie les attributs de transaction d’un objet de **connexion** .|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ADO](./ado-api-reference.md)   
 [Collections ADO](./ado-collections.md)   
 [Propriétés dynamiques ADO](./ado-dynamic-properties.md)   
 [Annexe B : erreurs ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [Événements ADO](./ado-events.md)   
 [Méthodes ADO](./ado-methods.md)   
 [Modèle objet ADO](./ado-object-model.md)   
 [Objets et interfaces ADO](./ado-objects-and-interfaces.md)   
 [Propriétés ADO](./ado-properties.md)