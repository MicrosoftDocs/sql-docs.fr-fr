---
description: Suppression de composants SSMA pour Sybase (SybaseToSQL)
title: Suppression de SSMA pour les composants Sybase (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: aec09593-17d9-4ec2-ac56-3cd8851406fd
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 02a782e583463e5a986e3c7c1a42152fc27b14cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069939"
---
# <a name="removing-ssma-for-sybase-components-sybasetosql"></a>Suppression de composants SSMA pour Sybase (SybaseToSQL)
Une fois que vous avez terminé la migration des bases de données de Sybase Adaptive Server Enterprise (ASE) vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous souhaiterez peut-être désinstaller les composants SSMA. Vous pouvez désinstaller les composants clients à tout moment, mais vous ne devez pas désinstaller le pack d’extension de, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sauf si vous êtes sûr que vos bases de données migrées n’utilisent plus de fonctions dans le schéma **ssma_syb** de la base de données **sysdb** .  
  
## <a name="uninstalling-the-ssma-for-sybase-client"></a>Désinstallation du client SSMA pour Sybase  
Vous pouvez désinstaller SSMA à l’aide d' **Ajout/suppression de programmes**.  
  
**Pour désinstaller SSMA**  
  
1.  Dans le Panneau de configuration, ouvrez **Ajout/Suppression de programmes**.  
  
2.  Sélectionnez **Assistant Migration Microsoft SQL Server pour Sybase**, puis cliquez sur **supprimer**.  
  
3.  Pour confirmer que vous souhaitez désinstaller SSMA, cliquez sur **Oui**.  
  
## <a name="uninstalling-the-extension-pack"></a>Désinstallation du pack d’extension  
Si vous êtes sûr que vos bases de données migrées n’utilisent pas d’objets dans le schéma **sysdb.ssma_syb** , vous pouvez supprimer le pack d’extension à l’aide de la tâche **Ajout/suppression de programmes**.  
  
Pour désinstaller le pack d’extension  
  
1.  Dans le Panneau de configuration, ouvrez **Ajout/Suppression de programmes**.  
  
2.  Sélectionnez **Assistant Migration Microsoft SQL Server pour le pack d’extension Sybase**, puis cliquez sur **supprimer**.  
  
3.  Pour confirmer que vous souhaitez désinstaller le pack d’extension, cliquez sur **Oui**.  
  
4.  Sur la page instances avec scripts de base de données utilitaires, cliquez sur **suivant**.  
  
5.  Sur la page Paramètres de connexion, sélectionnez la méthode d’authentification, puis cliquez sur **suivant**.  
  
    L’authentification Windows utilise vos informations d’identification Windows pour essayer de se connecter à l’instance de SQL Server. Si vous sélectionnez SQL Server l’authentification, vous devez entrer un nom de connexion et un mot de passe SQL Server.  
  
6.  Dans la page opération terminée, cliquez sur **OK**.  
  
7.  Dans la page terminer, cliquez sur **quitter**.  
  
Après la désinstallation de, vous pouvez vérifier que le schéma de **sysdb.ssma_syb** et, éventuellement, la base de données **sysdb** entière, a été supprimé à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . Toutefois, si vous utilisez d’autres produits SSMA, ils utilisent également la base de données **sysdb** . Si la base de données existe et que vous êtes sûr qu’aucune autre base de données ne référence des objets de cette base de données, vous pouvez détacher la base de données.  
  
## <a name="see-also"></a>Voir aussi  
[Installation de SSMA pour Sybase client &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md)  
[Installation des composants SSMA sur SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)  
  
