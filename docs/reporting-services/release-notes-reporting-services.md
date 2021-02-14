---
title: Notes de publication pour Reporting Services 2017 et versions ultérieures | Microsoft Docs
description: Découvrez les modifications qui ont été apportées à SQL Server Reporting Services (SSRS) dans les versions 2017 et ultérieures.
ms.date: 10/11/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.reviewer: maggies
author: casualoak
ms.author: rhys
monikerRange: '>=sql-server-2017'
ms.openlocfilehash: ccf46bca321f3b23b1cd9216aa122e2202ff8c24
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031151"
---
# <a name="release-notes-for-sql-server-reporting-services-ssrs-2017-and-later"></a>Notes de publication de SQL Server Reporting Services (SSRS) 2017 et versions ultérieures

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2017-and-later](../includes/ssrs-appliesto-2017-and-later.md)]

Cet article décrit les modifications apportées à [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] (SSRS) pour les versions 2017 et ultérieures.

Pour obtenir les notes de publication pour les contrôles Visionneuse de rapports, consultez [Notes de publication des contrôles Visionneuse de rapports pour WebForms et WinForms de SSRS](application-integration/release-notes-ssrs-application-integration.md).

<!--
We are "standardizing" all our 'Release Notes' style articles:

  - File names:   'release-notes-[TechArea-Name].md'


  - Content format:   2-column tables.  No longer using bullet lists.

    - Ideally, the 'Details' column should contain a link to another SSRS documentation article wherein the particular issue fix is discussed in any way.  Or if there is more to say beyond one sentence, the other sentences of elaboration would go into the 'Details' column.


  - H2 header names are kept short, for better display.


  - Try to keep all Release Notes in one .md file.  Avoid having multiple R.N. files whose names differ only by version or date.

    - Seriously consider erasing info about ancient releases that are so old that nobody cares about them anymore.  If you really want to retain ancient info, consider doing so in an HTML comment at the end of the .md file, just in case a Microsoft employee needs to re-examine ancient info.  In any case, this file cannot get ever longer, and some deletion or hiding of oldest info must eventually occur.


  - Do use '::: moniker range=' zones when version 2017 is no longer the only version represented inside this .md file.

    - Use the '=' operator on the moniker, not the '>=' operator.
    - In contrast, in our 'Whats New' articles we do use the '>=', rather than '='.

GeneMi, DevOps = 1467988 (MsEng > TechnicalContent) , 2019/03/19
-->
## <a name="sql-server-2019-reporting-services"></a>SQL Server 2019 Reporting Services

## <a name="15075454810-20200831"></a>15.0.7545.4810, 08/31/2020 
*(Version du produit : 15.0.1102.861)*

| Problème résolu | Détails |
| :---------- | :------ |
| Mises à jour de sécurité  | &nbsp; |
| Prise en charge des commentaires joints limitée pour ne plus autoriser les documents PDF  | &nbsp; |
| Correction de la troncation des noms de fichiers lors de l’exportation de rapports contenant un point dans le nom  | &nbsp; |
| Résolution d’un problème lié aux abonnements et à la culture zh-TW qui entraînait des erreurs de format de date non valide  | &nbsp; |
| Résolution d’un problème lié à certains rapports, où l’accès à l’option Paramètres lançait une boucle infinie  | &nbsp; |
| Résolution des problèmes liés aux guillemets simples dans les noms de rapport  | &nbsp; |
| Correction d’un problème dans l’accès d’URL empêchant la localisation de correspondances par FindString  | &nbsp; |
| Correction d’un problème où le texte de remplacement pour l’exportation PDF n’était pas correctement encodé pour les caractères multioctets  | &nbsp; |
| Correction de l’apparition indésirable d’une image vide sous un élément linéaire  | &nbsp; |
| Correction d’une erreur de prise en charge erronée pour l’authentification personnalisée dans l’édition web  | &nbsp; |
| Résolution d’un problème où un lecteur d’écran lisait une ligne supplémentaire et une colonne supplémentaire dans un tableau matriciel  | &nbsp; |
| Correction d’un problème de troncation d’image avec Ajuster à la taille pour la page entière  | &nbsp; |
| La mise à niveau de la ligne de commande ne requiert plus l’indicateur EULA  | &nbsp; |

## <a name="150724337714-20191101"></a>15.0.7243.37714, 2019/01/11
*(Version du produit : 15.0.1102.675)*

Version initiale.


## <a name="sql-server-2017-reporting-services"></a>SQL Server 2017 Reporting Services

## <a name="1406001669-20200831"></a>14.0.600.1669, 08/31/2020 

| Problème résolu | Détails |
| :---------- | :------ |
| Mises à jour de sécurité  | &nbsp; |
| Prise en charge des commentaires joints limitée pour ne plus autoriser les documents PDF  | &nbsp; |
| Correction de la troncation des noms de fichiers lors de l’exportation de rapports contenant un point dans le nom  | &nbsp; |
| Résolution d’un problème lié aux abonnements et à la culture zh-TW qui entraînait des erreurs de format de date non valide  | &nbsp; |

## <a name="1406001572-20200406"></a>14.0.600.1572, 06/04/2020 

| Problème résolu | Détails |
| :---------- | :------ |
| Mise à jour de l’interface utilisateur JQuery vers 1.12  | &nbsp; |
| Sensibilité à la casse de l’URL corrigée  | &nbsp; |
| Correction du positionnement des paramètres lorsqu’il existe de nombreux paramètres  | &nbsp; |
| Correction de FindString qui ne fonctionnait pas dans le rendu HTML5  | &nbsp; |
| Bibliothèques clientes Analysis Services mises à jour pour résoudre la dépréciation TLS 1.0/1.1 | &nbsp; |

## <a name="1406001451-20191113"></a>14.0.600.1451, 13/11/2019 

| Problème résolu | Détails |
| :---------- | :------ |
| Mises à jour de sécurité | &nbsp; |
| Les rapports paginés ne fonctionnaient pas correctement avec les paramètres de filtre quand l’instantané était activé  | &nbsp; |
| Les utilisateurs avec le rôle Lecteur et les paramètres par défaut n’avaient pas l’autorisation de télécharger des fichiers Excel  | &nbsp; |
| La mise à niveau vers Power BI Report Server à partir de SQL Server 2016 Reporting Services échouait | &nbsp; |
| Après la mise à niveau à partir de SQL Server 2012 Reporting Services, les abonnements échouaient avec un message « Un caractère non valide a été trouvé dans l'en-tête du courrier : ','. » | &nbsp; |
| Outil de configuration : l’annulation des fenêtres modales dans la section Base de données redémarrait le service Reporting Services | &nbsp; |
| L’expression de propriété BorderStyle des contrôles TextBox n’était pas rendue au format Excel  | &nbsp; |
| Un problème de pagination pouvait faire que certains rapports étaient bloqués lors du rendu de la même page sans jamais atteindre la dernière page du rapport | &nbsp; |

## <a name="1406001274-20190701"></a>14.0.600.1274, 01/07/2019

| Problème résolu | Détails |
| :---------- | :------ |
| Mises à jour de sécurité | &nbsp; |
| Impossible de sélectionner les jours de la semaine lors de la création d’une planification hebdomadaire partagée | &nbsp; |
| Le rapport n’affiche pas les retours chariot correctement au format Word | &nbsp; |
| System Center Operations Manager (SCOM) 2019 ne fonctionne plus avec les dernières mises à niveau de SSRS 2017 | &nbsp; |
| Une erreur se produisait lors de l'appel de l'extension d'autorisation pour les jeux de données partagés | &nbsp; |
| La logique a changé sur la procédure stockée GetAllProperties dans SSRS 2017 et PBIRS, ce qui rend la méthode ReportingService2010.GetPropertie du point de terminaison de service web incapable d’obtenir des données pour le rapport lié | &nbsp; |
| Un en-tête de ligne de grille simple dans le rapport mobile disparaît en cas de clic sur un élément de la grille | &nbsp; |
| Impossible d’utiliser le champ de date dans le paramètre d’abonnement contrôlé par les données | &nbsp; |
| Le tableau matriciel imbriqué affiche une petite police ou une police partielle dans SSRS 2016 et versions ultérieures | &nbsp; |
| Les abonnements avec le paramètre DateTime subissent une erreur après qu’un utilisateur avec des paramètres régionaux différents modifie l’abonnement | &nbsp; |
| Échec de la création d’un abonnement piloté par les données avec une extension de remise null avec le message « Une erreur de remise s'est produite » | &nbsp; |
| L’encodage URL est incorrect lors de la définition de la valeur au format Excel\Word | &nbsp; |

## <a name="1406001109-20190212"></a>14.0.600.1109, 12/02/2019

| Problème résolu | Détails |
| :---------- | :------ |
| Les planification des captures instantanées de rapport de cache devient « planification spécifique aux rapports » après modification de l’abonnement. | &nbsp; |
| rc:Toolbar=false ne fonctionne pas dans Express Edition. | &nbsp; |
| Certains caractères thaïs sont mal restitués à l’exportation de rapports paginés au format PDF. | &nbsp; |
| Un blocage se produit lors de la notification de fin des abonnements pilotés par les données. | &nbsp; |
| Les images incorporées ne s’affichent pas dans certaines circonstances, lorsque le paramètre rc:Toolbar=False est utilisé. | &nbsp; |
| Il n’est pas possible de créer des abonnements pilotés par les données pour les rapports qui utilisent des paramètres en cascade. | &nbsp; |
| Il n’est pas possible de modifier les abonnements configurés avec un intervalle non valide. | &nbsp; |
| Mises à jour de sécurité. | &nbsp; |
| L’interface utilisateur des rapports liés ne s’affiche pas. | &nbsp; |
| Certains rapports paginés comportant des contrôles de tableau matriciel imbriqué présentent des polices incorrectes. | &nbsp; |
| Un espace blanc est ajouté à tort à certains rapports paginés contenant des régions de données de tableau matriciel. | &nbsp; |
| Les lignes d’en-tête disparaissent en cas de développement des grilles de données simples d’un rapport mobile. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600906-20180912"></a>14.0.600.906, 12/09/2018

Le problème suivant a été résolu :

| Problème résolu | Détails |
| :---------- | :------ |
| L’authentification personnalisée ne renvoie pas les bonnes informations sur les cookies. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600892-20180831"></a>14.0.600.892, 31/08/2018

| Problème résolu | Détails |
| :---------- | :------ |
| À cause de la zone de texte dans le rectangle, celui-ci ne peut pas s’agrandir verticalement quand rc:Toolbar=False et que le texte est long. | &nbsp; |
| La taille du texte ne s’adapte pas si pageHeight est inférieur à 0,5 pouce. | &nbsp; |
| Un blocage se produit dans la base de données du catalogue SSRS lors de l’utilisation avec CRM. | &nbsp; |
| Les en-têtes de colonne alignés verticalement s’affichent mal lors du défilement vers le bas dans un rapport. | &nbsp; |
| Les utilisateurs ajoutés au rôle de création de rapports System Center Operations Manager ne peuvent pas accéder au portail web SSRS. | &nbsp; |
| Un caractère thaïlandais n’est pas correctement exporté dans le PDF. | &nbsp; |
| Changement de comportement du rôle Visiteur. | &nbsp; |
| rc:Toolbar=false ne fonctionne pas dans Express Edition. | &nbsp; |
| Barre de défilement verticale manquante dans la zone de message du paramètre. | &nbsp; |
| Runtime de rapport mobile mis à jour. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600744-20180425"></a>14.0.600.744, 25/04/2018

| Problème résolu | Détails |
| :---------- | :------ |
| La page Abonnement piloté par les données n’affiche pas l’option de livraison qui a été créée. | &nbsp; |
| En cas de mise à niveau de SSRS 2012 vers SSRS 2017, RSManagement génère une exception toutes les deux ou trois secondes. | &nbsp; |
| Il n’est pas possible de modifier les valeurs par défaut des paramètres à valeurs multiples dans IE11. | &nbsp; |
| Les planifications sont vides chaque fois qu’une planification partagée est exécutée. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600689-20180228"></a>14.0.600.689, 28/02/2018

| Problème résolu | Détails |
| :---------- | :------ |
| La visibilité de Paramètre de rapport dans un rapport lié est rétablie après modification de ses propriétés. | &nbsp; |
| Le paramètre d’URL rc:Toolbar=false ne fonctionne pas dans Express Edition. | &nbsp; |
| Si vous avez des expressions dans une zone de texte avec la propriété CanGrow définie sur false, les valeurs ne s’affichent pas. | &nbsp; |
| Le lien _En savoir plus_ est ajouté pour la clé de produit dans le programme d’installation. | &nbsp; |
| Le portail web avec l’authentification par formulaires personnalisée ignore le cookie à expiration décalé. | &nbsp; |
| L’exportation dans Word crée une hauteur de ligne inégale si le contenu de la ligne est vide. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600594-20180109"></a>14.0.600.594, 09/01/2018

Certaines mises à jour de sécurité ont été implémentées.

### <a name="140600490-20171101"></a>14.0.600.490, 01/11/2017

Les problèmes de mise à niveau des références SKU sont résolus.

## <a name="140600451-20170930"></a>14.0.600.451, 30/09/2017

Version initiale.

## <a name="next-steps"></a>Étapes suivantes

[Nouveautés de Reporting Services (SSRS)](what-s-new-in-sql-server-reporting-services-ssrs.md)

D’autres questions ? [Posez une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
