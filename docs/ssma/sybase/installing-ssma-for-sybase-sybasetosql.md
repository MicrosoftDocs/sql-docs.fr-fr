---
title: Installation de SSMA pour SAP ASE (SybaseToSQL) | Microsoft Docs
description: Utilisez ces articles pour installer, mettre à niveau et désinstaller Assistant Migration SQL Server pour SAP ASE, qui comprend une application cliente et un pack d’extension.
ms.custom: ''
ms.date: 11/29/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 8d5a4ce6-b747-46e3-9184-645d56e8b35c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 8fca8219dfe89c8229b8d296fe355b97417d9daf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073353"
---
# <a name="installing-ssma-for-sap-ase-sybasetosql"></a>Installation de SSMA pour SAP ASE (SybaseToSQL)
[!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration (SSMA) pour SAP Adaptive Server Enterprise (ASE) se compose d’une application cliente que vous utilisez pour effectuer une migration de SAP ASE vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. Il contient également un pack d’extension qui prend en charge la migration des données et l’utilisation des fonctions système ASE dans vos bases de données migrées.  
  
Installez l’application cliente sur l’ordinateur à partir duquel vous envisagez d’effectuer les étapes de migration. Installez les fichiers de Pack d’extension sur l’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur lequel les bases de données migrées doivent être hébergées.  
  
## <a name="upgrading-ssma-for-sap-ase"></a>Mise à niveau de SSMA pour SAP ASE  
Si vous souhaitez effectuer une mise à niveau vers une version ultérieure de SSMA pour SAP ASE, vous devez d’abord désinstaller le client et le pack d’extension de serveur. Installez ensuite la version plus récente.  
  
Si vous ouvrez un projet à partir d’une version antérieure de SSMA pour SAP ASE, SSMA vous demande si vous souhaitez convertir le projet vers la version la plus récente. Cliquez sur **Oui** pour travailler avec le projet dans la version plus récente de SSMA.  
  
## <a name="contents"></a>Contenu  
  
|Article|Description|  
|---------|---------------|  
|[Installation du client SSMA pour SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md)|Fournit des informations et des instructions sur l’installation du client SSMA pour SAP ASE.|  
|[Installation des composants SSMA sur SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)|Fournit des informations et des instructions sur l’installation du pack d’extension sur les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[Suppression de SSMA pour les composants SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/removing-ssma-for-sybase-components-sybasetosql.md)|Fournit des instructions pour la désinstallation du programme client et du pack d’extension.|  
  
## <a name="see-also"></a>Voir également  
[Migration de bases de données SAP ASE vers SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
