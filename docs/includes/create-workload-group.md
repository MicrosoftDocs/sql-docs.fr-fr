Crée un groupe de charges de travail du gouverneur de ressources et l'associe à un pool de ressources du gouverneur de ressources. Resource Governor n’est pas disponible dans toutes les éditions de [!INCLUDE[msCoName](msconame-md.md)][!INCLUDE[ssNoVersion](ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).

:::image type="icon" source="../database-engine/configure-windows/media/topic-link.gif"::: [Conventions syntaxiques de Transact-SQL](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

## <a name="syntax"></a>Syntaxe

```syntaxsql
CREATE WORKLOAD GROUP group_name
[ WITH
    ( [ IMPORTANCE = { LOW | MEDIUM | HIGH } ]
      [ [ , ] REQUEST_MAX_MEMORY_GRANT_PERCENT = value ]
      [ [ , ] REQUEST_MAX_CPU_TIME_SEC = value ]
      [ [ , ] REQUEST_MEMORY_GRANT_TIMEOUT_SEC = value ]
      [ [ , ] MAX_DOP = value ]
      [ [ , ] GROUP_MAX_REQUESTS = value ] )
 ]
[ USING {
    [ pool_name | "default" ]
    [ [ , ] EXTERNAL external_pool_name | "default" ] ]
    } ]
[ ; ]
```

## <a name="arguments"></a>Arguments

*group_name*</br>
Nom défini par l'utilisateur pour le groupe de charges de travail. *group_name* est alphanumérique, peut contenir jusqu’à 128 caractères, doit être unique dans une instance de [!INCLUDE[ssNoVersion](ssnoversion-md.md)] et doit respecter les règles applicables aux [identificateurs](../relational-databases/databases/database-identifiers.md).

IMPORTANCE = { LOW | **MEDIUM** | HIGH }</br>
Spécifie l'importance relative d'une demande dans le groupe de charges de travail. Le paramètre Importance peut avoir les valeurs suivantes, MEDIUM étant la valeur par défaut :

- LOW
- MEDIUM (valeur par défaut)
- HIGH

> [!NOTE]
> En interne, chaque paramètre d'importance est stocké sous la forme d'un nombre utilisé pour les calculs.

Le paramètre IMPORTANCE est local par rapport au pool de ressources : les groupes de charges de travail d'importance différente à l'intérieur du même pool de ressources s'affectent mutuellement, mais n'affectent pas les groupes de charges de travail dans un autre pool de ressources.

REQUEST_MAX_MEMORY_GRANT_PERCENT = *value*</br>
Spécifie la quantité de mémoire maximale qu'une requête unique peut prendre du pool. *value* est un pourcentage relatif à la taille du pool de ressources spécifiée par MAX_MEMORY_PERCENT.

*value* est un entier jusqu’à [!INCLUDE[ssSQL17](sssql17-md.md)], et une valeur flottante à compter de [!INCLUDE[sql-server-2019](sssql19-md.md)] et dans Azure SQL Managed Instance. La valeur par défaut est 25. La plage autorisée pour *value* est comprise entre 1 et 100.

> [!IMPORTANT]  
> La quantité spécifiée fait uniquement référence à la mémoire allouée à l'exécution de la requête.
>
> L’affectation de la valeur 0 à *value* empêche l’exécution de requêtes avec les opérations SORT et HASH JOIN dans les groupes de charges de travail définis par l’utilisateur.
>
> Il est déconseillé d’affecter à *value* une valeur supérieure à 70, car le serveur risque de ne pas pouvoir mettre de côté suffisamment de mémoire disponible si d’autres requêtes simultanées s’exécutent. Cela risque de provoquer l'erreur de délai d'attente de requête 8645.
>
> Si les besoins en mémoire de la requête dépassent la limite spécifiée par ce paramètre, le serveur effectue les opérations suivantes :
>
> - Pour les groupes de charges de travail définis par l'utilisateur, le serveur essaie de réduire le degré de parallélisme de la requête jusqu'à ce que ses besoins en mémoire tombent sous la limite ou jusqu'à ce que le degré de parallélisme soit égal à 1. Si les besoins en mémoire de la requête sont encore supérieurs à la limite, l'erreur 8657 se produit.
>
> - Pour les groupes de charges de travail internes et par défaut, le serveur autorise la requête à obtenir la mémoire requise.
>
> Sachez toutefois que dans les deux cas, l'erreur de délai d'attente 8645 se produit si le serveur dispose d'une mémoire physique insuffisante.

REQUEST_MAX_CPU_TIME_SEC = *value*</br>
Spécifie la quantité maximale de temps processeur, en secondes, qu'une demande peut utiliser. *value* doit être égal à 0 ou un entier positif. La valeur par défaut de *value* est 0, ce qui signifie illimité.

> [!NOTE]
> Par défaut, Resource Governor n’empêche pas une demande de continuer si le temps maximal est dépassé. Toutefois, un événement sera généré. Pour plus d’informations, consultez [Classe d’événements CPU Threshold Exceeded](../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md).
> [!IMPORTANT]
> À compter de [!INCLUDE[sssql16-md](sssql16-md.md)]SP2 et de [!INCLUDE[ssSQL17](sssql17-md.md)] CU3, quand [l’indicateur de trace 2422](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) est utilisé, Resource Governor abandonne une demande en cas de dépassement de la durée maximale.

REQUEST_MEMORY_GRANT_TIMEOUT_SEC = *value*</br>
Spécifie la durée maximale, en secondes, pendant laquelle une requête peut attendre qu’une allocation de mémoire (mémoire tampon de travail) devienne disponible. *value* doit être égal à 0 ou un entier positif. La valeur par défaut de *value*, 0, utilise un calcul interne basé sur le coût de requête pour déterminer le délai maximal.

> [!NOTE]
> Une requête n'échoue pas toujours lorsque le délai d'expiration d'allocation mémoire est atteint. Une requête échoue seulement si le nombre de requêtes exécutées simultanément est trop élevé. Autrement, la requête risque d'obtenir uniquement l'allocation mémoire minimale, d'où une dégradation des performances.

MAX_DOP = *value*</br>
Spécifie le **degré maximal de parallélisme (MAXDOP)** pour l’exécution de demandes parallèles. *value* doit être égal à 0 ou un entier positif. La plage autorisée pour *value* est comprise entre 0 et 64. Le paramètre par défaut de *value*, 0, utilise le paramètre global. MAX_DOP est géré comme suit :

> [!NOTE]
> Le groupe de charge de travail MAX_DOP remplace la [configuration du serveur pour le degré maximal de parallélisme](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) et la [configuration étendue à la base de données](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) **MAXDOP**.

> [!TIP]
> Pour définir cette option au niveau de la requête, utilisez l’[indicateur de requête](../t-sql/queries/hints-transact-sql-query.md) **MAXDOP**. Définir le degré maximal de parallélisme en tant qu'indicateur de requête est efficace tant qu'il ne dépasse pas le groupe de charges de travail MAX_DOP. Si la valeur d'indicateur de requête MAXDOP dépasse la valeur configurée avec Resource Governor, le [!INCLUDE[ssDEnoversion](ssdenoversion-md.md)] utilise la valeur de `MAX_DOP` Resource Governor. L’[indicateur de requête](../t-sql/queries/hints-transact-sql-query.md) MAXDOP remplace toujours la [configuration du serveur pour le degré maximal de parallélisme](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).
>
> Pour le faire au niveau de la base de données, utilisez la [configuration étendue à la base de données](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) **MAXDOP**.
>
> Pour ce faire au niveau du serveur, utilisez l’[option de configuration serveur](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) du **degré maximal de parallélisme (MAXDOP)** .

GROUP_MAX_REQUESTS = *value*</br>
Spécifie le nombre maximal de demandes simultanées autorisées à s'exécuter dans le groupe de charges de travail. *value* doit être égal à 0 ou un entier positif. La valeur par défaut de *value* est 0, qui autorise un nombre illimité de demandes. Lorsque le nombre maximal de requêtes est atteint, un utilisateur de ce groupe peut se connecter, mais est placé dans un état d'attente jusqu'à ce que le nombre de requêtes simultanées soit inférieur à la valeur spécifiée.

USING { *pool_name* |  **"default"** }</br>
Associe le groupe de charge de travail au pool de ressources défini par l’utilisateur identifié par *pool_name*. Cette opération revient en fait à placer le groupe de charges de travail dans le pool de ressources. Si *pool_name* n’est pas fourni ou si l’argument USING n’est pas utilisé, le groupe de charge de travail est placé dans le pool par défaut Resource Governor prédéfini.

"default" est un mot réservé et doit être placé entre des guillemets doubles ("") ou des crochets ([]) lorsqu'il est utilisé avec l'argument USING.

> [!NOTE]
> Les groupes de charges de travail et les pools de ressources prédéfinis utilisent tous des noms minuscules, tels que "default". Ce facteur doit être pris en considération pour les serveurs qui utilisent un classement qui respecte la casse. Les serveurs avec un classement qui ne respecte pas la casse, tel que SQL_Latin1_General_CP1_CI_AS, traitent "default" et "Default" comme identiques.

EXTERNAL external_pool_name | "default"</br>
**S’applique à :** [!INCLUDE[ssNoVersion](ssnoversion-md.md)] (à compter de [!INCLUDE[sssql16-md](sssql16-md.md)]).

Le groupe de charge de travail peut spécifier un pool de ressources externes. Vous pouvez définir un groupe de charge de travail et l’associer à deux pools :

- Un pool de ressources pour les charges de travail et les requêtes [!INCLUDE[ssNoVersion](ssnoversion-md.md)]
- Un pool de ressources externes pour les processus externes. Pour plus d’informations, consultez [sp_execute_external_script &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

## <a name="remarks"></a>Notes 

Quand `REQUEST_MEMORY_GRANT_PERCENT` est utilisé, la création d’index est autorisée à utiliser une mémoire d’espace de travail supérieure à celle qui lui a été initialement allouée, afin d’améliorer les performances. Cette gestion spéciale est prise en charge par le gouverneur de ressources dans [!INCLUDE[ssCurrent](ssnoversion-md.md)]. Toutefois, l'allocation initiale et toute allocation de mémoire supplémentaire sont limitées par les paramètres du pool de ressources et du groupe de charges de travail.

La limite `MAX_DOP` est définie par [tâche](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md). Il ne s’agit pas d’une limite par [requête](../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md). Cela signifie que lors d’une exécution de requête parallèle, une requête unique peut générer plusieurs tâches qui sont affectées à un [planificateur](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md). Pour plus d’informations, consultez le [Guide de l’architecture des threads et des tâches](../relational-databases/thread-and-task-architecture-guide.md).

Lorsque `MAX_DOP` est utilisé et qu’une requête est marquée comme étant en série au moment de la compilation, elle ne peut être reconvertie en requête parallèle au moment de l'exécution, indépendamment du groupe de charge de travail ou du paramètre de configuration du serveur. Après la configuration de `MAX_DOP`, il ne peut être diminué qu’en raison de la sollicitation de la mémoire. La reconfiguration du groupe de charges de travail n'est pas visible lors de l'attente dans la file d'attente d'allocation de mémoire.

### <a name="index-creation-on-a-partitioned-table"></a>Création d'un index sur une table partitionnée

La mémoire consommée par la création d'index sur une table partitionnée non alignée est proportionnelle au nombre de partitions impliquées. Si la mémoire totale requise dépasse la limite par requête (`REQUEST_MAX_MEMORY_GRANT_PERCENT`) imposée par le paramètre du groupe de charges de travail de Resource Governor, cette création d’index peut échouer. Étant donné que le groupe de charges de travail *"default"* permet à une requête de dépasser la limite par requête avec la mémoire minimale, l’utilisateur peut être en mesure d’exécuter la même création d’index dans le groupe de charges de travail *"default"* , si le pool de ressources *"default"* possède assez de mémoire totale configurée pour exécuter cette requête.

## <a name="permissions"></a>Autorisations

Nécessite l'autorisation `CONTROL SERVER`.

## <a name="example"></a> Exemple

Créez un groupe de charge de travail nommé `newReports` qui utilise les paramètres par défaut de Resource Governor et se trouve dans le pool par défaut de ce dernier. L'exemple spécifie le pool `default`, mais cela n'est pas obligatoire.

```sql
CREATE WORKLOAD GROUP newReports
WITH
    (REQUEST_MAX_MEMORY_GRANT_PERCENT = 2.5
      , REQUEST_MAX_CPU_TIME_SEC = 100
      , MAX_DOP = 4)    
USING "default" ;
GO
```

## <a name="see-also"></a> Voir aussi

- [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/alter-workload-group-transact-sql.md)
- [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/drop-workload-group-transact-sql.md)
- [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/create-resource-pool-transact-sql.md)
- [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/alter-resource-pool-transact-sql.md)
- [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/drop-resource-pool-transact-sql.md)
- [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../t-sql/statements/alter-resource-governor-transact-sql.md)