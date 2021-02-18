---
title: Configurer un ordinateur multirésident pour l’accès
description: Découvrez comment configurer SQL Server et le Pare-feu Windows pour fournir des connexions réseau à une instance de SQL Server dans un environnement multirésident.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- ports [SQL Server], multi-homed computer
- multi-homed computer [SQL Server] configuring ports
- firewall systems [Database Engine], multi-homed computer
ms.assetid: ba369e5b-7d1f-4544-b7f1-9b098a1e75bc
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8a105d59c32308da01f9d875e69cb417dad31b10
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336787"
---
# <a name="configure-a-multi-homed-computer-for-sql-server-access"></a>Configurer un ordinateur multirésident pour l'accès à SQL Server
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Lorsqu'un serveur doit fournir une connexion à plusieurs réseaux ou sous-réseaux, le scénario classique consiste à utiliser un ordinateur multirésident. Bien souvent, cet ordinateur se trouve dans un réseau de périmètre (également appelé sous-réseau filtré). Cet article explique comment configurer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le Pare-feu Windows avec fonctions avancées de sécurité pour fournir des connexions réseau à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans un environnement multirésident.  
  
> [!NOTE]  
>  Un ordinateur multirésident a plusieurs cartes réseau ou a été configuré afin d'utiliser plusieurs adresses IP pour une carte réseau unique. Un ordinateur à deux interfaces réseau a deux cartes réseau ou a été configuré afin d'utiliser deux adresses IP pour une carte réseau unique.  
  
 Avant d’aller plus loin dans cet article, vous devez vous familiariser avec les informations fournies dans l’article [Configurer le Pare-feu Windows pour autoriser l’accès à SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md). Cet article contient des informations de base sur le fonctionnement des composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec le pare-feu.  
  
 **Hypothèses pour cet exemple :**  
  
-   Il y a deux cartes réseau installées dans l'ordinateur. Une ou plusieurs des cartes réseau peuvent être sans fil. Vous pouvez simuler l'existence de deux cartes réseau en utilisant l'adresse IP d'une carte réseau et l'adresse IP de bouclage (127.0.0.1) en tant que deuxième carte réseau.  
  
-   Pour des raisons de simplicité, cet exemple utilise des adresses IPv4. Les mêmes procédures peuvent être effectuées à l'aide d'adresses IPv6.  
  
    > [!NOTE]  
    >  Les adresses IPv4 sont une série de quatre nombres appelés octets. Chaque nombre est inférieur à 255 et est séparé des autres nombres par un point, par exemple 127.0.0.1. Les adresses IPv6 sont une série de huit nombres hexadécimaux séparés par des signes deux-points, par exemple fe80:4898:23:3:49a6:f5c1:2452:b994.  
  
-   Les règles de pare-feu peuvent autoriser l'accès via un port spécifique, par exemple le port 1433. Toutefois, les règles de pare-feu peuvent également autoriser l'accès au programme du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] (sqlservr.exe). Aucune méthode n'est meilleure que l'autre. Dans la mesure où un serveur situé dans un réseau de périmètre est plus vulnérable aux attaques que des serveurs situés sur un intranet, cet article part du principe que vous souhaitez disposer d’un contrôle plus précis et que vous voulez sélectionner individuellement les ports que vous ouvrez. C’est la raison pour laquelle cet article part de l’hypothèse que vous configurez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de sorte qu’il soit à l’écoute sur un port fixe. Pour plus d’informations sur les ports utilisés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Configurer le Pare-feu Windows pour autoriser l’accès à SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
-   Cet exemple configure l’accès au [!INCLUDE[ssDE](../../includes/ssde-md.md)] à l’aide du port TCP 1433. Les autres ports qui correspondent à l'utilisation de composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] différents peuvent être configurés à l'aide des mêmes étapes générales.  
  
 **Les étapes générales de cet exemple sont les suivantes :**  
  
-   Déterminez les adresses IP de l'ordinateur.  
  
-   Configurez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de sorte qu'il soit à l'écoute sur un port TCP spécifique.  
  
-   Configurez le Pare-feu Windows avec fonctions avancées de sécurité.  
  
## <a name="optional-procedures"></a>Procédures facultatives  
 Si vous connaissez déjà les adresses IP disponibles pour votre ordinateur et utilisées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez ignorer ces procédures.  
  
#### <a name="to-determine-the-ip-addresses-available-on-the-computer"></a>Pour déterminer les adresses IP disponibles sur l'ordinateur  
  
1.  Sur l’ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé, cliquez sur **Démarrer**, sur **Exécuter**, tapez **cmd** , puis [!INCLUDE[clickOK](../../includes/clickok-md.md)].  
  
2.  Dans la fenêtre d’invite de commandes, tapez **ipconfig,** , puis appuyez sur Entrée pour visualiser la liste des adresses IP disponibles sur cet ordinateur.  
  
    > [!NOTE]  
    >  La commande **ipconfig** répertorie parfois de nombreuses connexions possibles, notamment les connexions déconnectées. La commande **ipconfig** peut énumérer à la fois les adresses IPv4 et IPv6.  
  
3.  Notez les adresses IPv4 et IPv6 qui sont utilisées. Les autres informations de la liste, telles que les adresses temporaires, les masques de sous-réseau et les passerelles par défaut sont des informations importantes pour la configuration d'un réseau TCP/IP. Toutefois, ces informations ne sont pas utilisées dans cet exemple.  
  
#### <a name="to-determine-the-ip-addresses-and-ports-used-by-ssnoversion"></a>Pour déterminer les adresses IP et les ports utilisés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1.  Cliquez sur **Démarrer**, pointez sur **Tous les programmes**, sur [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], sur **Outils de configuration**, puis cliquez sur **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Gestionnaire de configuration**.  
  
2.  Dans **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager**, dans le volet de la console, développez **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration réseau**, développez **Protocoles pour \<instance name>** puis, double-cliquez sur **TCP/IP**.  
  
3.  Dans la boîte de dialogue **Propriétés TCP/IP** , sous l’onglet **Adresses IP** , plusieurs adresses IP apparaissent au format **IP1**, **IP2**, jusqu’à **IPAll**. Une de ces adresses correspond à l'adresse IP de la carte de bouclage, 127.0.0.1. D'autres adresses IP apparaissent pour chaque adresse IP configurée sur l'ordinateur.  
  
4.  Pour toutes les adresses IP, si la boîte de dialogue **Ports TCP dynamiques** contient **0**, cela indique que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] est à l'écoute sur les ports dynamiques. Cet exemple utilise des ports fixes au lieu de ports dynamiques, lesquels peuvent changer lors d'un redémarrage. Par conséquent, si la boîte de dialogue **Ports TCP dynamiques** contient **0**, supprimez le 0.  
  
5.  Notez le port TCP indiqué pour chaque adresse IP à configurer. Pour cet exemple, supposez que les deux adresses IP sont à l'écoute sur le port par défaut, 1433.  
  
6.  Si vous ne souhaitez pas que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise certains des ports disponibles, sous l'onglet **Protocole** , remplacez la valeur du paramètre **Écouter tout** par **Non**; sous l'onglet **Adresses IP** , remplacez ensuite la valeur **Actif** par **Non** pour les adresses IP que vous ne souhaitez pas utiliser.  
  
## <a name="configuring-windows-firewall-with-advanced-security"></a>Configuration du Pare-feu Windows avec fonctions avancées de sécurité  
 Après avoir identifié les adresses IP utilisées par l'ordinateur et les ports utilisés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez créer des règles de pare-feu, puis configurer ces règles pour des adresses IP spécifiques.  
  
#### <a name="to-create-a-firewall-rule"></a>Pour créer une règle de pare-feu  
  
1.  Sur l'ordinateur où [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé, ouvrez une session en tant qu'administrateur.  
  
2.  Cliquez sur **Démarrer**, puis sur **Exécuter**, tapez **wf.msc**, puis cliquez sur **OK**.  
  
3.  Dans la boîte de dialogue **Contrôle de compte d’utilisateur** , cliquez sur **Continuer** pour utiliser les informations d’identification d’administrateur et ouvrir le composant logiciel enfichable Pare-feu Windows avec fonctions avancées de sécurité.  
  
4.  Dans la page **Vue d'ensemble** , assurez-vous que le Pare-feu Windows est activé.  
  
5.  Dans le volet gauche, cliquez sur **Règles de trafic entrant**.  
  
6.  Cliquez avec le bouton droit sur **Règles de trafic entrant**, puis cliquez sur **Nouvelle règle** pour ouvrir **l’Assistant Nouvelle règle de trafic entrant**.  
  
7.  Vous pouvez créer une règle pour le programme [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Toutefois, dans la mesure où cet exemple utilise un port fixe, sélectionnez **Port**, puis cliquez sur **Suivant**.  
  
8.  Dans la page **Protocoles et ports** , sélectionnez **TCP**.  
  
9. Sélectionnez **Ports locaux spécifiques**. Tapez les numéros de port en les séparant par des virgules, puis cliquez sur **Suivant**. Dans cet exemple, vous devez configurer le port par défaut ; par conséquent, entrez **1433**.  
  
10. Dans la page **Action** , passez en revue les options. Dans cet exemple, vous n'utilisez pas le pare-feu pour forcer des connexions sécurisées. Par conséquent, cliquez sur **Autoriser la connexion**, puis sur **Suivant**.  
  
    > [!NOTE]  
    >  Votre environnement peut requérir des connexions sécurisées. Si vous sélectionnez l'une des options de connexions sécurisées, vous devrez peut-être configurer un certificat et l'option **Forcer le chiffrement** . Pour plus d’informations sur les connexions sécurisées, consultez [Activer les connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md) et [Activer les connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
11. Dans la page **Profil** , sélectionnez un ou plusieurs profils pour la règle. Si vous n'avez pas l'habitude d'utiliser des profils de pare-feu, cliquez sur le lien **En savoir plus sur les profils** dans le programme de pare-feu.  
  
    -   Si l'ordinateur est un serveur et s'il est disponible uniquement lorsqu'il est connecté à un domaine, sélectionnez **Domaine**, puis cliquez sur **Suivant**.  
  
    -   Si l'ordinateur est un ordinateur portable, il utilise probablement plusieurs profils lorsqu'il se connecte aux différents réseaux. Pour un ordinateur portable, vous pouvez configurer des fonctionnalités d'accès distinctes selon les différents profils. Par exemple, vous pouvez autoriser l'accès lorsque l'ordinateur utilise le profil Domaine mais refuser l'accès lorsqu'il utilise le profil Public.  
  
12. Dans la page **Nom** , indiquez le nom et la description de la règle, puis cliquez sur **Terminer**.  
  
13. Répétez cette procédure afin de créer une autre règle pour chaque adresse IP utilisée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Après avoir créé une ou plusieurs règles, procédez comme suit afin de configurer l'utilisation d'une règle pour chaque adresse IP de l'ordinateur.  
  
#### <a name="to-configure-the-firewall-rule-for-a-specific-ip-addresses"></a>Pour configurer la règle de pare-feu pour des adresses IP spécifiques  
  
1.  Dans la page **Règles de trafic entrant** du **Pare-feu Windows avec fonctions avancées de sécurité**, cliquez avec le bouton droit sur la règle que vous venez de créer, puis cliquez sur **Propriétés**.  
  
2.  Dans la boîte de dialogue **Propriétés de règle** , sélectionnez l'onglet **Étendue** .  
  
3.  Dans la zone **Adresse IP locale** , sélectionnez **Ces adresses IP**, puis cliquez sur **Ajouter**.  
  
4.  Dans la boîte de dialogue **Adresse IP** , sélectionnez **Cette adresse IP ou ce sous-réseau**, puis tapez l'une des adresses IP que vous souhaitez configurer.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
6.  Dans la zone **Adresse IP distante** , sélectionnez **Ces adresses IP**, puis cliquez sur **Ajouter**.  
  
7.  Utilisez la boîte de dialogue **Adresse IP** pour configurer la connectivité de l'adresse IP spécifique de l'ordinateur. Vous pouvez activer les connexions d'adresses IP spécifiques, de plages d'adresses IP, de sous-réseaux entiers ou de certains ordinateurs. Pour configurer correctement cette option, vous devez avoir une bonne compréhension du réseau. Pour plus d'informations sur le réseau, consultez l'administrateur réseau.  
  
8.  Pour fermer la boîte de dialogue **Adresse IP** , cliquez sur **OK**; cliquez ensuite sur **OK** pour fermer la boîte de dialogue **Propriétés de règle** .  
  
9. Pour configurer les autres adresses IP sur un ordinateur multirésident, répétez cette procédure en utilisant une autre adresse IP et une autre règle.  
  
## <a name="see-also"></a>Voir aussi  
 [Service SQL Server Browser &#40;moteur de base de données et SSAS&#41;](../../database-engine/configure-windows/sql-server-browser-service-database-engine-and-ssas.md)   
 [Se connecter à SQL Server par le biais d’un serveur proxy &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/connect-to-sql-server-through-a-proxy-server-sql-server-configuration-manager.md)  
  
  
