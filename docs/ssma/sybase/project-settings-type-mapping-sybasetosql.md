---
description: Paramètres du projet (Mappage de type) (SybaseToSQL)
title: Paramètres du projet (mappage de type) (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 2698fb3a-f9e6-4e04-94e0-dad289d7ed0a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9e9086feb8adcb64ba1a24191e91028fc870d488
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069916"
---
# <a name="project-settings-type-mapping-sybasetosql"></a>Paramètres du projet (Mappage de type) (SybaseToSQL)
La page mappage de type de la boîte de dialogue **paramètres du projet** contient des paramètres qui personnalisent la manière dont SSMA convertit les types de données Sybase Adaptive Server Enterprise (ASE) en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] types de données.  
  
La page mappage de type est disponible dans les boîtes de dialogue **paramètres du projet** et **paramètres du projet par défaut** .  
  
-   Pour spécifier les paramètres de mappage de type pour tous les futurs projets SSMA, dans le menu **Outils** , sélectionnez **paramètres du projet par défaut**, sélectionnez le type de projet de migration pour lequel les paramètres doivent être affichés ou modifiés dans la liste déroulante de la **version cible** de la migration, puis sélectionnez **mappage de type** en bas du volet gauche.  
  
-   Pour spécifier les paramètres du projet actif, dans le menu **Outils** , sélectionnez **paramètres du projet**, puis sélectionnez **mappage de type** en bas du volet gauche.  
  
## <a name="options"></a>Options  
**Type de source**  
Type de données ASE mappé.  
  
**Type de cible**  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Type de données cible pour le type de données ASE spécifié.  
  
Consultez le tableau de la section suivante pour le mappage de type SSMA pour Sybase par défaut.  
  
**Ajouter**  
Cliquez pour ajouter un type de données à la liste de mappage.  
  
**Modifier**  
Cliquez pour modifier le type de données sélectionné dans la liste mappage.  
  
**Supprimer**  
Cliquez pour supprimer le mappage de type de données sélectionné de la liste de mappage.  
  
**Rétablir les valeurs par défaut**  
Cliquez pour rétablir les valeurs par défaut SSMAs de la liste mappage de type.  
  
## <a name="default-type-mapping"></a>Mappage de type par défaut  
Le tableau suivant contient le mappage de type par défaut entre ASE et les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] types de données.  
  
|Type de données ASE|Type de données de SQL Server|  
|-----------------|------------------------|  
|**bigint**|**bigint**|  
|**binary**|**binary**|  
|**binaire [ \* .. 8000]**|**binaire [ \* ]**|  
|**binaire [8001... \* ]**|**varbinary(max)**|  
|**bit**|**bit**|  
|**char**|**char**|  
|**char varying**|**varchar**|  
|**char varying [ \* .. 8000]**|**VARCHAR [ \* ]**|  
|**char varying [8001.. \* ]**|**varchar(max)**|  
|**Char [ \* .. 8000]**|**Char [ \* ]**|  
|**Char [8001.. \* ;]**|**varchar(max)**|  
|**character**|**char**|  
|**character varying**|**varchar**|  
|**caractère variable [ \* .. 8000]**|**VARCHAR [ \* ]**|  
|**caractère variable [8001.. \* ]**|**varchar(max)**|  
|**caractère [ \* .. 8000]**|**Char [ \* ]**|  
|**caractère [8001... \* ]**|**varchar(max)**|  
|**date**|**date**|  
|**datetime**|**datetime2 [3]**|  
|**decembre**|**decimal**|  
|**Dec [ \* .. \* ]**|**Decimal [ \* ]**|  
|**Dec [ \* .. \* ] [\*..\*]**|**Decimal [ \* ] [ \* ]**|  
|**decimal**|**decimal**|  
|**Decimal [ \* .. \* ]**|**Decimal [ \* ]**|  
|**Decimal [ \* .. \* ] [\*..\*]**|**Decimal [ \* ] [ \* ]**|  
|**double précision**|**float [53]**|  
|**float**|**float [53]**|  
|**float [ \* .. 4,5**|**float [24]**|  
|**float [16.. \* ]**|**float [53]**|  
|**image**|**image**|  
|**int**|**int**|  
|**integer**|**int**|  
|**longsysname**|**nvarchar [255]**|  
|**money**|**money**|  
|**caractère national**|**nchar**|  
|**caractère national [ \* .. 4000]**|**NCHAR [ \* ]**|  
|**caractère national variable**|**nvarchar**|  
|**caractère national variable [ \* .. 4000]**|**nvarchar [ \* ]**|  
|**caractère national variable [4001... \* ]**|**nvarchar(max)**|  
|**caractère national [4001... \* ]**|**nvarchar(max)**|  
|**caractère national**|**nchar**|  
|**caractère national [ \* .. 4000]**|**NCHAR [ \* ]**|  
|**caractère national [4001... \* ]**|**nvarchar(max)**|  
|**caractère national variable**|**nvarchar**|  
|**caractère national variable [ \* .. 4000]**|**nvarchar [ \* ]**|  
|**caractère national variable [4001... \* ]**|**nvarchar(max)**|  
|**varchar national**|**nvarchar**|  
|**national VARCHAR [ \* .. 4000]**|**nvarchar [ \* ]**|  
|**national VARCHAR [4001.. \* ]**|**nvarchar(max)**|  
|**nchar**|**nchar**|  
|**NCHAR varying**|**nvarchar**|  
|**NCHAR varying [ \* .. 4000]**|**nvarchar [ \* ]**|  
|**NCHAR variable [4001... \* ]**|**nvarchar(max)**|  
|**NCHAR [ \* .. 4000]**|**NCHAR [ \* ]**|  
|**NCHAR [4001... \* ]**|**nvarchar(max)**|  
|**numeric**|**numeric**|  
|**Numeric [ \* .. \* ]**|**Numeric [ \* ]**|  
|**Numeric [ \* .. \* ] [\*..\*]**|**Numeric [ \* ] [ \* ]**|  
|**nvarchar**|**nvarchar**|  
|**nvarchar [ \* .. 4000]**|**nvarchar [ \* ]**|  
|**nvarchar [4001... \* ]**|**nvarchar(max)**|  
|**real**|**float [24]**|  
|**smalldatetime**|**smalldatetime**|  
|**smallint**|**smallint**|  
|**smallmoney**|**smallmoney**|  
|**sysname**|**nvarchar [128]**|  
|**sysname [ \* .. \* ]**|**nvarchar [255]**|  
|**text**|**text**|  
|**time**|**heure [3]**|  
|**timestamp**|**rowversion**|  
|**tinyint**|**tinyint**|  
|**unichar**|**nchar**|  
|**UNICHAR variable**|**nvarchar**|  
|**UNICHAR variable [ \* .. 4000]**|**nvarchar [ \* ]**|  
|**UNICHAR variable [4001... \* ]**|**nvarchar(max)**|  
|**UNICHAR [ \* .. 4000]**|**NCHAR [ \* ]**|  
|**UNICHAR [4001... \* ]**|**nvarchar(max)**|  
|**unitext**|**nvarchar(max)**|  
|**univarchar**|**nvarchar**|  
|**univarchar [ \* .. 4000]**|**nvarchar [ \* ]**|  
|**univarchar [4001... \* ]**|**nvarchar(max)**|  
|**unsigned bigint**|**numérique [20] [0]**|  
|**nombre entier non signé**|**bigint**|  
|**unsigned smallint**|**int**|  
|**unsigned tinyint**|**tinyint**|  
|**varbinary**|**varbinary**|  
|**varbinary [ \* .. 8000]**|**varbinary [ \* ]**|  
|**varbinary [8001... \* ]**|**varbinary(max)**|  
|**varchar**|**varchar**|  
|**VARCHAR [ \* .. 8000]**|**VARCHAR [ \* ]**|  
|**VARCHAR [8001... \* ]**|**varchar(max)**|  
  
