---
description: MSSQLSERVER_15401
title: MSSQLSERVER_15401
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 15401 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: de444d90aa3bad32b5e283cb7b4fc92c4f8cabd7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196917"
---
# <a name="mssqlserver_15401"></a>MSSQLSERVER_15401
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|15401|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|SEC_INVALIDLOGINORGROUP|
|Texte du message|Utilisateur ou groupe Windows NT '%s' introuvable. Vérifiez de nouveau le nom.|
||

## <a name="explanation"></a>Explication

Cette erreur se produit lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne parvient pas à créer une connexion basée sur un principal Windows (comme un utilisateur de domaine ou un groupe de domaine Windows). Un message d’erreur semblable au suivant s’affiche

> Erreur 15401 : Utilisateur ou groupe Windows NT '%s' introuvable. Vérifiez de nouveau le nom.

## <a name="cause"></a>Cause

L’erreur peut se produire pour l’une des raisons suivantes :

- La connexion n’existe pas dans Active Directory.
- Le contrôleur de domaine n’est pas disponible.
- Vous n’utilisez pas BUILTIN comme nom de domaine lors de l’ajout d’un compte local.
- Problèmes de résolution de noms.

## <a name="user-action"></a>Action requise

Consultez les sections suivantes pour découvrir les actions que vous pouvez entreprendre pour chacune des différentes causes mentionnées ci-dessus.

### <a name="verify-the-login-you-are-trying-to-add"></a>Vérifier la connexion que vous essayez d’ajouter

1. Vérifiez que la connexion Windows existe toujours dans le domaine. Votre administrateur réseau a peut-être supprimé la connexion Windows pour des raisons particulières, et vous ne pourrez peut-être pas accorder cet accès de connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
1. Vérifiez que vous orthographiez correctement le nom de domaine et le nom de connexion, et que vous utilisez le format suivant : `Domain\User`
1. Si la connexion existe et qu’elle est correcte mais que vous recevez toujours l’erreur, passez aux sections suivantes de cet article.

### <a name="verify-if-the-domain-controller-is-available"></a>Vérifier si le contrôleur de domaine est disponible

Vous pouvez recevoir l’erreur 15401 lorsque le contrôleur de domaine pour le domaine où se trouve la connexion (le même domaine ou un autre) n’est pas disponible pour une raison quelconque.

Si la connexion se trouve dans un domaine différent de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vérifiez que les approbations appropriées existent entre les domaines.

Vérifiez que le contrôleur de domaine de la connexion est accessible à l’aide de la commande ping de l’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vérifiez l’adresse IP et le nom du contrôleur de domaine.

### <a name="verify-the-domain-name-for-local-accounts"></a>Vérifier le nom de domaine pour les comptes locaux

Les comptes locaux (n’appartenant pas à un domaine) requièrent un traitement spécial. Si vous essayez d’ajouter un compte local à partir de l’ordinateur local qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], assurez-vous que vous utilisez BUILTIN comme nom de domaine.

### <a name="check-for-name-resolution-issues"></a>Rechercher les problèmes de résolution de noms

Si vous rencontrez des problèmes pour résoudre le nom d’un ordinateur impliqué dans l’ajout de la connexion ou du groupe, vous pouvez recevoir l’erreur 15401.

Vérifiez que votre mécanisme de résolution de noms (par exemple WINS, DNS, HOSTS ou LMHOSTS) est correctement configuré.

## <a name="see-also"></a>Voir aussi

- [Tester un canal entre l’ordinateur local et son domaine](/powershell/module/microsoft.powershell.management/test-computersecurechannel#example-1--test-a-channel-between-the-local-computer-and-its-domain)
- [LogonSessions v1.4](/sysinternals/downloads/logonsessions)
- [sp_change_users_login (Transact-SQL)](../system-stored-procedures/sp-change-users-login-transact-sql.md)