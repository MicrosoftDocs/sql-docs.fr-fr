  Modifie une configuration de groupe de charge de travail Resource Governor existante, et éventuellement l’assigne à un pool de ressources Resource Governor.  
  
 ![Icône du lien de rubrique](../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER WORKLOAD GROUP { group_name | "default" }  
[ WITH  
    ([ IMPORTANCE = { LOW | MEDIUM | HIGH } ]  
      [ [ , ] REQUEST_MAX_MEMORY_GRANT_PERCENT = value ]  
      [ [ , ] REQUEST_MAX_CPU_TIME_SEC = value ]  
      [ [ , ] REQUEST_MEMORY_GRANT_TIMEOUT_SEC = value ]   
      [ [ , ] MAX_DOP = value ]  
      [ [ , ] GROUP_MAX_REQUESTS = value ] )  
 ]  
[ USING { pool_name | "default" } ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments

 *group_name* | "**default**"       
 Nom d'un groupe de charge de travail existant défini par l'utilisateur ou du groupe de charge de travail par défaut du gouverneur de ressources.  
  
> [!NOTE]  
> Le gouverneur de ressources crée les groupes « par défaut » et internes lorsque [!INCLUDE[ssNoVersion](ssnoversion-md.md)] est installé.  
  
 L'option "default" doit être placée entre des guillemets doubles ("") ou des crochets ([]) lorsqu'elle est utilisée avec l'instruction ALTER WORKLOAD GROUP pour éviter tout conflit avec DEFAULT, qui est un mot réservé au système. Pour plus d'informations, consultez [Database Identifiers](../relational-databases/databases/database-identifiers.md).  
  
> [!NOTE]  
> Les groupes de charges de travail et les pools de ressources prédéfinis utilisent tous des noms en minuscule, tels que « default ». Ce facteur doit être pris en considération pour les serveurs qui utilisent un classement qui respecte la casse. Les serveurs avec un classement qui ne respecte pas la casse, tel que SQL_Latin1_General_CP1_CI_AS, traitent "default" et "Default" comme identiques.  
  
 IMPORTANCE = { LOW | **MEDIUM** | HIGH }       
 Spécifie l'importance relative d'une demande dans le groupe de charges de travail. Elle prend l'une des valeurs suivantes :  
  
-   LOW  
-   MEDIUM (valeur par défaut)  
-   HIGH  
  
> [!NOTE]  
> En interne, chaque paramètre d'importance est stocké sous la forme d'un nombre utilisé pour les calculs.  
  
 Le paramètre IMPORTANCE est local par rapport au pool de ressources : les groupes de charges de travail d'importance différente à l'intérieur du même pool de ressources s'affectent mutuellement, mais n'affectent pas les groupes de charges de travail dans un autre pool de ressources.  
  
 REQUEST_MAX_MEMORY_GRANT_PERCENT = *value*     
 Spécifie la quantité de mémoire maximale qu'une requête unique peut prendre du pool. *value* est un pourcentage relatif à la taille du pool de ressources spécifiée par MAX_MEMORY_PERCENT.  

*value* est un entier pouvant aller jusqu’à [!INCLUDE[ssSQL17](sssql17-md.md)] et une valeur flottante commençant par [!INCLUDE[sql-server-2019](sssqlv15-md.md)]. La valeur par défaut est 25. La plage autorisée pour *value* est comprise entre 1 et 100.
  
> [!NOTE]  
> La quantité spécifiée fait uniquement référence à la mémoire allouée à l'exécution de la requête.  
  
> [!IMPORTANT]
> L’affectation de la valeur 0 à *value* empêche l’exécution de requêtes avec les opérations SORT et HASH JOIN dans les groupes de charges de travail définis par l’utilisateur.     
>
> Il est déconseillé d’affecter à *value* une valeur supérieure à 70, car le serveur risque de ne pas pouvoir mettre de côté suffisamment de mémoire disponible si d’autres requêtes simultanées s’exécutent. Cela risque de provoquer l'erreur de délai d'attente de requête 8645.      
  
> [!NOTE]  
> Si les besoins en mémoire de la requête dépassent la limite spécifiée par ce paramètre, le serveur effectue les opérations suivantes :  
>   
> -  Pour les groupes de charges de travail définis par l'utilisateur, le serveur essaie de réduire le degré de parallélisme de la requête jusqu'à ce que ses besoins en mémoire tombent sous la limite ou jusqu'à ce que le degré de parallélisme soit égal à 1. Si les besoins en mémoire de la requête sont encore supérieurs à la limite, l'erreur 8657 se produit.  
>   
> -  Pour les groupes de charges de travail internes et par défaut, le serveur autorise la requête à obtenir la mémoire requise.  
>   
> Sachez toutefois que dans les deux cas, l'erreur de délai d'attente 8645 se produit si le serveur dispose d'une mémoire physique insuffisante.  
  
 REQUEST_MAX_CPU_TIME_SEC = *value*       
 Spécifie la quantité maximale de temps processeur, en secondes, qu'une demande peut utiliser. *value* doit être égal à 0 ou un entier positif. La valeur par défaut de *value* est 0, ce qui signifie illimité.  
  
> [!NOTE]  
> Par défaut, Resource Governor n’empêche pas une demande de continuer si le temps maximal est dépassé. Toutefois, un événement sera généré. Pour plus d’informations, consultez [Classe d’événements CPU Threshold Exceeded](../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md). 

> [!IMPORTANT]
> À compter de [!INCLUDE[ssSQL15](sssql16-md.md)]SP2 et de [!INCLUDE[ssSQL17](sssql17-md.md)] CU3, quand [l’indicateur de trace 2422](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) est utilisé, Resource Governor abandonne une demande en cas de dépassement de la durée maximale.
  
 REQUEST_MEMORY_GRANT_TIMEOUT_SEC =*value*  
 Spécifie la durée maximale, en secondes, pendant laquelle une requête peut attendre que l'allocation de mémoire (mémoire tampon de travail) devienne disponible.  
  
> [!NOTE]  
> Une requête n'échoue pas toujours lorsque le délai d'expiration d'allocation mémoire est atteint. Une requête échoue seulement si le nombre de requêtes exécutées simultanément est trop élevé. Autrement, la requête risque d'obtenir uniquement l'allocation mémoire minimale, d'où une dégradation des performances.  
  
 *value* doit être un entier positif. La valeur par défaut de *value*, 0, utilise un calcul interne basé sur le coût de requête pour déterminer le délai maximal.  
  
 MAX_DOP =*value*       
 Spécifie le degré maximal de parallélisme (DOP) pour les demandes parallèles. *value* doit être égal à 0 ou un entier positif compris entre 1 et 255. Quand *value* a la valeur 0, le serveur choisit le degré maximal de parallélisme. Il s’agit de la valeur par défaut et recommandée.  
  
> [!NOTE]  
> La valeur réelle définie par le [!INCLUDE[ssDE](ssde-md.md)] pour MAX_DOP peut être inférieure à la valeur spécifiée. La valeur finale est déterminée par la formule min(255, *nombre d’unités centrales)* .  
  
> [!CAUTION]  
> La modification de MAX_DOP peut affecter de façon négative les performances d'un serveur. Si vous devez modifier MAX_DOP, nous recommandons qu'il soit défini sur une valeur inférieure ou égale au nombre maximal de planificateurs matériels présents dans un nœud NUMA unique. Nous vous recommandons de ne pas affecter à MAX_DOP une valeur supérieure à 8.  
  
 MAX_DOP est géré comme suit :  
  
-   MAX_DOP en tant qu'indicateur de requête est honoré tant qu'il ne dépasse pas le groupe de charge de travail MAX_DOP.  
  
-   MAX_DOP en tant qu'indicateur de requête remplace toujours l'option « max degree of parallelism » de sp_configure.  
  
-   Le groupe de charge de travail MAX_DOP remplace le degré maximal de parallélisme sp_configure.  
  
-   Si la requête est marquée comme étant en série (MAX_DOP = 1) au moment de la compilation, elle ne peut être reconvertie en requête parallèle au moment de l’exécution, indépendamment du groupe de charge de travail ou du paramètre sp_configure.  
  
 Une fois le degré maximal de parallélisme (DOP) configuré, il ne peut être diminué que sous la sollicitation de l'allocation de mémoire. La reconfiguration du groupe de charges de travail n'est pas visible lors de l'attente dans la file d'attente d'allocation de mémoire.  
  
 GROUP_MAX_REQUESTS = *value*      
 Spécifie le nombre maximal de demandes simultanées autorisées à s'exécuter dans le groupe de charges de travail. *value* doit être égal à 0 ou un entier positif. La valeur par défaut de *value*, 0, autorise un nombre illimité de demandes. Lorsque le nombre maximal de requêtes est atteint, un utilisateur de ce groupe peut se connecter, mais est placé dans un état d'attente jusqu'à ce que le nombre de requêtes simultanées soit inférieur à la valeur spécifiée.  
  
 USING { *pool_name* | "**default**" }      
 Associe le groupe de charge de travail au pool de ressources défini par l’utilisateur et identifié par *pool_name*, ce qui revient en fait à placer le groupe de charge de travail dans le pool de ressources. Si *pool_name* n’est pas fourni, ou si l’argument USING n’est pas utilisé, le groupe de charges de travail est placé dans le pool par défaut Resource Governor prédéfini.  
  
 L'option "default" doit être placée entre des guillemets doubles ("") ou des crochets ([]) lorsqu'elle est utilisée avec l'instruction ALTER WORKLOAD GROUP pour éviter tout conflit avec DEFAULT, qui est un mot réservé au système. Pour plus d'informations, consultez [Database Identifiers](../relational-databases/databases/database-identifiers.md).  
  
> [!NOTE]  
> L'option "default" respecte la casse.  
  
## <a name="remarks"></a>Notes   
 L'instruction ALTER WORKLOAD GROUP est autorisée sur le groupe par défaut.  
  
 Les modifications apportées à la configuration du groupe de charge de travail ne sont pas appliquées tant que l'instruction ALTER RESOURCE GOVERNOR RECONFIGURE n'est pas exécutée. Quand vous modifiez un paramètre qui affecte le plan, le nouveau paramètre prend effet uniquement dans les plans précédemment mis en cache après l’exécution de DBCC FREEPROCCACHE (*pool_name*), où *pool_name* est le nom d’un pool de ressources Resource Governor avec lequel le groupe de charges de travail est associé.  
  
-   Si vous affectez la valeur 1 à MAX_DOP, l’exécution de DBCC FREEPROCCACHE n’est pas obligatoire, car des plans parallèles peuvent s’exécuter en mode série. Toutefois, cela risque de ne pas être aussi efficace qu’un plan compilé en tant que plan en série.  
  
-   Si vous remplacez la valeur 1 de MAX_DOP par 0 ou une valeur supérieure à 1, l’exécution de DBCC FREEPROCCACHE n’est pas obligatoire. Toutefois, les plans en série ne pouvant pas s’exécuter en parallèle, l’effacement du cache respectif permettra aux nouveaux plans d’être compilés à l’aide du parallélisme.  
  
> [!CAUTION]  
> L’effacement des plans mis en cache à partir d’un pool de ressources associé à plusieurs groupes de charges de travail affecte tous les groupes de charges de travail contenant le pool de ressources défini par l’utilisateur identifié par *pool_name*.  
  
 Lorsque vous exécutez des instructions DDL, nous vous recommandons de connaître les états Resource Governor. Pour plus d’informations, consultez [Resource Governor](../relational-databases/resource-governor/resource-governor.md).  
  
 REQUEST_MEMORY_GRANT_PERCENT : dans [!INCLUDE[ssVersion2005](ssversion2005-md.md)], la création d’index est autorisée à utiliser une mémoire d’espace de travail supérieure à celle initialement allouée afin d’améliorer les performances. Cette gestion spéciale est prise en charge par le Gouverneur de ressources dans les versions ultérieures, toutefois, l'allocation initiale et toute allocation de mémoire supplémentaire sont limitées par les paramètres du pool de ressources et du groupe de charge de travail.  
  
 **Création d’un index sur une table partitionnée**  
  
 La mémoire consommée par la création d'index sur une table partitionnée non alignée est proportionnelle au nombre de partitions impliquées.  Si la mémoire totale requise dépasse la limite par requête (REQUEST_MAX_MEMORY_GRANT_PERCENT) imposée par le paramètre du groupe de charges de travail du Gouverneur de ressources, cette création d'index peut ne pas s'exécuter. Étant donné que le groupe de charge de travail "default" permet à une requête de dépasser la limite par requête avec la mémoire minimale requise pour démarrer la compatibilité [!INCLUDE[ssVersion2005](ssversion2005-md.md)], l'utilisateur peut être en mesure d'exécuter la même création d'index dans le groupe de charge de travail "default", si le pool de ressources "default" possède assez de mémoire totale configurée pour exécuter cette requête.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation `CONTROL SERVER`.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant indique comment modifier l'importance des demandes dans le groupe par défaut en remplaçant la valeur `MEDIUM` par `LOW`.  
  
```sql  
ALTER WORKLOAD GROUP "default"  
WITH (IMPORTANCE = LOW);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
 L’exemple suivant indique comment déplacer un groupe de charge de travail du pool dans lequel il est contenu vers le pool par défaut.  
  
```sql  
ALTER WORKLOAD GROUP adHoc  
USING [default];  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Resource Governor](../relational-databases/resource-governor/resource-governor.md)   
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/create-workload-group-transact-sql.md)   
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/drop-workload-group-transact-sql.md)   
 [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/create-resource-pool-transact-sql.md)   
 [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/alter-resource-pool-transact-sql.md)   
 [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../t-sql/statements/alter-resource-governor-transact-sql.md)  
