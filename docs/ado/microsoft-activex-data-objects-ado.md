---
description: Microsoft ActiveX Data Objects (ADO)
title: Microsoft ActiveX Data Objects (ADO) | Microsoft Docs
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
helpviewer_keywords:
- ADO, about
ms.assetid: 2fa6237b-44b8-4b6c-9952-5acd80a54e20
author: rothja
ms.author: jroth
ms.openlocfilehash: 511b556971496633abd6d347ab416b75a8f9f27c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036039"
---
# <a name="microsoft-activex-data-objects-ado"></a>Microsoft ActiveX Data Objects (ADO)

ActiveX Data Objects est un modèle de programmation, ce qui signifie qu’il ne dépend d’aucun moteur principal donné. À l’heure actuelle, toutefois, le seul moteur prenant en charge le modèle ADO est OLE-DB. Il existe de nombreux fournisseurs OLE DB natifs, ainsi qu’un fournisseur OLE DB pour ODBC. ADO est utilisé dans les programmes C++ et Visual Basic pour se connecter à SQL Server et à d’autres bases de données. Bien entendu, il fonctionne également pour se connecter à Azure SQL Database dans le Cloud.

Chaque section de cet article décrit un composant d’ADO.

> [!NOTE]
> ADO.NET est différent d’ADO. ADO.NET, et de nombreux autres pilotes de connexion SQL et leurs langues, sont abordés à partir de [SQL Server pilotes](../connect/sql-connection-libraries.md).

  
## <a name="ado"></a>ADO  
 Microsoft ActiveX Data Objects (ADO) permet à vos applications clientes d’accéder à des données et de les manipuler à partir de diverses sources via un fournisseur de OLE DB. Ses principaux avantages sont la facilité d’utilisation, la vitesse élevée, la faible surcharge de mémoire et un faible encombrement du disque. ADO prend en charge des fonctionnalités clés pour la création d’applications client/serveur et Web.  
  
## <a name="ado-md"></a>ADO MD  
 Microsoft ActiveX Data Objects (Multidimensionnel) (ADO MD) offre un accès facile aux données multidimensionnelles à partir de langages tels que Microsoft Visual Basic et Microsoft Visual C++. ADO MD étend Microsoft ActiveX Data Objects (ADO) pour inclure des objets spécifiques aux données multidimensionnelles, tels que les objets CubeDef et CellSet. Avec ADO MD vous pouvez parcourir le schéma multidimensionnel, interroger un cube et récupérer les résultats.  
  
 Comme ADO, ADO MD utilise un fournisseur de OLE DB sous-jacent pour accéder aux données. Pour utiliser ADO MD, le fournisseur doit être un fournisseur de données multidimensionnelles (MDP) comme défini par le OLE DB pour la spécification OLAP. MDPs présente des données dans les vues multidimensionnelles au lieu des fournisseurs de données tabulaires (TDPs) qui présentent des données dans les vues tabulaires. Reportez-vous à la documentation de votre fournisseur de OLE DB OLAP pour obtenir des informations plus détaillées sur la syntaxe et les comportements spécifiques pris en charge par votre fournisseur.  
  
## <a name="rds"></a>RDS  
 RDS (Remote Data Service) est une fonctionnalité d’ADO, qui vous permet de déplacer des données d’un serveur vers une application cliente ou une page Web, de manipuler les données sur le client et de retourner des mises à jour au serveur en un seul aller-retour.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le  [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="adox"></a>ADOX  
 Microsoft ActiveX Data Objects Extensions pour la sécurité et le langage de définition de données (ADOX) est une extension du modèle de programmation et des objets ADO. ADOX comprend des objets pour la création et la modification d’un schéma, ainsi que pour la sécurité. Étant donné qu’il s’agit d’une approche basée sur les objets de la manipulation de schéma, vous pouvez écrire du code qui fonctionnera sur diverses sources de données, quelles que soient les différences dans leurs syntaxes natives.  
  
 ADOX est une bibliothèque associée aux objets ADO de base. Il expose des objets supplémentaires pour la création, la modification et la suppression d’objets de schéma, tels que des tables et des procédures. Il comprend également des objets de sécurité pour gérer les utilisateurs et les groupes, ainsi que pour accorder et révoquer des autorisations sur des objets.  
  
## <a name="documentation"></a>Documentation  
 [Problèmes de conception de la sécurité d’ADO](./guide/ado-security-design-issues.md)  
  
 [Guide du programmeur ADO](./guide/ado-programmer-s-guide.md)  
  
 Introduction à l’utilisation d’ADO, RDS, ADO MD et ADOX.  
  
 [Guide de référence du programmeur ADO](./reference/ado-programmer-s-reference.md)  
  
 Cette section de la documentation ADO contient des rubriques pour chaque objet, collection, propriété, propriété dynamique, méthode, événement et énumération ADO, RDS, ADO MD et ADOX.  
  
 [Glossaire ADO](./ado-glossary.md)  
  
## <a name="support"></a>Support  
 Pour obtenir une aide gratuite sur les problèmes liés à ADO, essayez de publier dans le groupe de discussion public ADO. Ce groupe de discussion est surveillé par les professionnels du support technique Microsoft qui couvrent ADO et par d’autres développeurs ADO expérimentés.  
  
 Vous trouverez plus d'informations sur les options de support sur le site Web Aide et support Microsoft.