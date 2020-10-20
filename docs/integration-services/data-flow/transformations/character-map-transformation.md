---
description: Transformation de la table de caractères
title: Table de caractères, transformation | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.charactertrans.f1
- sql13.dts.designer.charactermaptransformation.f1
helpviewer_keywords:
- mutually exclusive mapping [Integration Services]
- mapping data [Integration Services]
- string functions
- Character Map transformation [Integration Services]
ms.assetid: e0f50eb6-b893-400f-bb8c-fb3072cc2620
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b825ff5cee4ceba526d59cc30dc0bb1205896171
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92194633"
---
# <a name="character-map-transformation"></a>Transformation de la table de caractères

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  La transformation de la table de caractères applique des fonctions de chaîne, telles que la conversion de minuscules en majuscules, à des données de type caractère. Cette transformation fonctionne seulement sur les données de colonne de type de données chaîne.  
  
 La transformation de la table de caractères peut convertir les données de colonne sur place ou ajouter une colonne à la sortie de transformation et y insérer les données converties. Vous pouvez appliquer différents ensembles d'opérations de mappage à la même colonne d'entrée et placer les résultats dans des colonnes différentes. Par exemple, vous pouvez convertir la même colonne en majuscules et en minuscules, puis placer les résultats dans deux colonnes différentes.  
  
 Dans certaines circonstances, le mappage peut provoquer une troncation des données. Par exemple, la troncation peut se produire lorsque des caractères codés sur un octet sont mappés avec des caractères représentés sur plusieurs octets. La transformation de la table de caractères comprend une sortie d'erreur, qui permet de diriger les données tronquées vers une sortie distincte. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../../integration-services/data-flow/error-handling-in-data.md).  
  
 Cette transformation a une entrée, une sortie et une sortie d'erreur.  
  
## <a name="mapping-operations"></a>Opérations de mappage  
 Le tableau suivant décrit les opérations de mappage prises en charge par la transformation de la table de caractères.  
  
|Opération|Description|  
|---------------|-----------------|  
|Inversion d'octet|Inverse l'ordre des octets.|  
|Pleine chasse|Mappe des caractères à demi-chasse avec des caractères à pleine chasse.|  
|Demi-chasse|Mappe des caractères à pleine chasse avec des caractères à demi-chasse.|  
|Hiragana|Mappe des caractères katakana avec des caractères hiragana.|  
|Katakana|Mappe des caractères hiragana avec des caractères katakana.|  
|Casse linguistique|Applique une casse linguistique à la place des règles système. La casse linguistique fait référence aux fonctionnalités fournies par le mappage de casse simple API Win32 pour Unicode du turc et d'autres paramètres régionaux.|  
|Minuscules|Convertit des caractères en minuscules.|  
|Chinois simplifié|Mappe des caractères en chinois traditionnel avec des caractères en chinois simplifié.|  
|Chinois traditionnel|Mappe des caractères en chinois simplifié avec des caractères en chinois traditionnel.|  
|Majuscules|Convertit des caractères en majuscules.|  
  
## <a name="mutually-exclusive-mapping-operations"></a>Opérations de mappage s'excluant mutuellement  
 Plusieurs opérations peuvent être réalisées dans une transformation. Toutefois, certaines opérations de mappage s'excluent mutuellement. Le tableau suivant décrit les restrictions applicables à l'utilisation de plusieurs opérations sur la même colonne. Les opérations dans les colonnes Opération A et Opération B s'excluent mutuellement.  
  
|Opération A|Opération B|  
|-----------------|-----------------|  
|Minuscules|Majuscules|  
|Hiragana|Katakana|  
|Demi-chasse|Pleine chasse|  
|Chinois traditionnel|Chinois simplifié|  
|Minuscules|Hiragana, katakana, demi-chasse, pleine chasse|  
|Majuscules|Hiragana, katakana, demi-chasse, pleine chasse|  
  
## <a name="configuration-of-the-character-map-transformation"></a>Configuration de la transformation de la table de caractères  
 Vous pouvez configurer la transformation de la table de caractères comme suit :  
  
-   Spécifiez les colonnes à convertir.  
  
-   Spécifiez les opérations à appliquer à chaque colonne.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](../set-the-properties-of-a-data-flow-component.md)  
  
-   [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 Pour plus d'informations sur la définition des propriétés, cliquez sur l'une des rubriques suivantes :  
  
-   [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
-   [Trier des données pour les transformations de fusion et de jointure de fusion](../../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)  
  
## <a name="character-map-transformation-editor"></a>Éditeur de transformation de la table des caractères
  Utilisez la boîte de dialogue **Éditeur de transformation de la table des caractères** pour sélectionner les fonctions de chaîne à appliquer aux données de colonne, et indiquer si le mappage est une modification sur place ou s’il est ajouté sous la forme d’une nouvelle colonne.  
  
### <a name="options"></a>Options  
 **Colonnes d'entrée disponibles**  
 Activez les cases à cocher pour sélectionner les colonnes à transformer en utilisant des fonctions de chaîne. Vos sélections figurent dans le tableau ci-dessous.  
  
 **Colonne d'entrée**  
 Affiche les colonnes d'entrée sélectionnées dans le tableau ci-dessus. Vous pouvez également changer ou supprimer une sélection en utilisant la liste des colonnes d'entrée disponibles.  
  
 **Destination**  
 Indiquez si vous voulez enregistrer le résultat des opérations de chaîne sur place en utilisant la colonne existante, ou enregistrer les données modifiées sous la forme d'une nouvelle colonne.  
  
|Valeur|Description|  
|-----------|-----------------|  
|Nouvelle colonne|Enregistre les données dans une nouvelle colonne. Définissez le nom de la colonne sous **Alias de sortie**.|  
|Modification sur place|Enregistre les données modifiées dans la colonne existante.|  
  
 **opération**  
 Dans la liste, sélectionnez les fonctions de chaîne à appliquer aux données de la colonne.  
  
|Valeur|Description|  
|-----------|-----------------|  
|Minuscules|Convertit les caractères en minuscules.|  
|Majuscules|Convertit les caractères en majuscules|  
|Inversion d'octet|Convertit en inversant l'ordre d'octet.|  
|Hiragana|Convertit les caractères japonais katakana en caractères hiragana.|  
|Katakana|Convertit les caractères japonais hiragana en caractères katakana.|  
|Demi-chasse|Convertit les caractères pleine chasse en caractères demi-chasse.|  
|Pleine chasse|Convertit les caractères demi-chasse en caractères pleine chasse.|  
|Casse linguistique|Applique des règles de casse linguistique (mappage de casse simple Unicode pour le turc et d'autres paramètres locaux) à la place des règles système.|  
|Chinois simplifié|Convertit les caractères chinois traditionnels en caractères chinois simplifié.|  
|Chinois traditionnel|Convertit les caractères chinois simplifié en caractères chinois traditionnel.|  
  
 **Alias de sortie**  
 Permet de saisir un alias pour chaque colonne de sortie. La valeur par défaut est **Copie de** suivi du nom de la colonne d'entrée. Toutefois, vous pouvez choisir n'importe quel nom descriptif unique.  
  
 **Configurer la sortie d’erreur**  
 Utilisez la boîte de dialogue [Configurer la sortie d’erreur](../error-handling-in-data.md) pour définir les options de gestion des erreurs de cette transformation.  
  
