---
description: Service d’émission de jetons Revendications vers Windows (C2WTS) et Reporting Services
title: Service d’émission de jetons Revendications vers Windows (C2WTS) et Reporting Services | Microsoft Docs
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.topic: conceptual
ms.date: 09/15/2017
ms.openlocfilehash: 78d7265398cab553a9378fecc54b25a32d36e84d
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489829"
---
# <a name="claims-to-windows-token-service-c2wts-and-reporting-services"></a>Service d'émission de jetons Revendications vers Windows (C2WTS) et Reporting Services

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

Le service d’émission de jetons Revendications vers Windows (C2WTS) de SharePoint est nécessaire si vous souhaitez afficher les rapports en mode natif dans le [composant WebPart Visionneuse de rapports SQL Server Reporting Services](../report-server-sharepoint/deploy-report-viewer-web-part.md).

Il est également nécessaire avec le mode SharePoint de SQL Server Reporting Services, si vous souhaitez utiliser l’authentification Windows pour les sources de données situées hors de la batterie de serveurs SharePoint. C2WTS est nécessaire même si vos sources de données sont sur le même ordinateur que le service partagé. Toutefois dans ce scénario, la délégation contrainte n'est pas nécessaire.

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.

## <a name="report-viewer-native-mode-web-part-configuration"></a>Configuration du composant WebPart Visionneuse de rapports (mode natif)

Le composant WebPart Visionneuse de rapports peut être utilisé pour incorporer des rapports SQL Server Reporting Services en mode natif dans votre site SharePoint. Ce composant WebPart est disponible pour SharePoint 2013 et SharePoint 2016. SharePoint 2013 et SharePoint 2016 utilisent tous deux l’authentification basée sur les revendications. Par conséquent, C2WTS doit être configuré correctement et Reporting Services doit être configuré avec l’authentification Kerberos pour que les rapports s’affichent correctement.

1. Configurez votre instance Reporting Services (mode natif) pour l’authentification Kerberos en spécifiant le compte de service SSRS, en définissant un SPN et en mettant à jour le fichier rsreportserver.config pour utiliser le type d’authentification RSWindowsNegotiate. [Enregistrer un nom de principal du service (SPN) pour un serveur de rapports](../report-server/register-a-service-principal-name-spn-for-a-report-server.md)

2. Suivez les étapes dans [Étapes nécessaires pour configurer C2WTS](#steps-needed-to-configure-c2wts)
 

## <a name="sharepoint-mode-integration"></a>Intégration du mode SharePoint

**Cette section s’applique uniquement à SQL Server 2016 Reporting Services et antérieur.**

Le service d’émission de jetons Revendications vers Windows (C2WTS) SharePoint est nécessaire avec le mode SharePoint de SQL Server Reporting Services si vous souhaitez utiliser l’authentification Windows pour les sources de données situées hors de la batterie de serveurs SharePoint. Ceci vaut même si l’utilisateur accède aux sources de données avec l’authentification Windows, car la communication entre le serveur web frontal (WFE) et le service partagé Reporting Services s’effectue toujours via l’authentification basée sur les revendications.

## <a name="steps-needed-to-configure-c2wts"></a>Étapes nécessaires pour configurer C2WTS

Les jetons créés par C2WTS ne fonctionnent qu’avec la délégation contrainte (pour certains services uniquement) et l’option de configuration « avec n’importe quel protocole d’authentification » (transition de protocole).

Si votre environnement utilise la délégation contrainte Kerberos, le service SharePoint server et les sources de données externes doivent résider dans le même domaine Windows. Tout service qui s’appuie sur le service d’émission de jetons Revendications vers Windows (C2WTS) doit utiliser la délégation **contrainte** Kerberos pour permettre à C2WTS d’utiliser une transition de protocole Kerberos dans la conversion de revendications en informations d’identification Windows. Ces exigences s'appliquent à tous les services partagés SharePoint. Pour plus d’informations, consultez [Planifier l’authentification Kerberos dans SharePoint 2013](/SharePoint/security-for-sharepoint-server/kerberos-authentication-planning).  

1. Configurez le compte de domaine de service C2WTS. 

    **Comme bonne pratique, C2WTS doit s’exécuter sous sa propre identité de domaine.**

    * Créez un compte Active Directory et inscrivez-le comme compte géré dans SharePoint Server.
   
    * Configurez le service C2WTS pour utiliser le compte géré via l’Administration centrale de SharePoint > Sécurité > Configurer les comptes de service > Service Windows - Service d’émission de jetons Revendications vers Windows

    Ajoutez le compte de service C2WTS au groupe Administrateurs local sur chaque serveur d’applications sur lequel C2WTS sera utilisé. Pour le **Composant WebPart Visionneuse de rapports**, il s’agit des serveurs web frontaux. Pour le **mode intégré SharePoint**, il s’agit des serveurs d’applications sur lesquels le service Reporting Services s’exécute.
    * Accordez au compte C2WTS les autorisations suivantes dans la stratégie de sécurité locale sous Stratégies locales > Attribution des droits utilisateur :
        * Agir en tant que partie du système d'exploitation
        * Emprunter l'identité d'un client après authentification
        * Ouvrir une session en tant que service

    
2. Configurez la délégation pour le compte de service C2WTS.

    Le compte a besoin de la délégation contrainte avec transition de protocole; ainsi que d’autorisations de déléguer aux services avec lesquelles il doit communiquer (par exemple, moteur de base de données SQL Server, SQL Server Analysis Services). Pour configurer la délégation, vous pouvez utiliser le composant Utilisateurs et ordinateurs Active Directory et devez être administrateur du domaine.

    > [!IMPORTANT]
    > Dans tous les cas, les paramètres que vous configurez pour le compte de service C2WTS, sous l’onglet Délégation, doivent correspondre au compte de service principal utilisé. Pour le **composant WebPart Visionneuse de rapports**, il s’agit du compte de service de l’application web SharePoint. Pour le **mode intégré SharePoint**, il s’agit du compte de service Reporting Services.
    >
    > Par exemple, si vous autorisez le compte de service C2WTS à déléguer à un service SQL, vous devez faire de même sur le compte de service Reporting Services pour le mode intégré SharePoint.

    * Cliquez avec le bouton droit sur chaque compte de service et ouvrez la boîte de dialogue des propriétés. Dans la boîte de dialogue, cliquez sur l'onglet **Délégation** .

        L’onglet Délégation est visible uniquement si un SPN (Service Principal Name) a été affecté à l’objet. C2WTS ne nécessite pas de SPN sur le compte C2WTS, mais sans SPN, l’onglet **Délégation** ne sera pas visible. Une autre façon de configurer la délégation contrainte consiste à utiliser un utilitaire tel que **ADSIEdit**.

    * Les options de configuration principales dans l'onglet Délégation sont les suivantes :

        * Sélectionnez **N’approuver cet utilisateur que pour la délégation aux services spécifiés**
        * Sélectionnez **Utiliser tout protocole d’authentification**.

    * Sélectionnez **Ajouter** pour ajouter un service auquel déléguer.

    * Sélectionnez **Utilisateurs ou ordinateurs...&#42;** et entrez le compte qui héberge le service. Par exemple, si un serveur SQL Server s’exécute sous un compte nommé *sqlservice*, entrez `sqlservice`. 
      Pour le **composant WebPart Visionneuse de rapports**, il s’agit du compte de service pour l’instance Reporting Services (mode natif).

    * Sélectionnez la liste des services. Les SPN disponibles sur ce compte s’affichent. Si vous ne voyez pas le service sur ce compte, il est peut-être manquant ou placé sur un autre compte. Vous pouvez utiliser l’utilitaire SetSPN pour ajuster les SPN. Pour le **composant WebPart Visionneuse de rapports**, vous verrez le SPN HTTP configuré dans [Configuration du composant WebPart Visionneuse de rapports](#report-viewer-native-mode-web-part-configuration).

    * Cliquez sur OK pour fermer les boîtes de dialogue.

3. Configurer les *appelants autorisés* dans C2WTS

    C2WTS exige que les identités des appelants soient explicitement listées dans le fichier de configuration **c2WTShost.exe.config**. C2WTS n’accepte pas les requêtes de tous les utilisateurs authentifiés dans le système, sauf s’il est configuré pour cela. Dans ce cas, l’appelant est le groupe Windows WSS_WPG. Le fichier C2WTShost.exe.confi est enregistré à l’emplacement suivant :

    La modification du compte de service dans l’Administration centrale de SharePoint, pour le service C2WTS, ajoute ce compte au groupe WSS_WPG.

    **\Program Files\Windows Identity Foundation\v3.5\c2WTShost.exe.config**

    L'exemple suivant montre le fichier de configuration :

    ```
    <configuration>
      <windowsTokenService>
        <!--  
            By default no callers are allowed to use the Windows Identity Foundation Claims To NT Token Service.  
            Add the identities you wish to allow below.  
          -->
        <allowedCallers>
          <clear/>
          <add value="WSS_WPG" />
        </allowedCallers>
      </windowsTokenService>
    </configuration>
    ```

4. Démarrez (arrêtez et démarrez si déjà démarré) les revendications SharePoint vers Windows Token Service via l’Administration centrale de SharePoint dans la page **Gérer les services sur le serveur**. Le service doit être démarré sur le serveur qui effectuera l'action. Par exemple si vous avez un serveur web frontal et un serveur d’applications exécutant le service partagé SQL Server Reporting Services, il vous suffit de démarrer C2WTS sur le serveur d’applications. C2WTS est obligatoire sur un serveur web frontal uniquement si vous utilisez le composant WebPart Visionneuse de rapports.

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
