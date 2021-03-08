---
title: Installer PolyBase sur Windows
description: Apprenez à installer PolyBase comme un nœud unique ou un groupe de scale-out PolyBase. Vous pouvez utiliser un Assistant d’installation ou une invite de commandes. Enfin, activez PolyBase.
ms.date: 02/05/2021
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
helpviewer_keywords:
- PolyBase, installation
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: ca5c7911444dcc5cdd87787a80ee0d9e572797ef
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838083"
---
# <a name="install-polybase-on-windows"></a>Installer PolyBase sur Windows

[!INCLUDE [SQL Server Windows Only ](../../includes/applies-to-version/sql-windows-only.md)]

Pour installer une version d’essai de SQL Server, accédez à [Versions d’évaluation de SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016). 
   
## <a name="prerequisites"></a>Prérequis  
   
- Édition d’évaluation de SQL Server 64 bits.  
   
- Microsoft .NET Framework 4.5  

- Mémoire minimale : 4 Go. 
   
- Espace libre minimal sur le disque dur : 2 Go.
  
- Recommandé : Au moins 16 Go de RAM.
   
- TCP/IP doit être activé pour que PolyBase fonctionne correctement. TCP/IP est activé par défaut sur toutes les éditions de SQL Server, sauf sur les éditions SQL Server Express et Developer. Pour que PolyBase fonctionne correctement sur les éditions Express et Developer, vous devez activer la connectivité TCP/IP. Consultez [Activer ou désactiver un protocole réseau de serveur](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md).


>[!NOTE] 
> Il n'est possible d'installer PolyBase que sur une instance SQL Server par ordinateur.


>[!NOTE]
>Pour pouvoir utiliser PolyBase, vous devez disposer des autorisations de niveau serveur de contrôle ou sysadmin pour la base de données.

>[!IMPORTANT]
>Pour utiliser la fonctionnalité de calcul pushdown sur Hadoop, il faut que le cluster Hadoop cible soit doté des principaux composants HDFS, YARN et MapReduce, avec le serveur Job History activé. PolyBase envoie la requête émise via MapReduce et extrait l’état à partir du serveur Job History. L’absence de l’un ou l’autre composant entraîne l’échec de la requête.
  
## <a name="single-node-or-polybase-scale-out-group"></a>Nœud unique ou groupe de scale out PolyBase

Avant d’installer PolyBase sur vos instances de SQL Server, décidez si vous souhaitez une installation à un seul nœud ou un [groupe de scale out PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).

Pour un groupe de scale out PolyBase, vérifiez que :

- Toutes les machines figurent dans le même domaine.
- Vous utilisez les mêmes compte de service et mot de passe pendant l’installation de PolyBase.
- Vos instances de SQL Server peuvent communiquer entre elles sur le réseau.
- Les instances de SQL Server sont toutes la même version de SQL Server.

Une fois que vous avez installé PolyBase de manière autonome ou dans un groupe de scale out, vous ne pouvez plus changer. Pour modifier ce paramètre, vous devez désinstaller puis réinstaller la fonctionnalité.

## <a name="use-the-installation-wizard"></a>Utiliser l’Assistant Installation
   
1. Exécutez le fichier setup.exe de SQL Server.   
   
2. Sélectionnez **Installation**, puis **Nouvelle installation autonome de SQL Server, ou ajouter des fonctionnalités**.  
   
3. Sur la page de sélection de fonctionnalités, sélectionnez **Service de requête PolyBase pour données externes**.  

   ![Services PolyBase](../../relational-databases/polybase/media/install-wizard.png "Services PolyBase")  
   
   >[!NOTE]
   >SQL Server 2019 PolyBase comprend désormais une option supplémentaire **Connecteur Java pour les sources de données HDFS**. Pour plus d’informations sur cette fonctionnalité, consultez [SQL Server preview features](https://cloudblogs.microsoft.com/sqlserver/2019/04/24/sql-server-2019-community-technology-preview-2-5-is-now-available/).
   
4. Dans la page Configuration du serveur, configurez le **Service de moteur SQL Server PolyBase** et le **service Mouvement de données PolyBase SQL Server** à exécuter sous le même compte de domaine.  

   >[!IMPORTANT]
   >Dans un groupe de scale out PolyBase, le moteur PolyBase et le service Mouvement de données PolyBase doivent être exécutés sous le même compte de domaine, et ce sur tous les nœuds. Voir [Groupes de scale out PolyBase](#enable).

5. Dans la page Configuration de PolyBase, sélectionnez l’une des deux options. Pour plus d’informations, consultez [Groupes de scale out PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).  
   
   - Utilisez l’instance de SQL Server comme instance activée PolyBase autonome.  
   
     Choisissez cette option pour utiliser cette instance SQL Server comme nœud principal autonome.  
   
   - Utilisez l’instance de SQL Server dans le cadre du groupe de scale-out PolyBase. Cette option ouvre le pare-feu pour autoriser les connexions entrantes. Les connexions sont autorisées pour le moteur de base de données SQL Server, le moteur PolyBase SQL Server, le service Mouvement de données PolyBase SQL Server et SQL Browser. Le pare-feu autorise aussi les connexions entrantes à partir d’autres nœuds dans un groupe de scale out PolyBase.  
   
     Cette option active également les connexions du pare-feu MSDTC (Microsoft Distributed Transaction Coordinator) et modifie les paramètres du registre MSDTC.  
   
6. Dans la page Configuration de PolyBase, spécifiez une plage de ports avec au moins six ports. Le programme d’installation de SQL Server affecte les six premiers ports disponibles dans la plage.  

   >[!IMPORTANT]
   > Après l’installation, vous devez [activer la fonctionnalité PolyBase](#enable).


##  <a name="use-a-command-prompt"></a><a name="installing"></a> Utiliser une invite de commandes

Utilisez les valeurs de cette table pour créer des scripts d'installation. Le Moteur SQL Server PolyBase et le service Mouvement de données SQL Server PolyBase doivent s’exécuter sous le même compte. Dans un groupe de scale-out PolyBase, les services PolyBase doivent être exécutés sous le même compte de domaine, et ce sur tous les nœuds.  
   
<!--SQL Server 2016/2017-->
::: moniker range="= sql-server-2016 || = sql-server-2017"

|composant SQL Server|Paramètre et valeurs|Description|  
|--------------------------|--------------------------|-----------------|  
|Contrôle d'installation de SQL Server|**Obligatoire**<br /><br /> /FEATURES=PolyBase|Sélectionne la fonctionnalité PolyBase.|  
|Moteur SQL Server PolyBase|**Facultatif**<br /><br /> /PBENGSVCACCOUNT|Spécifie le compte pour le service de moteur. La valeur par défaut est **NT Authority\NETWORK SERVICE**.|  
|Moteur SQL Server PolyBase|**Facultatif**<br /><br /> /PBENGSVCPASSWORD|Spécifie le mot de passe du compte du service de moteur.|  
|Moteur SQL Server PolyBase|**Facultatif**<br /><br /> /PBENGSVCSTARTUPTYPE|Spécifie le mode de démarrage du moteur PolyBase : Automatique (par défaut), Désactivé ou Manuel.|  
|Mouvement de données PolyBase SQL Server |**Facultatif**<br /><br /> /PBDMSSVCACCOUNT|Spécifie le compte pour le service Mouvement de données. La valeur par défaut est **NT Authority\NETWORK SERVICE**.|  
|Mouvement de données PolyBase SQL Server |**Facultatif**<br /><br /> /PBDMSSVCPASSWORD|Spécifie le mot de passe du compte de déplacement des données.|  
|Mouvement de données PolyBase SQL Server |**Facultatif**<br /><br /> /PBDMSSVCSTARTUPTYPE|Spécifie le mode de démarrage du service de déplacement des données : Automatique (par défaut), Désactivé ou Manuel.|  
|PolyBase|**Facultatif**<br /><br /> /PBSCALEOUT|Spécifie si l’instance de SQL Server est utilisée dans le cadre d’un groupe de calcul de scale out PolyBase. <br />Valeurs prises en charge : True, False.|  
|PolyBase|**Facultatif**<br /><br /> /PBPORTRANGE|Spécifie une plage de ports avec au moins six ports pour les services PolyBase. Exemple :<br /><br /> `/PBPORTRANGE=16450-16460`|  

::: moniker-end
<!--SQL Server 2019-->
::: moniker range=">= sql-server-ver15 "

|composant SQL Server|Paramètre et valeurs|Description|  
|--------------------------|--------------------------|-----------------|  
|Contrôle d'installation de SQL Server|**Obligatoire**<br /><br /> /FEATURES= PolyBaseCore, PolyBaseJava, PolyBase | PolyBaseCore installe la prise en charge de toutes les fonctionnalités de PolyBase, à l’exception de la connectivité Hadoop. PolyBaseJava active la connectivité Hadoop. PolyBase installe les deux. |  
|Moteur SQL Server PolyBase|**Facultatif**<br /><br /> /PBENGSVCACCOUNT|Spécifie le compte pour le service de moteur. La valeur par défaut est **NT Authority\NETWORK SERVICE**.|  
|Moteur SQL Server PolyBase|**Facultatif**<br /><br /> /PBENGSVCPASSWORD|Spécifie le mot de passe du compte du service de moteur.|  
|Moteur SQL Server PolyBase|**Facultatif**<br /><br /> /PBENGSVCSTARTUPTYPE|Spécifie le mode de démarrage du moteur PolyBase : Automatique (par défaut), Désactivé ou Manuel.|  
|Mouvement de données PolyBase SQL Server |**Facultatif**<br /><br /> /PBDMSSVCACCOUNT|Spécifie le compte pour le service de déplacement des données. La valeur par défaut est **NT Authority\NETWORK SERVICE**.|  
|Mouvement de données PolyBase SQL Server |**Facultatif**<br /><br /> /PBDMSSVCPASSWORD|Spécifie le mot de passe du compte de déplacement des données.|  
|Mouvement de données PolyBase SQL Server |**Facultatif**<br /><br /> /PBDMSSVCSTARTUPTYPE|Spécifie le mode de démarrage du service de déplacement des données : Automatique (par défaut), Désactivé ou Manuel.|  
|PolyBase|**Facultatif**<br /><br /> /PBSCALEOUT|Spécifie si l’instance de SQL Server est utilisée dans le cadre d’un groupe de calcul de scale out PolyBase. <br />Valeurs prises en charge : True, False.|  
|PolyBase|**Facultatif**<br /><br /> /PBPORTRANGE|Spécifie une plage de ports avec au moins six ports pour les services PolyBase. Exemple :<br /><br /> `/PBPORTRANGE=16450-16460`|  

::: moniker-end

Après l’installation, vous devez [activer la fonctionnalité PolyBase](#enable).



**Exemple**

Cet exemple montre un exemple de script d’installation.  

```cmd
   
Setup.exe /Q /ACTION=INSTALL /IACCEPTSQLSERVERLICENSETERMS /FEATURES=SQLEngine,PolyBase   
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="\<fabric-domain>\Administrator"   
/INSTANCEDIR="C:\Program Files\Microsoft SQL Server" /PBSCALEOUT=TRUE   
/PBPORTRANGE=16450-16460 /SECURITYMODE=SQL /SAPWD="<StrongPassword>"   
/PBENGSVCACCOUNT="<DomainName>\<UserName>" /PBENGSVCPASSWORD="<StrongPassword>"   
/PBDMSSVCACCOUNT="<DomainName>\<UserName>" /PBDMSSVCPASSWORD="<StrongPassword>"  
   
```  

## <a name="enable-polybase"></a><a id="enable"></a> Activer PolyBase

Après l’installation, vous devez activer PolyBase pour accéder à ses fonctionnalités. Utilisez la commande Transact-SQL suivante. Ce paramètre est activé par défaut sur les instances de SQL 2019 déployées lors de l’installation du cluster Big Data.


```sql
exec sp_configure @configname = 'polybase enabled', @configvalue = 1;
RECONFIGURE;
```
## <a name="post-installation-notes"></a>Remarques suite à l’installation  

PolyBase installe trois bases de données utilisateur, DWConfiguration, DWDiagnostics et DWQueue. Ces bases de données sont destinées à une utilisation par PolyBase. Ne les modifiez pas et ne les supprimez pas.  

> [!CAUTION]
> L’ajout de PolyBase à une installation existante de SQL Server installera la fonctionnalité au niveau de la version du support d’installation, qui peut se trouver derrière les autres fonctionnalités de SQL Server au niveau version. Cela peut entraîner des erreurs ou un comportement inattendu. Suivez toujours l’installation de la fonctionnalité PolyBase en plaçant la nouvelle fonctionnalité au même niveau de version. Installez les Service Packs (SP), les mises à jour cumulatives (CU) et/ou les versions de distribution générale (GDR) en fonction des besoins. Pour déterminer la version de PolyBase, consultez [Déterminer la version, l’édition et le niveau de mise à jour de SQL Server et de ses composants](https://docs.microsoft.com/troubleshoot/sql/general/determine-version-edition-update-level#polybase).
   
### <a name="how-to-confirm-installation"></a><a id="confirminstall"></a> Comment vérifier l’installation  

Exécutez la commande suivante : Si PolyBase est installé, la valeur de retour est 1. Sinon, la valeur est 0.  

```sql  
SELECT SERVERPROPERTY ('IsPolyBaseInstalled') AS IsPolyBaseInstalled;  
```  

### <a name="firewall-rules"></a>Règles de pare-feu  

Le programme d’installation de SQL Server PolyBase crée les règles de pare-feu suivantes sur l’ordinateur :  
   
- PolyBase SQL Server - Moteur de base de données - \<SQLServerInstanceName> (TCP entrant)  
   
- PolyBase SQL Server - Services PolyBase - \<SQLServerInstanceName> (TCP entrant)  

- SQL Server PolyBase - SQL Browser - (UDP-entrant)  
   
Lors de l’installation, si vous utilisez l’instance de SQL Server dans le cadre d’un groupe de scale out PolyBase, ces règles sont activées. Le pare-feu s’ouvre pour autoriser les connexions entrantes. Elles sont autorisées pour le moteur de base de données SQL Server, le moteur PolyBase SQL Server, le service Mouvement de données PolyBase SQL Server et SQL Browser. Si le service de pare-feu de l’ordinateur n’est pas en cours d’exécution au moment de l’installation, la configuration de SQL Server ne peut pas activer ces règles. Dans ce cas, démarrez le service de pare-feu sur l’ordinateur et activez ces règles après l’installation.  
   
#### <a name="to-enable-the-firewall-rules"></a>Pour activer les règles de pare-feu  

1. Ouvrez le **Panneau de configuration**.  

2. Sélectionnez **Système et sécurité**, puis **Pare-feu Windows**.  
   
3. Sélectionnez **Paramètres avancés**, puis **Règles de trafic entrant**.  
   
4. Cliquez avec le bouton droit sur la règle désactivée, puis sélectionnez **Activer la règle**.  
   
### <a name="polybase-service-accounts"></a>Comptes de service PolyBase

Pour changer les comptes de service pour le moteur PolyBase et le service Mouvement de données PolyBase, désinstallez et réinstallez la fonctionnalité PolyBase.

## <a name="next-steps"></a>Étapes suivantes  

Consultez [PolyBase configuration](../../relational-databases/polybase/polybase-configuration.md).
