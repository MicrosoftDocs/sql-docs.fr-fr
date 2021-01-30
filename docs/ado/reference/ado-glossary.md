---
description: Glossaire ADO
title: Glossaire ADO | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: reference
helpviewer_keywords:
- ADO, glossary
ms.assetid: b0478836-4123-4357-969a-c5784fc28be5
author: rothja
ms.author: jroth
ms.openlocfilehash: 36ca4b14058b2728e60856f3c848452ad922107d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166233"
---
# <a name="ado-glossary"></a>Glossaire ADO
Cette rubrique définit les termes pertinents pour ADO.  
  
## <a name="a"></a>A  
 URL absolue  
 URL qualifiée complète qui spécifie l’emplacement d’une ressource qui réside sur Internet ou sur un intranet. Voir aussi *URL* et *URL relative*.  
  
 ActiveX (contrôle)  
 Composant COM in-process à inscription automatique qui a souvent un élément visuel au moment du design ou au moment de l’exécution. Les contrôles ActiveX peuvent également communiquer avec un conteneur de documents actifs, tel que Microsoft Internet Explorer.  
  
 ADISAPI (Advanced Data Internet Server Application Programming Interface)  
 DLL ISAPI qui fournit l’analyse, le contrôle d’automatisation, le marshaling d’objets Recordset et l’empaquetage MIME. Le composant ADISAPI fonctionne via l’API fournie par Internet Information Services (IIS). Voir aussi *ISAPI*.  
  
 fonction d'agrégation  
 Dans une requête, fonction telle que COUNT, AVG ou STDEV qui calcule une valeur à l’aide de toutes les lignes d’une colonne d’une table. En écrivant des expressions et en programmation, vous pouvez utiliser les fonctions d’agrégation SQL (y compris les trois mentionnées ci-dessus) et les fonctions d’agrégation de domaine pour déterminer diverses statistiques.  
  
 alias  
 Un autre nom que vous attribuez à une colonne ou une expression dans une instruction SQL SELECT, souvent plus rapide ou plus explicite. Par exemple, BobSales est l’alias dans l’instruction SELECT suivante : « SELECT WR-Sales As BobSales from SalesDB ». Un alias peut être utilisé pour affecter dynamiquement des colonnes aux liaisons de contrôle sur l’objet DataControl.  
  
 thread cloisonné  
 Modèle de thread COM dans lequel tous les appels à un objet se produisent sur un thread. Dans le thread cloisonné, COM synchronise et marshale les appels. Voir aussi *COMmddefcom*.  
  
 opération asynchrone  
 Opération qui retourne le contrôle au programme appelant sans attendre la fin de l’opération. Avant la fin de l’opération, l’exécution du code se poursuit. Voir aussi *opération synchrone*.  
  
## <a name="b"></a>B  
 entrée de liaison  
 Mappage entre un champ d’une table et une variable. Dans les extensions de Visual C++ ADO, les champs du **Recordset** sont mappés à des variables C/C++.  
  
 binaire  
 Valeur numérique destinée à une comparaison de valeurs bit par bit avec d’autres valeurs numériques, généralement pour marquer des options dans des valeurs de paramètre ou de retour. En général, cette comparaison est effectuée à l’aide d’opérateurs logiques au niveau du bit, tels que **and** et **or** dans Visual Basic, **&** et **&#124;** en C++.  
  
 Par exemple, les valeurs ADO **FieldAttributeEnum** peuvent être utilisées comme masques de masques pour déterminer les attributs d’un champ. Supposons que vous souhaitiez déterminer si un champ était modifiable. Vous pouvez le tester avec l’expression suivante dans Visual Basic :`Field.Attributes AND adFldUpdatable`  
  
 Si le résultat est TRUE, le champ peut être mis à jour.  
  
 signet  
 Marqueur qui identifie de façon unique une ligne dans un ensemble de lignes afin qu’un utilisateur puisse y accéder rapidement.  
  
 objet métier  
 Objet qui exécute un ensemble défini d’opérations, telles que la validation de données ou la logique de règle d’entreprise. Les objets métier résident généralement sur la couche intermédiaire.  
  
 règle d’entreprise  
 La combinaison des modifications de validation, des vérifications d’ouverture de session, des recherches dans la base de données, des stratégies et des transformations algorithmiques qui constituent le mode d’activité d’une entreprise. Également appelée *logique métier*.  
  
## <a name="c"></a>C  
 expression calculée  
 Expression qui n’est pas constante, mais dont la valeur dépend d’autres valeurs. Pour être évaluée, une expression calculée doit obtenir et calculer des valeurs à partir d’autres sources, généralement dans d’autres champs ou lignes.  
  
 chapitre  
 Référence à une plage de lignes d’une source de données. Dans ADO, un chapitre est généralement une référence à un autre **jeu d’enregistrements**.  
  
 Les colonnes de chapitre permettent de définir une relation *parent-enfant* où le *parent* est le **Recordset** contenant la colonne de chapitre et l' *enfant* est le **Recordset** représenté par le chapitre.  
  
 alias-Chapter  
 Alias qui fait référence à la colonne ajoutée au parent.  
  
 jeu de caractères  
 Mappage d’un jeu de caractères à leurs valeurs numériques. Par exemple, Unicode est un jeu de caractères 16 bits qui peut encoder tous les caractères connus et utilisé comme norme de codage de caractères dans le monde entier.  
  
 child  
 Partie dépendante d’une relation hiérarchique. Un enfant est un nœud dans une structure hiérarchique qui possède un autre nœud au-dessus de lui (plus proche de la racine). Voir aussi l' *alias d’enfant*, *relation parent-enfant*, *parent*.  
  
 alias-enfant  
 Alias qui fait référence à l’enfant. Voir aussi *alias*, *enfant*.  
  
 CLSID (identificateur de classe)  
 Identificateur unique universel (UUID) qui identifie un composant COM. Chaque composant COM a son CLSID dans le Registre Windows afin qu’il puisse être chargé par d’autres applications. Voir aussi *ProgID*, *com*.  
  
 couche cliente  
 Couche logique d’un système distribué qui présente généralement les données et traite les entrées de l’utilisateur, parfois appelées « *frontal*». En règle générale, la couche cliente demande des données à partir d’un serveur en fonction de l’entrée, puis met en forme et affiche le résultat. Voir aussi *niveau intermédiaire*, *niveau de source de données*, *application distribuée*.  
  
 COM (Component Object Model)  
 Norme binaire qui permet aux objets d’interagir dans un environnement réseau, quel que soit le langage dans lequel ils ont été développés ou sur quels ordinateurs ils résident. Les technologies basées sur COM incluent les contrôles ActiveX, l’automatisation et la liaison et l’incorporation d’objets (OLE). COM permet à un objet d’exposer ses fonctionnalités à d’autres composants et d’héberger des applications. Il définit à la fois comment l’objet s’expose et comment cette exposition fonctionne sur les processus et sur les réseaux. COM définit également le cycle de vie de l’objet.  
  
 Composant COM  
 Fichier binaire, tel que. dll,. ocx et certains fichiers. exe, qui prend en charge la norme COM pour fournir des objets. Un fichier de ce type contient du code pour une ou plusieurs fabriques de classes, classes COM, mécanismes d’entrée du Registre, code de chargement, etc.  
  
 opérateur de comparaison  
 Opérateur qui compare deux expressions et retourne une valeur booléenne.  
  
 Paramètre de critère qui peut être exprimé sous la forme « > » (supérieur à), « \<" (less than), "=" (equal), "> = » (supérieur ou égal à), « <= » (inférieur ou égal à), « <> » (différent de) ou « like » (critères spéciaux).  
  
 component  
 Objet qui encapsule les données et le code, et fournit un ensemble bien spécifié de services disponibles publiquement.  
  
 fichier composé  
 Implémentation du stockage structuré COM pour les fichiers. Un fichier composé stocke des objets distincts dans un seul fichier structuré composé de deux éléments principaux : les objets de stockage et les objets de flux. Ensemble, ils fonctionnent comme un système de fichiers dans un fichier.  
  
 Un certain nombre de fichiers individuels liés dans un fichier physique. Chaque fichier individuel dans un fichier composé est accessible comme s’il s’agissait d’un fichier physique unique.  
  
 constant  
 Valeur numérique ou de chaîne qui ne change pas. Les énumérations ADO nommées (constantes énumérées) peuvent être utilisées dans votre code au lieu des valeurs réelles, par exemple, **adUseClient** est une constante dont la valeur est 3. (Const adUseClient = 3). Voir aussi *énumération*.  
  
 cursor  
 Élément de base de données qui contrôle la navigation entre les enregistrements, la mise à jour des données et la visibilité des modifications apportées à la base de données par d’autres utilisateurs.  
  
## <a name="d"></a>D  
 liaison de données  
 Processus d’association des objets ou des contrôles d’une application à une source de données. Un contrôle associé à une source de données est appelé un *contrôle lié aux données*.  
  
 Le contenu d’un contrôle lié aux données est associé aux valeurs d’une base de données. Par exemple, un contrôle de grille qui est lié à un objet **Recordset** peut être mis à jour lorsque les lignes de l’ensemble d' **enregistrements** sont mises à jour. Lorsque de nouvelles valeurs sont récupérées par le **Recordset**, les nouvelles valeurs sont affichées dans la grille.  
  
 fournisseur de données  
 Logiciel qui expose des données à une application ADO, directement ou via un fournisseur de services. Voir aussi fournisseur de services.  
  
 mise en forme des données  
 Technique qui utilise une syntaxe formalisée (appelée **langage de forme**) pour définir un objet **Recordset** spécialisé (appelé *jeu d’enregistrements mis* en forme) qui ne contient pas uniquement des données, mais également des références à d’autres objets **Recordset** et/ou à des valeurs calculées en fonction de ces autres objets **Recordset** .  
  
 niveau de source de données  
 Couche logique d’un système distribué qui représente un ordinateur exécutant un SGBD, tel qu’une base de données SQL Server. Voir aussi *niveau client*, *couche intermédiaire*, *application distribuée*.  
  
 DCOM  
 Protocole câble qui permet aux composants COM de communiquer directement entre eux sur un réseau. Voir aussi *com*, *composant*.  
  
 DDL (langage de définition de données)  
 Les instructions dans SQL qui définissent, par opposition à manipuler, les données. Le schéma d’une base de données est créé ou modifié avec DDL. Par exemple, **Create table**, **Create index**, **Grant** et **Revoke** sont des instructions DDL SQL.  
  
 flux par défaut  
 Un flux de texte ou binaire (représenté par un objet de **flux** ) qui est associé à des objets **Record** ou **Recordset** lors de l’utilisation de certains fournisseurs de OLE DB, tels que le fournisseur Microsoft OLE DB pour la publication Internet. Le flux par défaut contient généralement le contenu d’un fichier tel que le code HTML de la racine d’un site Web.  
  
 application distribuée  
 Programme écrit de sorte que le traitement puisse être réparti entre plusieurs ordinateurs sur un réseau. En règle générale, une application distribuée est divisée *en couches de* présentation, de logique métier et de magasin de données. Voir aussi niveau client, niveau intermédiaire, niveau source de données.  
  
 Recordset déconnecté  
 Objet **Recordset** dans un cache client qui ne dispose plus d’une connexion active au serveur. Si la source de données d’origine doit être réutilisée pour une raison quelconque, telle que la mise à jour des données, la connexion doit être rétablie. Toutefois, les collections, les propriétés et les méthodes d’un **jeu d’enregistrements** déconnecté sont toujours accessibles.  
  
 DML (langage de manipulation de données)  
 Les instructions dans SQL qui manipulent, au lieu de définir, des données. Les valeurs d’une base de données sont sélectionnées et modifiées à l’aide de DML. Par exemple, **Insert**, **Update**, **Delete** et **Select** sont des instructions SQL DML.  
  
 fournisseur de source de document  
 Classe spéciale de fournisseurs qui gèrent des dossiers et des documents. Lorsqu’un document est représenté par un objet **enregistrement** ou qu’un dossier de documents est représenté par un objet **Recordset** , le fournisseur de source du document remplit ces objets avec un ensemble unique de champs qui décrivent les caractéristiques du document, au lieu du document proprement dit. Voir aussi enregistrement de ressource.  
  
 DSN (nom de la source de données)  
 Collection d’informations utilisées pour connecter votre application à une base de données ODBC particulière. Le gestionnaire de pilotes ODBC utilise ces informations pour créer une connexion à la base de données. Un DSN peut être stocké dans un fichier (DSN de fichier) ou dans le Registre Windows (un nom de source de fichiers d’ordinateur).  
  
 propriété dynamique  
 Propriété spécifique à un fournisseur de données ou au service de curseur. La collection Properties d’un objet est remplie automatiquement avec ces **Propriétés** (« dynamiquement »). Un objet n’a pas de propriétés dynamiques tant qu’il n’est pas connecté à une source de données par le biais d’un fournisseur de données particulier. Voir aussi fournisseur de données, curseur.  
  
## <a name="e"></a>E  
 Énumération  
 Liste de constantes nommées. Les valeurs énumérées ne doivent pas être uniques. Toutefois, le nom de chaque valeur doit être unique dans l’étendue où l’énumération est définie. Dans ADO, les énumérations sont utilisées pour les paramètres numériques et les valeurs de retour, afin d’ajouter une signification au code ADO et de protéger le développeur des valeurs numériques (qui peuvent changer d’une version à l’autres). Par exemple, pour ouvrir un **jeu d’enregistrements** statique, utilisez la valeur énumérée **adOpenStatic** : `Recordset.Open ,,adOpenStatic`  
  
 Également appelée *constante énumérée*. Voir aussi *constante*.  
  
 événement  
 Action reconnue par un objet, pour laquelle vous pouvez écrire du code pour répondre. Les événements peuvent être générés par l’exécution de la commande, la saisie semi-automatique des transactions, la navigation dans les jeux d’enregistrements et les mises à jour des données, entre autres actions. Voir aussi *Gestionnaire d’événements*.  
  
 gestionnaire d'événements  
 Un gestionnaire d’événements est le code exécuté lorsqu’un événement se produit. Voir aussi événement.  
  
## <a name="h"></a>H  
 gestionnaire  
 Routine qui gère une condition ou une opération commune et relativement simple, telle que la récupération d’erreur ou la gestion des données.  
  
 jeu d’enregistrements hiérarchique  
 **Jeu d’enregistrements** qui contient un autre **Recordset**. Voir aussi mise en forme des données, chapitre.  
  
 Pour plus d’informations, consultez [accès aux lignes d’un jeu d’enregistrements hiérarchique](../guide/data/accessing-rows-in-a-hierarchical-recordset.md).  
  
 hiérarchie  
 En général, une hiérarchie est une structure classée avec un niveau supérieur et des niveaux subordonnés. Dans ADO, les **recordsets** hiérarchiques sont utilisés pour représenter la relation parent-enfant entre un enregistrement et un chapitre. En outre, dans ADO, les objets d' **enregistrement** et de **flux** peuvent être utilisés pour accéder à des structures d’arborescences hiérarchiques, telles qu’un dossier et des documents. ADO MD comprend également des objets **Hierarchy** pour représenter une relation entre les niveaux d’une dimension dans un cube OLAP. Voir aussi recordsets hiérarchiques, relation parent-enfant, chapitre, arbre.  
  
## <a name="i-l"></a>I-L  
 ISAPI (Internet Server Application Programming Interface)  
 Ensemble de fonctions pour les serveurs Internet, tels qu’un serveur Windows NT® Server/Windows 2000 exécutant Microsoft® Internet Information Services (IIS).  
  
 Clé  
 Colonne ou colonnes dans une table qui identifie de façon unique une ligne ; souvent utilisée pour indexer une table.  
  
## <a name="m"></a>M  
 marshaling  
 Processus d’empaquetage, d’envoi et de désencapsulation des paramètres de méthode d’interface à travers les limites de thread ou de processus.  
  
 niveau intermédiaire  
 Couche logique dans un système distribué entre une interface utilisateur ou un client Web et la base de données. C’est généralement là que les objets métier sont instanciés. La couche intermédiaire est une collection de règles et de fonctions d’entreprise qui génèrent et opèrent lors de la réception d’informations. Pour ce faire, ils utilisent des règles métier, qui peuvent changer fréquemment et sont donc encapsulées dans des composants qui sont physiquement séparés de la logique d’application elle-même. Également connu sous le nom de *niveau du serveur d’applications*. Voir aussi application distribuée, couche client, couche de source de données.  
  
 MIME (Multipurpose Internet Mail extension)  
 Protocole Internet développé à l’origine pour permettre l’échange de messages électroniques avec du contenu riche sur des environnements de réseau, d’ordinateur et de messagerie hétérogènes. En pratique, MIME a également été adopté et étendu par les applications non-messagerie.  
  
 MIME est une norme qui permet de publier et de lire des données binaires sur Internet. L’en-tête d’un fichier contenant des données binaires contient le type MIME des données ; Cela indique aux programmes clients (navigateurs Web et packages de courrier électronique, par exemple) qu’ils doivent gérer les données d’une manière différente de celle qui gère le texte. Par exemple, l’en-tête d’un document Web contenant un graphique JPEG contient le type MIME spécifique au format de fichier JPEG. Cela permet à un navigateur d’afficher le fichier avec sa visionneuse JPEG, s’il en existe un.  
  
## <a name="n-o"></a>N-O  
 nœud  
 Élément dans une arborescence hiérarchique. Un nœud peut être la racine ou l’enfant d’un autre nœud. Un nœud peut également être le parent de plusieurs enfants. Voir aussi hiérarchie, arborescence, racine, enfant, parent.  
  
 variable objet  
 Variable qui contient une référence à un objet. Par exemple, `objCustomObject` est une variable qui pointe vers un objet de type CustomObject :`Set objCustomObject = CreateObject(adodb.Recordset)`  
  
 ODBC (Open Database Connectivity)  
 Interface de langage de programmation standard utilisée pour se connecter à une variété de sources de données. Ce nom est généralement accessible via le panneau de configuration, où les noms de sources de données (DSN) peuvent être affectés pour utiliser des pilotes ODBC spécifiques.  
  
 OLE DB  
 Ensemble d’interfaces qui exposent des données provenant de diverses sources à l’aide de COM. Les interfaces OLE DB fournissent aux applications un accès uniforme aux données stockées dans diverses sources d’informations. Ces interfaces prennent en charge la quantité de fonctionnalités SGBD appropriées à la source de données, ce qui permet au service informatique de partager ses données. Voir aussi COM.  
  
 verrouillage optimiste  
 Type de verrouillage dans lequel la page de données contenant un ou plusieurs enregistrements, y compris l’enregistrement en cours de modification, n’est pas disponible pour les autres utilisateurs uniquement lorsque l’enregistrement est mis à jour par la méthode de **mise à** jour, mais est disponible avant et après l’appel à **Update**.  
  
 Le verrouillage optimiste est utilisé lorsque l’objet **Recordset** est ouvert avec le paramètre **LockType** ou la propriété défini sur **adLockOptimistic** ou **adLockBatchOptimistic**. Voir aussi verrouillage pessimiste.  
  
 valeur ordinale  
 Emplacement numérique d’un élément dans une commande. Dans une collection ADO, la valeur ordinale du premier élément est égale à zéro (0). L’élément suivant est un (1), et ainsi de suite.  
  
## <a name="p"></a>P  
 commande paramétrée  
 Une requête ou une commande qui vous permet de définir des valeurs de paramètre avant l’exécution de la commande. Par exemple, une chaîne SQL peut être paramétrable en incorporant des marqueurs de paramètres dans la chaîne SQL (désignée par le caractère «  ? »). L’application spécifie ensuite des valeurs pour chaque paramètre et exécute la commande.  
  
 parent  
 Côté contrôle d’une relation hiérarchique. Dans une structure hiérarchique, un parent possède un ou plusieurs nœuds enfants situés directement sous celui-ci dans la hiérarchie. Voir aussi parent-alias, relation parent-enfant, enfant.  
  
 parent-alias  
 Alias qui fait référence au parent. Voir aussi alias, parent.  
  
 relation parent-enfant  
 Relation dans une structure hiérarchique dans laquelle le parent est un niveau supérieur et directement associé à un ou plusieurs enfants. Un enfant est un niveau inférieur et doit avoir un parent. Voir aussi parent, enfant.  
  
 verrouillage pessimiste  
 Type de verrouillage dans lequel la page contenant un ou plusieurs enregistrements, y compris l’enregistrement en cours de modification, n’est pas disponible pour les autres utilisateurs afin de garantir qu’une mise à jour sera effectuée. Le comportement de verrouillage pessimiste est défini par le fournisseur OLE DB. En règle générale, les enregistrements sont verrouillés lors de la modification et restent indisponibles jusqu’à ce que la méthode de **mise à jour** soit terminée.  
  
 Le verrouillage pessimiste est activé lorsque l’objet **Recordset** est ouvert avec le paramètre **LockType** ou la propriété valeur de **adLockPessimistic**. Voir aussi verrouillage optimiste.  
  
 regroupement  
 Optimisation des performances basée sur l’utilisation de collections de ressources pré-allouées, telles que des objets ou des connexions de base de données. Il est plus efficace de dessiner une ressource existante à partir du pool que de créer une nouvelle ressource.  
  
 ProgID (identificateur programmatique)  
 Nom unique mappé au registre Windows par une application COM. L’identificateur de programme (ProgID) d’une connexion ADO est «ADODB. Connexion». Voir aussi CLSID, COM.  
  
 proxy  
 Objet spécifique à l’interface qui fournit le marshaling de paramètres et la communication requis pour qu’un client appelle un objet d’application qui s’exécute dans un environnement d’exécution différent, par exemple sur un thread différent ou dans un autre processus. Le proxy est situé auprès du client et communique avec un stub correspondant qui se trouve avec l’objet d’application appelé. Voir aussi stub.  
  
## <a name="r"></a>R  
 URL relative  
 URL partiellement qualifiée qui spécifie une ressource sur Internet ou un intranet dont l’emplacement est relatif à un point de départ spécifié par une URL absolue ou un objet de connexion ADO équivalent. En effet, les URL absolues et relatives consitute une URL complète. Voir aussi URL et URL absolue.  
  
 source de données distante  
 Une source de données qui existe sur un autre ordinateur, plutôt que sur le système local (où l’application cliente s’exécute).  
  
 enregistrement de ressource  
 Enregistrement d’un fournisseur de source de document qui contient des champs pour la définition et la description d’un dossier ou d’un document. Le document lui-même n’est pas contenu dans l’enregistrement de ressource, mais il est généralement accessible par le flux par défaut ou un champ dans l’enregistrement de ressource contenant une URL. Voir aussi fournisseur de source de document, flux par défaut, URL.  
  
 ensemble de lignes  
 Ensemble de lignes d’une source de données, qui ont toutes le même schéma de champ. Un ensemble de lignes peut représenter tout ou partie des champs d’une table. Un ensemble de lignes peut également représenter une table virtuelle, créée par une requête ou une jointure de deux ou plusieurs tables. Dans ADO, les ensembles de lignes sont représentés par des objets **Recordset** .  
  
## <a name="s"></a>S  
 Étendue  
 Plage de référence pour un objet ou une variable, ou une plage d’enregistrements dans une vue ou une table. Par exemple, les variables locales peuvent uniquement être référencées dans la procédure dans laquelle elles ont été définies. Les variables publiques sont accessibles depuis n’importe où dans l’application. Les objets, tels que la base de données active, se trouvent dans l’étendue s’ils se trouvent dans le chemin de recherche défini. Les plages d’enregistrements peuvent être spécifiées avec une clause Scope dans de nombreuses commandes.  
  
 fournisseur de services  
 Logiciel qui encapsule un service en générant et en consommant des données, en augmentant les fonctionnalités de vos applications ADO. Il s’agit d’un fournisseur qui n’expose pas directement les données, mais fournit un service, tel que le traitement des requêtes. Le fournisseur de services peut traiter les données fournies par un fournisseur de données. Voir aussi fournisseur de données.  
  
 Recordset mis en forme  
 **Jeu d’enregistrements** dont les colonnes ont été spécifiquement définies pour contenir des données non seulement, mais également des références (nommées chapitres) à d’autres objets **Recordset** et/ou des valeurs calculées en fonction d’autres objets **Recordset** .  
  
 frère  
 Deux nœuds ou plus dans une structure hiérarchique qui se trouvent au même niveau dans la hiérarchie. Le nœud racine d’une hiérarchie n’a pas de frères.  
  
 procédure stockée  
 Collection précompilée de code, telle que les instructions SQL et les instructions facultatives de contrôle de Flow, stockées sous un nom et traitées comme une unité. Les procédures stockées sont stockées dans une base de données ; ils peuvent être exécutés avec un appel d’une application et autoriser les variables déclarées par l’utilisateur, l’exécution conditionnelle et d’autres fonctionnalités de programmation puissantes.  
  
 talon  
 Objet spécifique à l’interface qui fournit le marshaling des paramètres et la communication requis pour qu’un objet application reçoive des appels d’un client qui s’exécute dans un environnement d’exécution différent, par exemple sur un thread différent ou dans un autre processus. Le stub se trouve avec l’objet application et communique avec un proxy correspondant qui est localisé avec le client qui l’appelle. Voir aussi proxy.  
  
 sous-nœud  
 Consultez enfant.  
  
 opération synchrone  
 Opération lancée par du code qui se termine avant que l’opération suivante ne démarre. Voir aussi opération asynchrone.  
  
## <a name="t-z"></a>T-Z  
 Arborescence  
 Structure représentant une relation hiérarchique entre des éléments (nœuds). Il y a un nœud au niveau supérieur d’une arborescence (la racine). Sous la racine, il peut y avoir plusieurs enfants. Chaque enfant peut à son tour être le parent d’autres enfants, créant ainsi une branche comme une arborescence. Un dossier contenant des documents et d’autres dossiers est un exemple classique de structure arborescente. Voir aussi hiérarchie, nœud, racine, enfant, parent.  
  
 Serveur web  
 Ordinateur qui fournit des services Web et des pages aux utilisateurs intranet et Internet.