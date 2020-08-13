---
title: Créer un nom de connexion pour SQLRUserGroup
description: Créez un nom de connexion dans SQL Server pour SQLRUserGroup en utilisant l’authentification implicite pour se connecter au serveur afin que la conversion d’identité soit retournée à l’utilisateur appelant.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 01/25/2019
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: f3d7bd23c2a6b218e56f4de57d5b281b34d0687d
ms.sourcegitcommit: edba1c570d4d8832502135bef093aac07e156c95
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86484630"
---
# <a name="create-a-login-for-sqlrusergroup"></a>Créer un nom de connexion pour SQLRUserGroup
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Créez un [nom de connexion dans SQL Server](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/create-a-login) pour [SQLRUserGroup](../concepts/security.md#sqlrusergroup) quand une [connexion de bouclage](../../machine-learning/concepts/security.md#implied-authentication) dans votre script indique une *connexion approuvée* et que l’identité utilisée pour exécuter un objet qui contient votre code est un compte d’utilisateur Windows.

Les connexions approuvées sont celles qui présentent `Trusted_Connection=True` dans la chaîne de connexion. Quand SQL Server reçoit une demande spécifiant une connexion approuvée, il vérifie si l’identité de l’utilisateur Windows actif dispose d’un nom de connexion. Pour les processus externes s’exécutant en tant que compte de travail (par exemple, MSSQLSERVER01 de **SQLRUserGroup**), la demande échoue parce que ces comptes n’ont pas de nom de connexion par défaut.

Vous pouvez contourner l’erreur de connexion en créant un nom de connexion pour **SQLServerRUserGroup**. Pour plus d’informations sur les identités et les processus externes, consultez [Vue d’ensemble de la sécurité pour l’infrastructure d’extensibilité](../concepts/security.md).

> [!Note]
> Vérifiez que **SQLRUserGroup** dispose d’autorisations « Autoriser la connexion localement ». Par défaut, ce droit est accordé à tous les nouveaux utilisateurs locaux, mais ce droit peut être désactivé dans les stratégies de groupe plus strictes de certaines organisations.

## <a name="create-a-login"></a>Créer un compte de connexion

1. Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], dans l’Explorateur d’objets, développez **Sécurité**, cliquez avec le bouton droit sur **Connexions**, puis sélectionnez **Nouvelle connexion**.

2. Dans la boîte de dialogue **Nouvelle connexion**, cliquez sur **Rechercher**. (Ne tapez rien dans la zone pour l’instant.)
    
     ![Cliquez sur Rechercher pour ajouter une nouvelle connexion à Machine Learning](media/implied-auth-login1.png "Cliquez sur Rechercher pour ajouter une nouvelle connexion à Machine Learning")

3. Dans la zone **Sélectionner un utilisateur ou un groupe**, cliquez sur le bouton **Types d’objets**.

     ![Rechercher des types d’objet pour ajouter une nouvelle connexion à Machine Learning](media/implied-auth-login2.png "Rechercher des types d’objet pour ajouter une nouvelle connexion à Machine Learning")

4. Dans la boîte de dialogue **Types d’objets**, sélectionnez **Groupes**. Décochez toutes les autres cases.

     ![Sélectionner des groupes dans la boîte de dialogue Types d’objet](media/implied-auth-login3.png "Sélectionner des groupes dans la boîte de dialogue Types d’objet")

4. Cliquez sur **Avancé**, vérifiez que l’emplacement sur lequel porte la recherche est l’ordinateur actuel, puis cliquez sur **Rechercher**.

     ![Cliquez sur Rechercher maintenant pour obtenir la liste des groupes](media/implied-auth-login4.png "Cliquez sur Rechercher maintenant pour obtenir la liste des groupes")

5. Faites défiler la liste des comptes de groupe du serveur jusqu’à ce que vous en trouviez un commençant par `SQLRUserGroup`.
    
    + Le nom du groupe associé au service Launchpad de l’_instance par défaut_ est toujours **SQLRUserGroup**, que vous ayez installé R, Python ou les deux. Sélectionnez ce compte pour l’instance par défaut uniquement.
    + Si vous utilisez une _instance nommée_ , le nom de l’instance est ajouté au nom du groupe de travail par défaut, `SQLRUserGroup`. Par exemple, si votre instance se nomme « MLTEST », le nom de groupe d’utilisateurs par défaut pour cette instance est **SQLRUserGroupMLTest**.
 
    ![Exemple de groupes sur le serveur](media/implied-auth-login5.png "Exemple de groupes sur le serveur")
   
5. Cliquez sur **OK** pour fermer la boîte de dialogue de recherche avancée.

    > [!IMPORTANT]
    > Vérifiez que vous avez sélectionné le compte approprié pour l’instance. Chaque instance peut utiliser uniquement son propre service Launchpad et le groupe créé pour ce service. Les instances ne peuvent pas partager un service Launchpad ou des comptes de travail.

6. Cliquez sur **OK** encore une fois pour fermer la boîte de dialogue **Sélectionner un utilisateur ou un groupe**.

7. Dans la boîte de dialogue **Nouvelle connexion**, cliquez sur **OK**. Par défaut, la connexion est affectée au rôle **public** et est autorisée à se connecter au moteur de base de données.

## <a name="next-steps"></a>Étapes suivantes

+ [Vue d’ensemble de la sécurité](../concepts/security.md)
+ [Framework d’extensibilité](../concepts/extensibility-framework.md)
