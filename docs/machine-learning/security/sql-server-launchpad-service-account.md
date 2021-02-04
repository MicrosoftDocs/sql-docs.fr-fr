---
title: Configuration du compte Launchpad
description: Décrit comment modifier le compte de service SQL Server Launchpad utilisé pour l’exécution de scripts externes sur SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/17/2018
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 0aff998d79d79c2ac2ef06c37ffba4b01eb73973
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076857"
---
# <a name="sql-server-launchpad-service-configuration"></a>Configuration du service SQL Server Launchpad
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

[!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] est un service qui gère et exécute les scripts externes, à l’image du service de requête et d’indexation de texte intégral qui lance un hôte distinct pour le traitement de requêtes de texte intégral.

Pour plus d’informations, consultez les sections Launchpad dans [Architecture d’extensibilité dans SQL Server Machine Learning Services](../../machine-learning/concepts/extensibility-framework.md#launchpad) et [Vue d’ensemble de la sécurité du framework d’extensibilité dans SQL Server Machine Learning Services](../../machine-learning/concepts/security.md#launchpad).

## <a name="account-permissions"></a>Autorisations de compte

Par défaut, SQL Server Launchpad est configuré pour s’exécuter sous **NT Service\MSSQLLaunchpad**, qui dispose de toutes les autorisations nécessaires pour exécuter des scripts externes. La suppression des autorisations de ce compte peut empêcher Launchpad de démarrer ou d’accéder à l’instance SQL Server dans laquelle les scripts externes doivent être exécutés.

Si vous modifiez le compte de service, veillez à utiliser la [console Stratégie de sécurité locale](/windows/security/threat-protection/security-policy-settings/how-to-configure-security-policy-settings).

Les autorisations nécessaires pour ce compte sont indiquées dans le tableau suivant.

| Paramètre de stratégie de groupe | Nom de la constante |
|----------------------|---------------|
| [Ajuster les quotas de mémoire pour un processus](/windows/security/threat-protection/security-policy-settings/adjust-memory-quotas-for-a-process) | SeIncreaseQuotaPrivilege | 
| [Contourner la vérification de parcours](/windows/security/threat-protection/security-policy-settings/bypass-traverse-checking) | SeChangeNotifyPrivilege | 
| [Ouvrir une session en tant que service](/windows/security/threat-protection/security-policy-settings/log-on-as-a-service) | SeServiceLogonRight | 
| [Remplacer un jeton de niveau processus](/windows/security/threat-protection/security-policy-settings/replace-a-process-level-token) | SeAssignPrimaryTokenPrivilege | 

Pour plus d’informations sur les autorisations requises pour exécuter les services SQL Server, consultez [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).

<a name="bkmk_ChangingConfig"></a> 

## <a name="configuration-properties"></a>Propriétés de configuration

En règle générale, il n’y a aucune raison de modifier la configuration du service. Les propriétés pouvant être modifiées incluent le compte de service, le nombre de processus externes (20 par défaut) et la stratégie de réinitialisation de mot de passe pour les comptes de travail.

1. Ouvrez le [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md).

2. Sous Services SQL Server, cliquez avec le bouton droit sur SQL Server Launchpad et sélectionnez **Propriétés**.
  + Pour changer le compte de service, cliquez sur l’onglet **Ouvrir une session sur**.
  + Pour augmenter le nombre d’utilisateurs, cliquez sur l’onglet **Avancé** et changez le **Nombre de contextes de sécurité**.

> [!Note]
> Dans les premières versions de SQL Server 2016 R services, vous pouviez changer certaines propriétés du service en modifiant le fichier config [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)]. Ce fichier ne permet plus de modifier les configurations. Il convient désormais d’utiliser le Gestionnaire de configuration SQL Server pour changer la configuration du service, notamment le compte de service et le nombre d’utilisateurs.

## <a name="debug-settings"></a>Paramètres de débogage

Certaines propriétés ne peuvent être modifiées qu’à l’aide du fichier config de Launchpad, ce qui peut être utile dans des cas limités comme le débogage. Le fichier config est créé durant l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il est enregistré par défaut au format de fichier texte brut dans `<instance path>\binn\rlauncher.config`.

Pour pouvoir apporter des modifications à ce fichier, vous devez être administrateur sur l’ordinateur exécutant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si vous modifiez le fichier, nous vous recommandons de faire une copie de sauvegarde avant d’enregistrer les modifications.

Le tableau suivant liste les paramètres avancés pour [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], avec les valeurs autorisées.

|**Nom du paramètre**|**Type**|**Description**|
|----|----|----|
|JOB\_CLEANUP\_ON\_EXIT|Integer |Il s’agit d’un paramètre exclusivement interne. Ne modifiez pas cette valeur. </br></br>Spécifie si le dossier de travail temporaire créé pour chaque session de runtime externe doit être nettoyé une fois la session terminée. Ce paramètre est utile pour le débogage. </br></br>Les valeurs prises en charge sont **0** (désactivé) ou **1** (activé). </br></br>La valeur par défaut est 1, ce qui signifie que les fichiers journaux sont supprimés à la sortie.|
|TRACE\_LEVEL|Integer |Configure le niveau de verbosité de la trace de MSSQLLAUNCHPAD à des fins de débogage. Affecte les fichiers de trace dans le chemin spécifié par le paramètre LOG_DIRECTORY. </br></br>Les valeurs prises en charge sont les suivantes : **1** (erreur), **2** (performances), **3** (avertissement) et **4** (informations). </br></br>La valeur par défaut est 1, ce qui signifie uniquement les erreurs de sortie.|

Tous les paramètres prennent la forme d’une paire clé-valeur, chaque paramètre figurant sur une ligne distincte. Par exemple, pour changer le niveau de trace, vous devez ajouter la ligne `Default: TRACE_LEVEL=4`.

<a name="bkmk_EnforcePolicy"></a>

## <a name="enforcing-password-policy"></a>Application de la stratégie de mot de passe

Si votre organisation a mis en place une stratégie exigeant le changement périodique des mots de passe, vous devrez peut-être forcer le service Launchpad à regénérer les mots de passe chiffrés qu’il gère pour ses comptes de travail.

Pour activer ce paramètre et forcer l’actualisation des mots de passe, ouvrez le volet **Propriétés** du service Launchpad dans le Gestionnaire de configuration SQL Server, cliquez sur **Avancé**, puis affectez à **Réinitialiser le mot de passe des utilisateurs externes** la valeur **Oui**. Quand vous appliquez ce changement, les mots de passe sont immédiatement regénérés pour tous les comptes d’utilisateurs. Pour exécuter un script externe après ce changement, vous devez redémarrer le service Launchpad. À ce stade, les mots de passe qui viennent d’être générés sont lus.

Pour réinitialiser les mots de passe à intervalles réguliers, vous pouvez définir cet indicateur manuellement ou utiliser un script.

## <a name="next-steps"></a>Étapes suivantes

+ [Framework d’extensibilité](../concepts/extensibility-framework.md)
+ [Vue d’ensemble de la sécurité](../concepts/security.md)