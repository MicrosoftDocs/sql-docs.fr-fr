---
title: Propriétés de Protocoles pour MSSQLSERVER (onglet Indicateurs)
description: Découvrez comment utiliser l’onglet Indicateurs dans la boîte de dialogue Propriétés des Protocoles pour MSSQLSERVER pour afficher ou spécifier les options de chiffrement du protocole et de masquage de l'instance.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- MSSQLSERVER property protocols
ms.assetid: 4d38e6e9-f95f-4e79-ae45-89f631037528
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 7c8746571c13369b72e9c530cd4addfb1a681ad9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349691"
---
# <a name="protocols-for-mssqlserver-properties-flags-tab"></a>Propriétés de Protocoles pour MSSQLSERVER (onglet Indicateurs)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Lorsqu'un certificat est installé sur le serveur, utilisez l'onglet **Indicateurs** dans la boîte de dialogue **Propriétés de Protocoles pour MSSQLSERVER** pour afficher ou spécifier les options de chiffrement de protocole et de masquage d'instance. [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être redémarré pour activer ou désactiver le paramètre **ForceEncryption**.  
  
 Pour chiffrer les connexions, vous devez fournir un certificat au [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . Si aucun certificat n’est installé, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un certificat auto-signé lors du démarrage de l’instance. Ce certificat autosigné peut être utilisé à la place d'un certificat émanant d'une autorité de certification approuvée, mais il ne fournit ni authentification ni non-répudiation.  
  
> [!CAUTION]  
>  Les connexions TLS (Transport Layer Security), anciennement SSL (Secure Sockets Layer), chiffrées à l'aide d'un certificat autosigné n'offrent pas une sécurité renforcée. Elles sont vulnérables aux attaques de l’intercepteur. Ne faites jamais confiance à une connexion TLS utilisant des certificats autosignés dans un environnement de production ou sur des serveurs connectés à Internet.  
  
 Pour plus d'informations sur le chiffrement, consultez « Chiffrement des connexions à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]» dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Le processus de connexion est toujours chiffré. Quand l’option **ForceEncryption** a la valeur **Oui**, toutes les communications client/serveur sont chiffrées et les clients qui se connectent au [!INCLUDE[ssDE](../../includes/ssde-md.md)] doivent être configurés de manière à approuver l’autorité racine du certificat du serveur. Pour plus d’informations, consultez « Procédure : Activer des connexions chiffrées dans [!INCLUDE[ssDE](../../includes/ssde-md.md)] (Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="cluster-servers"></a>Serveurs clusters  
 Si vous souhaitez utiliser le chiffrement à l'aide d'un cluster de basculement, vous devez installer le certificat du serveur avec le nom DNS complet du serveur virtuel sur tous les nœuds du cluster de basculement. Si, par exemple, vous disposez d'un cluster à deux nœuds nommés « test1. *\<your company>* .com » et « test2. *\<your company>* .com » et d’un serveur virtuel nommé « virtsql », vous devez installer un certificat pour « virtsql. *\<your company>* .com » sur les deux nœuds. Vous pouvez ensuite activer la case à cocher **ForceEncryption** du **Gestionnaire de configurations SQL Server** pour configurer le chiffrement du cluster de basculement.  
  
## <a name="options"></a>Options  
 **ForceEncryption**  
 Force le chiffrement du protocole. Le chiffrement est une méthode permettant de préserver la confidentialité des informations en changeant les données en un format non lisible. Le chiffrement garantit que les données demeurent confidentielles, même si les paquets de transmission sont visualisés pendant la transmission. Pour utiliser une liaison de canal, choisissez la valeur **Activé** pour l'option **Forcer le chiffrement** et configurez la **Protection étendue** sous l'onglet **Avancé** .  
  
 **HideInstance**  
 Empêche le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser d’exposer cette instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] aux ordinateurs clients dont les utilisateurs tentent de localiser l’instance à l’aide du bouton **Parcourir** . Dans le cas d'instances nommées sur le serveur, les applications clientes doivent spécifier les informations de point de terminaison de protocole pour se connecter. Par exemple, le numéro de port ou le nom de canal nommé, tel que **tcp:server,5000**. Pour plus d'informations, consultez [Logging In to SQL Server](../../database-engine/configure-windows/logging-in-to-sql-server.md).  
  
 Pour plus d’informations, consultez « Procédure : activer les connexions de chiffrement au moteur de base de données (Gestionnaire de configuration SQL Server) » dans la documentation en ligne.  
  
  
