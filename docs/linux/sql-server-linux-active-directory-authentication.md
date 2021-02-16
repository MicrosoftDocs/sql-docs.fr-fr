---
title: 'Tutoriel : Utiliser l’authentification AD pour SQL Server sur Linux'
titleSuffix: SQL Server
description: Ce tutoriel présente les étapes de configuration de l’authentification AD (Active Directory) pour SQL Server sur Linux.
author: Dylan-MSFT
ms.author: dygray
ms.reviewer: vanto
ms.date: 12/18/2019
ms.topic: tutorial
ms.prod: sql
ms.custom: seodec18
ms.technology: linux
helpviewer_keywords:
- Linux, AAD authentication
ms.openlocfilehash: c6487f5c22a61cf1a4979af18d5cc3fdae5b0eb2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273150"
---
# <a name="tutorial-use-active-directory-authentication-with-sql-server-on-linux"></a>Tutoriel : Utiliser l’authentification Active Directory avec SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Ce didacticiel explique comment configurer [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Linux pour prendre en charge l’authentification Active Directory (AD), également appelée authentification intégrée. Pour une vue d’ensemble, consultez [Authentification Active Directory avec SQL Server sur Linux](sql-server-linux-active-directory-auth-overview.md).

Ce didacticiel contient les tâches suivantes :

> [!div class="checklist"]
> * joindre l’hôte [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] au domaine AD
> * créer un utilisateur AD [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour et définir le SPN
> * configurer le service [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] keytab
> * Sécuriser le fichier keytab
> * Configurer SQL Server pour utiliser le fichier keytab pour l’authentification Kerberos
> * créer des connexions basées sur AD dans Transact-SQL
> * se connecter à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à l’aide de l’authentification AD

## <a name="prerequisites"></a>Prérequis

Avant de configurer l’authentification AD, vous devez :

* Configurer un contrôleur de domaine AD (Windows) sur votre réseau  
* Installer [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]
  * [Red Hat Enterprise Linux (RHEL)](quickstart-install-connect-red-hat.md)
  * [SUSE Linux Enterprise Server (SLES)](quickstart-install-connect-suse.md)
  * [Ubuntu](quickstart-install-connect-ubuntu.md)

## <a name="join-ssnoversion-host-to-ad-domain"></a><a id="join"></a> Joindre l’hôte [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] au domaine AD

Joignez votre hôte SQL Server Linux à un contrôleur de domaine Active Directory. Pour plus d’informations sur la façon de joindre un domaine Active Directory, consultez [Joindre SQL Server sur un hôte Linux à un domaine Active Directory](sql-server-linux-active-directory-join-domain.md).

## <a name="create-ad-user-or-msa-for-ssnoversion-and-set-spn"></a><a id="createuser"></a> Créer un utilisateur AD (ou MSA) pour [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et définir le SPN

> [!NOTE]
> Les étapes suivantes utilisent votre [ nom de domaine complet](https://en.wikipedia.org/wiki/Fully_qualified_domain_name). Si vous êtes sur **Azure**, vous devez **[en créer un](/azure/virtual-machines/linux/portal-create-fqdn)** avant de continuer.

1. Sur votre contrôleur de domaine, exécutez la commande PowerShell [New-ADUser](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee617253(v=technet.10)) pour créer un nouvel utilisateur AD avec un mot de passe qui n’expire jamais. L’exemple suivant nomme le compte `mssql`, mais le nom du compte peut être tout ce que vous aimez. Vous serez invité à entrer un nouveau mot de passe pour le compte.

   ```PowerShell
   Import-Module ActiveDirectory

   New-ADUser mssql -AccountPassword (Read-Host -AsSecureString "Enter Password") -PasswordNeverExpires $true -Enabled $true
   ```

   > [!NOTE]
   > La meilleure pratique de sécurité consiste à disposer d’un compte AD dédié pour SQL Server, afin que les informations d’identification de SQL Server ne soient pas partagées avec d’autres services utilisant le même compte. Toutefois, vous pouvez éventuellement réutiliser un compte AD existant si vous connaissez le mot de passe du compte (qui est requis pour générer un fichier keytab à l’étape suivante). De plus, le compte doit être activé pour prendre en charge le chiffrement Kerberos AES 128 bits et 256 bits (attribut **msDS-SupportedEncryptionTypes**) sur le compte d’utilisateur. Pour confirmer que le chiffrement AES est activé pour le compte, localisez le compte dans l’utilitaire **Utilisateurs et ordinateurs Active Directory**, puis sélectionnez **Propriétés**. Localisez l’onglet **Comptes** dans **Propriétés** et validez que les deux cases à cocher ci-dessous sont sélectionnées. 
   >
   > 1. **Ce compte prend en charge le chiffrement AES 128 bits via Kerberos**
   >
   > 2. **Ce compte prend en charge le chiffrement AES 256 bits via Kerberos**

2. Définissez le nom de principal du service (SPN) pour ce compte à l’aide de l’outil **setspn.exe**. Le SPN doit être mis en forme exactement comme spécifié dans l’exemple suivant. Vous pouvez trouver le nom de domaine complet de la machine hôte [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en exécutant `hostname --all-fqdns` sur l'hôte [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Le port TCP doit être 1433, sauf si vous avez configuré [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour utiliser un numéro de port différent.

   ```PowerShell
   setspn -A MSSQLSvc/<fully qualified domain name of host machine>:<tcp port> mssql
   setspn -A MSSQLSvc/<netbios name of the host machine>:<tcp port> mssql
   ```

   > [!NOTE]
   > Si vous recevez une erreur, `Insufficient access rights`, vérifiez auprès de votre administrateur de domaine que vous disposez des autorisations suffisantes pour définir un SPN sur ce compte. Le compte utilisé pour inscrire un nom de principal du service doit avoir les autorisations **Write servicePrincipalName**. Pour plus d’informations, consultez [Inscrire un nom de principal du service pour les connexions Kerberos](../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).
   >
   > Si vous modifiez le port TCP à l’avenir, vous devez exécuter à nouveau la commande **setspn** avec le nouveau numéro de port. Vous devez également ajouter le nouveau nom de principal du service au service keytab SQL Server en suivant les étapes décrites dans la section suivante.

Pour plus d’informations, consultez [Inscrire un nom de principal du service pour les connexions Kerberos](../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).

## <a name="configure-ssnoversion-service-keytab"></a><a id="configurekeytab"></a> Configurer le service keytab [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

La configuration de l’authentification AD pour SQL Server sur Linux nécessite un compte AD (compte MSA ou compte d’utilisateur AD) et le nom de principal du service créé à la section précédente.

> [!IMPORTANT]
> Si le mot de passe du compte AD est modifié ou si le mot de passe du compte auquel les noms de principal du service sont affectés est modifié, vous devez mettre à jour le keytab avec le nouveau mot de passe et le numéro de version de la clé (KVNO). Certains services peuvent également faire pivoter les mots de passe automatiquement. Consultez les stratégies de rotation des mots de passe pour les comptes en question et alignez-les avec des activités de maintenance planifiées pour éviter les temps d’arrêt inattendus.

### <a name="spn-keytab-entries"></a><a id="spn"></a> Entrées keytab du SPN

1. Vérifiez le numéro de version de la clé (KVNO) pour le compte AD créé à l’étape précédente. En général, il s’agit de 2, mais il peut s’agir d’un autre entier si vous avez modifié le mot de passe du compte plusieurs fois. Sur la machine hôte SQL Server, exécutez les commandes suivantes :

    - Les exemples ci-dessous supposent que `user` se trouve dans le domaine `@CONTOSO.COM`. Remplacez l’utilisateur et le nom de domaine par vos noms d’utilisateur et de domaine.

   ```bash
   kinit user@CONTOSO.COM
   kvno user@CONTOSO.COM
   kvno MSSQLSvc/<fully qualified domain name of host machine>:<tcp port>@CONTOSO.COM
   ```

   > [!NOTE]
   > Les noms de principal du service peuvent prendre plusieurs minutes pour se propager dans votre domaine, en particulier si le domaine est volumineux. Si vous recevez l’erreur, `kvno: Server not found in Kerberos database while getting credentials for MSSQLSvc/<fully qualified domain name of host machine>:<tcp port>@CONTOSO.COM` veuillez patienter quelques minutes, puis réessayez.</br></br> Les commandes ci-dessus fonctionnent uniquement si le serveur a été joint à un domaine AD (opération abordée à la section précédente).

1. À l’aide de [**Ktpass**](/windows-server/administration/windows-commands/ktpass), ajoutez des entrées keytab pour chaque nom de principal du service en utilisant les commandes suivantes dans une invite de commandes Windows :

    - `<DomainName>\<UserName>` - Il peut s’agir d’un compte MSA ou d’un compte d’utilisateur AD.
    - `@CONTOSO.COM` - Utilisez votre nom de domaine.
    - `/kvno <#>` - Remplacez `<#>` par le numéro de version de clé obtenu à l’étape précédente.
    - `<StrongPassword>` - Utilisez un mot de passe fort.

   ```bash
   ktpass /princ MSSQLSvc/<fully qualified domain name of host machine>:<tcp port>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto aes256-sha1 /mapuser <DomainName>\<UserName> /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ MSSQLSvc/<fully qualified domain name of host machine>:<tcp port>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto rc4-hmac-nt /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ MSSQLSvc/<netbios name of the host machine>:<tcp port>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto aes256-sha1 /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ MSSQLSvc/<netbios name of the host machine>:<tcp port>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto rc4-hmac-nt /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ <UserName>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto aes256-sha1 /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>

   ktpass /princ <UserName>@CONTOSO.COM /ptype KRB5_NT_PRINCIPAL /crypto rc4-hmac-nt /mapuser <DomainName>\<UserName> /in mssql.keytab /out mssql.keytab -setpass -setupn /kvno <#> /pass <StrongPassword>
   ```

   > [!NOTE]
   > Les commandes ci-dessus permettent d’utiliser les méthodes de chiffrement AES et RC4 pour l’authentification AD. RC4 est une méthode de chiffrement plus ancienne. Si un niveau de sécurité plus élevé est nécessaire, vous pouvez créer les entrées keytab avec la méthode de chiffrement AES uniquement.
   > Les deux dernières entrées `UserName` doivent être en minuscules, sinon l’authentification d’autorisation risque d’échouer.

1. Après l’exécution de la commande ci-dessus, vous devez disposer d’un fichier keytab nommé mssql.keytab. Copiez le fichier sur l’ordinateur SQL Server, dans le dossier `/var/opt/mssql/secrets`.

1. Sécurisez le fichier keytab.

    Toute personne ayant accès à ce fichier keytab peut emprunter l’identité de SQL Server sur le domaine. Veillez donc à limiter l’accès au fichier de sorte que seul le compte mssql dispose d’un accès en lecture :

    ```bash
    sudo chown mssql:mssql /var/opt/mssql/secrets/mssql.keytab
    sudo chmod 400 /var/opt/mssql/secrets/mssql.keytab
    ```

1. Vous devez définir l’option de configuration suivante avec l’outil **mssql-conf** pour spécifier le compte à utiliser pour l’accès au fichier keytab.

   ```bash
   sudo mssql-conf set network.privilegedadaccount <username>
   ```

   > [!NOTE]
   > Entrez uniquement le nom d’utilisateur et non NomDomaine\NomUtilisateur ou username@domain. SQL Server se charge d’ajouter le nom de domaine au nom d’utilisateur le cas échéant.

1. Suivez les étapes ci-dessous pour configurer SQL Server de façon à commencer à utiliser le fichier keytab pour l’authentification Kerberos.

    ```bash
    sudo mssql-conf set network.kerberoskeytabfile /var/opt/mssql/secrets/mssql.keytab
    sudo systemctl restart mssql-server
    ```

    > [!TIP]
    > Désactivez éventuellement les connexions UDP au contrôleur de domaine pour améliorer les performances. Dans de nombreux cas, les connexions UDP échouent régulièrement lors de la connexion à un contrôleur de domaine. Vous pouvez donc définir des options de configuration dans **/etc/krb5.conf** pour ignorer les appels UDP. Modifiez **/etc/krb5.conf** et définissez les options suivantes :
    > ```bash
    > /etc/krb5.conf
    > [libdefaults]
    > udp_preference_limit=0
    > ```

À ce stade, vous êtes prêt à utiliser les connexions basées sur AD dans SQL Server.

## <a name="create-ad-based-logins-in-transact-sql"></a><a id="createsqllogins"></a> Créer des connexions basées sur AD dans Transact-SQL

1. Connectez-vous à SQL Server et créez un nouveau compte de connexion AD :

   ```sql
   CREATE LOGIN [CONTOSO\user] FROM WINDOWS;
   ```

1. Vérifiez que la connexion est maintenant répertoriée dans l’affichage catalogue système [sys. server_principals](../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) :

   ```sql
   SELECT name FROM sys.server_principals;
   ```

## <a name="connect-to-sql-server-using-ad-authentication"></a><a id="connect"></a> Se connecter à SQL Server à l’aide de l’authentification AD

Connectez-vous à une machine client à l’aide de vos informations d’identification de domaine. Vous pouvez maintenant vous connecter à SQL Server sans entrer à nouveau votre mot de passe à l’aide de l’authentification AD. Si vous créez une connexion pour un groupe AD, n’importe quel utilisateur AD membre de ce groupe peut se connecter de la même façon.

Le paramètre de chaîne de connexion spécifique pour que les clients utilisent l’authentification AD dépend du pilote que vous utilisez. Considérez les exemples dans les sections suivantes.

### <a name="sqlcmd-on-a-domain-joined-linux-client"></a>sqlcmd sur un client Linux joint à un domaine

Connectez-vous à un client Linux joint à un domaine à l’aide de **ssh** et de vos informations d’identification de domaine :

```bash
ssh -l user@contoso.com client.contoso.com
```

Vérifiez que vous avez installé le package [mssql-tools](sql-server-linux-setup-tools.md), puis connectez-vous à l’aide de **sqlcmd** sans spécifier d’informations d’identification :

```bash
sqlcmd -S mssql-host.contoso.com
```
À la différence de SQL Windows, l’authentification Kerberos fonctionne pour la connexion locale dans SQL Linux. Toutefois, vous devez toujours fournir le nom de domaine complet de l’hôte SQL Linux, et l’authentification AD ne fonctionnera pas si vous tentez de vous connecter à « . », « localhost », « 127.0.0.1 », etc.

### <a name="ssms-on-a-domain-joined-windows-client"></a>SSMS sur un client Windows joint à un domaine

Connectez-vous à un client Windows joint à un domaine à l’aide de vos informations d’identification de domaine. Assurez-vous que SQL Server Management Studio est installé, puis connectez-vous à votre instance de SQL Server (par exemple, `mssql-host.contoso.com`) en spécifiant l’**authentification Windows** dans la boîte de dialogue **Se connecter au serveur**.

### <a name="ad-authentication-using-other-client-drivers"></a>Authentification AD à l’aide d’autres pilotes clients

Le tableau suivant décrit les suggestions pour d’autres pilotes clients :

| Pilote client | Recommandation |
|---|---|
| **JDBC** | Utiliser l'authentification intégrée Kerberos pour se connecter à SQL Server. |
| **ODBC** | Utiliser l’authentification intégrée. |
| **ADO.NET** | Syntaxe de la chaîne de connexion. |

## <a name="additional-configuration-options"></a><a id="additionalconfig"></a> Options de configuration supplémentaires

Si vous utilisez des utilitaires tiers tels que [PBIS](https://www.beyondtrust.com/), [VAS](https://www.oneidentity.com/products/authentication-services/) ou [Centrify](https://www.centrify.com/) pour joindre l’hôte Linux au domaine AD et que vous souhaitez forcer SQL Server à utiliser la bibliothèque OpenLDAP directement, vous pouvez configurer l'option **disablesssd** avec **mssql-conf** comme suit :

```bash
sudo mssql-conf set network.disablesssd true
systemctl restart mssql-server
```

> [!NOTE]
> Certains utilitaires, tels que le **realmd**, sont configurés pour SSSD, tandis que d’autres outils tels que PBIS, VAS et Centrify ne configurent pas SSSD. Si l’utilitaire utilisé pour joindre le domaine AD n’est pas configuré SSSD, il est recommandé de configurer l’option **disablesssd** sur `true`. Bien que ce ne soit pas indispensable, étant donné que SQL Server tente d’utiliser SSSD pour AD avant de revenir au mécanisme OpenLDAP, il serait plus performant de le configurer afin que SQL Server effectue des appels OpenLDAP directement en ignorant le mécanisme SSSD.

Si votre contrôleur de domaine prend en charge LDAPS, vous pouvez forcer toutes les connexions de SQL Server sur les contrôleurs de domaine à être sur LDAPS. Pour vérifier que votre client peut contacter le contrôleur de domaine via LDAPS, exécutez la commande Bash `ldapsearch -H ldaps://contoso.com:3269`. Pour définir SQL Server pour utiliser uniquement LDAPS, exécutez la commande suivante :

```bash
sudo mssql-conf set network.forcesecureldap true
systemctl restart mssql-server
```

Cette opération utilise LDAPS sur SSSD si la jonction de domaine AD sur l’hôte a été effectuée via le package SSSD et que **disablesssd** n’a pas la valeur true. Si **disablesssd** est défini sur true et que **forcesecureldap** est aussi défini sur true, il utilisera le protocole LDAPS sur les appels de la bibliothèque OpenLDAP effectués par SQL Server.

### <a name="post-sql-server-2017-cu14"></a>Publier SQL Server 2017 CU14

À compter de SQL Server 2017 CU14, si SQL Server était joint à un contrôleur de domaine AD à l’aide de fournisseurs tiers et est configuré pour utiliser des appels OpenLDAP pour la recherche générale AD en défnissant **disablesssd** sur true, vous pouvez également utiliser **enablekdcfromkrb5** option permettant d’obliger SQL Server à utiliser la bibliothèque krb5 pour la recherche KDC au lieu de la recherche DNS inversée pour le serveur KDC.

Cela peut être utile pour le scénario dans lequel vous souhaitez configurer manuellement les contrôleurs de domaine auxquels SQL Server tente de communiquer. Et vous utilisez le mécanisme de la bibliothèque OpenLDAP à l’aide de la liste KDC dans **krb5.conf**.

Tout d’abord, définissez **disablesssd** et **enablekdcfromkrb5conf** sur true, puis redémarrez SQL Server :

```bash
sudo mssql-conf set network.disablesssd true
sudo mssql-conf set network.enablekdcfromkrb5conf true
systemctl restart mssql-server
```

Configurez ensuite la liste KDC dans **/etc/krb5.conf** comme suit :

```/etc/krb5.conf
[realms]
CONTOSO.COM = {
  kdc = dcWithGC1.contoso.com
  kdc = dcWithGC2.contoso.com
}
```

> [!NOTE]
> Bien qu’il ne soit pas recommandé, il est possible d’utiliser des utilitaires, tels que **realmd**, qui configurent SSSD lors de la jonction de l’hôte Linux au domaine, tout en configurant **disablesssd** sur true pour que SQL Server utilise des appels OpenLDAP à la place de SSSD pour les appels associés à Active Directory.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, nous avons parcouru la configuration de l’authentification Active Directory avec SQL Server sur Linux. Vous avez appris à :
> [!div class="checklist"]
> * joindre l’hôte [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] au domaine AD
> * créer un utilisateur AD [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour et définir le SPN
> * configurer le service [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] keytab
> * créer des connexions basées sur AD dans Transact-SQL
> * se connecter à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à l’aide de l’authentification AD

Explorez ensuite d’autres scénarios de migration pour SQL Server sur Linux.

> [!div class="nextstepaction"]
> [Chiffrement des connexions à SQL Server sur Linux](sql-server-linux-encrypted-connections.md)