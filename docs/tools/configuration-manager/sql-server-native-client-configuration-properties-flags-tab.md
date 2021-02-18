---
title: Propriétés de la configuration de SQL Server Native Client (onglet Indicateurs)
description: Découvrez les options de l’onglet Indicateurs de la boîte de dialogue Propriétés de configuration de SQL Server Native Client.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 59af121d-c8b9-4faa-91a1-b664f2c9b441
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 582c05b7dad6d952aeec4a2f9464759069159b2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349604"
---
# <a name="sql-server-native-client-configuration-properties-flags-tab"></a>Propriétés de la configuration de SQL Server Native Client (onglet Indicateurs)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Les clients [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installés sur cet ordinateur communiquent avec les serveurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant les protocoles fournis dans le fichier de bibliothèque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. Cette page permet de configurer l'ordinateur client de manière à ce qu'il demande une connexion chiffrée utilisant le protocole TLS (Transport Layer Security), anciennement SSL (Secure Sockets Layer). En cas d'impossibilité d'établir une connexion chiffrée, la connexion échoue.  
  
 Le processus de connexion est toujours chiffré. Les options ci-dessous s'appliquent uniquement aux données de chiffrement. Pour plus d'informations sur la manière dont [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] chiffre les communications et pour obtenir des instructions sur la manière de configurer le client pour qu'il approuve l'autorité racine du certificat du serveur, consultez « Chiffrement des connexions à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] » et « Procédure : Activer des connexions chiffrées dans [!INCLUDE[ssDE](../../includes/ssde-md.md)] (Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="options"></a>Options  
 **Forcer le chiffrement du protocole**  
 Demande une connexion utilisant le protocole TLS.  
  
 **Faire confiance au certificat de serveur**  
 Quand cette option a la valeur **Non**, le processus client tente de valider le certificat du serveur. Le client et le serveur doivent tous les deux posséder un certificat émanant d'une autorité de certification publique. Si le certificat n'est pas présent sur l'ordinateur client ou que la validation du certificat échoue, la connexion prend fin.  
  
 Quand l’option a la valeur **Oui**, le client ne valide pas le certificat du serveur, ce qui permet d’utiliser un certificat autosigné.  
  
 **Faire confiance au certificat de serveur** n’est disponible que si l’option **Forcer le chiffrement du protocole** a la valeur **Oui**.  
  
  
