---
description: Paramètres du projet (Migration) (MySQLToSQL)
title: Paramètres du projet (migration) (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 2a3cba9e-cd54-4a8b-b858-8fc4cf2580d9
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 92c6e91ca49046561c24d7716dd5a3e152e686f5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017369"
---
# <a name="project-settings-migration-mysqltosql"></a>Paramètres du projet (Migration) (MySQLToSQL)
La page migration de la boîte de dialogue **paramètres du projet** contient des paramètres qui personnalisent la manière dont SSMA migre les données de MySQL vers SQL Server.  
  
Le volet migration est disponible dans les boîtes de dialogue **paramètres du projet** et **paramètres du projet par défaut** .  
  
-   Pour spécifier les paramètres de tous les projets SSMA, dans le menu **Outils** , sélectionnez **paramètres du projet par défaut**, sélectionnez le type de projet dans la liste déroulante **version cible** de la migration pour laquelle vous souhaitez accéder aux paramètres, cliquez sur **général** en bas du volet gauche, puis cliquez sur **migration**.  
  
-   Pour spécifier les paramètres du projet actif, dans le menu **Outils** , sélectionnez **paramètres du projet**, cliquez sur **général** en bas du volet gauche, puis cliquez sur **migration**.  
  
## <a name="options"></a>Options  
  
### <a name="bulk-copy"></a>Copie en bloc  
  
|Terme|Définition|  
|--------|--------------|  
|**Taille de lot**|Spécifie la taille de lot utilisée lors de la migration des données.<br /><br />**Mode par défaut**: 1000<br /><br />**Mode optimiste**: 1000<br /><br />**Mode complet**: 1000|  
|**Contraintes de validation**|Spécifie si SSMA doit vérifier les contraintes lorsqu’il insère des données dans SQL Server tables.<br /><br />**Mode par défaut**: false<br /><br />**Mode optimiste**: false<br /><br />**Mode complet**: false|  
|**Exécuter les déclencheurs**|Spécifie si SSMA doit déclencher les déclencheurs d’insertion lorsqu’il ajoute des données aux tables SQL Server.<br /><br />**Mode par défaut**: false<br /><br />**Mode optimiste**: false<br /><br />**Mode complet**: false|  
|**Conserver l'identité**|Spécifie si SSMA conserve les valeurs d’identité MySQL lorsqu’il ajoute des données à SQL Server. Si la valeur est false, les valeurs d’identité sont affectées par la destination.<br /><br />**Mode par défaut**: true<br /><br />**Mode optimiste**: true<br /><br />**Mode complet**: true|  
|**Conserver les valeurs NULL**|Spécifie si SSMA conserve les valeurs NULL dans les données sources lorsqu’il ajoute des données à SQL Server, quelles que soient les valeurs par défaut spécifiées dans SQL Server.<br /><br />**Mode par défaut**: true<br /><br />**Mode optimiste**: true<br /><br />**Mode complet**: true|  
|**Verrou de table**|Spécifie si SSMA verrouille des tables lorsqu’il ajoute des données aux tables pendant la migration des données. Obtient un verrou de mise à jour en bloc pour la durée de l’opération de copie en bloc. Si la valeur est false, un verrou est défini au niveau de la ligne.<br /><br />**Mode par défaut**: false<br /><br />**Mode optimiste**: false<br /><br />**Mode complet**: false|  
  
### <a name="data-modification"></a>Modification de données  
  
|Terme|Définition|  
|--------|--------------|  
|**Migration de dates non valides**|Spécifie comment migrer des dates non valides avec like « 2007-04-23 » ou « 2000-06-31 10:00:00 » dans les formats de DATE et DATETIME.<br /><br />**Mode par défaut**: définir null<br /><br />**Mode optimiste**: définir null<br /><br />**Mode complet**: définir null|  
|**Migration des valeurs d’heure négatives**|Spécifie comment migrer des valeurs négatives comme « -30:11:00 » dans les colonnes de temps.<br /><br />**Mode par défaut**: définir null<br /><br />**Mode optimiste**: définir null<br /><br />**Mode complet**: définir null|  
|**DURÉE de la migration de plus de 24 heures**|Spécifie comment migrer des valeurs d’heure supérieures à « 23:59:59 » dans les colonnes de temps.<br /><br />**Mode par défaut**: définir null<br /><br />**Mode optimiste**: définir null<br /><br />**Mode complet**: définir null|  
|**Tronquer les valeurs binaires pour les ajuster à la colonne**|Si c’est le cas, SSMA tronque les valeurs binaires de MySQL qui ne tiennent pas dans les colonnes de la table SQL et génère le message d’erreur approprié. Si non, la ligne provoque une erreur<br /><br />**Mode par défaut**: non<br /><br />**Mode optimiste**: non<br /><br />**Mode complet**: non|  
|**Tronquer les valeurs de caractère pour les ajuster à la colonne**|SSMA tronque les valeurs des caractères de MySQL qui ne rentrent pas dans les colonnes de la table SQL et génère le message d’erreur approprié.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimiste**: non<br /><br />**Mode complet**: non|  
|**Migration de zéro date**|Spécifie comment migrer les dates zéro comme « 0000-00-00 » ou « 0000-00-00 00:00:00 » dans les colonnes DATE et DATETIME.<br /><br />**Mode par défaut**: définir null<br /><br />**Mode optimiste**: définir null<br /><br />**Mode complet**: définir null|  
|**Migration de zéro dans les dates**|Spécifie comment migrer des dates avec zéro partie comme « 2009-01-00 » ou « 2000-00-00 11:00:00 » dans les colonnes DATE et DATETIME.<br /><br />**Mode par défaut**: définir null<br /><br />**Mode optimiste**: définir null<br /><br />**Mode complet**: définir null|  
  
### <a name="migration-engine"></a>Moteur de migration  
  
|Terme|Définition|  
|--------|--------------|  
|**Moteur de migration**|Spécifie le moteur de base de données utilisé pendant la migration des données. La migration des données côté client fait référence au client SSMA qui récupère les données à partir de la source et insère en bloc ces données dans SQL Server. La migration des données côté serveur fait référence au moteur de migration de données SSMA (programme de copie en bloc) exécuté sur la boîte de SQL Server en tant que travail de l’agent SQL qui récupère les données de la source et les insère directement dans SQL Server évitant ainsi un saut client supplémentaire (meilleures performances).<br /><br />**Mode par défaut**: moteur de migration de données côté client<br /><br />**Mode optimiste**: moteur de migration de données côté client<br /><br />**Mode complet**: moteur de migration de données côté client|  
  
> [!IMPORTANT]  
> Lorsque l’option **moteur de migration** est définie sur le moteur de migration de **données côté serveur**, une nouvelle option de paramètre de projet utiliser le moteur de migration de **données côté serveur 32 bits** s’affiche. Il spécifie si l’utilitaire de copie en bloc 32 bits ou 64 bits est utilisé pour migrer des données.  
  
### <a name="misc"></a>Divers  
  
|Terme|Définition|  
|--------|--------------|  
|**Options de migration étendue des données**|Affiche des options de migration de données supplémentaires pour chaque table sous un onglet détail distinct.<br /><br />**Mode par défaut**: masquer<br /><br />**Mode optimiste**: masquer<br /><br />**Mode complet**: masquer|  
|**En cas d’erreur**|Arrête la migration des données lorsqu’une erreur se produit. Trois options s’imposent :<br /><br />**Arrêter la migration :** Arrête l’opération de migration des données<br /><br />**Passer au tableau suivant :** Arrête la migration des données vers la table actuelle et passe à la suivante<br /><br />**Passer au traitement suivant :** Arrête la migration des données vers le lot actuel et passe à la suivante<br /><br />**Mode par défaut**: passer au traitement suivant<br /><br />**Mode optimiste**: passer au lot suivant<br /><br />**Mode complet**: passer au lot suivant|  
  
### <a name="parallel-data-migration"></a>Migration de données parallèles  
  
|Terme|Définition|  
|--------|--------------|  
|**Mode de migration de données parallèles**|Spécifie le mode utilisé pour répliquer les threads afin d’activer la migration de données parallèles. En mode auto, SSMA choisit le nombre de threads (10 par défaut) dupliqués pour migrer les données. En mode personnalisé, l’utilisateur peut spécifier le nombre de threads en fourche pour migrer les données (la valeur minimale est 1 et la valeur maximale est 100). Actuellement, seul le moteur de migration de données côté client prend en charge la migration de données parallèles.<br /><br />**Mode par défaut**: auto<br /><br />**Mode optimiste**: auto<br /><br />**Mode complet**: auto|  
  
> [!IMPORTANT]  
> Lorsque l’option de **mode de migration de données en parallèle** est définie sur **personnalisé**, un nouveau paramètre de projet **nombre de threads** est affiché. Il spécifie le nombre de threads utilisés pour la migration des données.  
  
### <a name="spatial-data"></a>Données spatiales  
  
|Terme|Définition|  
|--------|--------------|  
|**Gestion des erreurs**|Spécifie comment gérer les erreurs lors de la migration de valeurs de types de données spatiales. Si’replace with NULL’est spécifié, toutes les valeurs spatiales provoquant des erreurs seront remplacées par NULL. Sinon, aucun remplacement n’est effectué.<br /><br />**Mode par défaut**: générer une erreur<br /><br />**Mode optimiste**: générer une erreur<br /><br />**Mode complet**: générer une erreur|  
|**Validation de la valeur**|Spécifie comment gérer les valeurs spatiales non valides. Si’Try make valide’est spécifié, une tentative est effectuée pour modifier les valeurs non valides afin de les rendre valides.<br /><br />**Mode par défaut**: rendre valide<br /><br />**Mode optimiste**: ne pas modifier<br /><br />**Mode complet**: rendre valide|  
  
