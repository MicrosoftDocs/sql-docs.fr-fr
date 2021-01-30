---
description: Méthodes ADO
title: Méthodes ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- ADO, methods
- methods [ADO]
ms.assetid: a38c5670-ba28-44f3-bd5b-fcb46880e904
author: rothja
ms.author: jroth
ms.openlocfilehash: dd98478cad7c13b2f4e6950586c1317c1d5d37dd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164911"
---
# <a name="ado-methods"></a>Méthodes ADO

|Méthode|Description|  
|-|-|  
|[AddNew](./addnew-method-ado.md)|Crée un nouvel enregistrement pour un objet **Recordset** pouvant être mis à jour.|  
|[Append](./append-method-ado.md)|Ajoute un objet à une collection. Si la collection est de **champs**, un nouvel objet de **champ** peut être créé avant d’être ajouté à la collection.|  
|[AppendChunk](./appendchunk-method-ado.md)|Ajoute des données à un **champ** de données de texte ou binaires volumineux, ou à un objet de **paramètre** .|  
|[BeginTrans, CommitTrans et RollbackTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md)|Gère le traitement des transactions dans un objet de **connexion** comme suit :<br /><br /> **BeginTrans** -commence une nouvelle transaction.<br /><br /> **CommitTrans** : enregistre les modifications et met fin à la transaction en cours. Elle peut également démarrer une nouvelle transaction.<br /><br /> **RollbackTrans** -annule toutes les modifications et met fin à la transaction en cours. Elle peut également démarrer une nouvelle transaction.|  
|[Annuler](./cancel-method-ado.md)|Annule l’exécution d’un appel de méthode asynchrone en attente.|  
|[CancelBatch](./cancelbatch-method-ado.md)|Annule une mise à jour par lot en attente.|  
|[CancelUpdate](./cancelupdate-method-ado.md)|Annule toutes les modifications apportées à la ligne actuelle ou nouvelle d’un objet **Recordset** , ou à la collection **Fields** d’un objet **Record** , avant d’appeler la méthode **Update** .|  
|[Effacer](./clear-method-ado.md)|Supprime tous les objets **Error** de la collection **Errors** .|  
|[Répliqué](./clone-method-ado.md)|Crée un objet **Recordset** dupliqué à partir d’un objet **Recordset** existant. Spécifie éventuellement que le clone doit être en lecture seule.|  
|[Close](./close-method-ado.md)|Ferme un objet ouvert et tous les objets dépendants.|  
|[CompareBookmarks,](./comparebookmarks-method-ado.md)|Compare deux signets et retourne une indication de leurs valeurs relatives.|  
|[CopyRecord](./copyrecord-method-ado.md)|Copie un fichier ou un répertoire, ainsi que son contenu, vers un autre emplacement.|  
|[CopyTo](./copyto-method-ado.md)|Copie le nombre spécifié de caractères ou d’octets (selon le **type**) dans le **flux** vers un autre objet de **flux** .|  
|[CreateParameter](./createparameter-method-ado.md)|Crée un objet de **paramètre** qui a les propriétés spécifiées.|  
|[Delete (collection de paramètres ADO)](./delete-method-ado-parameters-collection.md)|Supprime un objet de la collection de **paramètres** .|  
|[Delete (collection de champs ADO)](./delete-method-ado-fields-collection.md)|Supprime un objet de la collection de **champs** .|  
|[Delete (Recordset ADO)](./delete-method-ado-recordset.md)|Supprime l’enregistrement en cours ou un groupe d’enregistrements.|  
|[DeleteRecord](./deleterecord-method-ado.md)|Supprime un fichier ou un répertoire, ainsi que tous ses sous-répertoires.|  
|[Execute (commande ADO)](./execute-method-ado-command.md)|Exécute la requête, l’instruction SQL ou la procédure stockée spécifiée dans la propriété **CommandText** .|  
|[Execute (connexion ADO)](./execute-method-ado-connection.md)|Exécute la requête, l’instruction SQL, la procédure stockée ou le texte spécifique au fournisseur spécifié.|  
|[Rechercher](./find-method-ado.md)|Recherche dans un **Recordset** la ligne qui répond aux critères spécifiés.|  
|[Purge](./flush-method-ado.md)|Force le contenu du **flux** restant dans la mémoire tampon ADO à l’objet sous-jacent auquel le **flux** est associé.|  
|[get_OLEDBCommand, méthode](./get-oledbcommand-method.md)|Retourne la commande OLEDB sous-jacente, en propageant tout d’abord les informations sur les paramètres définis sur la commande ADO à la commande OLEDB.|  
|[GetChildren](./getchildren-method-ado.md)|Retourne un **jeu d’enregistrements** dont les lignes représentent les fichiers et les sous-répertoires du répertoire représenté par cet **enregistrement**.|  
|[GetChunk](./getchunk-method-ado.md)|Retourne l’intégralité ou une partie du contenu d’un objet de texte ou de **champ** de données binaire volumineux.|  
|[GetDataProviderDSO, méthode](./getdataproviderdso-method.md)|Récupère l’objet source de données OLEDB sous-jacent à partir du fournisseur de formes.|  
|[GetRows](./getrows-method-ado.md)|Récupère plusieurs enregistrements d’un objet **Recordset** dans un tableau.|  
|[GetString](./getstring-method-ado.md)|Retourne le **Recordset** sous forme de chaîne.|  
|[LoadFromFile](./loadfromfile-method-ado.md)|Charge le contenu d’un fichier existant dans un **flux**.|  
|[Déplacer](./move-method-ado.md)|Déplace la position de l’enregistrement actuel dans un objet **Recordset** .|  
|[MoveFirst, MoveLast, MoveNext et MovePrevious](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|Passe au premier enregistrement, dernier, suivant ou précédent dans un objet **Recordset** spécifié et convertit cet enregistrement en enregistrement actif.|  
|[MoveRecord,](./moverecord-method-ado.md)|Déplace un fichier, ou un répertoire et son contenu, vers un autre emplacement.|  
|[NextRecordset](./nextrecordset-method-ado.md)|Efface l’objet **Recordset** actuel et retourne le **jeu d’enregistrements** suivant en progressant dans une série de commandes.|  
|[Open (connexion ADO)](./open-method-ado-connection.md)|Ouvre une connexion à une source de données.|  
|[Open (enregistrement ADO)](./open-method-ado-record.md)|Ouvre un objet **enregistrement** existant ou crée un fichier ou un répertoire.|  
|[Open (Recordset ADO)](./open-method-ado-recordset.md)|Ouvre un curseur.|  
|[Open (objet Stream ADO)](./open-method-ado-stream.md)|Ouvre un objet de **flux** pour manipuler des flux de données binaires ou de texte.|  
|[OpenSchema](./openschema-method.md)|Obtient des informations de schéma de base de données à partir du fournisseur.|  
|[put_OLEDBCommand, méthode](./put-oledbcommand-method.md)|Cette méthode n’effectue aucune opération. elle retourne toujours S_OK.|  
|[Lire](./read-method.md)|Lit un nombre spécifié d’octets à partir d’un objet de **flux** .|  
|[ReadText](./readtext-method.md)|Lit un nombre spécifié de caractères à partir d’un objet de **flux** de texte.|  
|[Actualiser](./refresh-method-ado.md)|Met à jour les objets d’une collection pour refléter les objets disponibles et spécifiques au fournisseur.|  
|[Requery](./requery-method.md)|Met à jour les données dans un objet **Recordset** en exécutant à nouveau la requête sur laquelle l’objet est basé.|  
|[Resynchroniser](./resync-method.md)|Actualise les données de la base de données sous-jacente de l’objet **Recordset** actuel ou de la collection **Fields** d’un objet **Record** .|  
|[Save](./save-method.md)|Enregistre le **Recordset** dans un objet de fichier ou de **flux** .|  
|[SaveToFile](./savetofile-method.md)|Enregistre le contenu binaire d’un **flux** dans un fichier.|  
|[Seek](./seek-method.md)|Recherche l’index d’un **jeu d’enregistrements** pour localiser rapidement la ligne qui correspond aux valeurs spécifiées et modifie la position de ligne actuelle en cette ligne.|  
|[SetEOS](./seteos-method.md)|Définit la position qui correspond à la fin du flux.|  
|[SkipLine](./skipline-method.md)|Ignore une ligne entière lors de la lecture d’un flux de texte.|  
|[Stat](./stat-method.md)|Obtient des informations statistiques sur un flux ouvert.|  
|[Prise en charge](./supports-method.md)|Détermine si un objet **Recordset** spécifié prend en charge un type particulier de fonctionnalité.|  
|[Mettre à jour](./update-method.md)|Enregistre les modifications apportées à la ligne actuelle d’un objet **Recordset** ou à la collection **Fields** d’un objet **Record** .|  
|[UpdateBatch](./updatebatch-method.md)|Écrit toutes les mises à jour par lots en attente sur le disque.|  
|[Écrire](./write-method.md)|Écrit des données binaires dans un objet de **flux** .|  
|[WriteText](./writetext-method.md)|Écrit une chaîne de texte spécifiée dans un objet de **flux** .|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ADO](./ado-api-reference.md)   
 [Collections ADO](./ado-collections.md)   
 [Propriétés dynamiques ADO](./ado-dynamic-properties.md)   
 [Constantes énumérées ADO](./ado-enumerated-constants.md)   
 [Annexe B : erreurs ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [Événements ADO](./ado-events.md)   
 [Modèle objet ADO](./ado-object-model.md)   
 [Objets et interfaces ADO](./ado-objects-and-interfaces.md)   
 [Propriétés ADO](./ado-properties.md)