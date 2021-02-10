---
description: Utilisation de RDS avec le regroupement de connexions ODBC
title: Utilisation de RDS avec le regroupement de connexions ODBC | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connection pooling in RDS [ADO]
ms.assetid: e8b912c1-da5b-4e85-a000-1e6648a94237
author: rothja
ms.author: jroth
ms.openlocfilehash: 5c353d7e0038f40eea574b895f2cd55ea3f47585
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032591"
---
# <a name="using-rds-with-odbc-connection-pooling"></a>Utilisation de RDS avec le regroupement de connexions ODBC
Si vous utilisez une source de données ODBC, vous pouvez utiliser l’option de regroupement de connexions de Internet Information Services (IIS) pour obtenir des performances élevées de la charge du client. Le regroupement de connexions est un gestionnaire de ressources pour les connexions, ce qui maintient l’état ouvert sur les connexions fréquemment utilisées.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 Pour activer le regroupement de connexions, reportez-vous à la documentation Internet Information Services.  
  
 Notez que l’activation du regroupement de connexions peut soumettre le serveur Web à d’autres restrictions, comme indiqué dans la documentation de Microsoft Internet Information Services.  
  
 Pour vous assurer que le regroupement de connexions est stable et fournit des gains de performances supplémentaires, vous devez configurer Microsoft SQL Server pour utiliser la bibliothèque réseau de sockets TCP/IP.  
  
 Pour cela, vous devez procéder comme suit :  
  
-   Configurez l’ordinateur SQL Server pour utiliser des sockets TCP/IP.  
  
-   Configurez le serveur Web pour utiliser des sockets TCP/IP.  
  
## <a name="configuring-the-sql-server-computer-to-use-tcpip-sockets"></a>Configuration de l’ordinateur SQL Server pour utiliser des sockets TCP/IP  
 Sur l’ordinateur SQL Server, exécutez le programme d’installation de SQL Server afin que les interactions avec la source de données utilisent la bibliothèque réseau de sockets TCP/IP.  
  
### <a name="to-specify-the-tcpip-socket-network-library-on-the-sql-server-computer"></a>Pour spécifier la bibliothèque réseau de sockets TCP/IP sur l’ordinateur SQL Server  
  
### <a name="in-microsoft-sql-server-65"></a>Dans Microsoft SQL Server 6,5 :  
  
1.  Dans le menu Démarrer, pointez sur programmes, pointez sur Microsoft SQL Server 6,5, puis cliquez sur installation de SQL.  
  
2.  Cliquez deux fois sur continuer.  
  
3.  Dans la boîte de dialogue Microsoft SQL Server-options, sélectionnez Modifier la prise en charge du réseau, puis cliquez sur continuer.  
  
4.  Assurez-vous que la case Sockets TCP/IP est cochée, puis cliquez sur OK.  
  
5.  Cliquez sur continuer pour terminer et quitter le programme d’installation.  
  
### <a name="in-microsoft-sql-server-70"></a>Dans Microsoft SQL Server 7,0 :  
  
1.  Dans le menu Démarrer, pointez sur programmes, pointez sur Microsoft SQL Server 7,0, puis cliquez sur utilitaire réseau serveur.  
  
2.  Dans l’onglet général de la boîte de dialogue, cliquez sur Ajouter.  
  
3.  Dans la boîte de dialogue Ajouter une configuration de bibliothèque réseau, cliquez sur TCP/IP.  
  
4.  Dans les zones numéro de port et adresse du proxy, entrez le numéro de port et l’adresse proxy fournis par votre administrateur réseau.  
  
5.  Cliquez sur OK pour terminer et quitter le programme d’installation.  
  
## <a name="configuring-the-web-server-to-use-tcpip-sockets"></a>Configuration du serveur Web pour utiliser des sockets TCP/IP  
 Il existe deux options pour configurer le serveur Web de sorte qu’il utilise des sockets TCP/IP. Ce que vous faites dépend du fait que tous les serveurs SQL sont accessibles à partir du serveur Web ou qu’un SQL Server spécifique est accessible à partir du serveur Web.  
  
 Si tous les serveurs SQL sont accessibles à partir du serveur Web, vous devez exécuter l’utilitaire de configuration du client SQL Server sur l’ordinateur serveur Web. Les étapes suivantes modifient la bibliothèque réseau par défaut pour toutes les connexions SQL Server effectuées à partir de ce serveur Web IIS pour utiliser la bibliothèque réseau de sockets TCP/IP.  
  
### <a name="to-configure-the-web-server-all-sql-servers"></a>Pour configurer le serveur Web (tous les serveurs SQL)  
  
### <a name="for-microsoft-sql-server-65"></a>Pour Microsoft SQL Server 6,5 :  
  
1.  Dans le menu Démarrer, pointez sur programmes, pointez sur Microsoft SQL Server 6,5, puis cliquez sur utilitaire de configuration du client SQL.  
  
2.  Cliquez sur l’onglet Net Library.  
  
3.  Dans la zone réseau par défaut, sélectionnez Sockets TCP/IP.  
  
4.  Cliquez sur terminé pour enregistrer les modifications et quitter l’utilitaire.  
  
### <a name="for-microsoft-sql-server-70"></a>Pour Microsoft SQL Server 7,0 :  
  
1.  Dans le menu Démarrer, pointez sur programmes, sur Microsoft SQL Server 7,0, puis cliquez sur utilitaire réseau client.  
  
2.  Cliquez sur l’onglet General.  
  
3.  Dans la zone bibliothèque réseau par défaut, cliquez sur TCP/IP.  
  
4.  Cliquez sur OK pour enregistrer les modifications et quitter l’utilitaire.  
  
 Si vous accédez à un SQL Server spécifique à partir d’un serveur Web, vous devez exécuter l’utilitaire de configuration du client SQL Server sur l’ordinateur serveur Web. Pour modifier la bibliothèque réseau pour une connexion SQL Server spécifique, configurez le logiciel client SQL Server sur l’ordinateur serveur Web comme suit.  
  
### <a name="to-configure-the-web-server-a-specific-sql-server"></a>Pour configurer le serveur Web (un SQL Server spécifique)  
  
### <a name="for-microsoft-sql-server-65"></a>Pour Microsoft SQL Server 6,5 :  
  
1.  Dans le menu Démarrer, pointez sur programmes, pointez sur Microsoft SQL Server 6,5, puis cliquez sur utilitaire de configuration du client SQL.  
  
2.  Cliquez sur l'onglet Avancé.  
  
3.  Dans la zone serveur, tapez le nom du serveur auquel vous souhaitez vous connecter à l’aide des sockets TCP/IP.  
  
4.  Dans la zone nom de la DLL, sélectionnez Sockets TCP/IP.  
  
5.  Cliquez sur Ajouter/modifier. Toutes les sources de données qui pointent vers ce serveur utilisent désormais des sockets TCP/IP.  
  
6.  Cliquez sur Done.  
  
### <a name="for-microsoft-sql-server-70"></a>Pour Microsoft SQL Server 7,0 :  
  
1.  Dans le menu Démarrer, pointez sur programmes, pointez sur Microsoft SQL Server 7,0, puis cliquez sur utilitaire de configuration du client.  
  
2.  Cliquez sur l’onglet General.  
  
3.  Cliquez sur Ajouter.  
  
4.  Entrez l’alias du serveur dans la zone alias du serveur. Dans la zone bibliothèques réseau, cliquez sur TCP/IP. Dans la zone nom de l’ordinateur, entrez le nom de l’ordinateur qui écoute les clients de sockets TCP/IP. Dans la zone numéro de port, entrez le port sur lequel le SQL Server écoute.  
  
5.  Cliquez sur OK, puis à nouveau sur OK pour quitter l’utilitaire.  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts de base de RDS](./rds-fundamentals.md)