---
description: Scénario RDS
title: Scénario RDS | Microsoft Docs
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
ms.assetid: a7dcad87-aaf0-4b02-9660-472f8469761c
author: rothja
ms.author: jroth
ms.openlocfilehash: a61fc0351c7d7b7008ef34d048e66a39908562e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037579"
---
# <a name="rds-scenario"></a>Scénario RDS
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 L’application Carnet d’adresses est un scénario qui montre comment utiliser RDS (Remote Data Service) pour créer une application Web simple et orientée données, un carnet d’adresses d’entreprise en ligne. Ce scénario est utile pour les programmeurs Microsoft Visual Basic Scripting Edition (VBScript) et COM qui souhaitent apprendre à utiliser les contrôles ActiveX prenant en charge les données avec RDS, et pour les développeurs de logiciels plus expérimentés qui souhaitent créer des applications Web orientées données.  
  
 Ce scénario suppose que vous savez utiliser les balises de mise en page HTML de base, utiliser les techniques de liaison de données DHTML et programmer avec les contrôles ActiveX.  
  
 Si vous avez installé le kit de développement logiciel (SDK), vous trouverez le code source complet de l’exemple d’application de carnet d’adresses dans le répertoire du kit de développement logiciel (SDK) sur samples\dataaccess\rds\AddressBook\AddressBook.asp. Pour afficher le scénario de carnet d’adresses, dans Internet Explorer 4,0 ou version ultérieure, tapez **https:///RDS/AddressBook/addressbook.asp** , où *serveur* Web est le nom donné à votre ordinateur serveur Web Windows NT 4,0 ou Windows 2000 exécutant Internet Information Services (IIS) et ASP.  
  
## <a name="introduction-to-address-book"></a>Présentation du carnet d’adresses  
 L’exemple d’application Carnet d’adresses fournit un carnet d’adresses en ligne simple que vous pouvez utiliser pour publier un répertoire pouvant faire l’objet d’une recherche sur un intranet. Le carnet d’adresses est conçu pour permettre à un utilisateur d’entrer une chaîne de recherche dans un ou plusieurs champs pour demander des informations sur les employés. Pour vous montrer les fonctionnalités de base du service de données à distance, l’exemple d’application est intentionnellement petit, avec un nombre minimal d’objets et de champs de recherche.  
  
 L’interface d’application se compose des éléments suivants :  
  
-   Objet RDS non visuel **. DataControl** objet de liaison de données utilisé par le client pour se connecter à la base de données.  
  
-   Zones de texte HTML qui jouent le rôle de champs d’entrée pour les critères de recherche d’attributs d’employé.  
  
-   Boutons de commande HTML pour générer des requêtes, effacer les champs de recherche, mettre à jour la base de données avec les informations sur les employés, annuler les modifications en attente et parcourir les lignes de données affichées dans la grille.  
  
-   Liaison de données DHTML pour afficher les données retournées par les requêtes sur une base de données principale (via **RDS. DataControl** objet de liaison de données) dans une table.  
  
-   Les routines VBScript qui connectent chacun des éléments précédemment mentionnés et les autorisent à interagir. Le code VBScript est également utilisé pour initialiser les **services Bureau à distance. DataControl** et créer dynamiquement les en-têtes de colonne dans le tableau HTML à partir des noms de l’objet **RDS.** Champs du jeu d’enregistrements DataControl.  
  
 Suivez les liens de la rubrique pas à pas pour configurer et exécuter le scénario et pour en savoir plus sur le fonctionnement du scénario.  
  
 Ce scénario contient les rubriques suivantes.  
  
-   [Configuration système exigée pour l’application Carnet d’adresses](./system-requirements-for-the-address-book-application.md)  
  
-   [Exécution du script SQL Carnet d’adresses](./running-the-address-book-sql-script.md)  
  
-   [Exécution de l’exemple d’application Carnet d’adresses](./running-the-address-book-sample-application.md)  
  
-   [Objet de liaison de données de l’application Carnet d’adresses](./address-book-data-binding-object.md)  
  
-   [Boutons de commande de l’application Carnet d’adresses](./address-book-command-buttons.md)  
  
-   [Boutons de navigation de l’application Carnet d’adresses](./address-book-navigation-buttons.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Configuration système requise pour l’application Carnet d’adresses](./system-requirements-for-the-address-book-application.md)   
 [Microsoft ActiveX Data Objects (ADO)](../../microsoft-activex-data-objects-ado.md)   
 [Notions de base sur RDS](./rds-fundamentals.md)   
 [Tutoriel RDS](./rds-tutorial.md)