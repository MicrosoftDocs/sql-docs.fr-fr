---
description: sysmergeschemachange (Transact-SQL)
title: sysmergeschemachange (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergeschemachange_TSQL
- sysmergeschemachange
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergeschemachange system table
ms.assetid: ae9ce16e-6ee9-4c7c-8210-a9bf2c7efdf0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2a0315c49710ac324234710d30c223dfb9b27b12
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102002"
---
# <a name="sysmergeschemachange-transact-sql"></a>sysmergeschemachange (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient des informations sur les articles publiés générés par l'Agent d'instantané. Cette table est stockée dans les bases de données de publication et d’abonnement.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**pubid**|**uniqueidentifier**|ID de la publication.|  
|**artid**|**uniqueidentifier**|ID de l’article.|  
|**SchemaVersion**|**int**|Numéro de la dernière modification de schéma.|  
|**schemaguid**|**uniqueidentifier**|ID unique du dernier schéma.|  
|**schematype**|**int**|Type de modification du schéma :<br /><br /> **-1** = non valide.<br /><br /> **1** = commande SQL.<br /><br /> **2** = script de schéma.<br /><br /> **3** = utilitaire de copie en bloc (BCP) natif.<br /><br /> **4** = BCP de caractères.<br /><br /> **5** = dernière génération enregistrée.<br /><br /> **6** = dernière génération envoyée.<br /><br /> **7** = répertoire.<br /><br /> **8** = priorité.<br /><br /> **9** = période de rétention.<br /><br /> **10** = script de déclenchement.<br /><br /> **11** = ALTER TABLE.<br /><br /> **12** = Réinitialiser tout.<br /><br /> **13** = ALTER TABLE (non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ).<br /><br /> **14** = réinitialisation avec chargement.<br /><br /> **15** = contrainte et script d’index.<br /><br /> **16** = nettoyage des métadonnées.<br /><br /> **17** = dernière génération de mise à jour envoyée.<br /><br /> **18** = niveau de compatibilité descendante.<br /><br /> **19** = valider les informations de l’abonné.<br /><br /> **20** = bien partitionné.<br /><br /> **21** = programme de résolution personnalisé.<br /><br /> **22** = ordre de traitement de l’article.<br /><br /> **23** = publication dans la publication transactionnelle.<br /><br /> **24** = compenser les erreurs.<br /><br /> **25** = autre dossier d’instantanés.<br /><br /> **26** = en téléchargement seul.<br /><br /> **27** = suppression du suivi.<br /><br /> **40** = script d’instantané de pré-création.<br /><br /> **45** = script d’instantané de la création.<br /><br /> **46** = script utilisateur à la demande.<br /><br /> **50** = début de l’en-tête de capture instantanée<br /><br /> **51** = fin de l’en-tête de capture instantanée.<br /><br /> **52** = code de fin de capture instantanée.<br /><br /> **53** = adresse protocole FTP (FTP).<br /><br /> **54** = port FTP.<br /><br /> **55** = sous-répertoire FTP.<br /><br /> **56** = instantané compressé.<br /><br /> **57** = connexion FTP.<br /><br /> **58** = mot de passe FTP.<br /><br /> **60** = script de pré-création du système.<br /><br /> **61** = schéma de procédure stockée.<br /><br /> **62** = schéma de la vue.<br /><br /> **63** = schéma de la fonction définie par l’utilisateur.<br /><br /> **64** = index de la vue.<br /><br /> **65** = propriétés étendues.<br /><br /> **66** = Validate.<br /><br /> **67** = commande SQL de pré-capture instantanée.<br /><br /> **71** = validation d’instantané dynamique.<br /><br /> **80** = BCP 9,0 natif de la table système.<br /><br /> **81** = BCP 9,0.<br /><br /> **82** = table système native BCP 9,0 (Global uniquement).<br /><br /> **83** = BCP 9,0 (Global-uniquement).<br /><br /> **84** = table système native BCP 9,0 (Lightweight).<br /><br /> **85** = BCP 9,0 (Lightweight) comme caractère de table système.<br /><br /> **128** = BCP dynamique (bit).<br /><br /> **131** = BCP natif dynamique.<br /><br /> **132** = BCP à caractères dynamiques.<br /><br /> **208** = table système dynamique BCP 9,0 native.<br /><br /> **209** = BCP 9,0, caractères de table système dynamiques.<br /><br /> **210** = table système dynamique BCP 9,0 (Global-uniquement).<br /><br /> **211** = BCP 9,0, caractère de table système dynamique (Global uniquement).<br /><br /> **212** = table système dynamique BCP 9,0 (Lightweight).<br /><br /> **213** = BCP 9,0 (Lightweight) de table système dynamique.<br /><br /> **300** = actions DDL (Data Definition Language).<br /><br /> **1024** = contrôle d’instantané incrémentiel.<br /><br /> **1049** = dossier d’instantanés incrémentiels.<br /><br /> **1074** = en-tête de début d’instantané incrémentiel.<br /><br /> **1075** = en-tête de fin d’instantané incrémentiel.<br /><br /> **1076** = code de fin de l’instantané incrémentiel.<br /><br /> **1077** = adresse ftp incrémentielle.<br /><br /> **1078** = port FTP incrémentiel.<br /><br /> **1079** = sous-répertoire FTP incrémentiel.<br /><br /> **1080** = instantané compressé incrémentiel.<br /><br /> **1081** = connexion FTP incrémentielle.<br /><br /> **1082** = mot de passe FTP incrémentiel.|  
|**schematext**|**nvarchar (2000)**|Nom du fichier de script ou commande incluant un nom de fichier|  
|**schemastatus**|**tinyint**|Indique si une modification de schéma est en cours relative à l'article, pouvant alors prendre une des valeurs ci-dessous :<br /><br /> **0** = inactif.<br /><br /> **1** = actif.<br /><br /> Quand une modification de schéma est en attente, cette valeur est définie sur **1**.|  
|**schemasubtype**|**int**|Sous-type de modification du schéma :<br /><br /> **1** = ADDCOLUMN<br /><br /> **2** = DROPCOLUMN<br /><br /> **3** = ALTERCOLUMN<br /><br /> **4** = ADDPRIMARYKEY<br /><br /> **5** = ADDUNIQUE<br /><br /> **6** = ADDREFERENCE<br /><br /> **7** = DROPCONSTRAINT<br /><br /> **8** = AddDefault<br /><br /> **9** = ADDCHECK<br /><br /> **10** = DISABLETRIGGER<br /><br /> **11** = ENABLETRIGGER<br /><br /> **12** = DISABLETRIGGER<br /><br /> **13** = ENABLETRIGGER<br /><br /> **14** = ENABLECONSTRAINT<br /><br /> **15** = DISABLECONSTRAINT<br /><br /> **16** = ENABLECONSTRAINT<br /><br /> **17** = DISABLECONSTRAINT|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
