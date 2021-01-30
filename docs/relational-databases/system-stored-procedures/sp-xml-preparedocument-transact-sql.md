---
description: sp_xml_preparedocument (Transact-SQL)
title: sp_xml_preparedocument (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_xml_preparedocument_TSQL
- sp_xml_preparedocument
dev_langs:
- TSQL
helpviewer_keywords:
- sp_xml_preparedocument
ms.assetid: 95f41cff-c52a-4182-8ac6-bf49369d214c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8fc47f280e0c6f32e41cd7a0f7db34ff22ac98f1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201783"
---
# <a name="sp_xml_preparedocument-transact-sql"></a>sp_xml_preparedocument (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Lit le texte XML fourni en entrée, l'analyse à l'aide de l'analyseur MSXML (Msxmlsql.dll), puis fournit le document analysé dans un état exploitable. Ce document analysé est une représentation arborescente des différents nœuds du document XML : éléments, attributs, textes, commentaires, etc.  
  
 **sp_xml_preparedocument** retourne un handle qui peut être utilisé pour accéder à la représentation interne nouvellement créée du document XML. Ce handle est valide pour la durée de la session ou jusqu’à ce que le handle soit invalidé en exécutant **sp_xml_removedocument**.  
  
> [!NOTE]  
>  Un document analysé est stocké dans le cache interne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'analyseur MSXML utilise un huitième de la mémoire totale disponible pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour éviter de manquer de mémoire, exécutez **sp_xml_removedocument** pour libérer de la mémoire.  
  
> [!NOTE]  
>  À des fins de compatibilité descendante, **sp_xml_preparedocument** réduit les caractères CR (Char (13)) et LF (Char (10)) dans les attributs même si ces caractères sont codé.  
  
> [!NOTE]  
>  L’analyseur XML appelé par **sp_xml_preparedocument** peut analyser des DTD internes et des déclarations d’entité. Étant donné que les DTD et les déclarations d’entité construites à des fins malveillantes peuvent être utilisées pour effectuer une attaque par déni de service, nous recommandons vivement aux utilisateurs de ne pas passer directement des documents XML provenant de sources non approuvées à **sp_xml_preparedocument**.  
>   
>  Pour atténuer les attaques de développement d’entités récursives, **sp_xml_preparedocument** limite à 10 000 le nombre d’entités qui peuvent être développées sous une seule entité au niveau supérieur d’un document. Cette limite ne s'applique pas aux entités numériques ou de caractères. Elle permet de stocker des documents contenant de nombreuses références d'entités, mais elle empêche une entité d'être étendue de manière récursive dans une chaîne supérieure à 10 000 extensions.  
  
> [!NOTE]  
>  **sp_xml_preparedocument** limite le nombre d’éléments qui peuvent être ouverts à un moment donné à 256.  

 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_xml_preparedocument  
hdoc   
OUTPUT  
[ , xmltext ]  
[ , xpath_namespaces ]   
```  
  
## <a name="arguments"></a>Arguments  
 *hdoc*  
 Descripteur du document nouvellement créé. *hdoc* est un entier.  
  
 [ *XmlText* ]  
 Document XML initial. L'analyseur MSXML analyse ce document XML. *XmlText* est un paramètre de texte **: char**, **nchar**, **varchar**, **nvarchar**, **Text**, **ntext** ou **XML**. Lorsque la valeur est NULL (valeur par défaut), une représentation interne d'un document XML vide est créée.  
  
> [!NOTE]  
>  **sp_xml_preparedocument** peut uniquement traiter du texte ou du XML non typé. Si une valeur d'instance à utiliser en entrée est déjà du XML typé, commencez par la convertir en une nouvelle instance XML non typée ou en une chaîne, puis passez cette valeur en entrée. Pour plus d’informations, consultez [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
 [ *xpath_namespaces* ]  
 Spécifie les déclarations d'espaces de noms utilisées dans les expressions XPath de ligne et de colonne sous OPENXML. *xpath_namespaces* est un paramètre de texte **: char**, **nchar**, **varchar**, **nvarchar**, **Text**, **ntext** ou **XML**.  
  
 La valeur par défaut est **\<root xmlns:mp="urn:schemas-microsoft-com:xml-metaprop">** . *xpath_namespaces* fournit les URI d’espace de noms pour les préfixes utilisés dans les expressions XPath dans OpenXml au moyen d’un document XML bien formé. *xpath_namespaces* déclare le préfixe qui doit être utilisé pour faire référence à l’espace de noms **urn : schemas-microsoft-com : XML-reprop**; fournit des métadonnées sur les éléments XML analysés. Bien que cette technique vous permette de redéfinir le préfixe de l'espace de noms des métapropriétés, cet espace de noms n'est pas perdu. Le préfixe **MP** est toujours valide pour **urn : schemas-microsoft-com : XML-reprop** même si *xpath_namespaces* ne contient pas de déclaration de ce type.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou >0 (échec)  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-preparing-an-internal-representation-for-a-well-formed-xml-document"></a>R. Préparation d'une représentation interne pour un document XML correctement mis en forme  
 L'exemple suivant retourne un descripteur pour la représentation interne nouvellement créée du document XML fourni en entrée. L'appel à `sp_xml_preparedocument` contient un mappage de préfixes d'espaces de noms par défaut.  
  
```  
DECLARE @hdoc int;  
DECLARE @doc varchar(1000);  
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5" OrderDate="1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3" OrderDate="1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>';  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @hdoc OUTPUT, @doc;  
-- Remove the internal representation.  
exec sp_xml_removedocument @hdoc;  
```  
  
### <a name="b-preparing-an-internal-representation-for-a-well-formed-xml-document-with-a-dtd"></a>B. Préparation d'une représentation interne pour un document XML correctement mis en forme avec un schéma DTD  
 L'exemple suivant retourne un descripteur pour la représentation interne nouvellement créée du document XML fourni en entrée. La procédure stockée valide le document chargé sur le schéma DTD inclus dans le document. L'appel à `sp_xml_preparedocument` contient un mappage de préfixes d'espaces de noms par défaut.  
  
```  
DECLARE @hdoc int;  
DECLARE @doc varchar(2000);  
SET @doc = '  
<?xml version="1.0" encoding="UTF-8" ?>   
<!DOCTYPE root   
[<!ELEMENT root (Customers)*>  
<!ELEMENT Customers EMPTY>  
<!ATTLIST Customers CustomerID CDATA #IMPLIED ContactName CDATA #IMPLIED>]>  
<root>  
<Customers CustomerID="ALFKI" ContactName="Maria Anders"/>  
</root>';  
  
EXEC sp_xml_preparedocument @hdoc OUTPUT, @doc;  
```  
  
### <a name="c-specifying-a-namespace-uri"></a>C. Spécification d'un URI d'espace de noms  
 L'exemple suivant retourne un descripteur pour la représentation interne nouvellement créée du document XML fourni en entrée. L’appel à `sp_xml_preparedocument` conserve le `mp` préfixe du mappage d’espace de noms de métapropriété et ajoute le `xyz` préfixe de mappage à l’espace de noms `urn:MyNamespace` .  
  
```  
DECLARE @hdoc int;  
DECLARE @doc varchar(1000);  
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5"   
           OrderDate="1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3"   
           OrderDate="1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @hdoc OUTPUT, @doc, '<ROOT xmlns:xyz="urn:MyNamespace"/>';  
```  
  
## <a name="see-also"></a>Voir aussi  
 <br>[Procédures stockées XML (Transact-SQL)](../../relational-databases/system-stored-procedures/xml-stored-procedures-transact-sql.md)
 <br>[Procédures stockées système (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
 <br>[OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md)
 <br>[sys.dm_exec_xml_handles (Transact-SQL)](../system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)
 <br>[sp_xml_removedocument (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-xml-removedocument-transact-sql.md)
  
  
