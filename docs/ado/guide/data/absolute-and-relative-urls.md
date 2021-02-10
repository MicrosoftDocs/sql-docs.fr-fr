---
description: URL absolues et relatives
title: URL absolues et relatives | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- relative URLs [ADO]
- absolute URLs [ADO]
- URLs [ADO]
ms.assetid: 6a34a7ef-50cc-4c3d-82f7-106b9a8f3caf
author: rothja
ms.author: jroth
ms.openlocfilehash: 50535a71c1062bc04aa009bea1aab08d3cf1faf0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028694"
---
# <a name="absolute-and-relative-urls"></a>URL absolues et relatives
Une URL spécifie l’emplacement d’une cible stockée sur un ordinateur local ou en réseau. La cible peut être un fichier, un répertoire, une page HTML, une image, un programme, et ainsi de suite.  
  
 Une *URL absolue* contient toutes les informations nécessaires pour localiser une ressource.  
  
 Une *URL relative* localise une ressource à l’aide d’une URL absolue comme point de départ. En effet, l’URL complète de la cible est spécifiée par la concaténation des URL absolues et relatives.  
  
 Une *URL absolue* utilise le format suivant : *Scheme://Server/Path/Resource*  
  
 Une URL relative se compose généralement du *chemin d’accès* et, éventuellement, de la *ressource*, mais pas d’un *schéma* ou d’un *serveur*. Les tableaux suivants définissent les parties individuelles du format d’URL complet.  
  
 *mode*  
 Spécifie le mode d’accès à la *ressource* .  
  
 *server*  
 Spécifie le nom de l’ordinateur sur lequel se trouve la *ressource* .  
  
 *path*  
 Spécifie la séquence de répertoires menant à la cible. Si la *ressource* est omise, la cible est le dernier répertoire du *chemin d’accès*.  
  
 *resource*  
 S’il est inclus, la *ressource* est la cible, et est généralement le nom d’un fichier. Il peut s’agir d’un *fichier simple,* contenant un seul flux binaire d’octets, ou un *document structuré,* contenant un ou plusieurs stockages et flux binaires d’octets.  
  
## <a name="url-scheme-registration"></a>Inscription du schéma d’URL  
 Si un fournisseur prend en charge les URL, le fournisseur inscrit un ou plusieurs schémas d’URL. L’inscription signifie que toutes les URL utilisant le schéma appellera automatiquement le fournisseur inscrit. Par exemple, le schéma *http* est inscrit au [fournisseur Microsoft OLE DB pour la publication Internet](../appendixes/microsoft-ole-db-provider-for-internet-publishing.md). ADO suppose que toutes les URL avec le préfixe « http » représentent les dossiers ou les fichiers Web à utiliser avec le fournisseur de publication Internet. Pour plus d’informations sur les schémas enregistrés par votre fournisseur, consultez la documentation de votre fournisseur.  
  
## <a name="defining-context-with-a-url"></a>Définition du contexte avec une URL  
 L’une des fonctions d’une connexion ouverte, représentée par un objet de [connexion](../../reference/ado-api/connection-object-ado.md) , est de limiter les opérations ultérieures à la source de données représentée par cette connexion. Autrement dit, la connexion définit le contexte pour les opérations suivantes.  
  
 Avec ADO 2,7 ou une version ultérieure, une URL absolue peut également définir un contexte. Par exemple, lorsqu’un objet [Record](../../reference/ado-api/record-object-ado.md) est ouvert avec une URL absolue, un objet **Connection** est implicitement créé pour représenter la ressource spécifiée par l’URL.  
  
 Une URL absolue qui définit un contexte peut être spécifiée dans le paramètre *ActiveConnection* de la méthode [Open](../../reference/ado-api/open-method-ado-record.md) de l’objet **Record** . Une URL absolue peut également être spécifiée en tant que valeur du mot clé « URL = » dans le paramètre *ConnectionString* de la méthode [Open](../../reference/ado-api/open-method-ado-connection.md) de l’objet de **connexion** et du paramètre *ActiveConnection* de la méthode [Open](../../reference/ado-api/open-method-ado-recordset.md) de l’objet [Recordset](../../reference/ado-api/recordset-object-ado.md) .  
  
 Le contexte peut également être défini en ouvrant un **enregistrement** ou un objet **Recordset** qui représente un répertoire, car ces objets disposent déjà d’un objet de **connexion** implicitement ou explicitement déclaré qui spécifie Context.  
  
## <a name="scoped-operations"></a>Opérations délimitées  
 Le contexte définit également l’étendue, c’est-à-dire le répertoire et ses sous-répertoires qui peuvent participer aux opérations suivantes. L’objet **Record** a plusieurs méthodes délimitées qui fonctionnent sur un répertoire et tous ses sous-répertoires. Ces méthodes incluent [CopyRecord](../../reference/ado-api/copyrecord-method-ado.md), [MoveRecord](../../reference/ado-api/moverecord-method-ado.md)et [DeleteRecord](../../reference/ado-api/deleterecord-method-ado.md).  
  
## <a name="relative-urls-as-command-text"></a>URL relatives sous forme de texte de commande  
 Vous pouvez spécifier une commande à exécuter sur la source de données en tapant une chaîne dans le *paramètre CommandText* de la méthode [Execute](../../reference/ado-api/execute-method-ado-connection.md) de l’objet de **connexion** et dans le paramètre *source* de la méthode [Open](../../reference/ado-api/open-method-ado-recordset.md) de l’objet **Recordset** .  
  
 Une URL relative peut être spécifiée dans le paramètre *CommandText* ou *source* . L’URL relative ne représente pas en fait une commande, telle qu’une commande SQL. Il spécifie simplement les paramètres. Le contexte de la connexion active doit être une URL absolue et le paramètre d' *option* doit avoir la valeur **adCmdTableDirect**.  
  
 Par exemple, l’exemple de code suivant montre comment ouvrir un **Recordset** sur le fichier Readme25.txt du répertoire Winnt/system32 :  
  
```  
recordset.Open "system32/Readme25.txt", "URL=https://YourServer/Winnt/",,,adCmdTableDirect  
```  
  
 L’URL absolue dans la chaîne de connexion spécifie le serveur ( `YourServer` ) et le chemin d’accès ( `Winnt` ). Cette URL définit également le contexte.  
  
 L’URL relative dans le texte de la commande utilise l’URL absolue comme point de départ et spécifie le reste du chemin d’accès ( `system32` ) et le fichier à ouvrir ( `Readme25.txt` ).  
  
 Le champ d’options ( `adCmdTableDirect` ) indique que le type de commande est une URL relative.  
  
 En guise d’autre exemple, le code suivant ouvre un **jeu d’enregistrements** sur le contenu du `Winnt` répertoire :  
  
```  
recordset.Open "", "URL=https://YourServer/Winnt/",,,adCmdTableDirect  
```  
  
## <a name="ole-db-provider-supplied-url-schemes"></a>OLE DB Provider-Supplied des schémas d’URL  
 Le premier élément d’une URL qualifiée complète est le *schéma* utilisé pour accéder à la ressource identifiée par le reste de l’URL. Les exemples sont HTTP (Hypertext Transfer Protocol) et FTP (protocole FTP).  
  
 ADO prend en charge les fournisseurs OLE DB qui reconnaissent leurs propres schémas d’URL. Par exemple, le [fournisseur Microsoft OLE DB pour la publication Internet](../appendixes/microsoft-ole-db-provider-for-internet-publishing.md)*,* qui accède aux fichiers Windows 2000 « publiés », reconnaît le schéma http existant.  
  
## <a name="see-also"></a>Voir aussi  
 [Connection, objet (ADO)](../../reference/ado-api/connection-object-ado.md)   
 [Record, objet (ADO)](../../reference/ado-api/record-object-ado.md)   
 [Recordset, objet (ADO)](../../reference/ado-api/recordset-object-ado.md)