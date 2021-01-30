---
description: ConvertToString, méthode (RDS)
title: ConvertToString, méthode (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- ConvertToString method [ADO]
ms.assetid: b3f36bc8-6f69-49b0-83cd-2ccd3afebfbe
author: rothja
ms.author: jroth
ms.openlocfilehash: 4edcebbfdee8a9a1411189786593c129ee32771c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169041"
---
# <a name="converttostring-method-rds"></a>ConvertToString, méthode (RDS)
Convertit un [Recordset](../ado-api/recordset-object-ado.md) en une chaîne MIME qui représente les données du Recordset.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DataFactory.ConvertToString(Recordset)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *DataFactory*  
 Variable objet qui représente un objet [RDSServer. DataFactory](./datafactory-object-rdsserver.md) .  
  
 *Recordset*  
 Variable objet qui représente un objet **Recordset** .  
  
## <a name="remarks"></a>Notes  
 Avec les fichiers. asp, utilisez **ConvertToString** pour incorporer le **Recordset** dans une page HTML générée sur le serveur pour le transporter sur un ordinateur client.  
  
 **ConvertToString** charge d’abord le **jeu d’enregistrements** dans les tables de service de curseur, puis génère un flux au format MIME.  
  
 Sur le client, Remote Data Service peut convertir la chaîne MIME en un **jeu d’enregistrements** entièrement fonctionnel. Elle fonctionne bien pour gérer moins de 400 lignes de données, avec une largeur inférieure à 1024 octets par ligne. Vous ne devez pas l’utiliser pour la diffusion en continu des données BLOB et des jeux de résultats volumineux sur HTTP. Aucune compression de câble n’est effectuée sur la chaîne. par conséquent, les jeux de données volumineux mettent beaucoup de temps au transport via HTTP par rapport au format TableGram optimisé pour les câbles, défini et déployé par le service de données distant en tant que format de protocole de transport natif.  
  
> [!NOTE]
>  Si vous utilisez Active Server pages pour incorporer la chaîne MIME qui en résulte dans une page HTML cliente, sachez que les versions de VBScript antérieures à la version 2,0 limitent la taille de la chaîne à 32 Ko. Si cette limite est dépassée, une erreur est retournée. Conservez la portée de requête relativement petite lorsque vous utilisez l’incorporation MIME via des fichiers. asp. Pour résoudre ce problème, téléchargez la dernière version de VBScript à partir du site Web Microsoft Windows Script technologies.  
  
## <a name="applies-to"></a>S'applique à  
 [DataFactory, objet (RDSServer)](./datafactory-object-rdsserver.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ConvertToString, exemple de méthode (VB)](../ado-api/converttostring-method-example-vb.md)   
 [ConvertToString, exemple de méthode (VBScript)](./converttostring-method-example-vbscript.md)