---
description: Exécution de l’exemple d’application Carnet d’adresses
title: Exécution de l’exemple d’application de carnet d’adresses | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- address book application scenario [ADO]
- RDS scenarios [ADO]
ms.assetid: 3a2644e9-d634-4ae6-a5b7-13fb7b317ec7
author: rothja
ms.author: jroth
ms.openlocfilehash: b4e77339de5ec7ebddbdb1314228abf6eef8c15d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031811"
---
# <a name="running-the-address-book-sample-application"></a>Exécution de l’exemple d’application Carnet d’adresses
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 Pour exécuter l’application Carnet d’adresses, procédez comme suit.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
### <a name="to-run-this-application"></a>Pour exécuter cette application  
  
1.  Assurez-vous que Microsoft SQL Server est en cours d’exécution. Cliquez sur **Démarrer**, pointez sur tous les **programmes**, pointez sur **Microsoft SQL Server 7,0**, puis cliquez sur **Service Manager**. S’il y a une flèche verte dans le cercle blanc, SQL Server est en cours d’exécution. Si ce n’est pas le cas (il y aura un carré rouge dans le cercle blanc), cliquez sur **Démarrer/Continuer**.  
  
2.  Dans Microsoft Internet Explorer 4,0 ou version ultérieure, tapez l’adresse suivante :  
  
     **https://** *WebServer* **/RDS/AddressBook/AddrBook.asp**  
  
     où *serveur* Web est le nom du serveur Web sur lequel sont installés les composants du serveur RDS.  
  
3.  Vous pouvez ensuite essayer différents scénarios dans l’exemple d’application de carnet d’adresses, par exemple en recherchant une personne en fonction de son nom de messagerie électronique, en répertoriant toutes les personnes ayant le titre « responsable de programme » ou en modifiant des enregistrements existants. Cliquez sur **Rechercher** pour remplir la grille de données avec tous les noms disponibles.  
  
## <a name="see-also"></a>Voir aussi  
 [Objet de liaison de données de l’application Carnet d’adresses](./address-book-data-binding-object.md)