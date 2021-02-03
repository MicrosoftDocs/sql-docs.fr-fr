---
description: MSSQLSERVER_17832
title: MSSQLSERVER_17832 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17828 (Database Engine error)
- maxtokensize
- 17832 (Database Engine error)
- login packet (bad)
ms.assetid: bd56ffe4-0855-4ada-8aca-251fbc6ff2ce
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e11485d4acc9638741f3ed48943d314a3efb0449
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196636"
---
# <a name="mssqlserver_17832"></a>MSSQLSERVER_17832
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|ID de l’événement|17832|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SRV_BAD_LOGIN_PKT|  
|Texte du message|Le paquet d'ouverture de session servant à ouvrir la connexion présente une structure non valide ; la connexion a été fermée. Contactez le fournisseur de la bibliothèque cliente.%.*ls|  
  
## <a name="explanation"></a>Explication  
L'ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'a pas pu traiter le paquet d'ouverture de session du client, soit parce que le paquet n'a pas été créé correctement, soit parce qu'il a été endommagé pendant la transmission. Ce problème peut également être dû à la configuration de l'ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'adresse IP répertoriée correspond à l'adresse du client.  
  
### <a name="more-information"></a>Informations complémentaires  
Lorsque vous utilisez l'authentification Windows dans un environnement Kerberos, un client reçoit un ticket Kerberos qui contient un certificat PAC (Privilege Attribute Certificate). Le certificat PAC contient différents types de données d'autorisation, notamment les groupes dont l'utilisateur est membre, les droits dont il dispose et les stratégies qui s'appliquent à lui. Lorsque le client reçoit le ticket Kerberos, les informations contenues dans le certificat PAC sont utilisées pour générer le jeton d'accès de l'utilisateur. Le client présente le jeton à l'ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le cadre du paquet d'ouverture de session.  
  
Si le jeton a été créé incorrectement ou endommagé pendant la transmission, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas proposer d'informations supplémentaires sur le problème.  
  
Lorsque l'utilisateur est membre de nombreux groupes ou possède de nombreuses stratégies, le jeton peut devenir trop volumineux et ne pas les répertorier tous et toutes. Si le jeton dépasse la valeur **MaxTokenSize** du serveur, le client ne parvient pas à se connecter, ce qui génère une erreur réseau générale et l’erreur 17832 peut se produire. Ce problème peut affecter uniquement certains utilisateurs : ceux qui appartiennent à de nombreux groupes ou stratégies. Quand le problème est lié à la valeur **MaxTokenSize** du serveur, l’erreur 17832 mentionnée dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est accompagnée d’une erreur dont l’état est 9. Pour plus d’informations sur Kerberos et la valeur **MaxTokenSize**, consultez [KB327825](https://support.microsoft.com/kb/327825).  
  
## <a name="user-action"></a>Action de l'utilisateur  
Pour résoudre ce problème, augmentez la valeur **MaxTokenSize** du serveur à une taille assez importante pour contenir le jeton le plus volumineux de tous les utilisateurs de votre organisation. Pour rechercher la taille de jeton correcte pour votre organisation, envisagez d’utiliser l’application **Tokensz**.  
  
> [!CAUTION]  
> [!INCLUDE[ssNoteRegistry](../../includes/ssnoteregistry-md.md)]  
  
**Pour modifier MaxTokenSize sur le serveur**  
  
1.  Dans le menu **Démarrer** , cliquez sur **Exécuter**.  
  
2.  Tapez **regedit**, puis cliquez sur **OK**. (Si la boîte de dialogue **Contrôle de compte d’utilisateur** s’affiche, cliquez sur **Continuer**.)  
  
3.  Accédez à **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Lsa\Kerberos\Parameters**.  
  
4.  Si le paramètre **MaxTokenSize** est absent, cliquez avec le bouton droit sur **Paramètres**, pointez sur **Nouveau**, puis cliquez sur **Valeur DWORD (32 bits)** . Nommez l’entrée de Registre **MaxTokenSize**.  
  
5.  Cliquez avec le bouton droit sur **MaxTokenSize**, puis cliquez sur **Modifier**.  
  
6.  Dans la zone **Données de la valeur**, tapez la valeur **MaxTokenSize** souhaitée.  
  
    > [!NOTE]  
    > La valeur hexadécimale ffff (valeur décimale 65535) est la taille de jeton maximale recommandée. La définition de cette valeur permettra probablement de résoudre le problème, mais elle peut nuire aux performances de l'ordinateur. Nous vous recommandons de déterminer la plus petite valeur **MaxTokenSize** possible représentant le jeton le plus volumineux des utilisateurs de votre organisation et d’entrer cette valeur.  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
8.  Fermez l’**Éditeur de Registre**.  
  
9. Redémarrez l'ordinateur.  
  
