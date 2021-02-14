---
description: Paramètres du projet (Migration) (SybaseToSQL)
title: Paramètres du projet (migration) (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 82f8857f-7ab1-4738-ab6e-b1e95ea94924
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 96b5550449e11cd314e736a36e3845ba92f542c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069919"
---
# <a name="project-settings-migration-sybasetosql"></a>Paramètres du projet (Migration) (SybaseToSQL)
La page migration de la boîte de dialogue **paramètres du projet** contient des paramètres qui personnalisent la manière dont SSMA migre les données de Sybase Adaptive Server Enterprise (ASE) vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Le volet migration est disponible dans les boîtes de dialogue **paramètres du projet** et **paramètres du projet par défaut** .  
  
-   Pour spécifier les paramètres de tous les projets SSMA, dans le menu **Outils** , sélectionnez **paramètres du projet par défaut**, sélectionnez le type de projet de migration pour lequel les paramètres doivent être affichés/Changed dans la liste déroulante de la **version cible** de la migration. cliquez sur **général** en bas du volet gauche, puis cliquez sur **migration**.  
  
-   Pour spécifier les paramètres du projet actif, dans le menu **Outils** , sélectionnez **paramètres du projet**, cliquez sur **général** en bas du volet gauche, puis cliquez sur **migration**.  
  
## <a name="date-correction-options"></a>Options de correction de la date  
  
|Terme|Définition|  
|--------|--------------|  
|**Remplacer les dates non prises en charge**|Spécifie si SSMA doit corriger les dates antérieures à la date du DateTime la plus ancienne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  (01 janvier 1753).<br /><br />Pour conserver les valeurs de date actuelles, sélectionnez **ne rien faire**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’accepte pas les dates antérieures au 1er janvier 1753 dans une colonne DateTime. Si vous utilisez des dates antérieures, vous devez convertir les valeurs DateTime en valeurs de caractères.<br /><br />Pour convertir les dates antérieures au 1er janvier 1753 en NULL, sélectionnez **remplacer par null**.<br /><br />Pour remplacer les dates antérieures au 1er janvier 1753 par une Date prise en charge, sélectionnez **remplacer par la date de prise en charge la plus proche**.<br /><br />**Mode par défaut**: ne rien faire<br /><br />**Mode optimiste**: ne rien faire<br /><br />**Mode complet**: remplacez par la date de prise en charge la plus proche|  
  
## <a name="migration-engine"></a>Moteur de migration  
  
|Terme|Définition|  
|--------|--------------|  
|**Moteur de migration**|Spécifie le moteur de base de données utilisé pendant la migration des données. La migration des données côté client fait référence au client SSMA qui récupère les données à partir de la source et insère en bloc ces données dans SQL Server. La migration des données côté serveur fait référence au moteur de migration de données SSMA (programme de copie en bloc) exécuté sur la boîte de SQL Server en tant que travail de l’agent SQL qui récupère les données de la source et les insère directement dans SQL Server évitant ainsi un saut client supplémentaire (meilleures performances).<br /><br />**Mode par défaut**: moteur de migration de données côté client<br /><br />**Mode optimiste**: moteur de migration de données côté client<br /><br />**Mode complet**: moteur de migration de données côté client|  
  
> [!IMPORTANT]  
> Lorsque l’option **moteur de migration** est définie sur le moteur de migration de **données côté serveur**, une nouvelle option de paramètre de projet utiliser le moteur de migration de **données côté serveur 32 bits** s’affiche. Il spécifie si l’utilitaire de copie en bloc 32 bits ou 64 bits est utilisé pour migrer des données.  
  
## <a name="miscellaneous-options"></a>Options diverses  
  
|Terme|Définition|  
|--------|--------------|  
|**Taille de lot**|Spécifie la taille de lot utilisée lors de la migration des données.<br /><br />**Mode par défaut**: 10000<br /><br />**Mode optimiste**: 10000<br /><br />**Mode complet**: 10000|  
|**Contraintes de validation**|Spécifie si SSMA doit vérifier les contraintes lorsqu’il insère des données dans SQL Server tables.<br /><br />**Mode par défaut**: false<br /><br />**Mode optimiste**: false<br /><br />**Mode complet**: false|  
|**Délai d’expiration de la migration des données**|Spécifie le délai d’expiration utilisé pendant la migration des données<br /><br />**Mode par défaut**: 15<br /><br />**Mode optimiste**: 15<br /><br />**Mode complet**: 15|  
|**Options de migration étendue des données**|Affiche des options de migration de données supplémentaires pour chaque table sous un onglet détail distinct.<br /><br />**Mode par défaut**: masquer<br /><br />**Mode optimiste**: masquer<br /><br />**Mode complet**: masquer|  
|**Exécuter les déclencheurs**|Spécifie si SSMA doit déclencher les déclencheurs d’insertion lorsqu’il ajoute des données aux tables SQL Server.<br /><br />**Mode par défaut**: false<br /><br />**Mode optimiste**: false<br /><br />**Mode complet**: false|  
|**Conserver l'identité**|Spécifie si SSMA conserve les valeurs d’identité Sybase lorsqu’il ajoute des données à SQL Server. Si la valeur est false, les valeurs d’identité sont affectées par la destination.<br /><br />**Mode par défaut**: true<br /><br />**Mode optimiste**: true<br /><br />**Mode complet**: true|  
|**Conserver les valeurs NULL**|Spécifie si SSMA conserve les valeurs NULL dans les données sources lorsqu’il ajoute des données à SQL Server, quelles que soient les valeurs par défaut spécifiées dans SQL Server.<br /><br />**Mode par défaut**: true<br /><br />**Mode optimiste**: true<br /><br />**Mode complet**: true|  
|**En cas d’erreur**|Arrête la migration des données lorsqu’une erreur se produit. Trois options s’imposent :<br /><br />**Arrêter la migration :** Arrête l’opération de migration des données<br /><br />**Passer au tableau suivant :** Arrête la migration des données vers la table actuelle et passe à la suivante<br /><br />**Passer au traitement suivant :** Arrête la migration des données vers le lot actuel et passe à la suivante<br /><br />**Mode par défaut**: passer au traitement suivant<br /><br />**Mode optimiste**: passer au lot suivant<br /><br />**Mode complet**: passer au lot suivant|  
|**Partie fractionnaire ronde des nombres**|Spécifie s’il faut supprimer les parties fractionnaires des données numériques et décimales pendant la migration vers les types entiers ou afficher le message d’erreur si la partie fractionnaire est non triviale.<br /><br />**Mode par défaut**: non<br /><br />**Mode optimiste**: non<br /><br />**Mode complet**: non|  
|**Sybase Unicode endian**|Spécifie le type endian pour les chaînes Sybase Unicode. Les options suivantes peuvent être définies pour ce paramètre particulier :<br /><br />Little endian<br /><br />Big endian<br /><br />**Mode par défaut**: Little endian<br /><br />**Mode optimiste**: Little endian<br /><br />**Mode complet**: Little endian|  
|**Verrou de table**|Spécifie si SSMA verrouille des tables lorsqu’il ajoute des données aux tables pendant la migration des données. Obtient un verrou de mise à jour en bloc pour la durée de l’opération de copie en bloc. Si la valeur est false, un verrou est défini au niveau de la ligne.<br /><br />**Mode par défaut**: true<br /><br />**Mode optimiste**: true<br /><br />**Mode complet**: true|  
|**Utiliser des curseurs**|Si cette option est définie, les données sont récupérées de la base de données source à l’aide de curseurs.<br /><br />**Mode par défaut**: false<br /><br />**Mode optimiste**: false<br /><br />**Mode complet**: false|  
  
## <a name="parallel-data-migration"></a>Migration de données parallèles  
  
|Terme|Définition|  
|--------|--------------|  
|**Mode de migration de données parallèles**|Spécifie le mode utilisé pour répliquer les threads afin d’activer la migration de données parallèles. En mode auto, SSMA choisit le nombre de threads (10 par défaut) dupliqués pour migrer les données. En mode personnalisé, l’utilisateur peut spécifier le nombre de threads en fourche pour migrer les données (la valeur minimale est 1 et la valeur maximale est 100). Actuellement, seul le moteur de migration de données côté client prend en charge la migration de données parallèles.<br /><br />**Mode par défaut**: auto<br /><br />**Mode optimiste**: auto<br /><br />**Mode complet**: auto|  
  
> [!IMPORTANT]  
> Lorsque l’option de **mode de migration de données en parallèle** est définie sur **personnalisé**, un nouveau paramètre de projet **nombre de threads** est affiché. Il spécifie le nombre de threads utilisés pour la migration des données.  
  
