---
title: Configurer la collecte de données d’utilisation et de diagnostic pour SQL Server (CEIP) | Microsoft Docs
description: En savoir plus sur les informations que SQL Server collecte auprès des utilisateurs pour améliorer les produits. Découvrez comment configurer SQL Server pour ne pas envoyer ces informations.
author: MikeRayMSFT
ms.author: mikeray
ms.date: 03/27/2019
ms.topic: conceptual
ms.prod: sql
ms.custom: ''
ms.technology: configuration
ms.openlocfilehash: 6580e33a824807312fb7f2d37d5cab893dbab2a9
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596519"
---
# <a name="configure-usage-and-diagnostic-data-collection-for-sql-server-ceip"></a>Configurer les données d’utilisation et de diagnostic pour SQL Server (CEIP)

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

## <a name="summary"></a>Résumé

Par défaut, Microsoft SQL Server collecte des informations sur la façon dont ses clients utilisent l’application. Plus précisément, SQL Server recueille des données sur l’expérience d’installation, l’utilisation et les performances. Elles aident Microsoft à améliorer le produit pour mieux répondre aux besoins des clients. Par exemple, Microsoft collecte des informations sur les types de codes d’erreur rencontrés par les utilisateurs afin que nous puissions corriger les bogues associés, améliorer notre documentation sur l’utilisation de SQL Server et déterminer s’il faudrait ajouter des fonctionnalités au produit pour mieux servir des clients.

En particulier, Microsoft n’envoie par ce mécanisme aucune information de ces types :
- valeurs des tables utilisateur ;
- identifiants d’ouverture de session ou autres informations d’authentification ;
- Informations d’identification personnelle (PII)

L’exemple de scénario suivant comprend des informations sur l’utilisation des fonctionnalités, qui permettent d’améliorer le produit.

SQL Server 2017 prend en charge les index columnstore pour proposer des scénarios d’analytique rapides. Les index columnstore combinent une structure d’index traditionnelle en « arbre B » (« B-tree ») pour les données récemment insérées avec une structure compressée orientée colonnes permettant de compresser les données et accélérer l’exécution des requêtes. Le produit contient des heuristiques pour migrer des données de la structure en arbre B vers la structure compressée en arrière-plan, ce qui accélérer les résultats de requêtes ultérieurs.

Si l’opération en arrière-plan ne suit pas le rythme d’insertion des données, les performances des requêtes peuvent être plus lentes que prévu. Pour améliorer le produit, Microsoft collecte des informations sur la capacité de SQL Server à suivre le processus de compression automatique des données. L’équipe produit utilise ces informations pour ajuster la fréquence et le parallélisme du code qui effectue la compression. Cette requête est exécutée occasionnellement pour collecter ces informations afin que nous (Microsoft) puissions évaluer la vitesse de déplacement des données. Ceci nous permet d’optimiser les heuristiques du produit.  

```sql
SELECT object_id, type_desc, data_space_id, db_id() AS database_id FROM sys.indexes WITH(nolock) WHERE type = 5 or type = 6 
```

```sql
SELECT cntr_value as merge_policy_evaluation
FROM sys.dm_os_performance_counters WITH(nolock)
WHERE object_name LIKE '%columnstore%' 
AND counter_name ='Total Merge Policy Evaluations' 
AND instance_name = '_Total'
```

Sachez que ce processus se concentre sur les mécanismes nécessaires pour offrir une valeur ajoutée aux clients. L’équipe produit ne regarde pas les données de l’index, ni ne les envoie à Microsoft. SQL Server 2017 collecte et envoie toujours des informations sur l’expérience d’installation à partir du processus d’installation afin de nous permettre de trouver et de résoudre rapidement les problèmes d’installation que rencontre le client. SQL Server 2017 peut être configuré de façon à ne pas envoyer d’informations (pour une instance de serveur donnée) à Microsoft par le biais des mécanismes suivants :
- avec l’application de rapports d’erreurs et d’utilisation ;
- en définissant des sous-clés de Registre sur le serveur.

Pour SQL Server sur Linux, consultez la page [Commentaires client pour SQL Server sur Linux](../linux/usage-and-diagnostic-data-configuration-for-sql-server-linux.md).

> [!NOTE]
> Vous ne pouvez désactiver l’envoi d’informations à Microsoft que dans les versions payantes de SQL Server.

## <a name="remarks"></a>Notes
 - La suppression ou la désactivation du service CEIP SQL n’est pas prise en charge. 
 - La suppression des ressources CEIP SQL à partir du groupe de cluster n’est pas prise en charge. 

Pour refuser la collecte de données, consultez [Activation ou désactivation de l’audit local](usage-and-diagnostic-data-in-local-audit.md#turning-local-audit-on-or-off)

## <a name="error-and-usage-reporting-application"></a>Application de rapports d’erreurs et d’utilisation 

Après l’installation, le paramètre de collecte de données d’utilisation et de diagnostic des instances et composants de SQL Server peut être modifié au moyen de l’application de rapports d’erreurs et d’utilisation. Cette application est disponible dans le cadre de l’installation de SQL Server. Cet outil permet à chaque instance de SQL Server de configurer son propre paramètre de rapports d’utilisation.

> [!NOTE]
> L’application de rapports d’erreurs et d’utilisation est listée sous les outils de configuration de SQL Server. Vous pouvez utiliser cet outil pour gérer votre préférence en matière de rapports d’erreurs et de collecte de données d’utilisation et de diagnostic de la même manière que dans SQL Server 2017. Les rapports d’erreurs sont distincts de la collecte de données d’utilisation et de diagnostic ; par conséquent, ils peuvent être activés ou désactivés indépendamment de la collecte de données d’utilisation et de diagnostic sur l’utilisation. Les rapports d’erreurs recueillent les vidages sur incident envoyés à Microsoft, susceptibles de contenir des informations sensibles selon la [déclaration de confidentialité](./sql-server-privacy.md).

Pour lancer la création de rapports d’erreurs et d’utilisation de SQL Server, cliquez ou appuyez sur **Démarrer**, puis recherchez « Erreur » dans la zone de recherche. L’élément Rapports d’erreurs et d’utilisation de SQL Server s’affiche. Quand vous aurez lancé l’outil, vous pourrez gérer les données d’utilisation et de diagnostic ainsi que les erreurs graves collectés pour des instances et des composants installés sur cet ordinateur.

Dans les versions payantes, utilisez les cases à cocher « Rapports d’utilisation » pour gérer l’envoi de données d’utilisation et de diagnostic à Microsoft.

Dans les versions payantes et les versions gratuites, utilisez les cases à cocher « Rapports d’erreurs » pour gérer l’envoi de commentaires sur les erreurs graves et les vidages sur incident à Microsoft.

## <a name="set-registry-subkeys-on-the-server"></a>Définir des sous-clés de Registre sur le serveur

Les clients d’entreprise peuvent configurer les paramètres de stratégie de groupe pour accepter ou refuser la collecte des données d’utilisation et de diagnostic. Pour cela, il leur faut configurer une stratégie basée sur le Registre. Voici la clé de Registre et les paramètres adéquats :

- Pour les fonctionnalités de l’instance SQL Server :
    
    Subkey = HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\{InstanceID}\CPE
    
    Nom de l'entrée de Registre = CustomerFeedback
    
    Type d'entrée DWORD : 0 pour refuser ; 1 pour accepter
    
    {InstanceID} fait référence au type d’instance et à l’instance, comme dans les exemples suivants :

    - MSSQL14.CANBERRA pour le moteur de base de données SQL Server 2017 et le nom d’instance « CANBERRA »
    - MSAS14.CANBERRA pour SQL Server 2017 Analysis Services et le nom d’instance « CANBERRA »
    - MSRS14.CANBERRA pour SQL Server 2017 Reporting Services et le nom d’instance « CANBERRA »

- Pour toutes les fonctionnalités partagées :
    
    Subkey = HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\{Major Version}
    
    Nom de l'entrée de Registre = CustomerFeedback
    
    Type d'entrée DWORD : 0 pour refuser ; 1 pour accepter

> [!NOTE]
> {Major Version} fait référence à la version de SQL Server, par exemple, 140 pour SQL Server 2017

- Pour SQL Server Management Studio 17 et 18, reportez-vous à [Assistance utilisateur de SQL Server Management Studio](../ssms/sql-server-management-studio-telemetry-ssms.md)

## <a name="set-registry-subkeys-for-crash-dump-collection"></a>Définir des sous-clés de Registre pour la collecte des vidages sur incident

De façon similaire au comportement d’une version antérieure de SQL Server, les clients SQL Server Enterprise 2017 peuvent configurer les paramètres de stratégie de groupe sur le serveur afin d’accepter ou de refuser la collecte des vidages sur incident. Pour cela, il leur faut configurer une stratégie basée sur le Registre. Voici les clés de Registre et les paramètres adéquats : 

- Pour les fonctionnalités de l’instance SQL Server :

    Subkey = HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\{InstanceID}\CPE

    Nom RegEntry = EnableErrorReporting

    Type d'entrée DWORD : 0 pour refuser ; 1 pour accepter
 
    {InstanceID} fait référence au type d’instance et à l’instance, comme dans les exemples suivants : 

    - MSSQL14.CANBERRA pour le moteur de base de données SQL Server 2017 et le nom d’instance « CANBERRA »
    - MSAS14.CANBERRA pour SQL Server 2017 Analysis Services et le nom d’instance « CANBERRA »
    - MSRS14.CANBERRA pour SQL Server 2017 Reporting Services et le nom d’instance « CANBERRA »
 

- Pour toutes les fonctionnalités partagées :
    
    Subkey = HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\{Major Version}

    Nom RegEntry = EnableErrorReporting

    Type d'entrée DWORD : 0 pour refuser ; 1 pour accepter

> [!NOTE]
> {Major Version} fait référence à la version de SQL Server. Par exemple, « 140 » fait référence à SQL Server 2017.

La stratégie de groupe basée sur le Registre sur ces sous-clés de Registre est respectée par la collecte des vidages sur incident de SQL Server 2017. 

## <a name="crash-dump-collection-for-ssms"></a>Collecte des vidages sur incident pour SSMS
SSMS ne collecte pas son propre vidage sur incident. Les vidages sur incident liés à SSMS sont recueillis dans le cadre du Rapport d’erreurs Windows.

La procédure pour activer ou désactiver cette fonctionnalité dépend de la version du système d’exploitation. Pour l’activer ou la désactiver, suivez les étapes de l’article correspondant à votre version de Windows.
 
- Windows Server 2016 et Windows 10

    [Configurer les données de diagnostic Windows dans votre organisation](/windows/privacy/configure-windows-diagnostic-data-in-your-organization)
- Windows Server 2008 R2 et Windows 7

    [Paramètres des Rapports d’erreurs Windows](/windows/desktop/wer/wer-settings)
 
## <a name="feedback-for-analysis-services"></a>Commentaires pour Analysis Services

Pendant l’installation, SQL Server 2016 Analysis Services ajoute un compte spécial à votre instance d’Analysis Services. Ce compte est membre du rôle d’administrateur du serveur Analysis Services. Le compte est utilisé pour collecter des informations pour les commentaires provenant de l’instance d’Analysis Services.  

Vous pouvez configurer votre service de sorte qu’il n’envoie pas de données d’utilisation et de diagnostic, comme le décrit la section « Définir des sous-clés de Registre sur le serveur ». Toutefois, cela ne supprime pas le compte de service. 
 
[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]