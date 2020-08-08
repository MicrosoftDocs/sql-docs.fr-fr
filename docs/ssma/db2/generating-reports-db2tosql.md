---
title: Génération de rapports (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 69ef5fd9-190d-4c58-8199-b3f77d5e1883
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 1ae04ceabe7a2063f8b9806894ff8e4d55c577fd
ms.sourcegitcommit: e8f6c51d4702c0046aec1394109bc0503ca182f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87933798"
---
# <a name="generating-reports-db2tosql"></a>Génération de rapports (DB2ToSQL)
Les rapports de certaines activités effectuées à l’aide de commandes sont générés dans la console SSMA au niveau de l’arborescence d’objets.  
  
Pour générer des rapports, procédez comme suit :  
  
1.  Spécifiez le paramètre **Write-Resume-Report-to** . Le rapport associé est stocké en tant que nom de fichier (s’il est spécifié) ou dans le dossier que vous spécifiez. Le nom de fichier est prédéfini par le système, comme indiqué dans le tableau ci-dessous, où ** &lt; n &gt; ** est le numéro de fichier unique qui s’incrémente avec un chiffre à chaque exécution de la même commande.  
  
    Les rapports vis-à-vis sont les suivants :  
  
    |SL. Non.|Commande|Titre de rapport|  
    |-|-|-|  
    |1|générer un rapport d’évaluation|AssessmentReport &lt; n &gt; . LANGAGE|  
    |2|convertir-schéma|SchemaConversionReport &lt; n &gt; . LANGAGE|  
    |3|migrer-données|DataMigrationReport &lt; n &gt; . LANGAGE|  
    |4|Convert-SQL-Statement|ConvertSQLReport &lt; n &gt; . LANGAGE|  
    |5|synchroniser-cible|TargetSynchronizationReport &lt; n &gt; . LANGAGE|  
    |6|actualisation à partir de la base de données|SourceDBRefreshReport &lt; n &gt; . LANGAGE|  
  
    > [!IMPORTANT]  
    > Un rapport de sortie est différent du rapport d’évaluation. Le premier est un rapport sur les performances d’une commande exécutée, ce dernier est un rapport XML pour la consommation de programmation.  
  
    Pour les options de commande pour les rapports de sortie (à partir de SL. Non. 2-4 ci-dessus), reportez-vous à la section [exécution de la console SSMA &#40;DB2ToSQL&#41;](../../ssma/db2/executing-the-ssma-console-db2tosql.md) .  
  
2.  Indiquez l’étendue des détails souhaités dans le rapport de sortie à l’aide des paramètres de commentaires du rapport :  
  
    |SL. Non.|Commande et paramètre|Description de la sortie|  
    |-|-|-|  
    |1|Verbose = "false"|Génère un rapport de synthèse de l’activité.|  
    |2|Verbose = "true"|Génère un rapport d’état résumé et détaillé pour chaque activité.|  
  
    > [!NOTE]  
    > Les paramètres de commentaires du rapport spécifiés ci-dessus s’appliquent aux commandes Generate-Evaluation-Report, Convert-Schema, Migrate-Data et Convert-SQL-Statement.  
  
3.  Indiquez l’étendue des détails souhaités dans les rapports d’erreurs à l’aide des paramètres de rapport d’erreurs :  
  
    |SL. Non.|Commande et paramètre|Description de la sortie|  
    |-|-|-|  
    |1|rapport-Erreurs = « false »|Aucun détail sur les messages d’erreur/d’avertissement/d’informations.|  
    |2|Report-errors = "true"|Messages d’erreur/d’avertissement/d’informations détaillés.|  
  
    > [!NOTE]  
    > Les paramètres de rapport d’erreurs spécifiés ci-dessus s’appliquent aux commandes Generate-Evaluation-Report, Convert-Schema, Migrate-Data et Convert-SQL-Statement.  
  
**Exemple :**  
  
```  
<generate-assessment-report  
  
   object-name="<object-name>"  
  
   object-type="<object-type>"  
  
   verbose="<true/false>"  
  
   report-erors="<true/false>"  
  
   write-summary-report-to="<file-name/folder-name>"  
  
   assessment-report-folder="<folder-name>"  
  
   assessment-report-overwrite="<true/false>"/>  
```  
  
### <a name="synchronize-target"></a>synchroniser-cible :  
La commande **Synchronize-Target** a un paramètre **signal-Errors-to** qui spécifie l’emplacement du rapport d’erreurs pour l’opération de synchronisation. Ensuite, un fichier par son nom **TargetSynchronizationReport &lt; n &gt; . XML** est créé à l’emplacement spécifié, où ** &lt; n &gt; ** est le numéro de fichier unique qui s’incrémente avec un chiffre à chaque exécution de la même commande.  
  
**Remarque :** Si le chemin d’accès au dossier est donné, le paramètre « Report-Errors-to » devient un attribut facultatif pour la commande « Synchronize-Target ».  
  
```  
<!-- Example: Synchronize target entire Database with all attributes-->  
  
<synchronize-target  
  
   object-name="<object-name>"  
  
   on-error="report-total-as-warning/report-each-as-warning/fail-script"  
  
   report-errors-to="<file-name/folder-name>"/>  
```  
**nom de l’objet :** Spécifie le ou les objets pris en compte pour la synchronisation (il peut également avoir des noms d’objets individuels ou un nom d’objet de groupe).  
  
**en cas d’erreur :** Spécifie si les erreurs de synchronisation doivent être spécifiées en tant qu’avertissements ou erreurs. Options disponibles pour on-Error :  
  
-   Rapport-total-AVERTISSEMENT  
  
-   rapport-chaque AVERTISSEMENT  
  
-   échec du script  
  
### <a name="refresh-from-database"></a>actualisation à partir de la base de données :  
La commande **Refresh-from-Database** a un paramètre **Report-Errors-to** qui spécifie l’emplacement du rapport d’erreurs pour l’opération d’actualisation. Ensuite, un fichier par son nom **SourceDBRefreshReport &lt; n &gt; . XML** est créé à l’emplacement spécifié, où ** &lt; n &gt; ** est le numéro de fichier unique qui s’incrémente avec un chiffre à chaque exécution de la même commande.  
  
**Remarque :** Si le chemin d’accès au dossier est donné, le paramètre « Report-Errors-to » devient un attribut facultatif pour la commande « Synchronize-Target ».  
  
```  
<!-- Example: Refresh entire Schema (with all attributes)-->  
  
<refresh-from-database  
  
   object-name="<object-name>"  
  
   object-type ="<object-type>"  
  
   on-error="report-total-as-warning/report-each-as-warning/fail-script"  
  
   report-errors-to="<file-name/folder-name>"/>  
```  
**nom de l’objet :** Spécifie le ou les objets pris en compte pour l’actualisation (il peut également avoir des noms d’objets individuels ou un nom d’objet de groupe).  
  
**en cas d’erreur :** Spécifie s’il faut spécifier des erreurs d’actualisation comme avertissements ou erreurs. Options disponibles pour on-Error :  
  
-   Rapport-total-AVERTISSEMENT  
  
-   rapport-chaque AVERTISSEMENT  
  
-   échec du script  
  
## <a name="see-also"></a>Voir aussi  
[Exécution de la console SSMA](https://msdn.microsoft.com/ce63f633-067d-4f04-b8e9-e1abd7ec740b)  
  
