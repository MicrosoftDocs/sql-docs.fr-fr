---
title: PDO::prepare
description: Référence API pour la fonction PDO::prepare dans le Pilote Microsoft PDO_SQLSRV pour PHP pour SQL Server.
ms.custom: ''
ms.date: 01/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: a8b16fdc-c748-49be-acf2-a6ac7432d16b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e5e8a42aa2728ea8136c2889c18f26bafeea36ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039109"
---
# <a name="pdoprepare"></a>PDO::prepare
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Prépare une instruction en vue de son exécution.

## <a name="syntax"></a>Syntaxe

```
PDOStatement PDO::prepare ( $statement [, array(key_pair)] )
```

#### <a name="parameters"></a>Paramètres
$*instruction* : chaîne contenant l’instruction SQL.

*key_pair* : tableau contenant un nom et une valeur d’attribut. Pour plus d'informations, consultez la section Notes.

## <a name="return-value"></a>Valeur de retour
En cas de réussite, retourne un objet PDOStatement. En cas d’échec, retourne un objet PDOException, ou false en fonction de la valeur de `PDO::ATTR_ERRMODE`.

## <a name="remarks"></a>Notes
Le [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] n’évalue pas les instructions préparées avant leur exécution.

Le tableau suivant répertorie les valeurs possibles de *key_pair*.

|Clé|Description|
|-------|---------------|
|PDO::ATTR_CURSOR|Spécifie le comportement du curseur. La valeur par défaut est `PDO::CURSOR_FWDONLY`, un curseur avant sans défilement. `PDO::CURSOR_SCROLL` est un curseur avec défilement.<br /><br />Par exemple : `array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY )`.<br /><br />Quand il a la valeur `PDO::CURSOR_SCROLL`, vous pouvez ensuite utiliser `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE` pour définir le type de curseur avec défilement, qui est décrit ci-dessous.<br /><br />Pour plus d’informations sur les jeux de résultats et les curseurs dans le pilote PDO_SQLSRV, consultez [Types de curseurs &#40;pilote PDO_SQLSRV&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).|
|PDO::ATTR_EMULATE_PREPARES|Par défaut, cet attribut a la valeur false, que vous pouvez remplacer par `PDO::ATTR_EMULATE_PREPARES => true`. Pour plus d’informations et pour obtenir un exemple, consultez [Emulate Prepare](#emulate-prepare).|
|PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE|Spécifie le type de curseur avec défilement. Valide uniquement quand `PDO::ATTR_CURSOR` a la valeur `PDO::CURSOR_SCROLL`. Pour savoir quelles valeurs cet attribut peut prendre, voir ci-dessous.|
|PDO::SQLSRV_ATTR_DECIMAL_PLACES|Spécifie le nombre de décimales pour la mise en forme des valeurs monétaires extraites. Cette option fonctionne uniquement quand `PDO::SQLSRV_ATTR_FORMAT_DECIMALS` a la valeur true. Pour plus d’informations, consultez [Mise en forme des chaînes décimales et valeurs monétaires (pilote PDO_SQLSR)](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md).|
|PDO::SQLSRV_ATTR_DIRECT_QUERY|Si la valeur est True, spécifie l’exécution de requête directe. False est synonyme d’exécution d’instruction préparée. Pour plus d’informations sur `PDO::SQLSRV_ATTR_DIRECT_QUERY`, consultez [Exécution d’instruction directe et exécution d’instruction préparée dans le pilote PDO_SQLSRV](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md).|
|PDO::SQLSRV_ATTR_ENCODING|PDO::SQLSRV_ENCODING_UTF8 (par défaut)<br /><br />PDO::SQLSRV_ENCODING_SYSTEM<br /><br />PDO::SQLSRV_ENCODING_BINARY|
|PDO::SQLSRV_ATTR_FETCHES_DATETIME_TYPE|Spécifie s’il faut récupérer les types de date et d’heure en tant qu’objets [DateTime PHP](http://php.net/manual/en/class.datetime.php). Pour plus d’informations, consultez [Procédure : Récupérer des types date et heure sous forme d’objets datetime PHP à l’aide du pilote PDO_SQLSRV](../../connect/php/how-to-retrieve-datetime-objects-using-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_FETCHES_NUMERIC_TYPE|Gère les extractions numériques à partir de colonnes avec des types numériques SQL. Pour plus d’informations, consultez [PDO::setAttribute](../../connect/php/pdo-setattribute.md).|
|PDO::SQLSRV_ATTR_FORMAT_DECIMALS|Spécifie s’il faut ajouter des zéros au début des chaînes décimales si nécessaire. Si elle est définie, cette option active l’option `PDO::SQLSRV_ATTR_DECIMAL_PLACES` pour mettre en forme les types monétaires. Pour plus d’informations, consultez [Mise en forme des chaînes décimales et valeurs monétaires (pilote PDO_SQLSR)](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md).| 
|PDO::SQLSRV_ATTR_QUERY_TIMEOUT|Pour plus d’informations, consultez [PDO::setAttribute](../../connect/php/pdo-setattribute.md).|

En cas d’utilisation de `PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL`, vous pouvez utiliser `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE` pour spécifier le type de curseur. Par exemple, passez le tableau suivant à PDO::prepare pour définir un curseur dynamique :
```
array(PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL, PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE => PDO::SQLSRV_CURSOR_DYNAMIC));
```
Le tableau suivant montre les valeurs possibles pour `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE`. Pour plus d’informations sur les curseurs avec défilement, consultez [Types de curseurs &#40;pilote PDO_SQLSRV&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).

|Valeur|Description|
|---------|---------------|
|PDO::SQLSRV_CURSOR_BUFFERED|Crée un curseur statique côté client (mis en mémoire tampon), qui met en mémoire tampon le jeu de résultats sur l’ordinateur client.|
|PDO::SQLSRV_CURSOR_DYNAMIC|Crée un curseur dynamique (sans mise en mémoire tampon) côté serveur, qui vous permet d’accéder aux lignes dans n’importe quel ordre et reflète les modifications dans la base de données.|
|PDO::SQLSRV_CURSOR_KEYSET|Crée un curseur de jeu de clés côté serveur. Un curseur de jeu de clés ne met pas à jour le nombre de lignes si une ligne est supprimée de la table (une ligne supprimée est retournée sans valeur).|
|PDO::SQLSRV_CURSOR_STATIC|Crée un curseur statique côté serveur, qui vous permet d’accéder aux lignes dans n’importe quel ordre mais ne reflète pas les modifications dans la base de données.<br /><br />`PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL` implique `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE => PDO::SQLSRV_CURSOR_STATIC`.|

Vous pouvez fermer un objet PDOStatement en appelant `unset` :
```
unset($stmt);
```

## <a name="forward-only-example"></a>Exemple avant uniquement
Cet exemple montre comment utiliser PDO::prepare avec des marqueurs de paramètres et un curseur avant uniquement.

```
<?php
$database = "Test";
$server = "(local)";
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");

$col1 = 'a';
$col2 = 'b';

$query = "insert into Table1(col1, col2) values(?, ?)";
$stmt = $conn->prepare( $query, array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY, PDO::SQLSRV_ATTR_QUERY_TIMEOUT => 1  ) );
$stmt->execute( array( $col1, $col2 ) );
print $stmt->rowCount();
echo "\n";

$query = "insert into Table1(col1, col2) values(:col1, :col2)";
$stmt = $conn->prepare( $query, array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY, PDO::SQLSRV_ATTR_QUERY_TIMEOUT => 1  ) );
$stmt->execute( array( ':col1' => $col1, ':col2' => $col2 ) );
print $stmt->rowCount();

unset($stmt);
?>
```

## <a name="static-cursor-example"></a>Exemple de curseur statique
Cet exemple montre comment utiliser PDO::prepare avec un curseur statique côté serveur. Pour obtenir un exemple de curseur côté client, consultez [Types de curseurs &#40;pilote PDO_SQLSRV&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).

```
<?php
$database = "AdventureWorks";
$server = "(local)";
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");

$query = "select * from Person.ContactType";
$stmt = $conn->prepare( $query, array(PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL));
$stmt->execute();

echo "\n";

while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){
   print "$row[Name]\n";
}
echo "\n..\n";

$row = $stmt->fetch( PDO::FETCH_BOTH, PDO::FETCH_ORI_FIRST );
print_r($row);

$row = $stmt->fetch( PDO::FETCH_ASSOC, PDO::FETCH_ORI_REL, 1 );
print "$row[Name]\n";

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_NEXT );
print "$row[1]\n";

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_PRIOR );
print "$row[1]..\n";

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_ABS, 0 );
print_r($row);

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_LAST );
print_r($row);
?>
```

## <a name="targeted-example"></a>Exemple ciblé
Les deux extraits de code suivants montrent comment utiliser PDO::prepare avec des données ciblées pour les colonnes CHAR/VARCHAR. Comme l’encodage par défaut pour PDO::prepare est UTF-8, l’utilisateur peut pour éviter les conversions implicites à l’aide de l’option `PDO::SQLSRV_ENCODING_SYSTEM`.

**Option 1 :**
```
$options = array(PDO::SQLSRV_ATTR_ENCODING => PDO::SQLSRV_ENCODING_SYSTEM);
$statement = $pdo->prepare(
  'SELECT *
   FROM myTable
   WHERE myVarcharColumn = :myVarcharValue',
  $options
);

$statement->bindValue(':myVarcharValue', 'my data', PDO::PARAM_STR);
```

**Option 2 :**
```
$statement = $pdo->prepare(
  'SELECT *
   FROM myTable
   WHERE myVarcharColumn = :myVarcharValue'
);
$p = 'my data';
$statement->bindParam(':myVarcharValue', $p, PDO::PARAM_STR, 0, PDO::SQLSRV_ENCODING_SYSTEM);
```

<a name="emulate-prepare" />

## <a name="prepare-example"></a>Exemple de préparation

Cet exemple montre comment utiliser PDO::prepare avec `PDO::ATTR_EMULATE_PREPARES` défini sur true.

```
<?php
$serverName = "yourservername";
$username = "yourusername";
$password = "yourpassword";
$database = "tempdb";
$conn = new PDO("sqlsrv:server = $serverName; Database = $database", $username, $password);

$pdo_options = array();
$pdo_options[PDO::ATTR_EMULATE_PREPARES] = true;
$pdo_options[PDO::SQLSRV_ATTR_ENCODING] = PDO::SQLSRV_ENCODING_UTF8;

$stmt = $conn->prepare("CREATE TABLE TEST([id] [int] IDENTITY(1,1) NOT NULL,
                                          [name] nvarchar(max))",
                                          $pdo_options);
$stmt->execute();

$prefix = '가각';
$name = '가각ácasa';
$name2 = '가각sample2';

$stmt = $conn->prepare("INSERT INTO TEST(name) VALUES(:p0)", $pdo_options);
$stmt->execute(['p0' => $name]);
unset($stmt);

$stmt = $conn->prepare("SELECT * FROM TEST WHERE NAME LIKE :p0", $pdo_options);
$stmt->execute(['p0' => "$prefix%"]);
foreach ($stmt as $row) {
    echo "\n" . 'FOUND: ' . $row['name'];
}

unset($stmt);
unset($conn);
?>
```

Le pilote PDO_SQLSRV remplace en interne tous les espaces réservés par les paramètres qui sont liés par [PDOStatement::bindParam()](../../connect/php/pdostatement-bindparam.md). Ainsi, une chaîne de requête SQL sans espace réservé est envoyée au serveur. Prenez l’exemple suivant :

```
$statement = $PDO->prepare("INSERT into Customers (CustomerName, ContactName) VALUES (:cus_name, :con_name)");
$statement->bindParam(:cus_name, "Cardinal");
$statement->bindParam(:con_name, "Tom B. Erichsen");
$statement->execute();
```

Avec `PDO::ATTR_EMULATE_PREPARES` défini sur false (le cas par défaut), les données envoyées à la base de données sont :

```
"INSERT into Customers (CustomerName, ContactName) VALUES (:cus_name, :con_name)"
Information on :cus_name parameter
Information on :con_name parameter
```

Le serveur exécutera la requête en utilisant sa fonctionnalité de requête paramétrable pour lier les paramètres. En revanche, avec `PDO::ATTR_EMULATE_PREPARES` défini sur true, la requête envoyée au serveur est :

```
"INSERT into Customers (CustomerName, ContactName) VALUES ('Cardinal', 'Tom B. Erichsen')"
```

L’affectation de la valeur true à `PDO::ATTR_EMULATE_PREPARES` peut contourner certaines restrictions dans SQL Server. Par exemple, SQL Server ne prend pas en charge les paramètres nommés ou positionnels dans certaines clauses Transact-SQL. De plus, SQL Server ne peut pas lier plus de 2 100 paramètres.

> [!NOTE]
> Avec emulate prepares défini sur true, la sécurité des requêtes paramétrables n’est pas activée. Par conséquent, votre application doit garantir que les données liées aux paramètres ne contiennent pas de code Transact-SQL malveillant.

### <a name="encoding"></a>Encodage

Si l’utilisateur souhaite lier des paramètres avec des encodages différents (par exemple UTF-8 ou binaire), l’utilisateur doit indiquer clairement l’encodage dans le script PHP.

Le pilote PDO_SQLSRV vérifie d’abord l’encodage spécifié dans `PDO::bindParam()` (par exemple `$statement->bindParam(:cus_name, "Cardinal", PDO::PARAM_STR, 10, PDO::SQLSRV_ENCODING_UTF8)`).

S’il est introuvable, le pilote vérifie si un encodage est défini dans `PDO::prepare()` ou `PDOStatement::setAttribute()`. Autrement, le pilote utilise l’encodage spécifié dans `PDO::__construct()` ou `PDO::setAttribute()`.

En outre, à compter de la version 5.8.0, lors de l’utilisation de PDO::prepare avec `PDO::ATTR_EMULATE_PREPARES` défini sur la valeur true, l’utilisateur peut se servir des [types de chaînes étendues introduits dans PHP 7.2](https://wiki.php.net/rfc/extended-string-types-for-pdo) pour s’assurer que le préfixe `N` est utilisé. Les extraits de code ci-dessous présentent différentes alternatives.

> [!NOTE]
> Par défaut, emulate prepares est définie sur false, auquel cas les constantes de la chaîne PDO étendue seront ignorées.

**Utilisation de l’option de pilote PDO::SQLSRV_ENCODING_UTF8 lors de la liaison**

```
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true);
$stmt = $conn->prepare($sql, $options);
$stmt->bindParam(':value', $p, PDO::PARAM_STR, 0, PDO::SQLSRV_ENCODING_UTF8);
$stmt->execute();
```

**Utilisation de l’attribut PDO::SQLSRV_ATTR_ENCODING**

```
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true, PDO::SQLSRV_ATTR_ENCODING => PDO::SQLSRV_ENCODING_UTF8);
$stmt = $conn->prepare($sql, $options);
$stmt->execute([':value' => $p]);
```

**Utilisation de la constante PDO PDO::PARAM_STR_NATL**
```
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true);
$stmt = $conn->prepare($sql, $options);
$stmt->bindParam(':value', $p, PDO::PARAM_STR | PDO::PARAM_STR_NATL);
$stmt->execute();
```

**Définition du type de paramètre de chaîne par défaut PDO::PARAM_STR_NATL**
```
$conn->setAttribute(PDO::ATTR_DEFAULT_STR_PARAM, PDO::PARAM_STR_NATL);
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true);
$stmt = $conn->prepare($sql, $options);
$stmt->execute([':value' => $p]);
```

### <a name="limitations"></a>Limites

Comme vous pouvez le voir, la liaison est effectuée en interne par le pilote. Une requête valide est envoyée au serveur pour l’exécution sans aucun paramètre. Comparé au cas ordinaire, il existe certaines limitations quand la fonctionnalité de requête paramétrable n’est pas utilisée.

- Cela ne fonctionne pas pour les paramètres qui sont liés en tant que `PDO::PARAM_INPUT_OUTPUT`.
    - Quand l’utilisateur spécifie `PDO::PARAM_INPUT_OUTPUT` dans `PDO::bindParam()`, une exception PDO est levée.
- Cela ne fonctionne pas pour les paramètres qui sont liés en tant que paramètres de sortie.
    - Quand l’utilisateur crée une instruction préparée avec des espaces réservés qui sont destinés à des paramètres de sortie (autrement dit, un signe égal juste après un espace réservé, tel que `SELECT ? = COUNT(*) FROM Table1`), une exception PDO est levée.
    - Quand une instruction préparée appelle une procédure stockée avec un espace réservé comme argument pour un paramètre de sortie, aucune exception n’est levée, car le pilote ne peut pas détecter le paramètre de sortie. Toutefois, la variable fournie par l’utilisateur pour le paramètre de sortie restera inchangée.
- Les espaces réservés en double pour un paramètre encodé en binaire ne fonctionneront pas.

## <a name="see-also"></a>Voir aussi
[PDO, classe](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)

