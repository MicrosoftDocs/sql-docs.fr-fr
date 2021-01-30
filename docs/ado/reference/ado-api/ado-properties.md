---
description: Propriétés ADO
title: Propriétés ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- properties [ADO]
- ADO properties
ms.assetid: 0ac0d1a7-6c7a-4f4c-b115-428935e0f98b
author: rothja
ms.author: jroth
ms.openlocfilehash: bdacd558161c09d1d43f590f9f355e14da262b14
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164891"
---
# <a name="ado-properties"></a>Propriétés ADO

|Propriété|Description|  
|-|-|  
|[AbsolutePage](./absolutepage-property-ado.md)|Indique sur quelle page se trouve l’enregistrement en cours.|  
|[AbsolutePosition](./absoluteposition-property-ado.md)|Indique la position ordinale de l’enregistrement actif d’un objet **Recordset** .|  
|[ActiveCommand](./activecommand-property-ado.md)|Indique l’objet de **commande** qui a créé l’objet **Recordset** associé.|  
|[ActiveConnection](./activeconnection-property-ado.md)|Indique à quel objet de **connexion** la **commande**, le **jeu d’enregistrements** ou l’objet d' **enregistrement** spécifiés appartiennent actuellement.|  
|[ActualSize](./actualsize-property-ado.md)|Indique la longueur réelle de la valeur d’un champ.|  
|[Attributs](./attributes-property-ado.md)|Indique une ou plusieurs caractéristiques d’un objet.|  
|[BOF et EOF](./bof-eof-properties-ado.md)|**BOF** indique que la position actuelle de l’enregistrement est antérieure au premier enregistrement d’un objet Recordset.<br /><br /> **EOF** indique que la position actuelle de l’enregistrement est après le dernier enregistrement d’un objet Recordset.|  
|[Signet](./bookmark-property-ado.md)|Indique un signet qui identifie de façon unique l’enregistrement en cours dans un objet **Recordset** ou définit l’enregistrement en cours dans un objet **Recordset** sur l’enregistrement identifié par un signet valide.|  
|[CacheSize](./cachesize-property-ado.md)|Indique le nombre d’enregistrements d’un objet **Recordset** mis en cache localement en mémoire.|  
|[Chapitre](./chapter-property-ado.md)|Obtient ou définit un objet OLE DB **chapitre** à partir de/sur un objet **ADORecordsetConstruction** .|  
|[CharSet](./charset-property-ado.md)|Indique le jeu de caractères dans lequel le contenu d’un **flux** de texte doit être traduit.|  
|[CommandStream](./commandstream-property-ado.md)|Indique le flux utilisé comme entrée d’un objet **Command** .|  
|[CommandText](./commandtext-property-ado.md)|Indique le texte d’une commande à délivrer à un fournisseur.|  
|[CommandTimeout](./commandtimeout-property-ado.md)|Indique le délai d’attente lors de l’exécution d’une commande avant de mettre fin à la tentative et de générer une erreur.|  
|[CommandType](./commandtype-property-ado.md)|Indique le type d’un objet **Command** .|  
|[Propriété ConnectionString](./connectionstring-property-ado.md)|Indique les informations utilisées pour établir une connexion à une source de données.|  
|[ConnectionTimeout](./connectiontimeout-property-ado.md)|Indique le délai d’attente lors de l’établissement d’une connexion avant que la tentative ne soit abandonnée et qu’une erreur ne soit générée.|  
|[Count](./count-property-ado.md)|Indique le nombre d’objets dans une collection.|  
|[Ait](./cursorlocation-property-ado.md)|Indique l’emplacement du service de curseur.|  
|[CursorType](./cursortype-property-ado.md)|Indique le type de curseur utilisé dans un objet **Recordset** .|  
|[DataMember](./datamember-property.md)|Indique le nom du membre de données qui sera récupéré à partir de l’objet référencé par la propriété **DataSource** .|  
|[DataSource](./datasource-property-ado.md)|Indique un objet qui contient les données à représenter sous la forme d’un objet **Recordset** .|  
|[DefaultDatabase](./defaultdatabase-property.md)|Indique la base de données par défaut pour un objet de **connexion** .|  
|[DefinedSize](./definedsize-property.md)|Indique la capacité de données d’un objet de **champ** .|  
|[Description](./description-property.md)|Décrit un objet d' **erreur** .|  
|[Dialecte](./dialect-property.md)|Indique la syntaxe et les règles générales que le fournisseur utilisera pour analyser les propriétés **CommandText** ou **CommandStream** .|  
|[Sens](./direction-property.md)|Indique si le **paramètre** représente un paramètre d’entrée, un paramètre de sortie, ou les deux, ou si le paramètre est la valeur de retour d’une procédure stockée.|  
|[EditMode](./editmode-property.md)|Indique l’état de modification de l’enregistrement en cours.|  
|[EOS](./eos-property.md)|Indique si la position actuelle est à la fin du flux.|  
|[Filter](./filter-property.md)|Indique un filtre pour les données d’un **jeu d’enregistrements**.|  
|[HelpContext et HelpFile](./helpcontext-helpfile-properties.md)|Indique le fichier d’aide et la rubrique associés à un objet d' **erreur** .<br /><br /> **HelpContextID** retourne un ID de contexte, sous la forme d’une valeur **long** , pour une rubrique dans un fichier d’aide.<br /><br /> **HelpFile** retourne une valeur de **chaîne** qui prend la valeur d’un chemin d’accès entièrement résolu d’un fichier d’aide.|  
|[Index](./index-property.md)|Indique le nom de l’index actuellement en vigueur pour un objet **Recordset** .|  
|[IsolationLevel](./isolationlevel-property.md)|Indique le niveau d’isolation d’un objet de **connexion** .|  
|[Item](./item-property-ado.md)|Indique un membre spécifique d’une collection, par nom ou numéro ordinal.|  
|[LineSeparator](./lineseparator-property-ado.md)|Indique le caractère binaire à utiliser comme séparateur de lignes dans les objets de **flux** de texte.|  
|[Verrou](./locktype-property-ado.md)|Indique le type de verrou placé sur les enregistrements lors de la modification.|  
|[MarshalOptions](./marshaloptions-property-ado.md)|Indique les enregistrements qui doivent être remarshalés sur le serveur.|  
|[MaxRecords](./maxrecords-property-ado.md)|Indique le nombre maximal d’enregistrements à retourner à un **jeu d’enregistrements** à partir d’une requête.|  
|[Mode](./mode-property-ado.md)|Indique les autorisations disponibles pour la modification des données dans une **connexion**, un **enregistrement** ou un objet de **flux** .|  
|[Nom](./name-property-ado.md)|Indique le nom d’un objet.|  
|[NativeError](./nativeerror-property-ado.md)|Indique le code d’erreur spécifique au fournisseur pour un objet d' **erreur** particulier.|  
|[Nombre](./number-property-ado.md)|Indique le nombre qui identifie de façon unique un objet d' **erreur** .|  
|[NumericScale](./numericscale-property-ado.md)|Indique l’échelle des valeurs numériques dans un objet de **paramètre** ou de **champ** .|  
|[OriginalValue](./originalvalue-property-ado.md)|Indique la valeur d’un **champ** qui existait dans l’enregistrement avant toute modification.|  
|[PageCount](./pagecount-property-ado.md)|Indique le nombre de pages de données que contient l’objet **Recordset** .|  
|[PageSize](./pagesize-property-ado.md)|Indique le nombre d’enregistrements qui représentent une page dans le **jeu d’enregistrements**.|  
|[ParentRow](./parentrow-property-ado.md)|Définit le conteneur d’un objet OLE DB **Row** sur un objet **ADORecordConstruction** , de sorte que le parent de la ligne soit converti en objet ADO **Record** .|  
|[ParentURL](./parenturl-property-ado.md)|Indique une chaîne d’URL absolue qui pointe vers l' **enregistrement** parent de l’objet **enregistrement** actif.|  
|[Position](./position-property-ado.md)|Indique la position actuelle dans un objet de **flux** .|  
|[Précision](./precision-property-ado.md)|Indique le degré de précision des valeurs numériques dans un objet de **paramètre** ou pour les objets de **champ** numérique.|  
|[Prepared](./prepared-property-ado.md)|Indique s’il faut enregistrer une version compilée d’une commande avant l’exécution.|  
|[Fournisseur](./provider-property-ado.md)|Indique le nom du fournisseur pour un objet de **connexion** .|  
|[RecordCount](./recordcount-property-ado.md)|Indique le nombre d’enregistrements dans un objet **Recordset** .|  
|[RecordType](./recordtype-property-ado.md)|Indique le type d’objet d' **enregistrement** .|  
|[Ligne](./row-property-ado.md)|Obtient ou définit un objet OLE DB **ligne** à partir de/sur un objet **ADORecordConstruction** .|  
|[RowPosition](./rowposition-property-ado.md)|Obtient ou définit un objet **RowPosition** OLE DB à partir de/sur un objet **ADORecordsetConstruction** .|  
|[Ensemble de lignes](./rowset-property-ado.md)|Obtient ou définit un objet d' **ensemble de lignes** OLE DB à partir de/sur un objet **ADORecordsetConstruction** .|  
|[Source (erreur ADO)](./source-property-ado-error.md)|Indique le nom de l’objet ou de l’application qui a généré à l’origine une erreur.|  
|[Source (enregistrement ADO)](./source-property-ado-record.md)|Indique l’entité représentée par l’objet **Record** .|  
|[Source (ADO Recordset)](./source-property-ado-recordset.md)|Indique la source des données dans un objet **Recordset**|  
|[SQLState](./sqlstate-property.md)|Indique l’état SQL d’un objet d' **erreur** spécifique.|  
|[State](./state-property-ado.md)|Indique pour tous les objets applicables si l’état de l’objet est ouvert ou fermé. Indique pour tous les objets applicables exécutant une méthode asynchrone, que l’état actuel de l’objet soit en cours de connexion, en cours d’exécution ou en cours d’extraction|  
|[Status (champ ADO)](./status-property-ado-field.md)|Indique l’état d’un objet de **champ** .|  
|[Status (Recordset ADO)](./status-property-ado-recordset.md)|Indique l’état de l’enregistrement en cours concernant les mises à jour par lots ou d’autres opérations en bloc.|  
|[StayInSync](./stayinsync-property.md)|Indique, dans un objet **Recordset** hiérarchique, si la référence aux enregistrements enfants sous-jacents (autrement dit, le *chapitre*) change lorsque la position de la ligne parente change.|  
|[Stream, propriété](./stream-property.md)|Obtient ou définit un objet de **flux** OLE DB à partir de/sur un objet **ADOStreamConstruction** .|  
|[Type](./type-property-ado.md)|Indique le type opérationnel ou le type de données d’un **paramètre**, d’un **champ** ou d’un objet de **propriété** .|  
|[Type (ADO Stream)](./type-property-ado-stream.md)|Indique le type des données contenues dans le **flux** (binaire ou texte).|  
|[UnderlyingValue,](./underlyingvalue-property.md)|Indique la valeur actuelle dans la base de données d’un objet de **champ** .|  
|[Valeur](./value-property-ado.md)|Indique la valeur assignée à un **champ**, un **paramètre** ou un objet de **propriété** .|  
|[Version](./version-property-ado.md)|Indique le numéro de version ADO.|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ADO](./ado-api-reference.md)   
 [Collections ADO](./ado-collections.md)   
 [Propriétés dynamiques ADO](./ado-dynamic-properties.md)   
 [Constantes énumérées ADO](./ado-enumerated-constants.md)   
 [Annexe B : erreurs ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [Événements ADO](./ado-events.md)   
 [Méthodes ADO](./ado-methods.md)   
 [Modèle objet ADO](./ado-object-model.md)   
 [Objets et interfaces ADO](./ado-objects-and-interfaces.md)