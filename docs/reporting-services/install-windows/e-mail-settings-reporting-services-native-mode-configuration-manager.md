---
title: Paramètres d’e-mail dans SSRS en mode natif (Gestionnaire de configuration)| Microsoft Docs
description: SQL Server Reporting Services comprend une extension de la remise des e-mails que vous pouvez utiliser pour distribuer les rapports par e-mail.
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
f1_keywords:
- SQL13.rsconfigtool.emailsettings.F1
helpviewer_keywords:
- SQL11.rsconfigtool.emailsettings.F1
ms.assetid: cdad1529-bfa6-41fb-9863-d9ff1b802577
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 31ab6390286c748d8f9cb8f6b98b80c9c53818a1
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596928"
---
# <a name="e-mail-settings---reporting-services-native-mode-configuration-manager"></a>Paramètres de messagerie : mode natif de Reporting Services (Gestionnaire de configuration)
SQL Server Reporting Services comprend une extension de la remise des e-mails que vous pouvez utiliser pour distribuer les rapports par e-mail. Selon la façon dont vous définissez l'abonnement de messagerie électronique, une remise peut consister en une notification, un lien, une pièce jointe ou un rapport incorporé. L'extension de remise de courrier électronique fonctionne avec votre technologie de serveur de messagerie existante. Le serveur de messagerie doit être un serveur SMTP ou redirecteur. Le serveur de rapports se connecte à un serveur SMTP par le biais de bibliothèques CDO (Collaboration Data Objects) (cdosys.dll) fournies par le système d'exploitation.

L'extension de remise du courrier électronique par le serveur de rapports n'est pas configurée par défaut. Vous devez utiliser le Gestionnaire de configuration du serveur de rapports pour effectuer une configuration minimale de l’extension. Pour définir des propriétés avancées, vous devez modifier le fichier RSReportServer.config. Si vous ne pouvez pas configurer le serveur de rapports afin qu'il utilise cette extension, vous pouvez remettre les rapports dans un dossier partagé à la place. Pour plus d’informations, consultez Remise par partage de fichiers dans Reporting Services.

## <a name="configuration-requirements"></a>Configuration requise

- La remise du courrier électronique par le serveur de rapports est implémentée sur des objets de données de collaboration (CDO) et nécessite un serveur SMTP (Simple Mail Transfer Protocol) local ou distant, ou encore un redirecteur SMTP. Le protocole SMTP n'est pas pris en charge sur tous les systèmes d'exploitation Windows. Si vous utilisez l'édition Itanium de Windows Server 2008, le protocole SMTP n'est pas pris en charge. Pour plus d'informations sur les options de configuration fournies par le biais des objets CDO, consultez [Configuration CoClass](/previous-versions/exchange-server/exchange-10/ms526939(v=exchg.10)) sur MSDN.

Le compte d’authentification configuré doit être autorisé à envoyer des e-mails sur le serveur SMTP.

- L'extension de remise du courrier électronique utilise l'encodage UTF-8 dans les pièces jointes électroniques. Vous ne pouvez pas modifier cet encodage ; l'extension de rendu HTML prend en charge UTF-8 uniquement.

> [!NOTE] 
> L'extension par défaut de la remise du courrier électronique ne prend pas en charge la signature numérique et le chiffrement des messages sortants.

## <a name="setting-configuration-options-for-e-mail-delivery"></a>Définition des options de configuration pour la remise du courrier électronique
Avant d'utiliser la remise du courrier électronique par le serveur de rapports, vous devez définir les valeurs de configuration fournissant des informations sur le mode d'utilisation du serveur SMTP.

Pour configurer un serveur de rapports pour la remise par messagerie, procédez comme suit :

- Utilisez le Gestionnaire de configuration du serveur de rapports si vous spécifiez simplement un serveur SMTP et un compte d’utilisateur ayant l’autorisation d’envoyer des e-mails. Ce sont les paramètres minimum requis pour configurer l'extension de remise du courrier électronique par le serveur de rapports.

- (Facultatif) Utilisez un éditeur de texte pour spécifier les paramètres supplémentaires dans le fichier RSreportserver.config. Ce fichier contient tous les paramètres de configuration pour la remise du courrier électronique par le serveur de rapports. La spécification de paramètres supplémentaires dans ces fichiers est obligatoire si vous utilisez un serveur SMTP local ou si vous limitez la remise par messagerie à des hôtes spécifiques. Pour plus d’informations sur la recherche et la modification des fichiers de configuration, consultez [Modifier un fichier de configuration Reporting Services (RSreportserver.config)](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).

> [!NOTE] 
> Les paramètres du courrier électronique pour le serveur de rapports dépendent du CDO. Si vous souhaitez obtenir plus de détails sur des paramètres spécifiques, reportez-vous à la documentation de production CDO.

## <a name="configure-report-server-e-mail-using-the-report-server-configuration-manager"></a><a name="rsconfigman"/>Configurer l’e-mail du serveur de rapports à l’aide du Gestionnaire de configuration du serveur de rapports

1. Démarrez le Gestionnaire de configuration du serveur de rapports, puis connectez-vous à l’instance du serveur de rapports.

2. Dans **Adresse de l’expéditeur**, entrez l’adresse électronique à utiliser dans le champ **De :** d’un message généré. 

     Vous devez spécifier un compte d'utilisateur qui a l'autorisation d'envoyer du courrier depuis le serveur SMTP. La valeur indiquée pour l’ **Adresse de l’expéditeur** est enregistrée dans le champ `<From>` du fichier rsreportserver.config.  

3.  Dans **Serveur SMTP**, spécifiez le serveur ou la passerelle SMTP à utiliser. 

     Il peut s’agir d’une adresse IP, du nom NetBIOS d’un ordinateur sur l’intranet de votre entreprise ou d’un nom de domaine complet. La valeur indiquée pour le **Serveur SMTP** est enregistrée dans le champ `<SMTPServer>` du fichier rsreportserver.config.

4. Utilisez la liste déroulante **Authentification** pour spécifier le mode d’authentification auprès du serveur SMTP. Cette 

     - **Aucune authentification** signifie que vous vous connectez anonymement au serveur de messagerie qui a été spécifié.
     
          Cette option définit `<SendUsing>` sur la valeur **2** et `<SMTPAuthenticate>` sur la valeur **0** dans le fichier rsreportserver.config.
     
     - **Nom d’utilisateur et mot de passe (de base)** vous permet de spécifier un nom d’utilisateur et un mot de passe pour vous connecter au serveur de messagerie. Vous pouvez également sélectionner **Utiliser une connexion sécurisée** pour recourir à une connexion chiffrée à votre serveur de messagerie.
     
          Cette option définit `<SendUsing>` sur la valeur **2** et `<SMTPAuthenticate>` sur la valeur **1** dans le fichier rsreportserver.config. Le fait de sélectionner **Utiliser une connexion sécurisée** définit `SMTPUseSSL` sur **True**. **Nom d’utilisateur** est défini dans `<SendUserName>` sous forme chiffrée. **Mot de passe** est défini dans `<SendPassword>` sous forme chiffrée.
     
     - **Compte de service du serveur de rapports (NTLM)** utilise le compte de service que vous avez spécifié pour le serveur de rapports. Si vous utilisez le compte de service du serveur de reports pour l’authentification, vérifiez que le compte de service dispose des autorisations **Envoyer en tant que** sur le serveur SMTP.
     
          Cette option définit `<SendUsing>` sur la valeur **2** et `<SMTPAuthenticate>` sur la valeur **2** dans le fichier rsreportserver.config.

5. Sélectionnez **Appliquer**.

6. Vous pouvez éventuellement ajuster des champs supplémentaires pour la configuration de la messagerie électronique, dans le fichier rsreportserver.config.

## <a name="example-report-server-e-mail-configuration"></a>Configuration du courrier électronique pour Report Server
L'exemple suivant illustre les paramètres dans le fichier RSreportserver.config pour un serveur SMTP distant. Pour en savoir plus sur les descriptions des paramètres et les valeurs valides, consultez [Fichier de configuration Rsreportserver.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).

```
<RSEmailDPConfiguration>
     <SMTPServer>mySMTPServer.Adventure-Works.com</SMTPServer>
     <SMTPServerPort></SMTPServerPort>
     <SMTPAccountName></SMTPAccountName>
     <SMTPConnectionTimeout></SMTPConnectionTimeout>
     <SMTPServerPickupDirectory></SMTPServerPickupDirectory>
     <SMTPUseSSL>False</SMTPUseSSL>
     <SendUsing>2</SendUsing>
     <SMTPAuthenticate>2</SMTPAuthenticate>
     <From>my-rs-email-account@Adventure-Works.com</From>
     <EmbeddedRenderFormats>
          <RenderingExtension>MHTML</RenderingExtension>
     </EmbeddedRenderFormats>
     <PrivilegedUserRenderFormats></PrivilegedUserRenderFormats>
     <ExcludedRenderFormats>
          <RenderingExtension>HTMLOWC</RenderingExtension>
          <RenderingExtension>NULL</RenderingExtension>
          <RenderingExtension>RGDI</RenderingExtension>
     </ExcludedRenderFormats>
     <SendEmailToUserAlias>True</SendEmailToUserAlias>
     <DefaultHostName></DefaultHostName>
     <PermittedHosts>
          <HostName>Adventure-Works.com</HostName>
          <HostName>hotmail.com</HostName>
     </PermittedHosts>
     <SendUserName></SendUserName>
     <SendPassword></SendPassword>
</RSEmailDPConfiguration>
```
## <a name="configuration-options-for-setting-the-to-field-in-a-message"></a>Options de configuration pour la définition du champ À : dans un message
Les abonnements définis par l’utilisateur qui sont créés en fonction des autorisations accordées par la tâche Gérer les abonnements individuels contiennent un nom d’utilisateur prédéfini qui repose sur le compte d’utilisateur de domaine. Quand l’utilisateur crée l’abonnement, le nom du destinataire dans le champ **À :** est configuré automatiquement à l’adresse de la personne qui crée l’abonnement, au moyen du compte d’utilisateur de domaine.

Si vous utilisez un redirecteur ou un serveur SMTP qui utilise des comptes de messagerie différents du compte d'utilisateur de domaine, la remise des rapports échouera lorsque le serveur SMTP tentera de remettre le rapport à cet utilisateur.

Pour contourner ce problème, vous pouvez modifier les paramètres de configuration qui permettent aux utilisateurs d'entrer un nom dans le champ **À :** .

1. Ouvrez le fichier RSReportServer.config avec un éditeur de texte.

2. Affectez à `<SendEmailToUserAlias>` la valeur **False**.

3. Définissez `<DefaultHostName>` au nom DNS (Domain Name System) ou à l'adresse IP du redirecteur ou du serveur SMTP.

4. Enregistrez le fichier .

## <a name="configuration-options-for-remote-smtp-service"></a>Options de configuration pour le service SMTP distant
La connexion entre le serveur de rapports et le serveur ou redirecteur SMTP est déterminée par les paramètres de configuration suivants :

- `<SendUsing>` spécifie une méthode pour l’envoi de messages. Vous pouvez choisir entre un service SMTP réseau ou un répertoire de collecte du service SMTP local. Pour utiliser un service SMTP distant, cette valeur doit être définie sur **2** dans le fichier RSReportServer.config.
- `<SMTPServer>` spécifie le serveur ou le redirecteur SMTP distant. Cette valeur est obligatoire si vous utilisez un serveur ou un redirecteur SMTP distant.
- `<From>` définit la valeur qui s’affiche sur la ligne **De :** d’un e-mail. Cette valeur est obligatoire si vous utilisez un serveur ou un redirecteur SMTP distant.

D'autres valeurs utilisées pour le service SMTP distant comprennent ce qui suit (notez que vous n'avez pas besoin de les spécifier à moins de vouloir remplacer les valeurs par défaut).

- `<SMTPServerPort>` est configuré pour le port 25 par défaut.
- `<SMTPAuthenticate>` spécifie le mode de connexion du serveur de rapports au serveur SMTP distant. La valeur par défaut est **0** (ou aucune authentification). Dans ce cas, la connexion est effectuée par un accès anonyme. En fonction de la configuration de votre domaine, il est possible que le serveur de rapports et le serveur SMTP soient obligés d'être des membres du même domaine.
- Pour envoyer des e-mails aux listes de distribution limitée (par exemple, les listes de distribution qui acceptent des messages entrants uniquement à partir de comptes authentifiés), définissez `<SMTPAuthenticate>` sur **1** ou **2**. Si vous le définissez sur **1**, vous devez définir `<SendUserName>` et `<SendPassword>`. Nous vous recommandons d’effectuer cette opération par le biais du Gestionnaire de configuration du serveur de rapports, car il chiffre les valeurs de `<SendUserName>` et `<SendPassword>`.

### <a name="to-configure-a-remote-smtp-service-for-the-report-server"></a>Pour configurer un service SMTP distant pour le serveur de rapports

> [!NOTE] 
> Nous vous recommandons de configurer le serveur de messagerie au moyen du Gestionnaire de configuration du serveur de rapports.

1. Vérifiez que le service Report Server Windows a des autorisations **Send As** sur le serveur SMTP.

2. Ouvrez le fichier RSReportServer.config dans un éditeur de texte.

3. Vérifiez que `<UrlRoot>` est paramétré à l'adresse URL du serveur de rapports. Cette valeur est définie lorsque vous configurez le serveur de rapports et elle devrait normalement être déjà définie. Si elle n'est pas définie, tapez l'adresse URL du serveur de rapports.

4. Dans la section Remise, recherchez `<RSEmailDPConfiguration>`.
     
5. Dans `<SMTPServer>`, tapez le nom du serveur SMTP. Il peut s'agir d'une adresse IP, du nom UNC d'un ordinateur sur l'intranet de votre entreprise ou d'un nom de domaine complet.

6. Définissez `<SendUsing>` sur la valeur **2** pour utiliser le compte de service pour le serveur de rapports. Définissez `<SendUsing>` sur la valeur **1** pour une authentification de base. Si vous le définissez sur **1**, vous devez en plus fournir une valeur pour `<SendUserName>` et `<SendPassword>`. Si vous souhaitez que ces valeurs soient chiffrées, définissez l’authentification dans le Gestionnaire de configuration du serveur de rapports.

7. Définissez `<SMTPAuthenticate>` sur la valeur **1** si vous définissez `<SendUsing>` sur 1 ou 2.

7. Définissez `<From>`. Vous devez spécifier un compte d'utilisateur qui a l'autorisation d'envoyer du courrier depuis le serveur SMTP.

8. Enregistrez le fichier .

     Le serveur de rapports utilise automatiquement les nouveaux paramètres ; il n'est pas nécessaire de redémarrer le service. Vous pouvez spécifier des paramètres SMTP supplémentaires pour configurer comment le serveur SMTP est utilisé pour la remise par messagerie du serveur de rapports.

## <a name="configuration-options-for-local-smtp-service"></a>Options de configuration pour le service SMTP local
La configuration d'un service SMTP local est pratique si vous testez ou dépannez la remise du courrier électronique par le serveur de rapports. Par défaut, le service SMTP local n'est pas activé.

La connexion entre le serveur de rapports et le serveur ou le redirecteur SMTP local est déterminée par les paramètres de configuration suivants :

- **SendUsing** est défini sur **1**.
- **SMTPServerPickupDirectory** est défini sur un dossier de lecteur local.

  > [!NOTE] 
  > Assurez-vous de ne pas définir SMTPServer si vous utilisez un serveur SMTP local.

- **De** définit la valeur qui s’affiche sur la ligne **De :** d’un e-mail. Cette valeur est requise.

### <a name="to-configure-a-local-smtp-service-for-the-report-server"></a>Pour configurer un service SMTP local pour le serveur de rapports

1. Dans le panneau de configuration, sélectionnez **Activer ou désactiver des fonctionnalités Windows** pour démarrer l’ **Assistant Ajout de rôles et de fonctionnalités**.

2. Sélectionnez **Installation basée sur un rôle ou une fonctionnalité** et sélectionnez **Suivant**.

3. Sélectionnez le serveur sur lequel installer Internet Information Server (IIS), puis sélectionnez **Suivant**.

4. Sélectionnez **Suivant** dans la page *Rôles du serveur*.
     
5. Dans la page *Fonctionnalités* , sélectionnez **Serveur SMTP** , puis sélectionnez **Suivant**.

     Si vous êtes invité à ajouter des fonctionnalités requises pour le serveur SMTP, sélectionnez **Ajouter des fonctionnalités**.

6. Dans la page **Rôle Web Server (IIS)** , sélectionnez *Suivant* .

7. Dans la page **Services de rôle** , sélectionnez *Suivant* .

8. Dans la page **Confirmation** , sélectionnez **Installer** .

9. Vérifiez que le service Windows **SMTP (Simple Mail Transfer Protocol)** s’exécute dans la console Services.

     Pour configurer le serveur SMTP local, vous devez utiliser le Gestionnaire IIS 6.0 sous Outils d’administration.

10. Ouvrez le fichier RSReportServer.config dans un éditeur de texte.

11. Vérifiez que `<UrlRoot>` est paramétré à l'adresse URL du serveur de rapports. Cette valeur est définie lorsque vous configurez le serveur de rapports et elle devrait normalement être déjà définie. Si elle n’est pas définie, tapez l’adresse URL du service web pour votre serveur de rapports.

12. Dans la section Remise, recherchez `<RSEmailDPConfiguration>`.
     
13. Assurez-vous que `<SMTPServer>` est présent, mais vide.
     
14. Affectez à `<SendUsing>` la valeur 1.
     
14. Définissez `<SMTPAuthenticate>` sur 0.
     
15. Définissez `<SMTPServerPickupDirectory>` sur le dossier de collecte du Service SMTP.
     
     L’emplacement par défaut est *C:\inetpub\mailroot\Pickup*.
     
16. Définissez `<From>`. Ce paramètre définit la valeur qui s’affiche sur la ligne **De :** d’un e-mail.
     
17. Enregistrez le fichier .
  
## <a name="see-also"></a>Voir aussi  
[Report Server Configuration Manager (mode natif)](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  
[Modify a Reporting Services Configuration File (rsreportserver.config)](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)  
[Fichier de configuration Rsreportserver.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)
  
