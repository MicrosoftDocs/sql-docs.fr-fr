---
description: Instructions RESTORE – Arguments (Transact-SQL)
title: Arguments RESTORE (Transact-SQL)
ms.custom: ''
ms.date: 08/08/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE statement, arguments
- RESTORE statement
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c07e6ba84a511d81e1fb17718552a0ca2738271a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236239"
---
# <a name="restore-statements---arguments-transact-sql"></a>Instructions RESTORE – Arguments (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette rubrique documente les arguments qui sont décrits dans la section « Syntaxe » de l’instruction RESTORE {DATABASE|LOG} et du jeu associé d’instructions auxiliaires : RESTORE FILELISTONLY, RESTORE HEADERONLY, RESTORE LABELONLY, RESTORE REWINDONLY et RESTORE VERIFYONLY. La plupart des arguments sont pris en charge seulement par un sous-ensemble de ces six instructions. Cette prise en charge est précisée dans la description de chacun des arguments.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
 Pour la syntaxe, consultez les rubriques suivantes :  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
-   [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
-   [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
-   [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)  
  
-   [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)  
  
-   [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 DATABASE  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Spécifie la base de données cible. Si une liste de fichiers et de groupes de fichiers est fournie, seuls ceux-là seront restaurés.  
  
 Pour une base de données en mode de récupération complète ou utilisant les journaux de transactions, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nécessite dans la plupart des cas une sauvegarde de la fin du journal, avant la restauration de la base de données. Restaurer une base de données sans effectuer une sauvegarde de la fin du journal au préalable entraîne une erreur, sauf si l'instruction RESTORE DATABASE contient la clause WITH REPLACE ou WITH STOPAT, qui doit spécifier une heure ou une transaction qui s'est produite après la fin de la sauvegarde des données. Pour plus d’informations sur les sauvegardes de la fin du journal, consultez [Sauvegardes de la fin du journal &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
 LOG  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Spécifie qu'une sauvegarde du journal des transactions doit être appliquée à cette base de données. Les journaux de transactions doivent être sollicités dans un ordre séquentiel. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vérifie le journal des transactions sauvegardé pour s'assurer que les transactions vont être chargées dans la base de données correcte et dans l'ordre adéquat. Pour appliquer plusieurs journaux de transactions, utilisez l'option NORECOVERY sur toutes les opérations de restauration sauf la dernière.  
  
> [!NOTE]  
>  En général, le dernier journal restauré est la sauvegarde de la fin du journal. Une *sauvegarde de la fin du journal* est une sauvegarde du journal effectuée juste avant la restauration d’une base de données, généralement après un échec dans la base de données. L'utilisation d'une sauvegarde de la fin du journal à partir d'une base de données probablement endommagée empêche la perte de travail en capturant le journal qui n'a pas encore été sauvegardé (la fin du journal). Pour plus d’informations, consultez [Sauvegardes de la fin du journal &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
 Pour plus d’informations, consultez [Appliquer les sauvegardes du journal de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
 { _database\_name_ |  **@** _database\_name\_var_}  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Base de données dans laquelle est restaurée la base complète ou le journal des transactions. S’il est fourni comme variable ( **@** _database\_name\_var_), ce nom peut être spécifié comme constante de chaîne ( **@** _database\_name\_var_ = *database*\_*name*) ou comme variable de type de données chaîne de caractères, sauf pour les types de données **ntext** ou **text**.  
  
 \<file_or_filegroup_or_page> [ **,** ...*n* ]  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Spécifie le nom d'un fichier, d'un groupe de fichiers ou d'une page logique à inclure dans une instruction RESTORE DATABASE ou RESTORE LOG. Vous pouvez spécifier une liste de fichiers ou de groupes de fichiers.  
  
 Pour une base de données qui utilise le mode de récupération simple, les options FILE et FILEGROUP sont autorisées uniquement si les fichiers ou groupes de fichiers cibles sont en lecture seule ou s’il s’agit d’une restauration PARTIAL (qui aboutit à un [groupe de fichiers obsolète](../../relational-databases/backup-restore/remove-defunct-filegroups-sql-server.md)).  
  
 ?Pour une base de données utilisant le mode de restauration complète ou le mode de récupération utilisant les journaux de transactions, après l'utilisation de RESTORE DATABASE pour restaurer un ou plusieurs fichiers, groupes de fichiers, et/ou pages, en général, vous devez appliquer le journal des transactions aux fichiers contenant les données restaurées ; l'application du journal assure la cohérence de ces fichiers avec le reste de la base de données. Les exceptions sont les suivantes :  
  
-   Si les fichiers restaurés étaient en lecture seule avant leur dernière sauvegarde, il n’est alors pas nécessaire d’appliquer un journal des transactions et l’instruction RESTORE vous informe de cette situation.  
  
-   Si la sauvegarde contient le groupe de fichiers principal et qu'une restauration partielle est entreprise. Dans ce cas, le journal des restaurations n'est pas nécessaire car le journal est restauré automatiquement à partir du jeu de sauvegarde.  
  
FILE **=** { *logical_file_name_in_backup*|  **@** _logical\_file\_name\_in\_backup\_var_}  
 Désigne un fichier à inclure dans la restauration de la base de données.  
  
FILEGROUP **=** { *logical_filegroup_name* |  **@** _logical\_filegroup\_name\_var_ }  
 Désigne un groupe de fichiers à inclure dans la restauration de la base de données.  
  
 **Remarque** FILEGROUP est autorisé en mode de récupération simple uniquement si le groupe de fichiers spécifié est en lecture seule et s’il s’agit d'une restauration partielle (c’est-à-dire, si WITH PARTIAL est utilisé). Tout groupe de fichiers en lecture/écriture non restauré est indiqué dans un état défunt et ne peut être ensuite restauré dans la base de données résultante.  
  
READ_WRITE_FILEGROUPS  
 Sélectionne tous les groupes de fichiers en lecture/écriture. Cette option s'avère particulièrement utile lorsque vous disposez de groupes de fichiers en lecture seule à restaurer après des groupes de fichiers en lecture/écriture et avant les groupes de fichiers en lecture seule.  
  
PAGE = **'** _file_ **:** _page* [ **,** ...*n* ] **'**  
 Spécifie une liste d'une ou plusieurs pages pour une restauration de pages (disponible uniquement pour les bases de données qui utilisent le mode de restauration complète ou le mode de récupération utilisant les journaux de transactions). Les valeurs sont les suivantes :  
  
PAGE  
 Indique une liste d'un ou plusieurs fichiers et d'une ou plusieurs pages.  
  
 *file*  
 ID du fichier contenant une page spécifique à restaurer.  
  
 *page*  
 ID de la page à restaurer dans le fichier.  
  
 *n*  
 Espace réservé indiquant que plusieurs pages peuvent être spécifiées.  
  
 Le nombre maximal des pages qui peuvent être restaurées dans un fichier unique au cours d'une séquence de restauration est 1 000. Cependant, si vous disposez d'un nombre de pages endommagées plus important dans un fichier, pensez à restaurer la totalité du fichier au lieu des pages en question.  
  
> [!NOTE]  
>  Les restaurations de pages ne sont jamais récupérées.  
  
 Pour plus d’informations sur la restauration de pages, consultez [Restaurer des pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md).  
  
 [ **,** ...*n* ]  
 Espace réservé indiquant qu'il est possible de spécifier plusieurs fichiers, groupes de fichiers et pages dans une liste séparée par des virgules. Le nombre est illimité.  
  
FROM { \<backup_device> [ **,** ...*n* ]| \<database_snapshot> } spécifie en général les unités de sauvegarde à partir desquelles effectuer la restauration. Sinon, dans une instruction RESTORE DATABASE, la clause FROM peut spécifier le nom d'un instantané de base de données vers lequel vous restaurez la base de données, auquel cas, aucune clause WITH n'est autorisée.  
  
 Si la clause FROM est omise, la restauration de la sauvegarde n'a pas lieu. À la place, la base de données est récupérée. Cela vous permet de récupérer une base de données restaurée avec l'option NORECOVERY, ou de basculer vers un serveur de secours. Si la clause FROM est omise, il convient de spécifier NORECOVERY, RECOVERY ou STANDBY dans la clause WITH.  
  
 \<backup_device> [ **,** ...*n* ] spécifie les unités de sauvegarde logiques ou physiques à utiliser pour l’opération de restauration.  
  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md), [RESTORE REWINDONLY](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md), et [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 \<backup_device>::= spécifie une unité de sauvegarde logique ou physique à utiliser pour l’opération de sauvegarde de la manière suivante :  
  
 { _logical\_backup\_device\_name_ |  **@** _logical\_backup\_device\_name\_var_ }  
 Nom logique, qui doit respecter les règles applicables aux identificateurs, de l'unité de sauvegarde créée par **sp_addumpdevice** et à partir de laquelle est restaurée la base de données. S’il est fourni comme variable ( **@** _logical\_backup\_device\_name\_var_), le nom de l’unité de sauvegarde peut être spécifié sous la forme d’une constante de chaîne ( **@** _logical\_backup\_device\_name\_var_ = _logical\_backup\_device\_name_) ou d’une variable de type de données chaîne de caractères, sauf pour les types de données **ntext** ou **text**.  
  
 {DISK | TAPE } **=** { **'** _physical\_backup\_device\_name_ **'**  |  **@** _physical\_backup\_device\_name\_var_ }  
 Permet la restauration de sauvegardes à partir de l'unité de disque ou de bande spécifiée. Les unités de type disque et bande doivent être spécifiées avec leur nom réel (par exemple, le chemin complet et le nom de fichier) : `DISK ='Z:\SQLServerBackups\AdventureWorks.bak'` ou `TAPE ='\\\\.\TAPE0'`. S’il est spécifié comme variable ( **@** _physical\_backup\_device\_name\_var_), le nom de l’unité de sauvegarde peut être spécifié sous la forme d’une constante de chaîne ( **@** _physical\_backup\_device\_name\_var_ = '*physcial_backup_device_name*') ou d’une variable de type de données chaîne de caractères, sauf pour les types de données **ntext** ou **text**.  
  
 Si vous utilisez un serveur réseau pourvu d'un nom UNC (qui doit contenir le nom de l'ordinateur), spécifiez le type d'unité DISK. Pour plus d’informations sur l’utilisation de noms UNC, consultez [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
 Le compte sous lequel vous exécutez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit disposer d'un accès en lecture (READ) au serveur de réseau ou à l'ordinateur distant pour effectuer une opération RESTORE.  
  
 *n*  
 Correspond à un espace réservé indiquant qu’il est possible de spécifier jusqu’à 64 unités de sauvegarde dans une liste séparée par des virgules.  
  
 Pour savoir si une séquence de restauration nécessite autant d'unités de sauvegarde que celles utilisées pour créer le support de sauvegarde auquel appartiennent les sauvegardes, vous devez tenir compte du fait que la restauration s'effectue hors connexion ou bien en ligne, conformément à ce qui suit :  
  
-   La restauration hors ligne permet de restaurer une sauvegarde en utilisant moins d'unités que le nombre nécessaire pour créer la sauvegarde.  
  
-   La restauration en ligne nécessite toutes les unités de sauvegarde. Toute tentative avec un nombre d'unités inférieur échoue.  
  
 Étudions, par exemple, un cas dans lequel une base de données a été sauvegardée sur quatre lecteurs de bande reliés au serveur. Une restauration en ligne nécessite que vous disposiez de quatre lecteurs de bande connectés au serveur ; une restauration hors connexion vous permet de restaurer la sauvegarde si l'ordinateur comprend moins de quatre lecteurs.  
  
> [!NOTE]  
>  Lors de la restauration d'une sauvegarde à partir d'un support de sauvegarde miroir, vous ne pouvez spécifier qu'un seul miroir pour chaque famille de supports. En cas d'erreurs, cependant, l'utilisation d'autres miroirs permet une résolution rapide de certains problèmes de restauration. Vous pouvez remplacer un volume de supports endommagé par le volume correspondant d'un autre miroir. Sachez que pour les restaurations hors connexion, vous pouvez effectuer la restauration à partir d'un nombre de supports inférieur au nombre de familles de supports, mais que chaque famille n'est traitée qu'une seule fois.  
  
\<database_snapshot>::=  
**Pris en charge par :**  [RESTORE DATABASE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
DATABASE_SNAPSHOT **=** _database\_snapshot\_name_  
 Rétablit la base de données selon l’instantané de base de données spécifié par *database_snapshot_name*. L'option DATABASE_SNAPSHOT est disponible uniquement pour une restauration de base de données complète. Lors d'une opération de rétablissement, l'instantané de la base de données remplace une sauvegarde de base de données complète.  
  
 Une opération de rétablissement nécessite que l'instantané de base de données spécifié soit l'unique sur la base de données. Au cours de l'opération de restauration, l'instantané de base de données et la base de données de destination sont tous deux indiqués comme `In restore`. Pour plus d’informations, consultez la section « Notes » de la rubrique [RESTORE DATABASE](../../t-sql/statements/restore-statements-transact-sql.md).  
  
### <a name="with-options"></a>Options WITH  
 Spécifie les options à utiliser lors de l'opération de restauration. Pour obtenir un résumé permettant de savoir quelles instructions utilisent quelle option, consultez « Résumé de prise en charge des options WITH », plus loin dans ce chapitre.  
  
> [!NOTE]  
>  Les options WITH sont ici organisées dans le même ordre que dans la section « Syntaxe » de la rubrique [RESTORE {DATABASE|LOG}](../../t-sql/statements/restore-statements-transact-sql.md).  
  
 PARTIAL  
 **Pris en charge par :**  [RESTORE DATABASE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Spécifie une opération de restauration partielle qui restaure le groupe de fichiers primaire et tout groupe de fichiers secondaire indiqué. L'option PARTIAL sélectionne implicitement le groupe de fichiers primaire, en spécifiant que FILEGROUP = 'PRIMARY' n'est pas nécessaire. Pour restaurer un groupe de fichiers secondaire, vous devez explicitement spécifier le groupe de fichiers à l'aide de l'option FILE ou de l'option FILEGROUP.  
  
 L'option PARTIAL n'est pas autorisée sur des instructions RESTORE LOG.  
  
 L'option PARTIAL démarre l'étape initiale d'une restauration fragmentaire, qui permet la restauration ultérieure des groupes de fichiers restants. Pour plus d’informations, consultez [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).  
  
 [ **RECOVERY** | NORECOVERY | STANDBY ]  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 **RECOVERY**  
 Permet d'imposer, pendant la restauration, l'annulation des transactions non validées. Après la récupération, la base de données est prête à l'emploi. Si aucune des options NORECOVERY, RECOVERY ou STANDBY n'est spécifiée, RECOVERY est choisie par défaut.  
  
 Si d'autres opérations RESTORE (RESTORE LOG ou RESTORE DATABASE) sont planifiées, il faut spécifier NORECOVERY ou STANDBY.  
  
 Lors de la restauration de jeux de sauvegarde à partir d'une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il peut s'avérer nécessaire d'opérer une mise à niveau des bases de données. Cette mise à niveau est réalisée automatiquement lorsque WITH RECOVERY est spécifiée. Pour plus d’informations, consultez [Appliquer les sauvegardes du journal de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
> [!NOTE]  
>  Si la clause FROM est omise, il convient de spécifier NORECOVERY, RECOVERY ou STANDBY dans la clause WITH.  
  
 NORECOVERY  
 Permet d'empêcher, pendant la restauration, l'annulation des transactions non validées. Si un autre journal de transactions a été appliqué ultérieurement, spécifiez l'option NORECOVERY ou STANDBY. Si aucune des options NORECOVERY, RECOVERY ou STANDBY n'est spécifiée, RECOVERY est choisie par défaut. Au cours d'une opération de restauration hors connexion à l'aide de l'option NORECOVERY, la base de données n'est pas utilisable.  
  
 Pour la restauration d'une sauvegarde de base de données et d'un ou de plusieurs journaux de transactions, ou lorsque plusieurs instructions RESTORE sont nécessaires (par exemple en cas de sauvegarde complète d'une base suivie d'une sauvegarde différentielle), RESTORE nécessite l'utilisation de l'option WITH NORECOVERY sur toutes les instructions RESTORE, sauf sur la dernière. Une des méthodes recommandées consiste à utiliser WITH NORECOVERY sur TOUTES les instructions dans une séquence de restauration à plusieurs étapes jusqu'à atteindre le point de récupération souhaité, puis d'utiliser une instruction RESTORE WITH RECOVERY séparée pour la récupération uniquement.  
  
 Employée lors de la restauration d'un fichier ou d'un groupe de fichiers, NORECOVERY oblige la base de données à rester dans l'état de restauration lorsque la restauration est terminée. Cette option est utile dans l'une des situations suivantes :  
  
-   un script de restauration est en cours d'exécution et le journal est toujours appliqué ;  
  
-   une séquence de restaurations de fichiers est exécutée et il n'est pas nécessaire que la base soit utilisable entre les deux opérations de restauration.  
  
 Dans certains cas, RESTORE WITH NORECOVERY restaure la restauration par progression définie à un niveau suffisamment avancé afin d'assurer la cohérence avec la base de données. Dans ces cas, la restauration ne s'effectue pas et les données demeurent hors connexion, comme prévu avec cette option. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] émet cependant un message d'information qui indique que la restauration par progression définie peut dorénavant être récupérée à l'aide de l'option RECOVERY.  
  
STANDBY **=** _standby\_file\_name_  
 Indique un fichier journal des annulations qui permet d'annuler les effets de la récupération. L'option STANDBY est autorisée pour la restauration hors connexion (y compris la restauration partielle). L'option est désactivée pour la restauration en ligne. Toute tentative de spécification de l'option STANDBY pour une opération de restauration en ligne entraîne l'échec de l'opération de restauration. STANDBY n'est pas non plus autorisée lorsqu'une mise à niveau de base de données est nécessaire.  
  
 Le fichier d'annulation sert à conserver une préimage « copie à l'écriture » pour les pages modifiées au cours de la validation de l'annulation de RESTORE WITH STANDBY. Le fichier d'annulation permet la constitution d'une base de données accessible en lecture seule entre les restaurations des journaux de transactions ; elle est utilisable avec des serveurs en état de secours semi-automatique ou pour des récupérations spéciales, lorsqu'il s'avère utile d'inspecter la base de données entre les restaurations de journal. Après une opération RESTORE WITH STANDBY, le fichier d'annulation est automatiquement supprimé par l'opération RESTORE suivante. Si ce fichier d'annulation est supprimé manuellement avant l'opération RESTORE suivante, alors la totalité de la base de données doit être à nouveau restaurée. Pendant que la base de données se trouve dans un état STANDBY, vous devez traiter ce fichier d'annulation avec la même attention que pour n'importe quel autre fichier de base de données. Contrairement aux autres fichiers de base de données, ce fichier est uniquement maintenu ouvert par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] au cours des opérations de restauration actives.  
  
 *standby_file_name* spécifie un fichier d’annulation dont l’emplacement est stocké dans le journal de la base de données. Si un fichier existant utilise le nom spécifié, le fichier est remplacé ; sinon, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée le fichier.  
  
 La taille requise pour un fichier d'annulation spécifique dépend du volume d'actions d'annulation issues des transactions non validées au cours de la restauration.  
  
> [!IMPORTANT]  
>  S'il n'y a plus de place sur le disque contenant le fichier d'annulation indiqué, la restauration s'arrête.  
  
 Pour une comparaison de RECOVERY et NORECOVERY, consultez la section « Notes » de la rubrique [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md).  
  
LOADHISTORY  
 **Pris en charge par :**  [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
 Indique que l’opération de restauration charge les informations dans les tables d’historique **msdb**. Pour le jeu de sauvegarde unique faisant l’objet d’une vérification, l’option LOADHISTORY charge les informations sur les sauvegardes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stockées sur le support de sauvegarde dans les tables d’historique de restauration et de sauvegarde de la base de données **msdb**. Pour plus d’informations sur les tables d’historique, consultez [Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md).  
  
#### <a name="general_with_options--n-"></a>\<general_WITH_options> [ ,...n ]  
 Les options WITH générales sont toutes prises en charge dans les instructions RESTORE DATABASE et RESTORE LOG. Certaines de ces options sont aussi prises en charge par une ou plusieurs instructions auxiliaires, comme indiqué ci-dessous.  
  
##### <a name="restore-operation-options"></a>Options de l'opération de restauration  
 Ces options affectent le comportement de l'opération de restauration.  
  
MOVE **'** _logical\_file\_name\_in\_backup_ **'** TO **'** _operating\_system\_file\_name_ **'** [ ...*n* ]  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) et [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
 Indique que le fichier de données ou journal dont le nom logique est spécifié par *logical_file_name_in_backup* doit être déplacé en le restaurant à l’emplacement spécifié par *operating_system_file_name*. Le nom de fichier logique d'un fichier de données ou journal dans un jeu de sauvegarde correspond au nom logique qu'il portait dans la base de données au moment de la création du jeu de sauvegarde.  
  
*n* est un espace réservé indiquant que vous pouvez spécifier des instructions MOVE supplémentaires. Spécifiez une instruction MOVE pour chaque fichier logique du jeu de sauvegarde que vous voulez restaurer à un nouvel emplacement. Par défaut, le fichier *logical_file_name_in_backup* est restauré à son emplacement d’origine.  
  
> [!NOTE]  
>  Utilisez RESTORE FILELISTONLY pour obtenir une liste des fichiers logiques contenus dans le jeu de sauvegarde.  
  
 Si une instruction RESTORE est utilisée pour déplacer une base de données sur le même serveur ou la copier sur un serveur différent, l'option MOVE peut s'avérer nécessaire afin de déplacer les fichiers de la base et d'éviter toute collision avec des fichiers existants.  
  
 Utilisée avec RESTORE LOG, l'option MOVE ne peut servir qu'à déplacer les fichiers qui ont été ajoutés au cours de l'intervalle couvert par le journal en cours de restauration. Si, par exemple, la sauvegarde du journal contient une opération d'ajout de fichier pour le fichier `file23`, ce fichier peut être déplacé à 'laide de l'option MOVE sur RESTORE LOG.  
  
 Quand elle est utilisé avec la sauvegarde d’instantané [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l’option MOVE peut servir uniquement à déplacer des fichiers vers un objet blob Azure dans le même compte de stockage que l’objet blob d’origine. L’option MOVE ne peut pas être utilisée pour restaurer la sauvegarde d’instantané dans un fichier local ou un autre compte de stockage.  
  
 Si une instruction RESTORE VERIFYONLY est utilisée lorsque vous prévoyez de déplacer une base de données sur le même serveur ou de la copier sur un serveur différent, l'option MOVE peut s'avérer nécessaire afin de vérifier que l'espace disponible est nécessaire sur la cible et d'identifier les collisions éventuelles avec des fichiers existants.  
  
 Pour plus d’informations, consultez [Copier des bases de données avec la sauvegarde et la restauration](../../relational-databases/databases/copy-databases-with-backup-and-restore.md).  
  
CREDENTIAL  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) et [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 et versions ultérieures
  
 Utilisé uniquement pour la restauration d’une sauvegarde à partir du service de stockage Microsoft Blob Azure.  
  
> [!NOTE]  
>  Avec [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 jusqu’à [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], vous ne pouvez restaurer qu’à partir d’une seule unité quand l’opération s’effectue depuis une URL. Pour restaurer à partir de plusieurs unités quand l’opération s’effectue depuis une URL, vous devez utiliser [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] ou ultérieur, et utiliser des jetons de signature d’accès partagé (SAS). Pour plus d’informations, consultez [Activer la sauvegarde managée SQL Server sur Microsoft Azure](../../relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure.md) et [Simplification de la création d’informations d’identification SQL avec des jetons de signature d’accès partagé (SAP) sur le stockage Azure avec Powershell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell).  
  
 REPLACE  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit créer la base de données spécifiée et ses fichiers même s'il en existe une autre de même nom. En pareil cas, la base existante est supprimée. Lorsque l'option REPLACE n'est pas précisée, une vérification a lieu. Celle-ci évite le remplacement accidentel d'une autre base de données. Cette vérification garantit que l'instruction RESTORE DATABASE ne restaure pas la base de données sur le serveur courant si les deux conditions suivantes existent :  
  
-   la base de données nommée dans l'instruction RESTORE existe déjà sur le serveur en cours, et  
  
-   le nom de la base est différent du nom enregistré dans le jeu de sauvegarde.  
  
 REPLACE permet également à RESTORE de remplacer un fichier existant dont il est impossible de vérifier qu'il appartient à la base de données en cours de restauration. Normalement, RESTORE refuse de remplacer les fichiers existants. WITH REPLACE peut aussi être utilisé de la même manière que l'option RESTORE LOG.  
  
 REPLACE supplante également la nécessité de sauvegarder la fin du journal avant la restauration de la base de données.  
  
 Pour obtenir des informations sur l’impact de l’utilisation de l’option REPLACE, consultez [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
RESTART  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit redémarrer une restauration qui a été interrompue. RESTART relance la restauration au point de son interruption.  
  
RESTRICTED_USER  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md).  
  
 Limite l’accès de la base de données nouvellement restaurée aux membres des rôles **db_owner**, **dbcreator** ou **sysadmin**.  RESTRICTED_USER remplace l'option DBO_ONLY. L'option DBO_ONLY a été supprimée dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
 Utilisez avec l'option RECOVERY.  
  
##### <a name="backup-set-options"></a>Options du jeu de sauvegarde  
 Ces options se rapportent au jeu de sauvegarde qui contient la sauvegarde à restaurer.  
  
FILE **=** { *backup_set_file_number* |  **@** _backup\_set\_file\_number_ }  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) et [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Identifie le jeu de sauvegarde à restaurer. Ainsi, une valeur *numéro_fichier_jeu_sauvegarde* égale à **1** indique le premier jeu de sauvegarde sur le support de sauvegarde, et une valeur *numéro_fichier_jeu_sauvegarde* égale à **2** indique le second jeu. Vous pouvez obtenir le *numéro_fichier_jeu_sauvegarde* d’un jeu de sauvegarde en utilisant l’instruction [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) .  
  
 Quand il n’est pas spécifié, la valeur par défaut est **1**, sauf pour RESTORE HEADERONLY. Auquel cas, tous les jeux de sauvegarde dans le support de sauvegarde sont traités. Pour plus d'informations, consultez « Spécification d'un jeu de sauvegarde » plus loin dans cette rubrique.  
  
> [!IMPORTANT]  
>  Cette option FILE n’est pas liée à l’option FILE qui permet de spécifier un fichier de base de données, FILE **=** { *logical_file_name_in_backup* |  **@** _logical\_file\_name\_in\_backup\_var_ }.  
  
 PASSWORD  **=** { *password* |  **@** _password\_variable_ }  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) et [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Fournit le mot de passe du jeu de sauvegarde. Un mot de passe de jeu de sauvegarde est une chaîne de caractères.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Si un mot de passe a été spécifié lors de la création du jeu de sauvegarde, il est requis lors de chaque opération de restauration à partir du jeu de sauvegarde. La spécification d'un mot de passe incorrect ou d'un mot de passe pour un jeu de sauvegarde qui n'en possède pas constitue une erreur.  
  
> [!IMPORTANT]  
>  Ce mot de passe fournit uniquement une protection faible pour le support de sauvegarde. Pour plus d'informations, consultez la section « Autorisations » pour la rubrique adéquate.  
  
##### <a name="media-set-options"></a>Options du support de sauvegarde  
 Ces options s'appliquent à l'ensemble du support de sauvegarde.  
  
 MEDIANAME **=** { *media_name* |  **@** _media\_name\_variable_}  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) et [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Indique le nom du support. S'il est fourni, le nom du support doit correspondre au nom des volumes de sauvegarde ; sinon, l'opération de restauration prend fin. En l'absence de nom de support dans l'instruction RESTORE, aucun contrôle n'a lieu pour vérifier la correspondance des noms de support sur les volumes de sauvegarde.  
  
> [!IMPORTANT]  
>  L'emploi cohérent de noms de support dans les opérations de sauvegarde et de restauration offre une garantie supplémentaire de sécurité lors du choix des supports de restauration.  
  
 MEDIAPASSWORD **=** { *mediapassword* |  **@** _mediapassword\_variable_ }  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) et [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Fournit le mot de passe du support de sauvegarde. Un mot de passe de jeu de supports est une chaîne de caractères.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Si un mot de passe a été fourni lors du formatage du support de sauvegarde, il est requis pour accéder à tout jeu de sauvegarde sur le jeu de supports. La spécification d'un mot de passe incorrect ou d'un mot de passe pour un support de sauvegarde qui n'en possède pas constitue une erreur.  
  
> [!IMPORTANT]  
>  Ce mot de passe fournit uniquement une protection faible pour le support de sauvegarde. Pour plus d’informations, consultez la section « Autorisations » de l’instruction en question.  
  
 BLOCKSIZE **=** { *blocksize* |  **@** _blocksize\_variable_ }  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Indique, en octets, la taille physique du bloc. Les tailles prises en charge sont 512, 1024, 2048, 4096, 8192, 16384, 32768 et 65536 (64 Ko) octets. La valeur par défaut est 65536 pour les périphériques à bandes, 512 sinon. En règle générale, cette option est superflue car RESTORE sélectionne automatiquement une taille de bloc appropriée pour le périphérique. Si vous spécifiez explicitement une taille de bloc, la sélection automatique est annulée et remplacée.  
  
 Si vous restaurez une sauvegarde à partir d'un CD-ROM, spécifiez BLOCKSIZE=2048.  
  
> [!NOTE]  
>  En règle générale, cette option n'affecte les performances que si les données sont lues sur des périphériques à bandes.  
  
##### <a name="data-transfer-options"></a>Options de transfert de données  
 Ces options vous permettent d'optimiser le transfert de données à partir de l'unité de sauvegarde.  
  
 BUFFERCOUNT **=** { *buffercount* |  **@** _buffercount\_variable_ }  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Spécifie le nombre total de tampons d'E/S à utiliser pour l'opération de restauration. Vous pouvez spécifier n'importe quel entier positif ; toutefois, un nombre élevé de tampons peut provoquer des erreurs liées à une insuffisance de mémoire. En effet, l'espace d'adressage virtuel peut s'avérer inapproprié dans la tâche Sqlservr.exe.  
  
 L’espace total utilisé par les mémoires tampons est déterminé par : _buffercount_* *\** _maxtransfersize_.  
  
 MAXTRANSFERSIZE **=** { _maxtransfersize_ |  **@** _maxtransfersize\_variable_ }  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 Spécifie, en octets, la plus grande unité de transfert à utiliser entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le support de sauvegarde. Les valeurs possibles sont les multiples de 65536 octets (64 Ko), dans la limite de 4194304 octets (4 Mo).  
> [!NOTE]  
>  Quand FILESTREAM est configuré dans la base de données ou que celle-ci comprend des groupes de fichiers OLTP en mémoire, la valeur de `MAXTRANSFERSIZE` au moment de la restauration doit être supérieure ou égale à celle qui a été utilisée au moment où la sauvegarde a été créée.  
  
##### <a name="error-management-options"></a>Options de gestion des erreurs  
 Ces options vous permettent de déterminer si les sommes de contrôle de sauvegarde sont activées pour l’opération de restauration et si l’opération doit s’arrêter en présence d’une erreur.    
  
 { CHECKSUM | NO_CHECKSUM }  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) et [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Le comportement par défaut consiste à vérifier des sommes de contrôle si elles sont présentes et à poursuivre sans vérification si elles ne le sont pas.  
  
 CHECKSUM  
 Spécifie que des sommes de contrôle de sauvegarde doivent être vérifiées et, si la sauvegarde manque de sommes de contrôle de sauvegarde, entraîne l'échec de l'opération de restauration avec un message indiquant que des sommes de contrôle ne sont pas présentes.  
  
> [!NOTE]  
>  Les sommes de contrôle de page sont pertinentes pour les opérations de sauvegarde uniquement si les sommes de contrôle de sauvegarde sont utilisées.  
  
 Par défaut, lors de la détection d'une somme de contrôle incorrecte, RESTORE signale une erreur de somme de contrôle et s'arrête. Cependant, si vous spécifiez CONTINUE_AFTER_ERROR, RESTORE continue après le renvoi d'une erreur de somme de contrôle et du numéro de la page contenant la somme de contrôle incorrecte, si la corruption le permet.  
  
 Pour plus d’informations sur l’utilisation de sommes de contrôle de sauvegarde, consultez [Erreurs de support possibles pendant les opérations de sauvegarde et de restauration &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).  
  
 NO_CHECKSUM  
 Désactive explicitement la validation de sommes de contrôle par l'opération de restauration.  
  
 { **STOP_ON_ERROR** | CONTINUE_AFTER_ERROR }  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) et [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 STOP_ON_ERROR  
 Spécifie que l'opération de restauration s'arrête à la première erreur rencontrée. Il s'agit du comportement par défaut pour RESTORE, sauf pour VERIFYONLY qui possède CONTINUE_AFTER_ERROR par défaut.  
  
 CONTINUE_AFTER_ERROR  
 Spécifie que l'opération de restauration doit continuer après la détection d'une erreur.  
  
 Si une sauvegarde contient des pages endommagées, il est préférable de répéter l’opération de restauration en utilisant une autre sauvegarde qui ne contient pas ces erreurs (par exemple une sauvegarde effectuée avant que les pages aient été endommagées). Toutefois, en dernier ressort, vous pouvez restaurer une sauvegarde endommagée à l'aide de l'option CONTINUE_AFTER_ERROR de l'instruction RESTORE et essayer de sauver les données.  
  
##### <a name="filestream-options"></a>Options FILESTREAM  
 FILESTREAM ( DIRECTORY_NAME =*directory_name* )  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) et [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et ultérieur
  
 Nom de répertoire compatible avec Windows. Ce nom doit être unique parmi tous les noms de répertoire FILESTREAM au niveau de la base de données dans cette instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La comparaison d'unicité s'effectue sans respect de la casse, quels que soient les paramètres de classement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##### <a name="monitoring-options"></a>Options de surveillance  
 Ces options vous permettent de surveiller le transfert des données à partir de l'unité de sauvegarde.  
  
 STATS [ **=** _percentage_ ]  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) et [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
 Affiche un message à chaque fois qu'un autre pourcentage se termine et sert à évaluer l'état d'avancement de l'opération. Si *percentage* est omis, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affiche un message à chaque incrément de 10 pour cent (environ).  
  
 L'option STATS signale le pourcentage terminé comme seuil de rapport de l'intervalle suivant. Cela se situe approximativement au pourcentage spécifié ; par exemple, avec STATS=10, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée un rapport à cet intervalle environ ; ainsi, au lieu d'afficher précisément 40 %, l'option peut afficher 43 %. Pour les jeux de sauvegarde volumineux, cela n'est pas un problème car le pourcentage d'achèvement progresse très lentement entre les appels d'E/S terminés.  
  
##### <a name="tape-options"></a>Options des périphériques à bandes  
 Ces options sont utilisées uniquement pour les périphériques À BANDES. S'il ne s'agit pas d'un périphérique à bandes, ces options sont ignorées.  
  
 { **REWIND** | NOREWIND }  
 Ces options sont utilisées uniquement pour les périphériques À BANDES. Si vous n'utilisez pas un périphérique à bandes, ces options sont ignorées.  
  
 REWIND  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) et [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libère et rembobine la bande. REWIND est le paramètre par défaut.  
  
 NOREWIND  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) et [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
 La spécification de NOREWIND dans n'importe quel autre argument de restauration génère une erreur.  
  
 Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] maintient la bande ouverte après l'opération de sauvegarde. Cette option vous permet d’améliorer les performances quand vous effectuez plusieurs opérations de sauvegarde sur une bande.  
  
 NOREWIND implique NOUNLOAD, et ces options sont incompatibles dans une instruction RESTORE unique.  
  
> [!NOTE]  
>  Si vous utilisez NOREWIND, l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserve la propriété du lecteur de bande jusqu’à ce qu’une instruction BACKUP ou RESTORE s’exécutant dans le même processus utilise l’option REWIND ou UNLOAD ou jusqu’à l’arrêt de l’instance du serveur. Le fait de maintenir la bande ouverte empêche les autres processus d'y accéder. Pour savoir comment afficher la liste des bandes ouvertes et comment les fermer, consultez [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
 { **UNLOAD** | NOUNLOAD }  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md), [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md), [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md), [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md), [RESTORE REWINDONLY](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md), et [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).  
  
 Ces options sont utilisées uniquement pour les périphériques À BANDES. Si vous n'utilisez pas un périphérique à bandes, ces options sont ignorées.  
  
> [!NOTE]  
>  UNLOAD/NOUNLOAD est un paramètre de session qui reste en vigueur jusqu'à la fin de la session ou tant qu'il n'est pas réinitialisé par le choix de l'option opposée à celle en cours d'utilisation.  
  
 UNLOAD  
 Indique que la bande est automatiquement rembobinée et démontée lorsque la sauvegarde est terminée. UNLOAD est l'option par défaut au démarrage d'une session.  
  
 NOUNLOAD  
 Indique qu’après l’opération RESTORE, la bande reste chargée dans le lecteur de bande.  
  
#### <a name="replication_with_option"></a><replication_WITH_option>  
 Cette option est utile seulement si la base de données a été répliquée lors de la création de la sauvegarde.  
  
 KEEP_REPLICATION  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
Utilisez KEEP_REPLICATION quand vous couplez la réplication à la copie des journaux de transaction. Cette option évite la suppression des paramètres de réplication lorsqu'une sauvegarde de base de données ou de journal est restaurée sur un serveur en état de secours semi-automatique et que la base de données est récupérée. Il est impossible de spécifier cette option lors de la restauration d'une sauvegarde avec l'option NORECOVERY. Pour garantir un fonctionnement correct de la réplication après la restauration :  
  
-   Les bases de données **msdb** et **master** du serveur en état de secours actif doivent être synchronisées avec les bases de données **msdb** et **master** du serveur principal.  
  
-   Le serveur en état de secours semi-automatique doit être renommé pour utiliser le même nom que le serveur primaire.  
  
#### <a name="change_data_capture_with_option"></a><change_data_capture_WITH_option>  
 Cette option est utile seulement si la base de données a été activée pour la capture de données modifiées, lors de la création de la sauvegarde.  
  
 KEEP_CDC  
 **Pris en charge par :**  [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 KEEP_CDC permet d’empêcher la suppression des paramètres de capture de données modifiées quand une sauvegarde de base de données ou de fichier journal est restaurée sur un autre serveur et que la base de données est récupérée. Il est impossible de spécifier cette option lors de la restauration d'une sauvegarde avec l'option NORECOVERY.  
  
 La restauration de la base de données avec KEEP_CDC ne crée pas les travaux de capture de données modifiées. Pour extraire les modifications du journal après avoir restauré la base de données, recréez le travail de capture et le travail de nettoyage pour la base de données restaurée. Pour plus d’informations, consultez [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md).  
  
 Pour plus d’informations sur l’utilisation de la capture de données modifiées avec la mise en miroir de bases de données, consultez [Capture de données modifiées et autres fonctionnalités de SQL Server](../../relational-databases/track-changes/change-data-capture-and-other-sql-server-features.md).  
  
#### \<service_broker_WITH_options>  
 Active ou désactive la remise de messages [!INCLUDE[ssSB](../../includes/sssb-md.md)] ou définit un nouvel identificateur [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Cette option est utile seulement si [!INCLUDE[ssSB](../../includes/sssb-md.md)] a été activé pour la base de données lors de la création de la sauvegarde.  
  
 { ENABLE_BROKER  | ERROR_BROKER_CONVERSATIONS  | NEW_BROKER }  
 **Pris en charge par :**  [RESTORE DATABASE](../../t-sql/statements/restore-statements-transact-sql.md)  
  
 ENABLE_BROKER  
 Indique que la remise de messages [!INCLUDE[ssSB](../../includes/sssb-md.md)] est activée à la fin de la restauration, ce qui permet l’envoi immédiat de messages. Par défaut, la remise de messages [!INCLUDE[ssSB](../../includes/sssb-md.md)] est désactivée pendant une restauration. La base de données conserve l'identificateur Service Broker existant.  
  
 ERROR_BROKER_CONVERSATIONS  
 Termine toutes les conversations avec une erreur indiquant que la base de données est attachée ou restaurée. De cette façon, vos applications peuvent effectuer un nettoyage standard des conversations existantes. La remise des messages Service Broker est désactivée jusqu'à la fin de l'opération, puis est réactivée. La base de données conserve l'identificateur Service Broker existant.  
  
 NEW_BROKER  
 Spécifie qu'un nouvel identificateur Service Broker doit être assigné à la base de données. Dans la mesure où la base de données est considérée comme un nouveau Service Broker, toutes les conversations existantes dans la base de données sont immédiatement supprimées sans générer de message de fin de dialogue. Tout itinéraire faisant référence à l'ancien identificateur Service Broker doit être recréé avec le nouvel identificateur.  
  
#### \<point_in_time_WITH_options>  
 **Pris en charge par :**  [RESTORE {DATABASE|LOG}](../../t-sql/statements/restore-statements-transact-sql.md) et uniquement pour le mode de récupération complète ou le mode de récupération utilisant les journaux de transactions.  
  
 Vous pouvez restaurer une base de donnés à un point dans le temps ou une transaction spécifique en spécifiant le point de récupération cible dans une clause STOPAT, STOPATMARK ou STOPBEFOREMARK. Un point dans le temps ou une transaction spécifié(e) est toujours restauré(e) à partir d'une sauvegarde du journal. Dans chaque instruction RESTORE LOG de la séquence de restauration, vous devez spécifier votre point dans le temps ou transaction cible dans une clause STOPAT, STOPATMARK ou STOPBEFOREMARK identique.  
  
 En guise de condition préalable à une restauration limitée dans le temps, vous devez tout d'abord restaurer une sauvegarde de base de données complète dont le point de terminaison est antérieur à votre point de récupération cible. Pour vous aider à identifier la sauvegarde de base de données à restaurer, vous pouvez éventuellement spécifier votre clause WITH STOPAT, STOPATMARK ou STOPBEFOREMARK dans une instruction RESTORE DATABASE pour générer une erreur si une sauvegarde des données est trop récente pour le point dans le temps cible spécifié. Cependant, la sauvegarde de données complète est toujours restaurée, même si elle contient le point dans le temps cible.  
  
> [!NOTE]  
>  Les options RESTORE_DATABASE et RESTORE_LOG point-in-time WITH sont similaires, mais seule l’option RESTORE LOG prend en charge l’argument *mark_name*.  
  
 { STOPAT | STOPATMARK | STOPBEFOREMARK }   
 
 STOPAT **=** { **'** _datetime_ **'**  |  **@** _datetime\_var* }  
 Indique que la base de données doit être restaurée dans l’état qui était le sien à la date et à l’heure spécifiées par le paramètre *datetime* ou **@** _datetime\_var_. Pour plus d’informations sur la définition d’une date et d’une heure, consultez [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 Si une variable est utilisée pour STOPAT, elle doit être de type **varchar**, **char**, **smalldatetime** ou **datetime**. Seuls les enregistrements de journal écrits avant la date et l'heure spécifiées seront appliqués à la base de données.  
  
> [!NOTE]  
>  Si l'heure STOPAT spécifiée est postérieure à la dernière sauvegarde LOG, la base de données reste dans l'état de non restauration, comme si RESTORE LOG avait été exécuté avec NORECOVERY.  
  
 Pour plus d’informations, consultez [Restaurer une base de données SQL Server jusqu’à une limite dans le temps &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md).  
  
 STOPATMARK **=** { **'** _mark\_name_ **'**  |  **'** lsn:_lsn\_number_ **'** } [ AFTER **'** _datetime_ **'** ]  
 Indique une récupération à un point de récupération donné. La transaction spécifiée est incluse dans la récupération, mais elle n'est validée que si elle a été validée à l'origine lors de la véritable génération de la transaction.  
  
 RESTORE DATABASE et RESTORE LOG prennent en charge le paramètre *lsn_number*. Ce paramètre spécifie un numéro séquentiel dans le journal.  
  
 Le paramètre *mark_name* n’est pris en charge que par l’instruction RESTORE LOG. Ce paramètre identifie une marque de transaction dans la sauvegarde du fichier journal.  
  
 Dans une instruction RESTORE LOG, si la clause AFTER *datetime* est omise, la récupération s’arrête à la première marque portant le nom spécifié. Si une valeur est spécifiée pour AFTER *datetime*, la récupération s’arrête à la première marque portant le nom spécifié, exactement à *datetime* ou après.  
  
> [!NOTE]  
>  Si l'heure, la marque ou le NSE spécifié est postérieur à la dernière sauvegarde LOG, la base de données reste dans l'état de non restauration, comme si RESTORE LOG avait été exécuté avec NORECOVERY.  
  
 Pour plus d’informations, consultez [Utiliser les transactions marquées pour récupérer des bases de données associées uniformément &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md) et [Récupérer un numéro séquentiel dans le journal &#40;SQL Server&#41;](../../relational-databases/backup-restore/recover-to-a-log-sequence-number-sql-server.md).  
  
 STOPBEFOREMARK **=** { **'** _mark\_name_ **'**  |  **'** lsn:_lsn\_number_ **'** } [ AFTER **'** _datetime_ **'** ]  
 Indique une récupération jusqu'à un point de récupération donné. La transaction spécifiée n'est pas incluse dans la récupération et est restaurée lorsque WITH RECOVERY est utilisé.  
  
 RESTORE DATABASE et RESTORE LOG prennent en charge le paramètre *lsn_number*. Ce paramètre spécifie un numéro séquentiel dans le journal.  
  
 Le paramètre *mark_name* n’est pris en charge que par l’instruction RESTORE LOG. Ce paramètre identifie une marque de transaction dans la sauvegarde du fichier journal.  
  
 Dans une instruction RESTORE LOG, si la clause AFTER *datetime* est omise, la récupération s’arrête juste avant la première marque portant le nom spécifié. Si une valeur est spécifiée pour AFTER *datetime*, la récupération s’arrête juste avant la première marque portant le nom spécifié, exactement à *datetime* ou après.  
  
> [!IMPORTANT]  
>  Si une séquence de restauration partielle exclut tout groupe de fichiers FILESTREAM, la restauration limitée dans le temps n'est pas prise en charge. Vous pouvez forcer la séquence de restauration à continuer. Cependant, les groupes de fichiers FILESTREAM omis de l'instruction RESTORE ne pourront jamais être restaurés. Pour forcer une restauration limitée dans le temps, spécifiez l'option CONTINUE_AFTER_ERROR avec l'option STOPAT, STOPATMARK ou STOPBEFOREMARK. Si vous spécifiez l'option CONTINUE_AFTER_ERROR, la séquence de restauration partielle réussit et le groupe de fichiers FILESTREAM devient irrécupérable.  
  
## <a name="result-sets"></a>Jeux de résultats  
 Pour les jeux de résultats, consultez les rubriques suivantes :  
  
-   [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
-   [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
-   [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)  
  
## <a name="remarks"></a>Notes  
 Pour des remarques supplémentaires, consultez les rubriques suivantes :  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
-   [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
-   [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)  
  
-   [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)  
  
-   [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
## <a name="specifying-a-backup-set"></a>Spécification d'un jeu de sauvegarde  
 Un *jeu de sauvegarde* contient la sauvegarde issue d’une opération de sauvegarde réussie unique. Les instructions RESTORE, RESTORE FILELISTONLY, RESTORE HEADERONLY et RESTORE VERIFYONLY fonctionnent sur un jeu de sauvegarde unique dans le jeu de médias sur la ou les unités de sauvegarde spécifiées. Vous devez spécifier la sauvegarde requise à partir du jeu de médias. Vous pouvez obtenir le *numéro_fichier_jeu_sauvegarde* d’un jeu de sauvegarde en utilisant l’instruction [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) .  
  
 L'option permettant de spécifier le jeu de sauvegarde à restaurer est la suivante :  
  
 FILE **=** { *backup_set_file_number* |  **@** _backup\_set\_file\_number_ }  
  
 *backup_set_file_number* indique la position de la sauvegarde dans le support de sauvegarde. Quand *backup_set_file_number* a la valeur 1 (FILE = 1), il s’agit du premier jeu de sauvegarde sur le support de sauvegarde ; quand *backup_set_file_number* a la valeur 2 (FILE = 2), il s’agit du deuxième jeu de sauvegarde, et ainsi de suite.  
  
 Le comportement de cette option varie en fonction de l’instruction, comme l’indique le tableau suivant :  
  
|.|Comportement de l'option FILE du jeu de sauvegarde|  
|---------------|-----------------------------------------|  
|RESTORE|Le numéro par défaut du fichier de jeu de sauvegarde est 1. Seule une option FILE de jeu de sauvegarde est autorisée dans une instruction RESTORE. Il est important de spécifier les jeux de sauvegarde dans l'ordre.|  
|RESTORE FILELISTONLY|Le numéro par défaut du fichier de jeu de sauvegarde est 1.|  
|RESTORE HEADERONLY|Par défaut, tous les jeux de sauvegarde du support sont traités. L’ensemble de résultats RESTORE HEADERONLY retourne des informations sur chaque jeu de sauvegarde, notamment sa **Position** dans le support de sauvegarde. Pour obtenir des informations sur un jeu de sauvegarde donné, indiquez sa position en guise de valeur de *backup_set_file_number* de l’option FILE.<br /><br /> Remarque : Dans le cas d’un support de bande, RESTORE HEADER ne traite que les jeux de sauvegarde figurant sur la bande chargée.|  
|RESTORE VERIFYONLY|La valeur par défaut de *backup_set_file_number* est 1.|  
  
> [!NOTE]  
>  L’option FILE qui permet de spécifier un jeu de sauvegarde n’a aucun rapport avec l’option FILE qui permet de spécifier un fichier de base de données, FILE **=** { *logical_file_name_in_backup* |  **@** _logical\_file\_name\_in\_backup\_var_ }.  
  
## <a name="summary-of-support-for-with-options"></a>Résumé de prise en charge des options WITH  
 Les options WITH suivantes sont prises en charge uniquement par l’instruction RESTORE : BLOCKSIZE, BUFFERCOUNT, MAXTRANSFERSIZE, PARTIAL, KEEP_REPLICATION, { RECOVERY | NORECOVERY | STANDBY }, REPLACE, RESTART, RESTRICTED_USER et { STOPAT | STOPATMARK | STOPBEFOREMARK }  
  
> [!NOTE]  
>  L'option PARTIAL est uniquement prise en charge par RESTORE DATABASE.  
  
 Le tableau suivant répertorie les options WITH utilisées par une ou plusieurs instructions et indique quelles instructions prennent en charge quelle option. Une coche (√) indique qu’une option est prise en charge ; un tiret (-) indique qu’une option n’est pas prise en charge.  
  
|Option WITH|RESTORE|RESTORE FILELISTONLY|RESTORE HEADERONLY|RESTORE LABELONLY|RESTORE REWINDONLY|RESTORE VERIFYONLY|  
|-----------------|-------------|--------------------------|------------------------|-----------------------|------------------------|------------------------|  
|{ CHECKSUM<br /><br /> &#124; NO_CHECKSUM }|√|√|√|√|-|√|  
|{ CONTINUE_AFTER_ERROR<br /><br /> &#124; STOP_ON_ERROR }|√|√|√|√|-|√|  
|FILE<sup>1</sup>|√|√|√|-|-|√|  
|LOADHISTORY|-|-|-|-|-|√|  
|MEDIANAME|√|√|√|√|-|√|  
|MEDIAPASSWORD|√|√|√|√|-|√|  
|MOVE|√|-|-|-|-|√|  
|PASSWORD|√|√|√|-|-|√|  
|{ REWIND &#124; NOREWIND }|√|REWIND uniquement|REWIND uniquement|REWIND uniquement|-|√|  
|STATS|√|-|-|-|-|√|  
|{ UNLOAD &#124; NOUNLOAD }|√|√|√|√|√|√|  
  
 <sup>1</sup> FILE **=** _backup\_set\_file\_number_, qui est différent de {FILE | FILEGROUP}.  
  
## <a name="permissions"></a>Autorisations  
 Pour les autorisations, consultez les rubriques suivantes :  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
-   [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
-   [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
-   [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)  
  
-   [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)  
  
-   [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
## <a name="examples"></a>Exemples  
 Pour les exemples, consultez les rubriques suivantes :  
  
-   [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
-   [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
-   [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
## <a name="see-also"></a>Voir aussi  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)   
 [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)   
 [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)   
 [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md)  
