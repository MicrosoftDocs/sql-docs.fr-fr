---
description: Créer et gérer des attributions de rôles
title: Créer et gérer des attributions de rôles | Microsoft Docs
ms.date: 05/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- removing role assignments
- roles [Reporting Services], assignments
- security [Reporting Services], role assignments
- modifying role assignments
- deleting role assignments
ms.assetid: 086d0987-b43c-4834-8372-e08fb4b432f8
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e1be919afaea2693329ae6fe957fb6fc5ff3357f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015199"
---
# <a name="create-and-manage-role-assignments"></a>Créer et gérer des attributions de rôles

Une *attribution de rôle* est une stratégie de sécurité qui détermine les autorisations d’un utilisateur ou d’un groupe. Les autorisations déterminent si l’utilisateur ou le groupe peut accéder ou modifier un élément de serveur de rapports spécifique ou effectuer une tâche. Elle se compose d'un seul nom de compte d'utilisateur ou de groupe et d'une ou de plusieurs définitions de rôles.

Les attributions de rôles ont comme portée le *niveau élément* ou le *niveau système*.

- Une attribution de rôle au niveau élément est créée pour un élément ou une branche spécifique de l’arborescence des dossiers sur le serveur de rapports. Vous naviguez jusqu'à un dossier ou un élément spécifique pour lui créer une attribution de rôle.

- Les attributions de rôles au niveau système permettent aux utilisateurs sélectionnés d’effectuer des tâches qui affectent le site du serveur de rapports dans son ensemble. Ces tâches incluent :
  - Création de planifications partagées
  - Gérer les travaux
  - Traitement de rapports
  - Définition de propriétés

La sécurité au niveau système n’implique pas d’accès aux éléments de l’arborescence des dossiers du serveur de rapports.

## <a name="creating-an-item-level-role-assignment"></a>Création d'une attribution de rôle au niveau élément

De là, vous pouvez créer une attribution de rôle distincte pour chaque compte d’utilisateur ou de groupe qui nécessite un accès au serveur de rapports. Si le compte se trouve sur un domaine autre que celui qui contient le serveur de rapports, incluez le nom de domaine. Après avoir spécifié un compte, vous choisissez une ou plusieurs définitions de rôles. Les définitions de rôles s'additionnent. L’ensemble combiné de toutes les tâches de toutes les définitions est pris en charge dans l’attribution relative à un utilisateur ou à un groupe spécifique.

Pour activer un accès étendu, vous choisissez un élément qui est élevé dans la hiérarchie de dossiers (par exemple le dossier racine Accueil). Plus tard, vous pourrez créer des attributions de rôles pour verrouiller différentes zones spécifiques dans la hiérarchie de dossiers.

Vous devez être membre du groupe local Administrateurs sur l'ordinateur serveur de rapports pour créer une attribution de rôle. Vous pouvez déléguer cette responsabilité en affectant d’autres utilisateurs au rôle **Gestionnaire de contenu** .

Pour créer ou gérer des attributions de rôles, ou pour plus d’informations, consultez [Accorder à un utilisateur l’accès à un serveur de rapports](../../reporting-services/security/grant-user-access-to-a-report-server.md)
  
## <a name="creating-a-system-level-role-assignment"></a>Création d'une attribution de rôle au niveau système

Les attributions de rôles au niveau système et au niveau élément sont compatibles. Vous pouvez créer une attribution de rôle au niveau système pour chaque utilisateur ou groupe qui possède une attribution de rôle au niveau élément.

Les attributions de rôles au niveau système incluent un large éventail d’autorisations, mais elles n’incluent pas les autorisations qui font partie d’une attribution de rôle au niveau élément.

Contrairement aux autorisations système sur un ordinateur, les rôles système dans les serveurs de rapports n’accordent pas d’autorisations primordiales comprenant toutes les tâches possibles. Au lieu de cela, les attributions de rôles au niveau système sont simplement un ensemble des tâches dont la portée est le site du serveur de rapports. Les attributions de rôles système déterminent si les utilisateurs peuvent afficher les propriétés des applications (telles que l’image ou le titre de la page d’accueil), afficher ou gérer des planifications partagées, ou utiliser le Générateur de rapports.

Pour créer ou gérer une attribution de rôles au niveau système, ou pour plus d’informations, consultez [Accorder à un utilisateur l’accès à un serveur de rapports](../../reporting-services/security/grant-user-access-to-a-report-server.md) et [Rôles prédéfinis](../../reporting-services/security/role-definitions-predefined-roles.md).  

## <a name="modifying-a-role-assignment"></a>Modification d'une attribution de rôle

Vous pouvez modifier une attribution de rôle à tout moment. Vos modifications prennent effet lorsque vous enregistrez l'attribution de rôle. Les sessions utilisateur ne sont pas affectées par les modifications d'attributions de rôles. Si un utilisateur a un rapport ouvert, et que vous modifiez une attribution de rôle pour en refuser l’accès, l’utilisateur peut continuer à utiliser le rapport pendant cette session active.

Si vous ajoutez un compte d’utilisateur à un groupe qui fait déjà partie d’une attribution de rôle, un délai s’écoulera avant que le compte d’utilisateur puisse accéder aux éléments. Ce délai est dû à la mise en cache des jetons d'authentification par IIS (Internet Information Services). Vous pouvez attendre que les jetons soient actualisés (généralement après quinze minutes) ou réinitialiser IIS pour mettre à jour le cache immédiatement.

Vous pouvez uniquement modifier une attribution de rôle à la fois. Vous ne pouvez pas effectuer d’opération de recherche et de remplacement globale afin de changer des noms de définition de rôle ou des paramètres d’attribution de rôle, ou de trouver toutes les attributions de rôles qui incluent un utilisateur ou un groupe spécifique.

## <a name="deleting-a-role-assignment"></a>Suppression d'une attribution de rôle

Vous pouvez supprimer des attributions de rôles en cochant les cases en regard de chaque attribution que vous voulez supprimer, puis en cliquant sur **Supprimer**. Vous pouvez également supprimer des attributions de rôles en cliquant sur **Rétablir la sécurité parente**. Quand vous sélectionnez ce bouton, les attributions de rôles existantes pour l’élément sont supprimées et remplacées par les attributions héritées de l’élément parent.

## <a name="see-also"></a>Voir aussi

[Accorder un accès utilisateur l'accès à un serveur de rapports](../../reporting-services/security/grant-user-access-to-a-report-server.md)  
[Attributions de rôles](../../reporting-services/security/role-assignments.md)  
[Définitions de rôles](../../reporting-services/security/role-definitions.md)  
[Rôles prédéfinis](../../reporting-services/security/role-definitions-predefined-roles.md)  
[Octroi d'autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)
