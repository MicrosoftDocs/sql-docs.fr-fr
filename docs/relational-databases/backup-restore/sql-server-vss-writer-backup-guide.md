---
title: Applications de sauvegarde SQL Server - Service de cliché instantané des volumes (VSS) et SQL Writer
description: Décrit le composant SQL Writer et son rôle dans le processus de création et de restauration des instantanés VSS pour les bases de données SQL Server.
ms.custom: ''
ms.date: 07/24/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.openlocfilehash: 36fbf9ac4010797561e0852b96b6df6fd85b640d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347304"
---
# <a name="sql-server-back-up-applications---volume-shadow-copy-service-vss-and-sql-writer"></a>Applications de sauvegarde SQL Server - Service de cliché instantané des volumes (VSS) et SQL Writer
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

SQL Server assure une prise en charge du service de cliché instantané (VSS, Volume Shadow Copy Service) en fournissant un enregistreur (SQL Writer) afin qu’une application de sauvegarde tierce puisse utiliser le framework VSS pour sauvegarder des fichiers de base de données. Ce document décrit le composant SQL Writer et son rôle dans le processus de création et de restauration des instantanés VSS pour les bases de données SQL Server. Il contient également des détails sur la configuration et l’utilisation de l’enregistreur SQL pour interagir avec des applications de sauvegarde dans le framework VSS.


## <a name="introduction"></a>Introduction

SQL Server prend en charge la création d’instantanés à partir de données SQL Server avec VSS. Ceci se fait via la mise à disposition d’un enregistreur conforme à VSS (l’enregistreur SQL) afin qu’une application de sauvegarde tierce puisse utiliser le framework VSS pour sauvegarder des fichiers de base de données. Ce document décrit le composant SQL Writer et son rôle dans le processus de création et de restauration des instantanés VSS pour les bases de données SQL Server. Il contient également des détails sur la configuration et l’utilisation de l’enregistreur SQL pour interagir avec des applications de sauvegarde dans le contexte du framework VSS.

## <a name="definition-of-terms"></a>Définition des termes

- **Virtual Device Interface (VDI)**  : SQL Server fournit une API appelée VDI (Virtual Device Interface) qui permet aux éditeurs de logiciels indépendants d’intégrer SQL Server dans leurs produits en fournissant une prise en charge des opérations de sauvegarde et de restauration. Conçues pour offrir une fiabilité et des performances optimales, ces API prennent en charge l’éventail complet des fonctions de sauvegarde et de restauration de SQL Server, notamment la gamme complète des fonctionnalités de sauvegarde à chaud et d’instantanés. Pour plus d’informations, consultez [SQL Server 2005 Virtual Backup Device Interface Specification](https://www.microsoft.com/download/details.aspx?id=17282). 

- **Demandeur** : Processus (automatisé ou avec une interface graphique utilisateur) qui demande qu’un ou plusieurs jeux d’instantanés soient pris sur un ou plusieurs volumes d’origine. Dans ce document, un demandeur est également utilisé pour impliquer une application de sauvegarde qui crée un instantané de bases de données SQL Server.

## <a name="about-vss"></a>À propos de VSS

VSS fournit l’infrastructure système permettant d’exécuter des applications VSS sur des systèmes Windows. Bien qu’il soit largement transparent pour les utilisateurs et les développeurs, VSS effectue les opérations suivantes :

- Il coordonne les activités des fournisseurs, des enregistreurs et des demandeurs lors de la création et de l’utilisation des clichés instantanés.
- Il apporte le fournisseur système par défaut.
- Il implémente la fonctionnalité de pilote de bas niveau nécessaire au fonctionnement des fournisseurs.

Le service VSS démarre à la demande ; il doit donc être activé pour les opérations VSS puissent être effectuées.

## <a name="vss-components"></a>Composants de VSS

VSS coordonne les activités des composants coopérants suivants : 

- Les fournisseurs sont propriétaires des données de cliché instantané et instancient les clichés instantanés.
- Les enregistreurs sont des applications qui modifient les données et qui participent au processus de synchronisation des clichés instantanés.
- Les demandeurs lancent la création et la destruction des clichés instantanés. Notre conception est axée sur le scénario où le demandeur est une application de sauvegarde.

VSS fournit la coordination entre ces parties : 

![Diagramme montrant comment VSS fournit la coordination entre ces parties.](media/sql-server-vss-writer-backup-guide/coordinations.png)

Ce diagramme montre tous les composants qui participent à une activité de capture instantanée VSS classique. Dans ce scénario, SQL Server (y compris l’enregistreur SQL) agit comme un enregistreur dans une des zones de l’enregistreur.  Exchange Server ou d’autres produits peuvent être des enregistreurs de ce type.

Dans le reste de ce document, il est supposé que le lecteur est familiarisé avec la terminologie de l’infrastructure VSS.  Pour plus d’informations, consultez la documentation sur le [Service VSS](/windows/win32/vss/volume-shadow-copy-service-overview).

## <a name="about-sql-writer"></a>À propos de SQL Writer

L’enregistreur SQL est un enregistreur VSS fourni par SQL Server. Il gère l’interaction de VSS avec SQL Server. L’enregistreur SQL est fourni avec SQL Server en tant que service autonome et il est installé dans le cadre de l’installation de SQL Server. Par défaut, l’enregistreur SQL n’est pas activé. Il doit être activé explicitement pour s’exécuter sur la machine serveur.

Rôle de l’enregistreur SQL dans une opération de sauvegarde d’instantané VSS : 

![Instantané VSS](media/sql-server-vss-writer-backup-guide/sql-vss-snapshot.png)



## <a name="configuring-the-sql-writer"></a>Configuration de l’enregistreur SQL

Le service SQL Writer est installé dans le système dans le cadre de l’installation de SQL Server et il est configuré pour démarrer automatiquement au démarrage de Windows. 

### <a name="sql-writer-service-account"></a>Compte du service SQL Writer

Lors de l’installation, le compte SQL Writer est installé pour utiliser le compte système local. Comme l’enregistreur SQL doit communiquer avec SQL Server en utilisant des API VDI exclusives, le compte SQL Writer doit disposer de droits d’accès suffisants à la fois pour SQL Server et pour VSS.  La configuration du service en tant que compte système local fournit des droits suffisants pour que le service s’exécute correctement.

  > [!NOTE]
  > Pour que le service SQL Writer fonctionne correctement, il est important de vérifier que le compte système local n’est pas supprimé du rôle « sa » de l’instance SQL Server.

### <a name="enabling-and-starting-sql-writer"></a>Activation et démarrage de SQL Writer

Pour pouvoir démarrer et utiliser l’enregistreur SQL, vous devez effectuer les opérations suivantes :

Le service SQL Writer peut être activé en marquant ce service comme étant « Automatique ». Pour ouvrir les services via le Panneau de configuration, cliquez sur Démarrer, cliquez sur Panneau de configuration, double-cliquez sur Outils d’administration, puis double-cliquez sur Services. Dans le volet Services, double-cliquez sur le service SQL Writer et définissez la propriété « Type de démarrage » sur « Automatique ».

Le service peut aussi être démarré en sélectionnant le bouton « Démarrer » sous la propriété « État du service » dans l’écran des propriétés du service mentionné ci-dessus.

Pour plus de commodité, le service SQL Writer effectue automatiquement ces modifications la première fois qu’il est démarré.

  > [!NOTE]
  > Dans certains cas où une instance de SQL Server Express est installée et où une application utilise la fonctionnalité Instances utilisateur, l’enregistreur SQL peut être démarré automatiquement par SQL Server. Ceci permet de faciliter l’énumération de ces instances utilisateur lors d’une opération de sauvegarde VSS. 

## <a name="backuprestore-operations-and-supported-versions"></a>Opérations de sauvegarde/restauration et versions prises en charge

### <a name="version-support"></a>Prise en charge de la version

L’enregistreur SQL est fourni avec SQL Server et prend en charge seulement les instances SQL Server. 

L’enregistreur SQL énumère aussi les instances SQL Server Express. Les instances utilisateur lancées par SQL Server Express sont également énumérées par l’enregistreur SQL.


## <a name="supported-backuprestore-operations"></a>Opérations de sauvegarde/restauration prises en charge

SQL Server (avec l’enregistreur SQL) prend en charge les modes d’opérations de sauvegarde VSS suivants :

- Non basé sur les composants
- Basé sur les composants

### <a name="noncomponent-based-backup-operations"></a>Opérations de sauvegarde non basées sur les composants

Les sauvegardes non basées sur les composants sélectionnent implicitement des bases de données en utilisant la liste des volumes dans le jeu d’instantanés. L’enregistreur SQL vérifie si des bases de données sont endommagées, générant une erreur s’il en trouve. Une base de données endommagée est une base de données dans laquelle un sous-ensemble de fichiers est sélectionné par la liste des volumes.

Dans le modèle non basé sur les composants, seules les bases de données avec le modèle de récupération simple sont prises en charge. La restauration par progression après une restauration n’est pas prise en charge.

### <a name="component-based-backup-operations"></a>Opérations de sauvegarde basées sur les composants

Les sauvegardes basées sur les composants sont préférables et recommandées avec l’enregistreur SQL, car l’application (l’application de sauvegarde VSS) sélectionne explicitement les bases de données à partir des métadonnées retournées par l’enregistreur SQL. Le jeu d’instantanés doit inclure tous les volumes nécessaires à la sauvegarde de ces bases de données. L’infrastructure VSS n’ajoute pas automatiquement les volumes nécessaires pour le jeu de bases de données sélectionné. Tous les volumes de stockage doivent être inclus dans le jeu d’instantanés de volume. L’application de sauvegarde doit vérifier que tous les volumes de stockage sont inclus dans le jeu d’instantanés.  L’enregistreur SQL détecte les bases de données endommagées (avec des volumes de stockage en dehors du jeu d’instantanés) et fait échouer la sauvegarde.

Le reste de cette rubrique part du principe que les sauvegardes basées sur les composants sont utilisées dans le cadre du processus de création d’instantanés VSS pour SQL Server.

## <a name="features-supported-by-sql-writer"></a>Fonctionnalités prises en charge par SQL Writer


- **Texte intégral** : L’enregistreur SQL signale les conteneurs de catalogue de texte intégral avec des spécifications de fichiers récursifs sous les composants de base de données dans le document des métadonnées de l’enregistreur.  Ils sont automatiquement inclus dans la sauvegarde quand le composant de base de données est sélectionné.
- **Sauvegarde/restauration différentielle** : L’enregistreur SQL prend en charge la sauvegarde/restauration différentielle via deux mécanismes différentiels de VSS : Fichier partiel et fichier différencié par date/heure de dernière modification.
- **Fichier partiel** :   L’enregistreur SQL utilise le mécanisme Fichier partiel de VSS pour signaler les plages d’octets modifiées dans ses fichiers de base de données.  
- **Fichier différencié par date/heure de dernière modification** : L’enregistreur SQL utilise le mécanisme Fichier différencié par date/heure de dernière modification de VSS pour signaler les fichiers modifiés dans les catalogues de texte intégral.
- **Restauration avec déplacement** : L’enregistreur SQL prend en charge la spécification d’une nouvelle cible de VSS lors de la restauration.  La spécification d’une nouvelle cible de VSS permet de placer ailleurs un fichier de base de données/un fichier journal ou un conteneur de catalogues de texte intégral dans le cadre de l’opération de restauration.
- **Renommage d’une base de données** : Un demandeur peut avoir besoin de restaurer une base de données SQL avec un nouveau nom, en particulier si la base de données doit être restaurée côte à côte avec la base de données d’origine. L’enregistreur SQL prend en charge le renommage d’une base de données lors de l’opération de restauration, à condition que la base de données reste dans l’instance SQL d’origine.
- **Sauvegarde de copie uniquement** : Il est parfois nécessaire d’effectuer une sauvegarde destinée à un usage particulier, par exemple quand vous devez effectuer une copie d’une base de données à des fins de test.  Cette sauvegarde ne doit pas impacter les procédures globales de sauvegarde et de restauration de la base de données. L’utilisation de l’option COPY_ONLY spécifie que la sauvegarde est effectuée « hors circuit » et ne doit pas affecter la séquence normale des sauvegardes. L’enregistreur SQL prend en charge le type de sauvegarde « Copie uniquement » avec des instances SQL Server.
- **Récupération automatique d’un instantané de base de données** :   En général, un instantané d’une base de données SQL Server obtenu en utilisant le framework VSS est dans un état non récupéré. Les données de l’instantané ne sont pas accessibles de façon sécurisée avant de passer à la phase de récupération afin d’annuler les transactions en cours et de placer la base de données dans un état cohérent. Une application de sauvegarde VSS peut demander la récupération automatique des instantanés dans le cadre du processus de création des instantanés.

Ces nouvelles fonctionnalités et leur utilisation sont décrites plus en détail dans la section Détails des options de sauvegarde et de restauration de cette rubrique.

### <a name="what-is-not-supported"></a>Ce qui n’est pas pris en charge

- Les sauvegardes de journaux ne sont pas prises en charge par l’enregistreur SQL.
- La sauvegarde de fichiers/groupes de fichiers n’est pas prise en charge.
- La restauration de pages n’est pas prise en charge.
- Les instantanés de base de données ne sont pas pris en charge, et ils sont ignorés lors de la création d’instantanés VSS basés sur les composants et non basés sur les composants. 
- Les bases de données avec fermeture automatique ou les bases de données avec arrêt activé.
- Linux ne fournit pas de framework VSS : SQL Writer n’est donc pas disponible sur Linux

Le tableau suivant liste les types de sauvegardes d’instantanés qui sont pris en charge par SQL Writer/SQL Server avec l’infrastructure VSS pour toutes les éditions de SQL Server.

| **Opération de sauvegarde/restauration**                 | **Basée sur les composants**           | **Non basée sur les composants** |
|:-------------------------------------------- | :---------------------------- | :--------------------- |
|Sauvegarde de données complète </br> (Y compris le catalogue de texte intégral)| Oui                     | Oui                    |
|Restauration complète                                  | Oui                           | Oui                    |
|Restauration complète (Pas de récupération)                    | Oui                           | Non                     |
|Sauvegarde différentielle                           | Oui                           | Non                     |
|Restauration différentielle                          | Oui                           | Non                     |
|Restauration avec déplacement                             | Oui                           | Non                     |
|Renommage d’une base de données                               | Oui                           | Non                     |
|Sauvegarde de copie uniquement                              | Oui                           | Non                     |
|Instantanés récupérés automatiquement                       | Oui                           | Non                     |
|Sauvegarde de fichier journal                                    | Non                            | Non                     |
|Instantanés de base de données                            | Non                            | Non                     |
|Bases de données avec fermeture automatique</br> Bases de données avec arrêt | Oui                        | Non                     |
|Bases de données de groupe de disponibilité                  | Oui                           | Non sur secondaire        | 




## <a name="snapshot-creation-process"></a>Processus de création des instantanés

Le framework VSS coordonne les activités d’un demandeur (une application de sauvegarde) et l’enregistreur SQL lors de la création d’instantanés SQL Server. Pour activer cette coordination, le framework VSS définit les interfaces du demandeur et de l’enregistreur.  Ces interfaces doivent être implémentées par les applications des demandeurs et les enregistreurs participants. SQL Writer implémente les interfaces d’enregistreur nécessaires. Dans le cadre du processus de création des instantanés, les interfaces de l’enregistreur SQL sont appelées par le framework VSS. L’enregistreur SQL interagit avec les instances SQL Server sur le système pour faciliter la création des instantanés.

Le framework VSS définit un ensemble d’API destinées à être utilisées par un demandeur ou une application de sauvegarde. Un développeur d’applications de sauvegarde doit suivre ces modèles d’appel d’API pour travailler avec le processus de création d’instantanés du framework VSS. Les sections suivantes décrivent le processus de création d’instantané du point de vue de l’enregistreur SQL. Elles détaillent également certaines interactions internes entre le demandeur, le framework VSS, l’enregistreur SQL et les instances SQL Server. Pour plus d’informations sur ces étapes et sur les interfaces du framework VSS, consultez la documentation sur le service VSS.    

  > [!NOTE]
  > Il est supposé que le lecteur est familiarisé avec l’infrastructure VSS et avec le processus de création de sauvegardes en général. Ces sections fournissent des informations supplémentaires sur la façon dont l’enregistreur SQL participe au processus de création de sauvegardes de VSS.

## <a name="snapshot-creation-workflow"></a>Workflow de création d’un instantané

![Flux de données](media/sql-server-vss-writer-backup-guide/dataflow-chart.png)

L’image montre le diagramme de flux de données lors d’une opération de création/sauvegarde d’un instantané basé sur les composants. Pour mieux comprendre les tâches de base impliquées dans la réalisation d’une sauvegarde, il est utile de décomposer cette vue d’ensemble selon les phases suivantes :

- Initialisation de la sauvegarde
- Phase de découverte de la sauvegarde
- Tâches de présauvegarde
- Sauvegarde réelle des fichiers
- Fin de la sauvegarde



### <a name="backup-initialization"></a>Initialisation de la sauvegarde

Lors de cette phase de la sauvegarde, le demandeur (l’application de sauvegarde) se lie à l’interface de capture instantanée **IvssBackupComponents** et l’initialise en vue de la sauvegarde. Il appelle également l’API VSS **IVssGatherWriterMetadata** pour indiquer à l’infrastructure VSS de collecter les métadonnées auprès de tous les enregistreurs.

Le framework VSS appelle chacun des enregistreurs inscrits, y compris l’enregistreur SQL pour les métadonnées de l’enregistreur en utilisant l’événement **OnIdentify**. L’enregistreur SQL interroge les instances SQL Server pour obtenir les informations des métadonnées de sauvegarde pour chaque base de données et crée le document des métadonnées de l’enregistreur. Cette phase est également appelée « Énumération des métadonnées ».

Le document des métadonnées de l’enregistreur contient les informations passées par l’enregistreur au demandeur (l’application de sauvegarde). Le document des métadonnées de l’enregistreur contient les informations suivantes :

- L’ID et le nom convivial de l’application.
- L’emplacement des fichiers et des composants.
- Les fichiers qui doivent être inclus et exclus dans une sauvegarde.
- Options à utiliser au moment de la restauration.
- Ces informations sont repassées au demandeur via l’infrastructure VSS.

### <a name="sql-writer-metadata-document"></a>Document des métadonnées de l’enregistreur SQL

Il s’agit d’un document XML créé par un enregistreur (dans le cas présent, l’enregistreur SQL) en utilisant l’interface **IVssCreateWriterMetadata**, et qui contient des informations sur l’état et les composants de l’enregistreur. Les détails de la structure d’un document des métadonnées de l’enregistreur sont décrits dans la documentation de l’API VSS. Voici quelques-unes des informations du document des métadonnées de l’enregistreur SQL.

- Informations d’identification de l’enregistreur
    - **Writer name** (Nom de l’enregistreur) - L"SqlServerWriter"
    - **Writer class ID** (ID de classe de l’enregistreur) -  0xa65faa63, 0x5ea8, 0x4ebc, 0x9d, 0xbd, 0xa0, 0xc4, 0xdb, 0x26, 0x91, 0x2a 
    - **Writer instance ID** (ID d’instance de l’enregistreur)  - L"SQL Server:SQLWriter" 
    - **VSSUsageType** - VSS_UT_USERDATA 
    - **VSSSourceType** - VSS_ST_TRANSACTEDDB 
- Writer Level Information (Informations au niveau de l’enregistreur) - VSS_APP_BACK_END 
- Restore Method Specification (Spécification de la méthode de restauration) – VSS_RME_RESTORE_IF_CAN_REPLACE.
- Supported Backup schema (Schéma de sauvegarde pris en charge) (API IVssCreateWriterMetadata::SetBackupSchema)
    - VSS_BS_DIFFERENTIAL – Sauvegarde différentielle
    - VSS_BS_TIMESTAMPED – Basée sur l’horodatage – pour les fichiers de catalogue de texte intégral.
    - VSS_BS_LAST_MODIFY – Sauvegarde différentielle basée sur la date/heure de dernière modification.
    - VSS_BS_WRITER_SUPPORTS_NEW_TARGET – Prend en charge l’option Nouvel emplacement cible.
    - VSS_BS_WRITER_SUPPORTS_RESTORE_WITH_MOVE – Prend en charge la restauration « avec déplacement »
    - VSS_BS_COPY – Prend en charge l’option de sauvegarde « Copie uniquement ».
- Informations au niveau du composant. Ceci contient des informations spécifiques au niveau du composant fournies par l’enregistreur SQL.
   - **Type** - VSS_CT_FILEGROUP
   - **Name** (Nom) - Nom du composant (nom de la base de données)
   - **Logical path** (Chemin logique) : de l’instance de serveur (se présente sous la forme « serveur\nom-instance » pour les instances nommées, et « serveur » pour l’instance par défaut.)
   - **Component Flags** (Indicateurs de composant)
   - **VSS_CF_APP_ROLLBACK_RECOVERY** – Indique que les instantanés SQL Server nécessitent toujours une phase de « récupération » pour rendre les fichiers cohérents et utilisables pour les scénarios de non-sauvegarde (c’est-à-dire les annulations d’application).
   - Selectionnable (Sélectionnable) - True
   - Selectable for Restore (Sélectionnable pour la restauration) - True 
   - Restore Method supported (Méthodes de restauration prises en charge) – VSS_RME_RESTORE_IF_CAN_REPLACE

La seule extension de la structure du jeu de composants dans SQL Server est l’introduction de catalogues de texte intégral.  Les catalogues de texte intégral sont des répertoires de conteneurs, qui ne peuvent pas être exprimés sous forme de fichiers journaux ou de fichiers de base de données VSS, étant donné que les fichiers de base de données VSS et les fichiers journaux n’ont pas de spécification récursive.  Ainsi, l’enregistreur SQL utilise un composant de groupe de fichiers VSS (VSS_CT_FILEGROUP) pour représenter le composant de niveau base de données, et des groupes de fichiers pour représenter les fichiers de base de données, les fichiers journaux et les fichiers de catalogue de texte intégral.

Un exemple de document des métadonnées de l’enregistreur est fourni à la fin de ce document.

### <a name="backup-discovery"></a>Découverte de la sauvegarde
Dans cette phase, un demandeur examine le document des métadonnées de l’enregistreur, puis crée et remplit un **Document des composants de la sauvegarde** avec chaque composant à sauvegarder. Il spécifie également les options et les paramètres de sauvegarde nécessaires dans le cadre de ce document. Pour l’enregistreur SQL, chaque instance de base de données qui doit être sauvegardée est un composant distinct.

#### <a name="backup-components-document"></a>Document des composants de la sauvegarde
Il s’agit d’un document XML créé par un demandeur (en utilisant l’interface **IVssBackupComponents**) au cours de la configuration d’une opération de restauration ou de sauvegarde. Le document des composants de la sauvegarde contient une liste des composants explicitement inclus, provenant d’un ou de plusieurs enregistreurs, qui participent à une opération de sauvegarde ou de restauration. Il ne contient pas d’informations sur les composants inclus implicitement. En revanche, un document des métadonnées de l’enregistreur contient seulement des composants d’enregistreur qui peuvent participer à une sauvegarde. Les détails de la structure d’un document des composants de la sauvegarde sont décrits dans la documentation de l’API VSS.

#### <a name="prebackup-tasks"></a>Tâches de présauvegarde
Les tâches de présauvegarde sous VSS sont axées sur la création d’un cliché instantané des volumes contenant des données pour la sauvegarde. L’application de sauvegarde enregistre les données du cliché instantané, et non pas le volume réel.

Les demandeurs attendent généralement les enregistreurs pendant la préparation de la sauvegarde et pendant la création du cliché instantané. Si l’enregistreur SQL participe à l’opération de sauvegarde, il doit configurer ses fichiers, et être prêt pour la sauvegarde et pour le cliché instantané.

#### <a name="prepare-for-backup"></a>Préparer la sauvegarde
Le demandeur doit définir le type de l’opération de sauvegarde qui doit être effectuée (**IVssBackupComponents::SetBackupState**), puis indiquer aux enregistreurs via VSS de préparer une opération de sauvegarde en utilisant **IVssBackupComponents::PrepareForBackup**.

L’enregistreur SQL est autorisé à accéder au document des composants de la sauvegarde, qui détaille les bases de données qui doivent être sauvegardées. Tous les volumes de stockage doivent être inclus dans le jeu d’instantanés de volume. L’enregistreur SQL détecte les bases de données endommagées (avec des volumes de stockage en dehors du jeu d’instantanés) et fait échouer la sauvegarde lors de l’événement PostSnapshot.

**Lancer un instantané** : Le demandeur lance le processus de capture instantanée en appelant l’interface DoSnapshotSet du framework VSS.

**Créer un instantané** : Cette phase implique une série d’interactions entre le framework VSS et l’enregistreur SQL.

1. _Préparer la capture instantanée_. L’enregistreur SQL appelle SQL Server pour préparer la création de l’instantané.
1. _Freeze_ (Figer) : L’enregistreur SQL appelle SQL Server pour figer toutes les E/S de base de données pour chacune des bases de données sauvegardées dans l’instantané. Une fois que l’événement freeze retourne à l’infrastructure VSS, VSS crée l’instantané.
1. _Thaw_ (Libérer). Lors de cet événement, l’enregistreur SQL appelle les instances SQL Server pour « libérer » ou reprendre les opérations d’E/S normales.


La phase de création de l’instantané est rapide (moins de 60 secondes), afin d’éviter le blocage de toutes les écritures dans la base de données.

**Après l’instantané** : Si la récupération automatique est nécessaire pour l’instantané, l’enregistreur SQL effectue la récupération automatique pour chaque base de données sélectionnée pour l’instantané. Pour obtenir une explication détaillée, consultez Instantanés récupérés automatiquement.

#### <a name="actual-backup-of-files"></a>Sauvegarde réelle des fichiers

Dans cette phase, le demandeur peut déplacer si nécessaire les données vers un support de sauvegarde. Dans cette étape, les interactions sont entre le demandeur et le framework VSS. L’enregistreur SQL n’est pas impliqué.

1. Obtenir l’état de l’enregistreur. Retourne l’état des enregistreurs. Le demandeur peut avoir à gérer des défaillances qui se produisent ici.
1. Effectuer la sauvegarde.

Le demandeur peut déplacer si nécessaire les données vers un support de sauvegarde.

#### <a name="backup-complete"></a>Sauvegarde terminée

Cet événement indique que la sauvegarde a été effectuée avec succès.

C’est aussi le moment où l’enregistreur SQL peut valider la sauvegarde en tant que base différentielle, si la sauvegarde actuelle est une sauvegarde complète de la base de données (et non pas une sauvegarde de copie uniquement).


  > [!NOTE]
  > Le demandeur doit envoyer explicitement cet événement (événement de sauvegarde terminée) pour permettre à l’enregistreur SQL de valider les sauvegardes de base différentielles. Si cet événement n’est pas reçu, la sauvegarde créée ne sera pas une sauvegarde de « base différentielle » éligible.

#### <a name="save-writer-metadata"></a>Enregistrer les métadonnées de l’enregistreur

Le demandeur doit enregistrer le document des composants de la sauvegarde et les métadonnées de sauvegarde de chaque composant ainsi que les données sauvegardées à partir de l’instantané. Ces métadonnées de l’enregistreur sont nécessaires à l’enregistreur SQL/SQL Server pour les opérations de restauration.

#### <a name="backup-termination"></a>Fin de la sauvegarde

Le demandeur termine le cliché instantané en libérant l’interface **IVssBackupComponents** ou en appelant **IVssBackupComponents::DeleteSnapshots**.

## <a name="restore-process"></a>Processus de restauration

Cette section décrit le workflow de l’opération de restauration et les différentes étapes impliquées.

### <a name="restore-operation-workflow"></a>Workflow de l’opération de restauration

L’illustration suivante montre le diagramme des flux de données lors d’une opération de restauration VSS. Pour mieux comprendre les tâches de base impliquées dans la réalisation d’une restauration, il est utile de décomposer cette vue d’ensemble selon les étapes suivantes :

- Initialisation de la restauration
- Préparation pour la restauration
- Restauration réelle des fichiers
- Nettoyage et fin de la restauration

![Flux du processus de restauration](media/sql-server-vss-writer-backup-guide/restore-process-flow.png)

Dans tous les scénarios de restauration basée sur les composants VSS, la restauration de base de données est gérée par l’enregistreur SQL selon deux phases distinctes.

- **Pré-restauration** :  L’enregistreur SQL gère la validation, la fermeture des descripteurs de fichier, etc.
- **Post-restauration** :  L’enregistreur SQL attache la base de données et effectue si nécessaire une récupération après incident.

Entre ces deux phases, l’application de sauvegarde est chargée de déplacer les données pertinentes sous SQL.

### <a name="restore-initialization"></a>Initialisation de la restauration

Pendant la phase d’initialisation d’une restauration, le demandeur doit avoir accès aux documents des composants de la sauvegarde qui ont été stockés.

Le document des composants de la sauvegarde qui est généré pendant l’opération de sauvegarde est stocké dans le cadre des données de sauvegarde. L’application de sauvegarde doit passer ces données au framework VSS. L’enregistreur SQL obtient l’accès à ces données au début du processus de restauration.

#### <a name="prepare-for-restore"></a>Préparer à la restauration

Lors de la préparation à une restauration, un demandeur utilise le document des composants de la sauvegarde stocké pour déterminer ce qui doit être restauré et comment.  Le demandeur sélectionne les composants à restaurer et définit les options de restauration appropriées en fonction des besoins.

Si une application de sauvegarde prévoit d’appliquer des sauvegardes différentielles ou de fichiers journaux en plus de l’opération de restauration en cours (c’est-à-dire que « Restaurer sans récupération » est nécessaire), l’option suivante doit être définie dans le cadre de la création de composants pour chaque base de données à restaurer.

```
IVssBackupComponents::SetAdditionalRestores(true)
```

Une fois que tous les détails nécessaires ont été définis dans le document des composants de la sauvegarde, le demandeur fait l’appel de **IVssBackupComponents::PreRestore** pour générer un événement PreRestore (Pré-restauration) via VSS, qui sera géré par les enregistreurs.

L’enregistreur SQL examine le document des composants de la sauvegarde fourni pour identifier les bases de données appropriées, en supprimant tous les fichiers supplémentaires créés depuis le moment de la sauvegarde. Il vérifie également les espaces disque et ferme tous les descripteurs de fichiers de base de données ouverts, pour que le demandeur puisse copier les données nécessaires au cours de la phase de restauration. Cette phase permet la détection de toutes les conditions d’erreur préalables avant que le demandeur ne copie réellement le fichier. SQL Server place aussi la base de données en état de restauration.  À partir de ce point, la base de données ne peut pas être démarrée tant qu’une restauration n’a pas réussi.

#### <a name="restore-files"></a>Restaurer des fichiers

Il s’agit d’une action spécifique au demandeur. Il incombe au demandeur (l’application de sauvegarde) de copier les fichiers de base de données nécessaires (ou de copier les plages de données pertinentes pour les restaurations différentielles) aux emplacements appropriés. L’enregistreur SQL n’est pas impliqué dans cette opération.

#### <a name="cleanup-and-termination"></a>Nettoyage et fin

Une fois que toutes les données sont restaurées aux bons endroits, un appel effectué depuis un demandeur pour signaler que l’opération de restauration est terminée (**IvssBackupComponents::PostRestore**) permet à l’enregistreur SQL de savoir que des actions de post-restauration peuvent être démarrées.  À ce stade, l’enregistreur SQL effectue la phase de restauration par progression de la récupération après incident. Si la récupération n’est pas demandée (c’est-à-dire si SetAdditionalRestores(true) n’est pas spécifié par le demandeur), la phase d’annulation de l’étape de récupération est également effectuée au cours de cette phase.

## <a name="backup-and-restore-option-details"></a>Détails des options de sauvegarde et de restauration

Cette section décrit en détail toutes les options de sauvegarde et de restauration prises en charge par SQL Writer.

### <a name="requestor-creates-a-volume-shadow-copy"></a>Le demandeur crée un cliché instantané de volume

L’enregistreur SQL peut être impliqué dans le processus de création de cliché instantané de volume (en dehors du contexte de la sauvegarde/restauration), car le ou les volumes de stockage des fichiers de base de fichiers ont été ajoutés au jeu d’instantanés de volume.  Dans ce cas, l’enregistreur SQL participe seulement à la coordination de l’énumération des métadonnées et des événements Freeze (Figer), Thaw (Libérer), PrepareForSnapshot et PostSnapshot (pour plus d’informations, consultez le diagramme des flux de données).

### <a name="full-backuprestore"></a>Sauvegarde/restauration complète

L’enregistreur SQL prend en charge les opérations de sauvegarde/restauration complète à la fois en mode non basé sur les composants et en mode basé sur les composants.

### <a name="noncomponent-based-backup-and-restore"></a>Sauvegarde et restauration non basée sur les composants

Dans une sauvegarde/restauration non basée sur les composants, le demandeur spécifie un volume ou une arborescence de dossiers à sauvegarder/restaurer. Toutes les données du volume/dossier spécifié sont sauvegardées/restaurées.

#### <a name="backup"></a>Sauvegarde

Dans une sauvegarde non basée sur les composants, l’enregistreur SQL sélectionne implicitement des bases de données en utilisant la liste des volumes dans le jeu d’instantanés.  L’enregistreur vérifie si des bases de données sont endommagées, générant une erreur s’il en trouve. Une base de données endommagée est une base de données dans laquelle un sous-ensemble de fichiers est sélectionné par la liste des volumes.  La restauration par progression (restaurations différentielles ou de journaux) après une restauration n’est pas prise en charge via l’enregistreur SQL.

#### <a name="restore"></a>Restaurer

Le demandeur restaure la ou les bases de données qui ont été sauvegardées en mode non basé sur les composants.  De telles restaurations ne peuvent pas être suivies d’une restauration avec restauration par progression, comme la restauration de journaux ou la restauration différentielle.

Pour les opérations de restauration non basée sur les composants, la restauration doit être effectuée avec l’instance SQL Server hors connexion ou bien avec les bases de données cibles supprimées/détachées de façon à garantir que les fichiers sont hors connexion.  Les fichiers sont copiés sur place, puis la ou les bases de données attachées. Tout cela se produit en dehors de l’étendue de l’enregistreur SQL.

### <a name="component-based-backup-and-restore"></a>Sauvegarde et restauration basées sur les composants

Dans une sauvegarde basée sur les composants, le demandeur sélectionne explicitement les composants de base de données (à partir des métadonnées retournées par l’enregistreur SQL au client) à sauvegarder/restaurer.

#### <a name="backup"></a>Sauvegarde

Dans une sauvegarde basée sur les composants, tous les volumes de stockage des bases de données sélectionnées doivent être inclus dans le jeu d’instantanés de volume.  Sinon, l’enregistreur SQL détectera des bases de données endommagées (avec des volumes de stockage en dehors du jeu d’instantanés) et fera échouer la sauvegarde.  Une sauvegarde complète sauvegarde les données de la base de données et tous les fichiers journaux nécessaires pour ramener la base de données à un état cohérent au niveau transactionnel au moment de la restauration.

#### <a name="full-restore-without-rollforward"></a>Restauration complète sans restauration par progression

Une restauration complète de la sauvegarde de base de données est parfois réalisée sans effectuer de restauration par progression supplémentaire. Cela peut être dû au fait qu’il n’y a pas de métadonnées pour faciliter la restauration par progression ou que, dans certains cas, les restaurations par progression ne sont pas nécessaires. Cette section aborde brièvement ces deux situations.  

#### <a name="no-metadatano-rollforward"></a>Pas de métadonnées/pas de restauration par progression

Si aucune métadonnée de l’enregistreur (métadonnées de sauvegarde basée sur les composants) n’est enregistrée au cours de l’opération de sauvegarde, la restauration doit être effectuée avec l’instance SQL Server hors connexion ou avec les bases de données cibles supprimées/détachées, de façon à garantir que les fichiers sont hors connexion.  Les fichiers sont copiés sur place, puis la ou les bases de données attachées. Tout cela se produit en dehors de l’étendue de l’enregistreur SQL.

#### <a name="metadata-exist-but-no-additional-rollforward-is-needed"></a>Des métadonnées existent, mais aucune restauration par progression supplémentaire n’est nécessaire

Le demandeur restaure la ou les bases de données qui ont été sauvegardées en mode basé sur les composants, mais aucune restauration par progression n’est demandée. Dans ce cas, SQL Server effectue une récupération sur incident sur la base de données dans le cadre de la restauration.

**Restauration complète avec restaurations par progression supplémentaires** : Le demandeur peut demander une restauration en spécifiant l’option SetAdditionalRestores(true).  Cette option indique que le demandeur va procéder ensuite à d’autres restaurations par progression (comme une restauration de journal, une restauration différentielle, etc.). Ceci indique à SQL Server de ne pas effectuer l’étape de récupération à la fin de l’opération de restauration.

Ceci est possible seulement si les métadonnées de l’enregistreur ont été enregistrées lors de la sauvegarde et sont disponibles pour l’enregistreur SQL au moment de la restauration. Le service SQL Server doit être en cours d’exécution avant que le demandeur indique à VSS d’effectuer l’activité de restauration.

L’enregistreur SQL attend la séquence suivante :

1. Préparation de la restauration de chaque base de données. Cette activité implique la fermeture de tous les descripteurs de fichiers afin de permettre la copie/le montage des fichiers de base de données par l’application du demandeur.
1. Les fichiers sont copiés/montés par l’application du demandeur.
1. Finaliser la restauration (avec NORECOVERY).  Les bases de données sont mises en ligne, mais dans l’état « En cours de restauration ».

Des sauvegardes SQL conventionnelles, différentielles ou de journaux peuvent ensuite être utilisées pour restaurer par progression la base de données via VDI/T-SQL, ou en appliquant la restauration différentielle avec l’infrastructure VSS.

### <a name="full-text-support"></a>Prise en charge du texte intégral

L’enregistreur SQL signale les conteneurs de catalogues de texte intégral avec des spécifications de fichiers récursifs sous les composants de base de données dans le document des métadonnées de l’enregistreur.  Ils sont automatiquement inclus dans la sauvegarde quand le composant de base de données est sélectionné.

### <a name="differential-backuprestore"></a>Sauvegarde/restauration différentielle

Une opération de sauvegarde différentielle sauvegarde seulement les données qui ont été modifiées depuis la sauvegarde complète de base la plus récente. Une sauvegarde différentielle contient seulement les parties des fichiers de base de données qui ont changé. Pour effectuer une telle sauvegarde, l’application de sauvegarde ou le demandeur a besoin d’informations sur l’emplacement des modifications dans les fichiers de base de données, afin que les sections appropriées du ou des fichiers puissent être sauvegardées. Lors d’une opération de sauvegarde différentielle, l’enregistreur SQL fournit ces informations au format spécifié par « Informations sur les fichiers partiels VSS ». Ces informations peuvent être utilisées pour sauvegarder seulement la partie modifiée des fichiers de base de données.

### <a name="backup"></a>Sauvegarde

Le demandeur peut lancer une sauvegarde différentielle en définissant l’option DIFFERENTIAL (**VSS_BT_DIFFERENTIAL**) dans le document des composants de la sauvegarde (**IVssBackupComponents::SetBackupState**) lors du lancement d’une opération de sauvegarde avec VSS.  L’enregistreur SQL passe les informations sur les fichiers partiels (qui lui sont retournées par SQL Server) à VSS.  Le demandeur peut obtenir ces informations sur les fichiers en appelant des API VSS (**IVssComponent::GetPartialFile**). Ces informations sur les fichiers partiels permettent au demandeur de choisir seulement les plages d’octets modifiées à sauvegarder pour les fichiers de base de données.

Pendant la phase des tâches de pré-sauvegarde, l’enregistreur SQL vérifie qu’il existe une seule base différentielle pour chaque base de données sélectionnée.

Lors de l’événement PostSnapshot, l’enregistreur SQL obtient les informations des fichiers partiels auprès de SQL Server et les ajoute au document des composants de la sauvegarde avec un appel de **IVssComponent::AddPartialFile**.  

  > [!NOTE]
  > L’enregistreur SQL ne prend en charge qu’une seule base de référence différentielle pour les sauvegardes différentielles. Les bases de référence multiples ne sont pas prises en charge.

### <a name="partial-file-information-format"></a>Format des informations des fichiers partiels

Pour chaque base de données à sauvegarder lors d’une sauvegarde différentielle, l’enregistreur SQL stocke les informations de fichiers partiels pour chaque fichier de base de données. Ces informations sont utilisées par le demandeur ou par l’application de sauvegarde pour copier seulement les parties pertinentes du fichier sur le support de sauvegarde lors de la sauvegarde réelle des fichiers. Pour plus d’informations sur le format de ces informations de fichiers partiels, consultez la documentation du service VSS.

Un demandeur peut déterminer ces fichiers en appelant **IVssComponent::GetPartialFileCount** et **IVssComponent::GetPartialFile**.  **IVssComponent::GetPartialFile** retourne un chemin et un nom de fichier pointant vers le fichier, et une chaîne de plages indiquant ce qui doit être sauvegardé dans le fichier.

Pour plus d’informations sur la récupération des informations des fichiers partiels, consultez la [documentation de VSS](/windows/win32/vss/volume-shadow-copy-service-overview).

### <a name="back-up-files"></a>Sauvegarder les fichiers

Au cours de cette phase, l’application de sauvegarde doit examiner les métadonnées de l’enregistreur stockées dans le document des composants de la sauvegarde et sauvegarder seulement les parties pertinentes des fichiers. (Pour les fichiers de catalogue de texte intégral, cette sauvegarde doit être effectuée en fonction des horodatages des fichiers. Ceci est décrit plus loin dans ce document.)

Une sauvegarde différentielle sera toujours effectuée relativement à la sauvegarde de base la plus récente qui existe pour la base de données.  Au moment de la restauration, SQL Server détecte les sauvegardes de base et différentielles qui ne correspondent pas. Il incombe donc à l’application de sauvegarde ou à l’administrateur système de vérifier que la sauvegarde différentielle est bien relative à la sauvegarde complète attendue.  Si une procédure hors circuit a effectué une autre sauvegarde complète, l’application de sauvegarde peut ne pas être en mesure de restaurer la sauvegarde différentielle, car elle n’est pas « propriétaire » de la sauvegarde de base.

Actuellement, si les informations sur les plages d’octets (informations des fichiers partiels) sont trop volumineuses (dépassant 64 Ko dans la taille de la mémoire tampon), SQL Server génère une erreur demandant à l’utilisateur d’effectuer une sauvegarde complète.

### <a name="interesting-cases-during-backup"></a>Cas intéressants lors de la sauvegarde

L’ajout/suppression/réduction/expansion/renommage logique/renommage physique constituent des cas intéressants dans la sauvegarde.

**Fichiers récemment ajoutés après la réalisation de la sauvegarde de base**

Ces fichiers sont inclus dans la spécification partielle, car chaque en-tête du fichier de base de données SQL doit se trouver dans la spécification partielle.  Outre la page d’en-tête, toutes les pages allouées doivent être incluses dans la spécification partielle.

**Fichiers supprimés après la réalisation de la sauvegarde de base**

Une fois la sauvegarde de base effectuée, des fichiers de données peuvent être supprimés.  Ces fichiers ne sont pas inclus dans le document des métadonnées de l’enregistreur lors de la sauvegarde différentielle.  En outre, il n’y aura pas d’informations partielles associées au fichier supprimé.

**Fichiers réduits après la réalisation de la sauvegarde de base**

Les informations partielles ne sont pas collectées auprès des fichiers tant que les réductions de fichiers n’ont pas été désactivées sur le serveur.  Ceci garantit que nous n’incluons jamais d’informations partielles qui correspondent à la région réduite d’un fichier de données.  

**Fichiers ayant grandi après la réalisation de la sauvegarde de base**

Si la croissance a eu lieu avant que la collecte des informations partielles, ces informations doivent avoir inclus les pages allouées dans la région d’extension.  Si la croissance a eu lieu après que les informations partielles ont été collectées, ces informations n’incluent pas les modifications dans la région d’extension.  Dans les sections suivantes, nous allons voir que ces modifications seront restaurées via la restauration par progression du journal.

**Fichier renommé logiquement après la réalisation de la sauvegarde de base**

Un renommage logique du fichier n’affecte pas la sauvegarde ou la restauration, car le nom logique du fichier n’est référencé nulle part dans le document des métadonnées de l’enregistreur ou dans le document des composants de la sauvegarde.  (Pour plus d’informations, consultez Document des métadonnées de l’enregistreur : un exemple.)

**Fichier renommé physiquement après la réalisation de la sauvegarde de base**

Un renommage de fichier de base de données physique ne prend pas effet tant que la base de données n’a pas redémarré.  Par conséquent, les informations de configuration de la base de données ou les informations du chemin de fichier dans la mémoire tampon des informations partielles sont toujours basées sur les anciens chemins physiques, qui sont les seuls chemins valides vers ces fichiers de base de données sur l’instantané.

### <a name="restore"></a>Restaurer

Lors d’une restauration différentielle, les métadonnées de sauvegarde renvoyées par le demandeur à l’enregistreur SQL contiennent les informations sur le type de sauvegarde.  Par conséquent, aucun traitement spécial de l’enregistreur SQL n’est nécessaire.  SQL Server déterminera lui-même qu’il s’agit d’une restauration différentielle.  SQL Server gère ce type de restauration différentielle de la même façon qu’une restauration différentielle native qui n’est pas effectuée via VSS.

### <a name="prerestore-phase"></a>Phase de pré-restauration

Pendant cette phase, SQL Server redimensionne tous les fichiers à la taille appropriée en fonction des métadonnées des fichiers de la sauvegarde différentielle.  Si le fichier s’est agrandi, SQL Server met à zéro la partie qui s’est agrandie.  Si un nouveau fichier doit être créé (il a été créé après la réalisation de la sauvegarde de base), SQL Server met à zéro le nouveau fichier. Il ferme également tous les descripteurs de fichiers afin que l’application de sauvegarde puisse remplacer les fichiers par les données restaurées à partir du support de sauvegarde.

### <a name="restore-files"></a>Restaurer des fichiers

Le client doit restaurer les fichiers en fonction de la spécification des fichiers partiels.  Les données doivent être restaurées sur le même décalage/la même plage du fichier de base de données que ce qui est spécifié dans la spécification des fichiers partiels stockée dans les métadonnées de l’enregistreur.

L’ajout/suppression/expansion/réduction/renommage logique/renommage physique de fichiers de base de données constituent à nouveau des cas intéressants au moment de la restauration.

**Si un fichier de base de données a été ajouté après la réalisation de la sauvegarde de base complète**

Ces fichiers doivent avoir été créés au préalable par SQL Server lors de la phase de préparation de la restauration.  Ils doivent avoir été étendus à la taille appropriée et mis à zéro.  Le client doit seulement étendre les données en fonction de la spécification partielle (la spécification partielle inclut toutes les extensions allouées).

**Si un fichier de base de données a été supprimé après la réalisation de la sauvegarde de base complète**

Les informations partielles fournies par SQL Server n’incluent pas d’informations de suivi pour ce type de suppression de fichier.  SQL Server est responsable de la détection des fichiers à supprimer, en comparant les métadonnées des fichiers restaurés aux conteneurs existants et en les supprimant.  Cette opération est effectuée avant la restauration à titre d’étape de préparation.

**Si un fichier de base de données s’est agrandi depuis la réalisation de la sauvegarde de base complète**

Ces fichiers doivent avoir été étendus à la taille appropriée par SQL Server lors de la phase de préparation de la restauration.  La région étendue doit aussi avoir été mise à zéro par SQL Server.  Par conséquent, le client peut sans problème étendre les données même dans la région agrandie, en fonction de la spécification partielle.  Si le fichier a été agrandi après que les informations partielles ont été collectées, les modifications dans la région agrandie seront restaurées via la relecture du journal qui a été sauvegardé avec la sauvegarde différentielle.

**Si un fichier de base de données s’est réduit depuis la réalisation de la sauvegarde de base complète**

SQL Server est chargé de tronquer le fichier à la taille nécessaire en fonction des métadonnées.  Cette opération est effectuée avant la restauration à titre d’étape de préparation.

**Si un fichier de base de données a été renommé logiquement depuis la réalisation de la sauvegarde de base complète**

Ceci n’affecte pas la restauration, car le nom logique n’apparaît pas dans le document des métadonnées de l’enregistreur ou dans le document des composants de la sauvegarde.  La modification du nom logique est restaurée quand le client applique la modification au fichier de base de données principal, qui contient les informations du catalogue système.

**Si un fichier de base de données a été renommé physiquement depuis la réalisation de la sauvegarde de base complète**

Si, au moment de la sauvegarde différentielle, le renommage n’a pas pris effet, le client restaure toujours les données à l’ancien emplacement.  Un redémarrage de la base de données après la restauration entraînera la prise en compte du changement de nom physique.  Si, au moment de la sauvegarde différentielle, le renommage de fichier physique a déjà pris effet, les données partielles, s’il y en a, ont été sauvegardées à partir du nouveau chemin physique.  

### <a name="post-restore"></a>Après la restauration

Lors des événements postérieurs à la restauration, l’enregistreur SQL effectue l’opération normale de restauration par progression et la récupération (si SetAdditionalRestores() est défini sur False) de la base de données.

## <a name="differential-backuprestore-for-full-text-catalogs"></a>Sauvegarde/restauration différentielle pour les catalogues de texte intégral

Les catalogues de texte intégral SQL Server font partie des ressources de base de données qui doivent être sauvegardées ou restaurées avec le reste des fichiers de base de données.  Une sauvegarde différentielle est basée sur un horodatage pour le catalogue de texte intégral.  La sauvegarde/restauration différentielle VSS de SQL Server a une seule sauvegarde de base.  En d’autres termes, il n’y aura pas différentes bases pour les différents conteneurs.  Pour la sauvegarde du catalogue de texte intégral VSS, cela signifie que pour tous les conteneurs de catalogues de texte intégral, la sauvegarde différentielle sera basée sur un seul horodatage, contrairement au cas de la sauvegarde différentielle SQL Native, où il y a une base d’horodatage par conteneur de catalogue de texte intégral.

Dans VSS, cet horodatage est exprimé sous la forme d’une propriété à l’échelle du composant, définie lors de la sauvegarde complète et utilisée lors d’une sauvegarde différentielle suivante.  

### <a name="onidentify"></a>OnIdentify

Dans OnIdentify, l’enregistreur SQL appelle IVssCreateWriterMetadata::SetBackupSchema() pour définir la valeur VSS_BS_TIMESTAMPED.  Ceci indique à l’application de sauvegarde que l’enregistreur SQL est propriétaire de la gestion de la base différentielle.

### <a name="setting-the-base-timestamp"></a>Définition de l’horodatage de base

L’horodateur de base est défini lors d’une sauvegarde complète.  Dans **OnPostSnapshot()** , l’enregistreur appelle **IVssComponent::SetBackupStamp()** pour stocker l’horodatage avec le composant dans le document de la sauvegarde.

### <a name="differential-backup"></a>Sauvegarde différentielle

L’application de sauvegarde récupère cet horodatage auprès de la sauvegarde complète de base et rend l’horodatage disponible pour l’enregistreur en appelant **IVssComponent::GetBackupStamp()** pour récupérer l’horodatage de base auprès de la sauvegarde de base précédente.  Ensuite, il le rend disponible pour l’enregistreur en appelant **IVssBackupComponent::SetPreviousBackupStamp()** .  L’enregistreur récupère ensuite le datage en appelant **IVssComponent::GetPreviousBackupStamp()** et le convertit en un horodatage utilisé pour **IVssComponent::AddDifferencedFilesByLastModifyTime()** .  

**Responsabilité de l’application de sauvegarde lors d’une sauvegarde différentielle** : Lors d’une sauvegarde différentielle, l’application de sauvegarde est chargée des opérations suivantes :

- Sauvegarde de tout fichier (le fichier entier) dont l’horodatage de la dernière modification est postérieur à l’horodatage spécifié par la « date/heure de la dernière modification » pour le fichier défini dans le composant.
- Suivi et détection des fichiers supprimés.  

**Responsabilités de l’application de sauvegarde lors d’une restauration différentielle** : Lors d’une restauration différentielle, l’application de sauvegarde est chargée des opérations suivantes :

- Restauration de tous les fichiers qui ont été sauvegardés, soit en créant un nouveau fichier s’il n’existe pas déjà, soit en remplaçant un fichier existant s’il existe déjà.
- Agrandissement du fichier avant d’étendre le contenu si le fichier restauré est plus grand que le fichier existant.
- Troncation du fichier à la même taille que celle du fichier restauré si le fichier restauré est plus petit que le fichier existant.
- Suppression de tous les fichiers qui doivent être supprimés, c’est-à-dire les fichiers qui ne doivent pas exister selon le point dans le temps de la sauvegarde différentielle.

### <a name="copy-only-backup"></a>Sauvegarde de copie uniquement

Il est parfois nécessaire d’effectuer une sauvegarde destinée à un usage spécial. Par exemple, il peut être nécessaire de faire une copie d’une base de données à des fins de test.  Cette sauvegarde ne doit pas impacter les procédures globales de sauvegarde et de restauration de la base de données. L’utilisation de l’option COPY_ONLY spécifie que la sauvegarde est effectuée « hors circuit » et ne doit pas affecter la séquence normale des sauvegardes. L’enregistreur SQL prend en charge le type de sauvegarde « Copie uniquement » avec des instances SQL Server.

Lors la phase de découverte de la sauvegarde, l’enregistreur SQL indique sa capacité à effectuer une sauvegarde de copie uniquement en définissant l’option du schéma de sauvegarde pris en charge VSS_BS_COPY avec un appel de **IVssCreateWriterMetadata::SetBackupSchema**. Le demandeur peut définir le type de sauvegarde en tant que sauvegarde de copie uniquement en définissant l’option VSS_BACKUP_TYPE sur VSS_BT_COPY avec l’appel de **IVssBackupComponents::SetBackupState**.

Quand une sauvegarde de copie uniquement est sélectionnée, il est supposé que les fichiers sur le disque seront copiés sur un support de sauvegarde (par le demandeur), quel que soit l’état de l’historique de sauvegarde de chaque fichier. SQL Server ne met pas à jour l’historique des sauvegardes. Ce type de sauvegarde ne constitue pas une sauvegarde de base pour d’autres opérations de sauvegarde différentielle, et il ne perturbe pas l’historique des sauvegardes différentielles précédentes.

### <a name="restore-with-move"></a>Restauration avec déplacement

VSS permet à l’application de sauvegarde (au demandeur) de spécifier une nouvelle cible de restauration avec l’appel de **IVssComponent::SetNewTarget**.  Dans Restore () et PostRestore (), l’enregistreur SQL vérifie s’il existe au moins une nouvelle cible spécifiée. Il incombe à l’application de sauvegarde de copier physiquement le ou les fichiers au nouvel emplacement lors de la restauration/copie réelle du fichier.

L’application de sauvegarde est autorisée à spécifier seulement de nouvelles cibles pour le chemin physique, mais pas la spécification du fichier lui-même.  Par exemple, pour un fichier de base de données situé dans c:\data\test.mdf, le nom de fichier réel test.mdf ne peut pas être changé.  Seul le chemin c:\data peut être modifié.  Pour un conteneur de catalogue de texte intégral situé dans c:\ftdata\foo, comme la spécification du fichier dans VSS est « * » et que la spécification du chemin dans VSS est c:\ftdata\foo, le chemin complet peut être changé.  

### <a name="database-rename"></a>Renommage d’une base de données

Un demandeur peut avoir besoin de restaurer une base de données SQL avec un nouveau nom, en particulier si la base de données doit être restaurée côte à côte avec la base de données d’origine.  Cette option peut être spécifiée par le demandeur lors de l’opération de restauration en définissant une option de restauration personnalisée avec "New Component Name" = <"Nouveau nom"> en utilisant l’appel VSS **IVssBackupComponents::SetRestoreOptions()** (dans le paramètre wszRestoreOptions).

L’enregistreur SQL prend alors tout le contenu de la valeur de New Component Name (Nouveau nom du composant) et l’utilise comme nouveau nom pour la base de données restaurée. Si aucune option n’est spécifiée, SQL restaure la base de données avec son nom d’origine (nom du composant).

  > [!NOTE]
  > L’enregistreur SQL ne prend pas actuellement en charge le « Renommage entre instances » pour déplacer une base de données vers une nouvelle instance.

### <a name="auto-recovered-snapshots"></a>Instantanés récupérés automatiquement

En général, un instantané d’une base de données SQL Server obtenu en utilisant le framework VSS est dans un état non récupéré. Les données de l’instantané ne sont pas accessibles de façon sécurisée avant de passer à la phase de récupération pour annuler les transactions en cours et de placer la base de données dans un état cohérent. Comme l’instantané est dans un état en lecture seule, il ne peut pas être récupéré par le processus normal d’attachement de la base de données.

Il est possible d’effectuer une récupération automatique des instantanés dans le cadre du processus de création de l’instantané. Dans le cadre du document des métadonnées de l’enregistreur, l’enregistreur SQL spécifie l’indicateur de composant « VSS_CF_APP_ROLLBACK_RECOVERY » pour indiquer que la récupération doit être effectuée pour la base de données sur l’instantané avant que la base de données ne soit accessible lors de la spécification du jeu d’instantanés. Le demandeur peut indiquer que l’instantané doit être un instantané d’annulation d’application (tous les fichiers de base de données d’un instantané sont censés être dans un état cohérent pour être utilisés par l’application) ou un instantané de sauvegarde (un instantané utilisé pour la sauvegarde des données à restaurer ultérieurement dans le cas d’une défaillance du système).

Le demandeur doit définir VSS_VOLSNAP_ATTR_ROLLBACK_RECOVERY pour indiquer que ce composant sera sauvegardé à des fins autre qu’une sauvegarde.  VSS met ensuite en corrélation VSS_CF_APP_ROLLBACK_RECOVERY que l’enregistreur SQL a spécifié sur le composant sélectionné avec VSS_VOLSNAP_ATTR_ROLLBACK_RECOVERY, et détermine que la récupération automatique est en cours.  VSS rend l’instantané accessible en écriture pendant une période limitée et ajoute automatiquement le bit VSS_VOLSNAP_ATTR_AUTORECOVERY au contexte de l’instantané.

Dans le cas de SQL Server, la récupération automatique doit être appliquée seulement aux instantanés d’annulation d’application, mais pas aux instantanés de sauvegarde. Pour les instantanés d’annulation d’application, un processus de récupération automatique est lancé par l’enregistreur SQL lors de l’événement PostSnapShot. Ce processus effectue les opérations suivantes pour chaque base de données SQL Server explicitement sélectionnée (par le demandeur) dans le jeu d’instantanés :

- Attacher la base de données d’instantané à l’instance SQL Server d’origine (c’est-à-dire l’instance à laquelle la base de données d’origine est attachée).
- Récupérer la base de données (ceci se produit dans le cadre de l’opération d’attachement).
- Réduire la taille des fichiers journaux.

    Ceci réduit la quantité de « copies sur écriture » inutiles qui doivent être effectuées par l’infrastructure VSS si le fournisseur VSS est un « fournisseur de logiciels ». La réduction des fichiers journaux est le comportement par défaut. Ceci peut être désactivé en définissant la valeur de la clé de Registre suivante sur 1.
    
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SQLWriter\
    Settings\DisableLogShrink
    
    Ceci peut être utile dans les scénarios où l’instantané peut être utilisé pour exporter des données à partir d’une page spécifique (à un moment précis dans le temps) à partir du journal afin de résoudre un problème dans la base de données en ligne.

- Détachez la base de données.

Nous disposons à présent d’un instantané cohérent et récupéré qui peut être attaché pour y effectuer des requêtes.

### <a name="multi-database-transactions"></a>Transactions sur plusieurs bases de données

Dans certains cas, les bases de données d’instantané peuvent contenir des transactions de plusieurs bases de données en cours. Lors de l’opération de récupération, l’enregistreur SQL attache la base de données sur les captures instantanées avec l’option Presumed Abort (Abandon présumé). Ceci annule toutes les transactions portant sur plusieurs bases de données qui n’ont pas encore été validées (y compris les transactions qui sont dans un état Prêt à valider). Ceci peut entraîner des incohérences entre les bases de données du jeu d’instantanés. Prenons l’exemple de deux bases de données A et B. Il y a une transaction distribuée entre ces deux bases de données et cette transaction est dans l’état Validé dans la base de données A et dans l’état Prêt à valider dans la base de données B. Dans le cadre du processus de récupération automatique, cette transaction est validée dans la base de données A et annulée dans la base de données B. Ceci peut aboutir à des incohérences dans le jeu d’instantanés.

Le composant Microsoft Distributed Transaction Coordinator (MS DTC) qui sera publié selon ce qui est prévu pour Longhorn pour l’infrastructure VSS corrige ce problème d’incohérence pour les transactions portant sur des bases de données sur différentes instances SQL Server. La prochaine version de SQL Server corrigera ces incohérences pour les transactions portant sur des bases de données au sein d’une même instance SQL Server.

### <a name="security-implications-for-autorecovered-snapshots"></a>Implications en matière de sécurité pour les instantanés récupérés automatiquement

Pour les instantanés VSS, après la récupération automatique, les fichiers sont sécurisés avec des listes de contrôle d’accès (ACL) pour autoriser l’accès seulement au groupe intégré spécial auquel appartient le compte SQL Server.  Ceci implique qu’un membre du groupe des administrateurs ou de ce groupe spécial sera en mesure d’attacher la base de données. Le client qui demande un attachement des fichiers de base de données sur un instantané doit être membre du groupe intégré/des administrateurs ou du compte SQL Server.

### <a name="special-cases"></a>Cas particuliers

Cette section décrit quelques cas particuliers rencontrés lors des opérations de sauvegarde et de restauration basées sur SQL Writer.

#### <a name="autoclose-databases"></a>Bases de données avec fermeture automatique

Pour les sauvegardes non basées sur les composants, la fermeture automatique des bases de données est effectuée lors de la vérification des conditions d’endommagement, mais les bases de données fermées automatiquement ne sont pas figées explicitement lors des opérations de sauvegarde.

Le scénario attendu ici est que de nombreuses bases de données fermées peuvent exister et que nous voulons réduire le coût de l’instantané.  Les bases de données fermées automatiquement sont généralement utilisées dans les configurations de bas niveau où les ressources sont rares.

#### <a name="file-list"></a>Liste de fichiers

La liste de fichiers pour chaque base de données est déterminée lors d’une étape d’énumération avant l’événement de préparation à la sauvegarde.  Si la liste des fichiers de base de données change entre l’énumération et le moment où la base de données est figée, la base de données peut être endommagée, à moins que l’application ne revérifie la liste des fichiers. Nous ne pensons pas que ce soit un problème, mais c’est un point dont les fournisseurs doivent être informés.

#### <a name="stopped-instances"></a>Instances arrêtées

Si une instance de SQL Server n’est pas en cours d’exécution au moment où l’étape d’énumération se produit, aucune des bases de données de ces instances ne peut être sélectionnée.

Si une instance s’arrête dans l’intervalle entre l’énumération et l’événement de préparation pour la sauvegarde, toutes les bases de données de l’instance arrêtée sont ignorées.

#### <a name="system-and-user-databases"></a>Bases de données système et utilisateur

Dans SQL Server, les bases de données système incluent les bases de données master, model et msdb qui sont fournies et installées dans le cadre de SQL Server. Cette section décrit comment ces bases de données sont gérées dans un processus de sauvegarde d’instantané VSS.

### <a name="system-databases"></a>Bases de données système

La base de données master ne peut être restaurée qu’en arrêtant l’instance, en remplaçant les fichiers de la base de données (effectué par l’application de sauvegarde), puis en redémarrant l’instance. Aucune restauration par progression n’est possible.

L’enregistreur SQL prend en charge la restauration des bases de données model et msdb en ligne, sans arrêter l’instance.


#### <a name="simple-recovery-model-user-databases"></a>Bases de données utilisateur avec le mode de récupération simple

Si la base de données master est restaurée avec des bases de données utilisateur qui utilisent le mode de récupération simple, les bases de données utilisateur peuvent être restaurées avec la même technique que la base de données master : une fois l’instance arrêtée, copiez ou montez simplement le ou les volumes.  Quand l’instance SQL est démarrée, tout est récupéré.

#### <a name="rolling-forward-user-databases"></a>Bases de données utilisateur avec restauration par progression

Si les bases de données utilisateur doivent être récupérées et restaurées par progression conjointement avec la récupération de la base de données master, l’instance ne doit pas démarrer et récupérer les bases de données master et utilisateur ensemble.

La procédure est la suivante :

1. Vérifiez que l’instance SQL Server est arrêtée.
1. Effectuez la restauration en deux phases.
    1. Restaurez les bases de données système et les bases de données utilisateur qui doivent être récupérées en même temps (c’est-à-dire les bases de données utilisateur en mode de récupération simple) via la copie de fichiers / montage de volumes via VSS.
        1. Si les bases de données utilisateur à restaurer par progression ne se trouvent pas sur le même volume que les bases de données système, ce volume ne doit pas être rétabli pour l’instant. Ce scénario nécessite une planification avant d’effectuer la sauvegarde.
        1. Si les bases de données utilisateur se trouvent sur le même volume que les bases de données système, les bases de données utilisateur doivent être masquées pour SQL Server.
    1. Démarrez l’instance SQL Server en utilisant le paramètre -f.  (Quand vous utilisez l’option de démarrage -f, seule la base de données master peut être restaurée.)
        1. Lancez une commande ALTER DATABASE \<x> SET OFFLINE pour chaque base de données à restaurer par progression.  (Détacher la base de données est une alternative)
        1. Arrêtez l’instance SQL Server.
        1. Démarrez l’instance SQL Server (les fichiers pour les bases de données utilisateur à restaurer par progression ne sont pas visibles par SQL Server).

Utilisez VSS pour restaurer les bases de données utilisateur avec l’option WITH NORECOVERY, comme décrit dans la section « Restauration complète avec restauration par progression ».

## <a name="appendix"></a>Annexe

### <a name="writer-metadata-document--an-example"></a>Document des métadonnées de l’enregistreur :  Exemple

Prenons un exemple de base de données nommée DB1, qui appartient à l’instance SQL Server Instance1 sur la machine Server1, et qui contient les fichiers journaux/de base de données suivants :

- Fichier de base de données nommé « primary » stocké sur c:\db\DB1.mdf
- Fichier de base de données nommé « secondary » stocké sur c:\db\DB1.ndf
- Fichier journal de base de données nommé « log » stocké sur c:\db\DB1.ldf
- Catalogue de texte intégral nommé « foo » stocké sous le répertoire c:\db\ftdata\foo
- Catalogue de texte intégral nommé « bar » stocké sous le répertoire c:\db\ftdata\bar.

Voici les métadonnées de l’enregistreur de la base de données :

**Composant du groupe de fichiers de niveau base de données**

- ComponentType: VSS_CT_FILEGROUP
- LogicalPath: "Server1\Instance1"
- ComponentName: "DB1"
- Caption: NULL
- pbIcon: NULL
- cbIcon: 0
- bRestoreMetadata: FALSE
- NotifyOnBackupComplete: TRUE
- Selectable: TRUE
- SelectableForRestore: TRUE
- ComponentFlags: VSS_CF_APP_ROLLBACK_RECOVERY

**Fichier du groupe de fichiers**

- LogicalPath: "Server1\Instance1"
- GroupName: "DB1"
- Path: "c:\db"
- FileSpec: "DB1.mdf"
- Recursive: FALSE
- AlternatePath: NULL
- BackupTypeMask: VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED
- Filegroup file
- LogicalPath: "Server1\Instance1"
- GroupName: "DB1"
- Path: "c:\db"
- FileSpec: "DB1.ndf"
- Recursive: FALSE
- AlternatePath: NULL
- BackupTypeMask: VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

**Fichier du groupe de fichiers**

- LogicalPath: "Server1\Instance1"
- GroupName: "DB1"
- Path: "c:\db"
- FileSpec: "DB1.ldf"
- Recursive: FALSE
- AlternatePath: NULL
- BackupTypeMask: VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

**Fichier du groupe de fichiers :**

- LogicalPath: "Server1\Instance1"
- GroupName: "DB1"
- Path: "c:\db\ftdata\foo"
- FileSpec: "*"
- Recursive: TRUE
- AlternatePath: NULL
- BackupTypeMask: VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

**Fichier du groupe de fichiers :**

- LogicalPath: "Server1\Instance1"
- GroupName: "DB1"
- Path: "c:\db\ftdata\bar"
- FileSpec: "*"
- Recursive: TRUE
- AlternatePath: NULL
- BackupTypeMask: VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

Si l’instance du serveur est l’instance par défaut sur la machine, le chemin logique devient une partie : "Server1".


    
## <a name="see-also"></a>Voir aussi  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Sauvegardes de type copie seule &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)   
 [Sauvegardes des journaux de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)   
 [Appliquer les sauvegardes du journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)    
 [Guide d’architecture et gestion du journal des transactions SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)
  
