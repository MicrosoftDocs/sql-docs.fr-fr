---
title: Utiliser mise à jour dans un exemple d’application ASP (SQLXML)
description: Affichez un exemple d’utilisation d’un mise à jour SQLXML dans une application ASP (Active Server Pages).
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- ASP applications [SQLXML]
- Active Server Pages
- updategrams [SQLXML], ASP applications
ms.assetid: 10eff799-4c39-4b52-8b38-7ea6f68454a8
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f3cb1db0f03b911f695b85c6efed8a43240d8258
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97459955"
---
# <a name="using-an-updategram-in-a-sample-asp-application-sqlxml-40"></a>Utilisation d'un code de mise à jour (updategram) dans un exemple d'application ASP (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Cette application ASP (Active Server Pages) vous permet de mettre à jour des informations client dans la table Person.Contact dans l'exemple de base de données AdventureWorks dans Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. L'application effectue les actions suivantes :  
  
-   Elle invite l'utilisateur à entrer un ID de contact.  
  
-   Elle utilise cette valeur d'ID de client pour exécuter un modèle afin d'extraire des informations de contact de la table Person.Contact.  
  
-   Elle affiche ces informations en utilisant un formulaire HTML.  
  
 L'utilisateur peut alors mettre à jour les informations de contact mais pas l'ID de contact (car ContactID est la clé primaire). Après que l'utilisateur a soumis les informations, un code de mise à jour (updategram) est exécuté et tous les paramètres du formulaire sont passés au code de mise à jour.  
  
 Le modèle suivant est le premier modèle (GetContact.xml). Enregistrez ce modèle dans le répertoire associé au nom virtuel du type de **modèle** .  
  
```  
<root xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
   <sql:header>  
      <sql:param name="cid"></sql:param>  
   </sql:header>  
   <sql:query>  
      SELECT  *   
      FROM    Person.Contact  
      WHERE   ContactID=@cid   
      FOR XML AUTO  
   </sql:query>  
</root>  
```  
  
 Le modèle suivant est le deuxième modèle (UpdateContact.xml). Enregistrez ce modèle dans le répertoire associé au nom virtuel du type de **modèle** .  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:header>  
   <updg:param name="cid"/>  
   <updg:param name="title" />  
   <updg:param name="firstname" />  
   <updg:param name="lastname" />  
   <updg:param name="emailaddress" />  
   <updg:param name="phone" />  
</updg:header>  
<updg:sync >  
   <updg:before>  
      <Person.Contact ContactID="$cid" />   
   </updg:before>  
   <updg:after>  
      <Person.Contact ContactID="$cid"   
       Title="$title"  
       FirstName="$firstname"  
       LastName="$lastname"  
       EmailAddress="$emailaddress"  
       Phone="$phone"/>  
   </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
 Le code suivant est l'application ASP (SampleASP.asp). Enregistrez-le dans le répertoire associé à une racine virtuelle que vous créez à l'aide de l'utilitaire Gestionnaire des services Internet. (Cette racine virtuelle n'est pas créée à l'aide d'IIS Virtual Directory Management pour l'utilitaire [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] car IIS Virtual Directory Management pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne peut pas identifier ou accéder à des applications ASP.)  
  
> [!NOTE]  
>  Dans le code, vous devez remplacer le « ServerName » par le nom du serveur exécutant les services Internet (Microsoft Internet Information Services (IIS)).  
  
```  
<% LANGUAGE=VBSCRIPT %>  
<%  
  Dim ContactID  
  ContactID=Request.Form("cid")  
%>  
<html>  
<body>  
<%  
  'If a ContactID value is not yet provided, display this form.  
  if ContactID="" then  
%>  
<!-- If the ContactID has not been specified, display the form that allows users to enter an ID. -->  
<form action="AdventureWorksContacts.asp" method="POST">  
<br>  
Enter ContactID: <input type=text name="cid"><br>  
<input type=submit value="Submit this ID" ><br><br>  
<-- Otherwise, if a ContactID is entered, display the second part of the form where the user can change customer information. -->  
<%  
  else  
%>  
<form name="Contacts" action="https://localhost/AdventureWorks/Template/UpdateContact.xml" method="POST">  
You may update customer information below.<br><br>  
<!-- A comment goes here to separate the parts of the application or page. -->  
<br>  
<%  
  ' Load the document in the parser and extract the values to populate the form.  
    Set objXML=Server.CreateObject("MSXML2.DomDocument")  
    ObjXML.setProperty "ServerHTTPRequest", TRUE  
  
    objXML.async=False  
    objXML.Load("https://localhost/AdventureWorks/Template/GetContact.xml?cid=" & ContactID)  
    set objCustomer=objXML.documentElement.childNodes.Item(0)  
  
  ' In retrieving data from the database, if a value in the column is NULL there  
  '  is no attribute for the corresponding element. In this case,  
  ' skip the error generation and go to the next attribute.  
  
  On Error Resume Next  
  
  Response.Write "Contact ID: <input type=text readonly=true style='background-color:silver' name=cid value="""  
  Response.Write objCustomer.attributes(0).value  
  Response.Write """><br><br>"  
  
  Response.Write "Title: <input type=text name=title value="""  
  Response.Write objCustomer.attributes(1).value  
  Response.Write """><br><br>"  
  
  Response.Write "First Name: <input type=text name=firstname value="""  
  Response.Write objCustomer.attributes(2).value  
  Response.Write """><br>"  
  
  Response.Write "Last Name: <input type=text name=lastname value="""  
  Response.Write objCustomer.attributes(3).value  
  Response.Write """><br><br>"  
  
  Response.Write "Email Address: <input type=text name=emailaddress value="""  
  Response.Write objCustomer.attributes(4).value  
  Response.Write """><br><br>"  
  
  Response.Write "Phone: <input type=text name=phone value="""  
  Response.Write objCustomer.attributes(9).value  
  Response.Write """><br><br>"  
  
  set objCustomer=Nothing  
  Set objXML=Nothing  
%>  
<input type="submit" value="Submit this change" ><br><br>  
<input type=hidden name="contenttype" value="text/xml">  
<input type=hidden name="eeid" value="<%=ContactID%>"><br><br>  
<% end if %>  
  
</form>  
</body>  
</html>  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Considérations sur la sécurité mise à jour &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
