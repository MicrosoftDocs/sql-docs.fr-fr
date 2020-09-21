---
title: Journalisation de l’activité
description: Découvrir comment configurer différentes combinaisons d’options de journalisation lors de l’utilisation des Pilotes Microsoft pour PHP pour SQL Server
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- logging activity
ms.assetid: a777b3d9-2262-4e82-bc82-b62ad60d0e55
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6301b429191b0f563a5f1dea08bd6e8d92a0c46a
ms.sourcegitcommit: d1051f05a7db81ec62d9785bb6af572408f3d4e0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88680544"
---
# <a name="logging-activity"></a>Journalisation de l’activité
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Par défaut, les erreurs et avertissements générés par le [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] ne sont pas enregistrés. Cette rubrique explique comment configurer la journalisation de l’activité.  
  
## <a name="logging-activity-using-the-pdo_sqlsrv-driver"></a>Journalisation de l’activité à l’aide du pilote PDO_SQLSRV  
La seule configuration disponible pour le pilote PDO_SQLSRV est l’entrée pdo_sqlsrv.log_severity dans le fichier php.ini.  
  
Ajoutez ce qui suit à la fin de votre fichier php.ini :  
  
```  
[pdo_sqlsrv]  
pdo_sqlsrv.log_severity = <number>  
```  
  
**log_severity** peut avoir l’une des valeurs suivantes :  
  
|Valeur|Description|  
|---------|---------------|  
|0|La journalisation est désactivée (valeur par défaut si rien n’est défini).|  
|-1|Spécifie que les erreurs, les avertissements et les notifications doivent être enregistrés.|  
|1|Spécifie que les erreurs sont consignées.|  
|2|Spécifie que les avertissements sont consignés.|  
|4|Spécifie que les notifications sont consignées.|  
  
Les informations de journalisation seront ajoutées au fichier phperrors.log.  
  
PHP lit le fichier de configuration lors de l’initialisation et stocke les données dans une mémoire cache. Il fournit aussi une API pour mettre à jour ces paramètres et les utiliser immédiatement, et est écrit dans le fichier de configuration. Cette API permet aux scripts d’application de modifier les paramètres même après l’initialisation de PHP.  
  
## <a name="logging-activity-using-the-sqlsrv-driver"></a>Journalisation de l’activité à l’aide du pilote SQLSRV  
Pour activer la journalisation, vous pouvez utiliser la fonction [sqlsrv_configure](../../connect/php/sqlsrv-configure.md) ou modifier le fichier php.ini. Vous pouvez enregistrer l’activité en cas d’initialisation, de connexion, d’exécution d’instruction ou de fonction d’erreur. Vous pouvez également spécifier s’il faut enregistrer les erreurs, les avertissements, les avis ou les trois.  
  
> [!NOTE]  
> Vous pouvez configurer l’emplacement du fichier journal dans le fichier php.ini.  
  
### <a name="turning-logging-on"></a>Activation de la journalisation  
Vous pouvez activer la journalisation en utilisant la fonction [sqlsrv_configure](../../connect/php/sqlsrv-configure.md) pour spécifier la valeur du paramètre **LogSubsystems**. Par exemple, la ligne de code suivante configure le pilote pour enregistrer l’activité des connexions :  
  
`sqlsrv_configure("LogSubsystems", SQLSRV_LOG_SYSTEM_CONN);`  
  
Le tableau suivant décrit les constantes que vous pouvez utiliser comme valeur du paramètre **LogSubsystems** :  
  
|Valeur (entier équivalent entre parenthèses)|Description|  
|-----------------------------------------------|---------------|  
|SQLSRV_LOG_SYSTEM_ALL (-1)|Active la journalisation de tous les sous-systèmes.|  
|SQLSRV_LOG_SYSTEM_OFF (0)|Désactive la journalisation. Il s’agit de la valeur par défaut.|  
|SQLSRV_LOG_SYSTEM_INIT (1)|Active la journalisation de l’activité d’initialisation.|  
|SQLSRV_LOG_SYSTEM_CONN (2)|Active la journalisation de l’activité de connexion.|  
|SQLSRV_LOG_SYSTEM_STMT (4)|Active la journalisation de l’activité d’instruction.|  
|SQLSRV_LOG_SYSTEM_UTIL (8)|Active la journalisation de l’activité de fonctions d’erreur (telle que handle_error et handle_warning).|  
  
Vous pouvez définir plusieurs valeurs à la fois pour le paramètre **LogSubsystems** à l’aide de l’opérateur logique OR (|). Par exemple, la ligne de code suivante active la journalisation de l’activité pour les connexions et les instructions :  
  
`sqlsrv_configure("LogSubsystems", SQLSRV_LOG_SYSTEM_CONN | SQLSRV_LOG_SYSTEM_STMT);`  
  
Vous pouvez également activer la journalisation en spécifiant une valeur entière pour le paramètre **LogSubsystems** dans le fichier php.ini. Par exemple, ajoutez la ligne suivante à la section `[sqlsrv]` du fichier php.ini pour activer la journalisation de l’activité de connexion :  
  
`sqlsrv.LogSubsystems = 2`  
  
En ajoutant des valeurs entières, vous pouvez spécifier plusieurs options à la fois. Par exemple, ajoutez la ligne suivante à la section `[sqlsrv]` du fichier php.ini pour activer la journalisation de l’activité de connexion et d’instruction :  
  
`sqlsrv.LogSubsystems = 6`  
  
### <a name="logging-errors-warnings-and-notices"></a>Journalisation des erreurs, avertissements et avis  
Après avoir activé la journalisation, vous devez spécifier les éléments à enregistrer. Vous pouvez enregistrer un ou plusieurs des éléments suivants : erreurs, avertissements et avis. Par exemple, la ligne de code suivante spécifie que seuls les avertissements doivent être enregistrés :  
  
`sqlsrv_configure("LogSeverity", SQLSRV_LOG_SEVERITY_WARNING);`  
  
> [!NOTE]  
> Le paramètre par défaut de **LogSeverity** est **SQLSRV_LOG_SEVERITY_ERROR**. Si la journalisation est activée et qu’aucun paramètre pour **LogSeverity** n’a été spécifié, seules les erreurs sont enregistrées.  
  
Le tableau suivant décrit les constantes que vous pouvez utiliser comme valeur du paramètre **LogSeverity** :  
  
|Valeur (entier équivalent entre parenthèses)|Description|  
|-----------------------------------------------|---------------|  
|SQLSRV_LOG_SEVERITY_ALL (-1)|Spécifie que les erreurs, les avertissements et les notifications doivent être enregistrés.|  
|SQLSRV_LOG_SEVERITY_ERROR (1)|Spécifie que les erreurs sont consignées. Il s’agit de la valeur par défaut.|  
|SQLSRV_LOG_SEVERITY_WARNING (2)|Spécifie que les avertissements sont consignés.|  
|SQLSRV_LOG_SEVERITY_NOTICE (4)|Spécifie que les notifications sont consignées.|  
  
Vous pouvez définir plusieurs valeurs à la fois pour le paramètre **LogSeverity** à l’aide de l’opérateur logique OR (|). Par exemple, la ligne de code suivante spécifie que les erreurs et les avertissements doivent être enregistrés :  
  
`sqlsrv_configure("LogSeverity", SQLSRV_LOG_SEVERITY_ERROR | SQLSRV_LOG_SEVERITY_WARNING);`  
  
> [!NOTE]  
> Le fait de spécifier une valeur pour le paramètre **LogSeverity** n’active pas la journalisation. Pour activer la journalisation, attribuez une valeur au paramètre **LogSubsystems**, puis spécifiez la gravité des éléments enregistrés en affectant une valeur à **LogSeverity**.  
  
Vous pouvez également affecter une valeur au paramètre **LogSeverity** en spécifiant des valeurs entières dans le fichier php.ini. Par exemple, ajoutez la ligne suivante à la section `[sqlsrv]` du fichier php.ini pour activer uniquement la journalisation des avertissements :  
  
`sqlsrv.LogSeverity = 2`  
  
En ajoutant des valeurs entières, vous pouvez spécifier plusieurs options à la fois. Par exemple, ajoutez la ligne suivante à la section `[sqlsrv]` du fichier php.ini pour activer la journalisation des erreurs et des avertissements :  
  
`sqlsrv.LogSeverity = 3`  
  
## <a name="see-also"></a>Voir aussi  
[Guide de programmation pour les pilotes Microsoft pour PHP pour SQL Server](../../connect/php/programming-guide-for-php-sql-driver.md)

[Constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md)

[sqlsrv_configure](../../connect/php/sqlsrv-configure.md)

[sqlsrv_get_config](../../connect/php/sqlsrv-get-config.md)

[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  
  
