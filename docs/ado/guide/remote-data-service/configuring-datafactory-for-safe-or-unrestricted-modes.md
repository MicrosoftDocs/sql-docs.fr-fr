---
description: Configuration de DataFactory en mode sans échec ou unrestricted
title: Configuration de DataFactory pour les modes sécurisés ou non restreints | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- DataFactory configuration in RDS [ADO]
ms.assetid: 8ff24805-dc7a-42ae-b600-5bad0e3f51b8
author: rothja
ms.author: jroth
ms.openlocfilehash: a1727608476b8522f0e9ec6f09ce471de4ffb419
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032045"
---
# <a name="configuring-datafactory-for-safe-or-unrestricted-modes"></a>Configuration de DataFactory en mode sans échec ou unrestricted
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 Par défaut, ADO est installé avec une configuration [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) « sécurisée ». Le mode sans échec pour les composants serveur RDS signifie que les conditions suivantes sont remplies :  
  
1.  Le gestionnaire est requis avec RDSServer. DataFactory (cela est imposé par un paramètre de clé de registre).  
  
2.  Le gestionnaire par défaut, msdfmap. Handler, est inscrit, est présent dans la liste des gestionnaires sécurisés et est marqué comme gestionnaire par défaut.  
  
3.  Msdfmap.ini fichier est installé dans le répertoire Windows. Vous devez configurer ce fichier en fonction de vos besoins, avant d’utiliser RDS en mode à trois niveaux.  
  
 Si vous le souhaitez, vous pouvez configurer une installation non restreinte de **DataFactory** . **DataFactory** peut être utilisé directement sans le gestionnaire personnalisé. Les utilisateurs peuvent toujours utiliser un gestionnaire personnalisé en modifiant les chaînes de connexion, mais cela n’est pas obligatoire. Pour plus d’informations sur les implications de l’utilisation de l’objet **RDSServer. DataFactory** , consultez [sécurisation des applications RDS](./securing-rds-applications.md).  
  
 Le fichier de Registre handsafe. reg a été fourni pour configurer les entrées de Registre du gestionnaire pour une configuration sécurisée. Pour qu’elle s’exécute en mode sans échec, exécutez handsafe. reg.  
  
 Après l’exécution de handsafe. reg, vous devez arrêter et redémarrer le service de publication World Wide Web sur le serveur Web en tapant les commandes suivantes dans une fenêtre d’invite de commandes : « NET STOP W3SVC » et « NET START W3SVC ».  
  
## <a name="see-also"></a>Voir aussi  
 [Personnalisation de DataFactory](./datafactory-customization.md)   
 [Concepts de base de RDS](./rds-fundamentals.md)