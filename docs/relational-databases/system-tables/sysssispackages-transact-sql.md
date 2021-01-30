---
description: sysssispackages (Transact-SQL)
title: sysssispackages (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysdtspackages90_TSQL
- sysdtspackages90
dev_langs:
- TSQL
helpviewer_keywords:
- sysssispackages system table
ms.assetid: 66155dcd-dcdb-4e33-a242-1625828ad8d2
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 31f1833808260508f29a858381d258f21ebd744a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186659"
---
# <a name="sysssispackages-transact-sql"></a>sysssispackages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque package enregistré dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette table est stockée dans la base de données **msdb** .  
  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Identificateur unique du package.|  
|**id**|**uniqueidentifier**|GUID du package.|  
|**description**|**nvarchar**|Description facultative du package.|  
|**création**|**datetime**|Date à laquelle le package a été créé.|  
|**FolderId**|**uniqueidentifier**|GUID du dossier logique dans lequel [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] répertorie le package.|  
|**ownersid**|**varbinary**|Identificateur de sécurité unique de l'utilisateur qui a créé le package.|  
|**packagedata**|**image**|Package.|  
|**packageformat**|**int**|Format dans lequel le package est enregistré :<br /><br /> La valeur 2 indique que le package est enregistré au [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] format.<br /><br /> La valeur 3 indique que le package est enregistré au format [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ou version ultérieure.|  
|**PackageType**|**int**|Client à l'origine du package. Les valeurs possibles sont les suivantes :<br /><br /> 0 (valeur par défaut)<br /><br /> 1 ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant importation et exportation)<br /><br /> 3 ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] réplication)<br /><br /> 5 ( [!INCLUDE[ssIS](../../includes/ssis-md.md)] Concepteur)<br /><br /> 6 (Assistant ou Concepteur de plan de maintenance).<br /><br /> <br /><br /> Notez que les valeurs de cette colonne correspondent à l' <xref:Microsoft.SqlServer.Dts.Runtime.DTSPackageType> énumération.|  
|**vermajor**|**int**|Dernière version majeure du package.|  
|**verminor**|**int**|Dernière version mineure du package.|  
|**verbuild**|**int**|Dernière build du package.|  
|**vercomments**|**nvarchar**|Commentaires sur la version du package.|  
|**verid**|**uniqueidentifier**|GUID de la version du package.|  
|**IsEncrypted**|**bit**|Valeur booléenne qui indique si le package est chiffré.|  
|**readrolesid**|**varbinary**|Rôle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui peut charger des packages.|  
|**writerolesid**|**varbinary**|Rôle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui peut enregistrer des packages.|  
  
## <a name="see-also"></a>Voir aussi  
 [Packages Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-packages.md)  
  
  
