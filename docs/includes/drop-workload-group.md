Supprime un groupe de charges de travail du gouverneur de ressources défini par l'utilisateur existant.

:::image type="icon" source="../database-engine/configure-windows/media/topic-link.gif"::: [Conventions syntaxiques de Transact-SQL](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

## <a name="syntax"></a>Syntaxe

```syntaxsql
DROP WORKLOAD GROUP group_name
[;]
```

## <a name="arguments"></a>Arguments

*group_name* : nom d'un groupe de charges de travail défini par l'utilisateur existant.

## <a name="remarks"></a>Notes 

L'instruction DROP WORKLOAD GROUP n'est pas autorisée sur les groupes interne ou par défaut du gouverneur de ressources.

Lorsque vous exécutez des instructions DDL, nous vous recommandons de connaître les états du gouverneur de ressources. Pour plus d’informations, consultez [Resource Governor](../relational-databases/resource-governor/resource-governor.md).

Si un groupe de charges de travail contient des sessions actives, sa suppression ou son déplacement vers un pool de ressources différent échoue lorsque l'instruction ALTER RESOURCE GOVERNOR RECONFIGURE est appelée pour appliquer la modification. Pour éviter ce problème, vous pouvez suivre l'une des actions suivantes :

- Attendez la déconnexion de toutes les sessions du groupe affecté, puis réexécutez l'instruction ALTER RESOURCE GOVERNOR RECONFIGURE.

- Arrêtez explicitement les sessions du groupe affecté à l'aide de la commande KILL, puis réexécutez l'instruction ALTER RESOURCE GOVERNOR RECONFIGURE.

- Redémarrez le serveur. Au terme du processus de redémarrage, le groupe supprimé ne sera pas créé, et un groupe déplacé utilisera la nouvelle affectation de pool de ressources.

- Dans un scénario dans lequel vous publiez l'instruction DROP WORKLOAD GROUP mais décidez que vous ne souhaitez pas arrêter explicitement des sessions pour appliquer la modification, vous pouvez recréer le groupe en utilisant le nom qu'il portait avant l'émission de l'instruction DROP, puis déplacer le groupe dans le pool de ressources d'origine. Pour appliquer les modifications, exécutez l'instruction ALTER RESOURCE GOVERNOR RECONFIGURE.

## <a name="permissions"></a>Autorisations

Requiert l'autorisation CONTROL SERVER.

## <a name="examples"></a>Exemples

L'exemple suivant supprime le groupe de charges de travail nommé `adhoc`.

```
DROP WORKLOAD GROUP adhoc;
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```

## <a name="see-also"></a> Voir aussi

- [gouverneur de ressources](../relational-databases/resource-governor/resource-governor.md)
- [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/create-workload-group-transact-sql.md)  
- [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../t-sql/statements/alter-workload-group-transact-sql.md)
- [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/create-resource-pool-transact-sql.md)
- [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/alter-resource-pool-transact-sql.md)
- [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../t-sql/statements/drop-resource-pool-transact-sql.md)
- [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../t-sql/statements/alter-resource-governor-transact-sql.md)  