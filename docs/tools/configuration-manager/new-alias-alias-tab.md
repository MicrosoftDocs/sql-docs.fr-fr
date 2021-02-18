---
title: Nouvel alias (onglet Alias)
description: Découvrez comment créer un alias, un autre nom pour une instance SQL utilisée lors de la connexion à cette instance. Affichez des exemples d’utilisation d’un alias.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 785eb6fb-f67e-449d-b1c8-c38dfbb95ef6
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: d2a34ab643359f7afca304b64328638675f8a742
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349775"
---
# <a name="new-alias-alias-tab"></a>Nouvel alias (onglet Alias)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Un alias est un nom de remplacement permettant d'établir une connexion. L'alias encapsule les éléments requis d'une chaîne de connexion, puis les expose sous un nom choisi par l'utilisateur. La page **Alias** de la boîte de dialogue **Alias - Nouveau** vous permet de spécifier les éléments de la chaîne de connexion d’un alias. Pour modifier la chaîne de connexion d’un alias existant, consultez [Propriétés de &#60;Alias&#62; &#40;onglet Alias&#41;](../../tools/configuration-manager/alias-properties-alias-tab.md).  
  
 Il n'est pas nécessaire de renseigner toutes les valeurs de la grille **Propriétés** . Le protocole sélectionné détermine les combinaisons valides. Voir les rubriques indiquées ci-après pour obtenir des exemples de combinaisons valides.  
  
 **Nom de l'alias**  
 Nom (alias) à utiliser pour faire référence à cette connexion.  
  
 **Nom du canal** / **Numéro de port**  
 Éléments supplémentaires de la chaîne de connexion. Le nom de cette zone varie en fonction du protocole sélectionné.  
  
 **Protocole**  
 Protocole utilisé pour la connexion.  
  
 **Serveur**  
 Nom de l’instance [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers laquelle la connexion est établie.  
  
## <a name="when-to-use-an-alias"></a>Cas d'utilisation d'un alias  
 Par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se connecte à une instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant le protocole de **mémoire partagée** , et à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un autre ordinateur à l’aide du protocole **TCP/IP** ou de **Canaux nommés**. Créez un alias lorsque vous utilisez le protocole TCP/IP ou des canaux nommés, et que vous souhaitez fournir une chaîne de connexion personnalisée ou utiliser un nom différent du nom de serveur pour la connexion.  
  
### <a name="examples"></a>Exemples  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’étant pas à l’écoute sur le port TCP/IP 1433 par défaut, vous pouvez fournir une chaîne de connexion comportant un numéro de port différent.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'étant pas à l'écoute sur le canal nommé par défaut, vous pouvez fournir une chaîne de connexion comportant un nom de canal différent.  
  
-   Une application doit normalement se connecter à une base de données située sur le serveur nommé `ACCT`, mais cette base de données a été consolidée sous la forme d'une instance nommée `ACCT` sur un serveur nommé `CENTRAL`. L'application ne peut pas être facilement modifiée. Créez un alias nommé `ACCT`, avec une chaîne de connexion pointant vers `CENTRAL\ACCT`.  
  
## <a name="creating-a-valid-connection-string"></a>Création d'une chaîne de connexion valide  
 Les rubriques suivantes décrivent et illustrent des combinaisons valides de propriétés d'alias :  
  
-   [Création d'une chaîne de connexion valide à l'aide du protocole de mémoire partagée](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)  
  
-   [Création d’une chaîne de connexion valide à l’aide du protocole TCP/IP](../../tools/configuration-manager/creating-a-valid-connection-string-using-tcp-ip.md)  
  
-   [Création d’une chaîne de connexion valide avec des canaux nommés](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130))  
  
