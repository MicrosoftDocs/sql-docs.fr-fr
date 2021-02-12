---
description: Paramètres du projet (conversion) (DB2ToSQL)
title: Paramètres du projet (conversion) (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 538c93cf-c5bb-43d5-b758-186d9fb00c19
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 5d01a893a2284a85fd4540d532500b7915a97326
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072343"
---
# <a name="project-settings-conversion-db2tosql"></a>Paramètres du projet (conversion) (DB2ToSQL)
La page conversion de la boîte de dialogue **paramètres du projet** contient des paramètres qui personnalisent la manière dont SSMA convertit la syntaxe DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe.  
  
Le volet conversion est disponible dans les boîtes de dialogue **paramètres du projet** et **paramètres du projet par défaut** :  
  
-   Pour spécifier les paramètres de tous les projets SSMA, dans le menu **Outils** , cliquez sur **paramètres du projet par défaut**, sélectionnez le type de projet de migration pour lequel les paramètres doivent être affichés ou modifiés dans la liste déroulante de la **version cible** de la migration, puis cliquez sur **général** en bas du volet gauche, puis sur **conversion**.  
  
-   Pour spécifier les paramètres du projet actif, dans le menu **Outils** , cliquez sur **paramètres du projet**, puis sur **général** en bas du volet gauche, et enfin sur **conversion**.  
  
## <a name="conversion-messages"></a>Messages de conversion  
  
### <a name="generate-messages-about-issues-applied"></a>Générer des messages sur les problèmes appliqués  
Spécifie si SSMA génère des messages d’information au cours de la conversion, les affiche dans le volet de sortie et les ajoute au code converti.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste :** º  
  
**Mode complet :** º  
  
## <a name="miscellaneous-options"></a>Options diverses  
  
### <a name="cast-rownum-expressions-as-integers"></a>Convertir des expressions ROWNUM en entiers  
Lorsque SSMA convertit des expressions ROWNUM, il convertit l’expression en une clause TOP, suivie de l’expression. L’exemple suivant montre ROWNUM dans une instruction DELETE DB2 :  
  
`DELETE FROM Table1`  
  
`WHERE ROWNUM < expression and Field1 >= 2`  
  
L’exemple suivant illustre le résultat obtenu [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
`DELETE TOP (expression-1)`  
  
`FROM Table1`  
  
`WHERE Field1>=2`  
  
Le TOP requiert que l’expression des clauses supérieures corresponde à un entier. Si l’entier est négatif, l’instruction génère une erreur.  
  
-   Si vous sélectionnez **Oui**, SSMA effectue un cast de l’expression en tant qu’entier.  
  
-   Si vous sélectionnez **non**, SSMA marque toutes les expressions non entières comme une erreur dans le code converti.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/mode complet :** º  
  
**Mode optimiste :** Oui  
  
### <a name="default-schema-mapping"></a>Mappage de schéma par défaut  
Ce paramètre spécifie la manière dont les schémas DB2 sont mappés à des schémas de SQL Server. Deux options sont disponibles dans ce paramètre :  
  
1.  **Schéma dans la base de données :** Dans ce mode, le schéma DB2 « Sch1 » est mappé par défaut à « dbo » SQL Server schéma dans SQL Server base de données « Sch1 ».  
  
2.  **Schéma vers schéma :** Dans ce mode, le schéma DB2 « Sch1 » est mappé par défaut à « Sch1 » SQL Server schéma dans la base de données SQL Server par défaut fournie dans la boîte de dialogue de connexion.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** Schéma à base de données  
  
### <a name="conversion-ways-of-merge-statement"></a>Méthodes de conversion de l’instruction MERGE  
  
-   Si vous sélectionnez **à l’aide de l’instruction INSERT, Update et Delete**, SSMA convertit l’instruction de fusion en instructions INSERT, Update et DELETE.  
  
-   Si vous sélectionnez **à l’aide de l’instruction MERGE**, SSMA convertit l’instruction de fusion en instruction MERGE dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!WARNING]  
> Cette option de paramètre de projet est disponible uniquement dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** Utilisation de l’instruction MERGE  
  
### <a name="convert-calls-to-subprograms-that-use-default-arguments"></a>Convertir les appels aux sous-programmes qui utilisent des arguments par défaut  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les fonctions ne prennent pas en charge l’omission de paramètres dans l’appel de fonction. En outre, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les fonctions et les procédures ne prennent pas en charge les expressions comme valeurs de paramètre par défaut.  
  
-   Si vous sélectionnez **Oui** et qu’un appel de fonction omet des paramètres, SSMA insère le mot clé **default** dans la fonction et appelle à la position correcte. Ensuite, l’appel est marqué avec un avertissement.  
  
-   Si vous sélectionnez **non**, SSMA marquera les appels de fonction comme des erreurs.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** Oui  
  
### <a name="convert-count-function-to-count_big"></a>Convertit la fonction COUNT en COUNT_BIG  
Si vos fonctions COUNT sont susceptibles de retourner des valeurs supérieures à 2 147 483 647, ce qui correspond à 2<sup>31</sup>-1, vous devez convertir les fonctions en COUNT_BIG.  
  
-   Si vous sélectionnez **Oui**, SSMA convertit toutes les utilisations de COUNT en COUNT_BIG.  
  
-   Si vous sélectionnez **non**, les fonctions sont conservées en tant que nombre. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] renverra une erreur si la fonction retourne une valeur supérieure à 2<sup>31</sup>-1.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/mode complet :** Oui  
  
**Mode optimiste :** º  
  
### <a name="convert-forall-statement-to-while-statement"></a>Convertir l’instruction FORALL en instruction WHILe  
Définit la façon dont SSMA traitera les boucles FORALL sur les éléments de collection PL/SQL.  
  
-   Si vous sélectionnez **Oui**, SSMA crée une boucle while dans laquelle les éléments de la collection sont récupérés un par un.  
  
-   Si vous sélectionnez **non**, SSMA génère un ensemble de lignes à partir de la collection à l’aide de la méthode nodes () et l’utilise comme table unique. Cette solution est plus efficace, mais rend le code de sortie moins lisible.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste :** º  
  
**Mode complet :** Oui  
  
### <a name="convert-foreign-keys-with-set-null-referential-action-on-column-that-is-not-null"></a>Convertir les clés étrangères avec l’action référentielle SET NULL sur la colonne qui n’est pas NULL  
DB2 autorise la création de contraintes de clé étrangère, où une action de définition de valeur NULL n’a pas pu être effectuée, car les valeurs NULL ne sont pas autorisées dans la colonne référencée. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’autorise pas cette configuration de clé étrangère.  
  
-   Si vous sélectionnez **Oui**, SSMA générera des actions d’intégrité référentielle comme dans DB2, mais vous devrez apporter des modifications manuelles avant de charger la contrainte sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Par exemple, vous pouvez choisir aucune ACTION au lieu de définir la valeur NULL.  
  
-   Si vous sélectionnez **non**, la contrainte est marquée comme une erreur.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** º  
  
### <a name="convert-function-calls-to-procedure-calls"></a>Convertir les appels de fonction en appels de procédure  
Certaines fonctions DB2 sont définies en tant que transactions autonomes ou contiennent des instructions qui ne sont pas valides dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dans ce cas, SSMA crée une procédure et une fonction qui est un wrapper pour la procédure. La fonction convertie appelle la procédure d’implémentation.  
  
SSMA peut convertir les appels à la fonction wrapper en appels à la procédure. Cela crée un code plus lisible et peut améliorer les performances. Toutefois, le contexte ne l’autorise pas toujours ; par exemple, vous ne pouvez pas remplacer un appel de fonction dans la liste de sélection par un appel de procédure. SSMA offre plusieurs options pour couvrir les cas courants :  
  
-   Si vous sélectionnez **toujours**, SSMA tente de convertir les appels de fonction wrapper en appels de procédure. Si le contexte actuel n’autorise pas cette conversion, un message d’erreur est généré. De cette façon, aucun appel de fonction n’est laissé dans le code généré.  
  
-   Si vous sélectionnez dans la **mesure du possible**, SSMA effectue un déplacement vers les appels de procédure uniquement si la fonction a des paramètres de sortie. Lorsque le déplacement n’est pas possible, l’attribut de sortie du paramètre est supprimé. Dans tous les autres cas, SSMA quitte les appels de fonction.  
  
-   Si vous sélectionnez **Never**, SSMA laisse tous les appels de fonction en tant qu’appels de fonction. Ce choix peut parfois être inacceptable pour des raisons de performances.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** Lorsque cela est possible  
  
### <a name="convert-lock-table-statements"></a>Convertir les instructions de TABLE LOCK  
SSMA peut convertir de nombreuses instructions LOCK TABLE en indicateurs de table. SSMA ne peut pas convertir les instructions de TABLE de VERROUs qui contiennent des clauses de PARTITION, de sous-PARTITION, @dblink et NOWAIT et marque ces instructions avec des messages d’erreur de conversion.  
  
-   Si vous sélectionnez **Oui**, SSMA convertira les instructions Lock table prises en charge en indicateurs de table.  
  
-   Si vous sélectionnez **non**, SSMA marque toutes les instructions Lock table avec des messages d’erreur de conversion.  
  
Le tableau suivant montre comment SSMA convertit les modes de verrouillage DB2 :  
  
|Mode de verrouillage DB2|Indicateur de table SQL Server|  
|-|-|  
|PARTAGE DE LIGNES|ROWLOCK, HOLDLOCK|  
|LIGNE EXCLUSIVE|ROWLOCK, XLOCK, HOLDLOCK|  
|PARTAGE DE MISE À JOUR = PARTAGE DE LIGNES|ROWLOCK, HOLDLOCK|  
|PARTAGER|TABLOCK, HOLDLOCK|  
|PARTAGER UNE LIGNE EXCLUSIVE|TABLOCK, XLOCK, HOLDLOCK|  
|Or|TABLOCKX, HOLDLOCK|  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** Oui  
  
### <a name="convert-open-for-statements-for-ref-cursor-out-parameters"></a>Convertir les instructions OPEN-FOR pour les paramètres de sortie de REF CURSOR  
Dans DB2, l’instruction OPEN-FOR peut être utilisée pour retourner un jeu de résultats au paramètre OUT d’un sous-programme de type REF CURSOR. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , les procédures stockées retournent directement les résultats des instructions SELECT.  
  
SSMA peut convertir de nombreuses instructions OPEN-FOR en instructions SELECT.  
  
-   Si vous sélectionnez **Oui**, SSMA convertit l’instruction Open-for en une instruction SELECT, qui retourne le jeu de résultats au client.  
  
-   Si vous sélectionnez **non**, SSMA génère un message d’erreur dans le code converti et dans le volet de sortie.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** Oui  
  
### <a name="convert-record-as-a-list-of-separates-variables"></a>Convertir l’enregistrement en une liste de variables distinctes  
SSMA peut convertir les enregistrements DB2 en variables distinctes et en variables XML avec une structure spécifique.  
  
-   Si vous sélectionnez **Oui**, SSMA convertit l’enregistrement en une liste de variables distinctes, si possible.  
  
-   Si vous sélectionnez **non**, SSMA convertit l’enregistrement en variables XML avec une structure spécifique.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** Oui  
  
### <a name="convert-substr-function-calls-to-substring-function-calls"></a>Convertir les appels de fonction SUBSTR en appels de fonction de sous-chaîne  
SSMA peut convertir les appels de fonction de DB2 SUBSTR en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appels de fonction de **sous-chaîne** , selon le nombre de paramètres. Si SSMA ne peut pas convertir un appel de fonction SUBSTR ou si le nombre de paramètres n’est pas pris en charge, SSMA convertit l’appel de fonction SUBSTR en un appel de fonction SSMA personnalisé.  
  
-   Si vous sélectionnez **Oui**, SSMA convertit les appels de fonction substr qui utilisent trois paramètres en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **sous-chaîne**. D’autres fonctions SUBSTR seront converties pour appeler la fonction SSMA personnalisée.  
  
-   Si vous sélectionnez **non**, SSMA convertit l’appel de fonction substr en appel de fonction SSMA personnalisé.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste :** Oui  
  
**Mode complet :** º  
  
### <a name="convert-subtypes"></a>Convertir les sous-types  
SSMA peut convertir les sous-types PL/SQL de deux manières :  
  
-   Si vous sélectionnez **Oui**, SSMA crée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] un type défini par l’utilisateur à partir d’un sous-type et l’utilise pour chaque variable de ce sous-type.  
  
-   Si vous sélectionnez **non**, SSMA remplace toutes les déclarations sources du sous-type par le type sous-jacent et convertit le résultat comme d’habitude. Dans ce cas, aucun autre type n’est créé dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** º  
  
### <a name="convert-synonyms"></a>Convertir les synonymes  
Les synonymes des objets DB2 suivants peuvent être migrés vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   Tables et tables d’objets  
  
-   Vues et vues d’objets  
  
-   Procédures stockées et fonctions  
  
-   Vues matérialisées  
  
Les synonymes des objets DB2 suivants peuvent être remplacés par des références directes aux objets :  
  
-   Séquences  
  
-   .  
  
-   Objets de schéma de classe Java  
  
-   Types d'objets définis par l'utilisateur  
  
D’autres synonymes ne peuvent pas être migrés. SSMA génère des messages d’erreur pour le synonyme et toutes les références qui utilisent le synonyme.  
  
-   Si vous sélectionnez **Oui**, SSMA créera [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des synonymes et des références d’objets directs conformément aux listes précédentes.  
  
-   Si vous sélectionnez **non**, SSMA crée des références d’objet directes pour tous les synonymes répertoriés ici.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** Oui  
  
### <a name="convert-to_chardate-format"></a>Convertir TO_CHAR (date, format)  
SSMA peut convertir la TO_CHAR DB2 (date, format) en procédures à partir de la base de données sysdb.  
  
-   Si vous sélectionnez **à l’aide de TO_CHAR_DATE fonction**, SSMA convertit le to_char (date, format) en TO_CHAR_DATE fonction à l’aide de la langue anglaise pour la conversion.  
  
-   Si vous sélectionnez **à l’aide de TO_CHAR_DATE_LS fonction (nls Care)**, SSMA convertit le to_char (date, format) en TO_CHAR_DATE_LS fonction à l’aide de la langue de session pour la conversion  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste :** Utilisation de TO_CHAR_DATE fonction  
  
**Mode complet :** Utilisation de TO_CHAR_DATE_LS fonction (soins NLS)  
  
### <a name="convert-transaction-processing-statements"></a>Convertir les instructions de traitement des transactions  
SSMA peut convertir les instructions de traitement des transactions DB2 :  
  
-   Si vous sélectionnez **Oui**, SSMA convertit les instructions de traitement de transactions DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instructions.  
  
-   Si vous sélectionnez **non**, SSMA marque les instructions de traitement de transaction comme des erreurs de conversion.  
  
> [!NOTE]  
> DB2 ouvre les transactions de manière implicite. Pour émuler ce comportement sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez ajouter BEGIN TRANSACTION instructions manuellement à l’emplacement où vous souhaitez que vos transactions démarrent. Vous pouvez également exécuter la commande SET IMPLICIT_TRANSACTIONS ON au début de votre session. SSMA ajoute des IMPLICIT_TRANSACTIONS définies automatiquement lors de la conversion de sous-routines avec des transactions autonomes.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** Oui  
  
### <a name="emulate-db2-null-behavior-in-order-by-clauses"></a>Émuler le comportement de DB2 null dans les clauses ORDER BY  
Les valeurs NULL sont classées différemment dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et DB2 :  
  
-   Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , les valeurs NULL sont les valeurs les plus basses dans une liste ordonnée. Dans une liste croissante, les valeurs NULL s’affichent en premier.  
  
-   Dans DB2, les valeurs NULL sont les valeurs les plus élevées dans une liste ordonnée. Par défaut, les valeurs NULL apparaissent en dernier dans une liste d’ordre croissant.  
  
-   DB2 a des valeurs NULL en premier et des clauses NULL, ce qui vous permet de modifier la façon dont DB2 classe les valeurs NULL.  
  
SSMA peut émuler le comportement de l’ORDONNANCEment DB2 en vérifiant les valeurs NULL. Ensuite, il trie d’abord les valeurs NULL dans l’ordre spécifié, puis les trie par d’autres valeurs.  
  
-   Si vous sélectionnez **Oui**, SSMA convertit l’instruction DB2 d’une manière qui émule le comportement de l’outil de classement DB2.  
  
-   Si vous sélectionnez **non**, SSMA ignore les règles DB2 et génère un message d’erreur quand il rencontre les premières clauses null et les dernières valeurs NULL.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste :** º  
  
**Mode complet :** Oui  
  
### <a name="emulate-row-count-exceptions-in-select"></a>Émuler les exceptions de nombre de lignes dans SELECT  
Si une instruction SELECT avec une clause INTO ne retourne pas de lignes, DB2 lève une exception NO_DATA_FOUND. Si l’instruction retourne deux lignes ou plus, l’exception TOO_MANY_ROWS est levée. L’instruction convertie dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne lève pas d’exception si le nombre de lignes est différent d’un.  
  
-   Si vous sélectionnez **Oui**, SSMA ajoute l’appel à la procédure sysdb db_error_exact_one_row_check après chaque instruction SELECT. Cette procédure émule les exceptions NO_DATA_FOUND et TOO_MANY_ROWS. Il s’agit de la valeur par défaut, qui permet de reproduire le comportement DB2 le plus près possible. Vous devez toujours choisir **Oui** si le code source possède des gestionnaires d’exceptions qui traitent ces erreurs. Notez que si l’instruction SELECT apparaît à l’intérieur d’une fonction définie par l’utilisateur, ce module est converti en une procédure stockée, car l’exécution de procédures stockées et le déclenchement d’exceptions ne sont pas compatibles avec le contexte de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fonction.  
  
-   Si vous sélectionnez **non**, aucune exception n’est générée. Cela peut être utile lorsque SSMA convertit une fonction définie par l’utilisateur et que vous souhaitez qu’elle reste une fonction dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** Oui  
  
### <a name="generate-error-for-dbms_sqlparse"></a>Génère une erreur pour DBMS_SQL. ANALYS  
  
-   Si vous sélectionnez **erreur**, SSMA génère une erreur lors de la DBMS_SQL de la conversion. Analys.  
  
-   Si vous sélectionnez **Avertissement**, SSMA génère un avertissement au DBMS_SQL de la conversion. Analys.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** Erreurs  
  
### <a name="generate-rowid-column"></a>Générer la colonne ROWID  
Lorsque SSMA crée des tables dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , il peut créer une colonne ROWID. Lorsque les données sont migrées, chaque ligne obtient une nouvelle valeur UNIQUEIDENTIFIER générée par la fonction NEWID ().  
  
-   Si vous sélectionnez **Oui**, la colonne ROWID est créée sur toutes les tables et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère des GUID au fur et à mesure que vous insérez des valeurs. Choisissez toujours **Oui** si vous envisagez d’utiliser le testeur SSMA.  
  
-   Si vous sélectionnez **non**, les colonnes ROWID ne sont pas ajoutées aux tables.  
  
-   **Ajouter la colonne ROWID pour les tables avec déclencheurs** ajoutez ROWID pour les tables contenant des déclencheurs.  
  
> [!WARNING]  
> Paramètre par défaut dans le cas de SQL Server 2005, SQL Server 2008 et SQL Server 2012 et 2014 est **Ajouter la colonne ROWID pour les tables avec des déclencheurs**.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste :** Ajouter une colonne ROWID pour les tables avec des déclencheurs  
  
**Mode complet :** Oui  
  
### <a name="generate-unique-index-on-rowid-column"></a>Générer un index unique sur la colonne ROWID  
Spécifie si SSMA génère ou non une colonne d’index unique sur la colonne de ROWID générée. Si l’option est définie sur « Oui », l’index unique est généré et, s’il est défini sur « non », l’index unique n’est pas généré sur la colonne ROWID.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** Oui  
  
### <a name="local-modules-conversion"></a>Conversion de modules locaux  
Définit le type de sous-programme de DB2 imbriqué (déclaré dans une procédure stockée autonome ou une fonction).  
  
-   Si vous sélectionnez **inline**, les appels de sous-programme imbriqués seront remplacés par son corps.  
  
-   Si vous sélectionnez des **procédures stockées**, le sous-programme imbriqué sera converti en SQL Server procédure stockée, et ses appels seront remplacés lors de cet appel de procédure.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** Inline  
  
### <a name="use-isnull-in-string-concatenation"></a>Utilisation de ISNULL dans une concaténation de chaînes  
DB2 et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retournent des résultats différents lorsque les concaténations de chaînes incluent des valeurs NULL. DB2 traite la valeur NULL comme un jeu de caractères vide. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une valeur NULL.  
  
-   Si vous sélectionnez **Oui**, SSMA remplace le caractère de concaténation DB2 (| |) par le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] caractère de concaténation (+). SSMA vérifie également les expressions des deux côtés de la concaténation pour les valeurs NULL.  
  
-   Si vous sélectionnez **non**, SSMA remplace les caractères de concaténation, mais ne vérifie pas la valeur null.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
### <a name="use-isnull-in-replace-function-calls"></a>Utilisation de ISNULL dans les appels de fonction Replace  
L’instruction ISNULL est utilisée dans les appels de fonction Replace pour émuler le comportement DB2. Les options suivantes sont disponibles pour ce paramètre :  
  
-   YES  
  
-   Non  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste :** º  
  
**Mode complet :** Oui  
  
### <a name="use-isnull-in-concat-function-calls"></a>Utiliser ISNULL dans les appels de fonction CONCAt  
L’instruction ISNULL est utilisée dans les appels de fonction CONCAt pour émuler le comportement DB2. Les options suivantes sont disponibles pour ce paramètre :  
  
-   YES  
  
-   Non  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste :** º  
  
**Mode complet :** Oui  
  
### <a name="use-native-convert-function-when-possible"></a>Utiliser la fonction native Convert lorsque cela est possible  
  
-   Si vous sélectionnez **Oui**, SSMA convertit le to_char (date, format) en fonction Convert Native lorsque cela est possible.  
  
-   Si vous sélectionnez **non**, SSMA convertit le to_char (date, format) en TO_CHAR_DATE ou TO_CHAR_DATE_LS (il est défini par les options « convertir les to_char (date, format) »).  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste :** Oui  
  
**Mode complet :** º  
  
### <a name="use-selectfor-xml-when-converting-selectinto-for-record-variable"></a>Utilisez SELECT... FOR XML lors de la conversion de SELECT... INTO pour la variable d’enregistrement  
Spécifie s’il faut générer un jeu de résultats XML lorsque vous sélectionnez dans une variable d’enregistrement.  
  
-   Si vous sélectionnez **Oui**, l’instruction Select retourne Xml.  
  
-   Si vous sélectionnez **non**, l’instruction Select retourne un jeu de résultats.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** º  
  
## <a name="returning-clause-conversion"></a>Returning, conversion de clause  
  
### <a name="convert-returning-clause-in-delete-statement-to-output"></a>Conversion de la clause Returning dans l’instruction DELETE en sortie  
DB2 fournit une clause Returning pour obtenir immédiatement les valeurs supprimées. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit cette fonctionnalité avec la clause OUTPUT.  
  
-   Si vous sélectionnez **Oui**, SSMA convertira les CLAUSEs renvoyées dans les instructions DELETE en clauses de sortie. Étant donné que les déclencheurs sur une table peuvent modifier des valeurs, la valeur retournée peut être différente dans la mesure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où elle se trouvait dans DB2.  
  
-   Si vous sélectionnez **non**, SSMA génère une instruction SELECT avant les instructions DELETE pour récupérer les valeurs retournées.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** Oui  
  
### <a name="convert-returning-clause-in-insert-statement-to-output"></a>Conversion de la clause Returning dans l’instruction INSERT en sortie  
DB2 fournit une clause Returning pour obtenir immédiatement des valeurs insérées. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit cette fonctionnalité avec la clause OUTPUT.  
  
-   Si vous sélectionnez **Oui**, SSMA convertit une clause Returning dans une instruction INSERT en sortie. Étant donné que les déclencheurs sur une table peuvent modifier des valeurs, la valeur retournée peut être différente dans la mesure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où elle se trouvait dans DB2.  
  
-   Si vous sélectionnez **non**, SSMA émule la fonctionnalité DB2 en insérant, puis en sélectionnant des valeurs dans une table de référence.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** Oui  
  
### <a name="convert-returning-clause-in-update-statement-to-output"></a>Conversion de la clause Returning dans l’instruction UPDATE en sortie  
DB2 fournit une clause Returning pour obtenir immédiatement les valeurs mises à jour. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit cette fonctionnalité avec la clause OUTPUT.  
  
-   Si vous sélectionnez **Oui**, SSMA convertira les CLAUSEs renvoyées dans les instructions Update en clauses de sortie. Étant donné que les déclencheurs sur une table peuvent modifier des valeurs, la valeur retournée peut être différente dans la mesure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où elle se trouvait dans DB2.  
  
-   Si vous sélectionnez **non**, SSMA génère des instructions SELECT après les instructions Update pour récupérer les valeurs retournées.  
  
Lorsque vous sélectionnez un mode de conversion dans la zone **mode** , SSMA applique le paramètre suivant :  
  
**Mode par défaut/optimiste/mode complet :** Oui  
  
## <a name="sequence-conversion"></a>Conversion de séquence  
  
### <a name="convert-sequence-generator"></a>Convertir le générateur de séquence  
Dans DB2, vous pouvez utiliser une séquence pour générer des identificateurs uniques.  
  
SSMA peut convertir les séquences vers les éléments suivants.  
  
-   À l’aide de SQL Server Générateur de séquence (cette option est disponible uniquement lors de la conversion en SQL Server 2012 et SQL Server 2014).  
  
-   À l’aide du générateur de séquence SSMA.  
  
-   Utilisation de l’identité de colonne.  
  
L’option par défaut lors de la conversion en SQL Server 2012 ou SQL Server 2014 est d’utiliser le générateur de séquences SQL Server. Toutefois, SQL Server 2012 et SQL Server 2014 ne prennent pas en charge l’obtention de la valeur de séquence actuelle (par exemple, celle de la méthode CURRVAL de la séquence DB2). Consultez le site du blog de l’équipe SSMA pour obtenir de l’aide sur la migration de la méthode CURRVAL de la séquence DB2.  
  
SSMA fournit également une option permettant de convertir une séquence DB2 en émulateur de séquence SSMA. Il s’agit de l’option par défaut lors de la conversion en SQL Server avant 2012  
  
Enfin, vous pouvez également convertir une séquence assignée à une colonne de table en SQL Server valeurs d’identité. Vous devez spécifier le mappage entre les séquences et une colonne d’identité sous l’onglet de **table** DB2.  
  
### <a name="convert-currval-outside-triggers"></a>Convertir les CURRVAL en dehors des déclencheurs  
Visible uniquement lorsque le générateur de séquence Convert a la valeur à **l’aide de l’identité de colonne**. Étant donné que les séquences DB2 sont des objets distincts des tables, de nombreuses tables qui utilisent des séquences utilisent un déclencheur pour générer et insérer une nouvelle valeur de séquence. SSMA commente ces instructions ou les marque comme des erreurs lorsque les commentaires génèrent des erreurs.  
  
-   Si vous sélectionnez **Oui**, SSMA marque toutes les références aux déclencheurs externes sur la séquence convertie CURRVAL avec un avertissement.  
  
-   Si vous sélectionnez **non**, SSMA marque toutes les références aux déclencheurs externes sur la séquence convertie CURRVAL avec une erreur.  
  
## <a name="see-also"></a>Voir aussi  
[Référence de l’interface utilisateur &#40;DB2ToSQL&#41;](../../ssma/db2/user-interface-reference-db2tosql.md)  
  
