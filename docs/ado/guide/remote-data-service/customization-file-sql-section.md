---
description: Fichier de personnalisation, section SQL
title: Section SQL du fichier de personnalisation | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL section in RDS [ADO]
- customization file in RDS [ADO]
ms.assetid: e65c2871-9986-44ff-b8b7-7f5eda91b3fa
author: rothja
ms.author: jroth
ms.openlocfilehash: bc9b9415628f27cd68cecbf02de2c52165c8d509
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031991"
---
# <a name="customization-file-sql-section"></a>Fichier de personnalisation, section SQL
La section **SQL** peut contenir une nouvelle chaîne SQL qui remplace la chaîne de commande du client. S’il n’existe aucune chaîne SQL dans la section, la section sera ignorée.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 La nouvelle chaîne SQL peut être *paramétrable*. Autrement dit, les paramètres de la chaîne SQL de la section **SQL** (désignés par le caractère «  ? ») peuvent être remplacés par des arguments correspondants dans un *identificateur* dans la chaîne de commande cliente (désignée par une liste délimitée par des virgules entre parenthèses). L’identificateur et la liste d’arguments se comportent comme un appel de fonction.  
  
 Par exemple, supposons que la chaîne de commande du client est `"CustomerByID(4)"` , que l’en-tête de la section SQL soit `[SQL CustomerByID]` et que la nouvelle chaîne de la section SQL soit `"SELECT * FROM Customers WHERE CustomerID = ?".` le gestionnaire génère `"SELECT * FROM Customers WHERE CustomerID = 4"` et utilise cette chaîne pour interroger la source de données.  
  
 Si la nouvelle instruction SQL est la chaîne null (""), la section est ignorée.  
  
 Si la nouvelle chaîne d’instruction SQL n’est pas valide, l’exécution de l’instruction échoue. Le paramètre client est effectivement ignoré. Vous pouvez effectuer cette opération intentionnellement pour « désactiver » toutes les commandes SQL clientes en spécifiant :  
  
```console
[SQL default]   
SQL = " "  
```  
  
## <a name="syntax"></a>Syntaxe  
 Une entrée de chaîne SQL de remplacement se présente sous la forme suivante :  
  
 **SQL =**   
 ***sqlString***  
  
|Partie|Description|  
|----------|-----------------|  
|**SQL**|Chaîne littérale qui indique qu’il s’agit d’une entrée de section SQL.|  
|***sqlString***|Chaîne SQL qui remplace la chaîne du client.|  
  
## <a name="see-also"></a>Voir aussi  
 [Section de connexion au fichier de personnalisation](./customization-file-connect-section.md)   
 [Section journaux des fichiers de personnalisation](./customization-file-logs-section.md)   
 [Section UserList du fichier de personnalisation](./customization-file-userlist-section.md)   
 [Personnalisation de DataFactory](./datafactory-customization.md)   
 [Paramètres client requis](./required-client-settings.md)   
 [Fonctionnement du fichier de personnalisation](./understanding-the-customization-file.md)   
 [Écriture d’un gestionnaire personnalisé](./writing-your-own-customized-handler.md)