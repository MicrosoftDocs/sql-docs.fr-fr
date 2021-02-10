---
description: 'Annexe A : fournisseurs de données et de services'
title: 'Annexe A : fournisseurs | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data providers [ADO]
- providers [ADO]
- ADO, providers
- service providers [ADO]
- service components [ADO]
ms.assetid: e2581b47-b11e-4e1e-b96c-d39c77c5b48a
author: rothja
ms.author: jroth
ms.openlocfilehash: a49518c1fcc1618884751d503def46a29dd9c048
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029483"
---
# <a name="appendix-a-data-and-service-providers"></a>Annexe A : fournisseurs de données et de services
Cette section traite de trois types de fournisseurs : les fournisseurs de données, les fournisseurs de services et les composants de service. Les fournisseurs se répartissent en deux catégories : celles fournissant des données et celles fournissant des services. Un *fournisseur de données* possède ses propres données et l’expose sous forme tabulaire à votre application. Un *fournisseur de services* encapsule un service en générant et en consommant des données, en augmentant les fonctionnalités de vos applications ADO. Un fournisseur de services peut également être défini comme un *composant de service*, qui doit fonctionner conjointement avec d’autres fournisseurs de services ou composants.

## <a name="data-providers"></a>Fournisseurs de données
 ADO est puissant et flexible, car il peut se connecter à n’importe quel autre fournisseur de données et exposer toujours le même modèle de programmation, quelles que soient les fonctionnalités spécifiques d’un fournisseur donné.

 Toutefois, étant donné que chaque fournisseur de données est unique, la façon dont votre application interagit avec ADO varie légèrement selon le fournisseur de données. Les différences appartiennent généralement à l’une des trois catégories suivantes :

-   Paramètres de connexion dans la propriété [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) .

-   Utilisation de l’objet de [commande](../../reference/ado-api/command-object-ado.md) .

-   Comportement du [Recordset](../../reference/ado-api/recordset-object-ado.md) spécifique au fournisseur.

 Les détails de chacun des fournisseurs de données actuellement disponibles auprès de Microsoft sont répertoriés comme suit.

|Domaine|Rubrique|
|----------|-----------|
|Bases de données ODBC|[Fournisseur Microsoft OLE DB pour ODBC](./microsoft-ole-db-provider-for-odbc.md)|
|Service d’indexation Microsoft|[Fournisseur Microsoft OLE DB pour le service d'indexation Microsoft](./microsoft-ole-db-provider-for-microsoft-indexing-service.md)|
|Service Active Directory|[Fournisseur Microsoft OLE DB pour le service Microsoft Active Directory](./microsoft-ole-db-provider-for-microsoft-active-directory-service.md)|
|Bases de données Microsoft Jet|[Fournisseur de OLE DB pour Microsoft Jet](./microsoft-ole-db-provider-for-microsoft-jet.md)|
|Microsoft SQL Server|[Fournisseur Microsoft OLE DB pour SQL Server](./microsoft-ole-db-provider-for-sql-server.md)|
|Bases de données Oracle|[Fournisseur Microsoft OLE DB pour Oracle](./microsoft-ole-db-provider-for-oracle.md)|
|Publication Internet|[Fournisseur Microsoft OLE DB pour la publication Internet](./microsoft-ole-db-provider-for-internet-publishing.md)|
|Sources de données simples|[Fournisseur Microsoft OLE DB simple](./microsoft-ole-db-simple-provider.md)|

## <a name="provider-specific-dynamic-properties"></a>Provider-Specific les propriétés dynamiques
 Les collections [Properties](../../reference/ado-api/properties-collection-ado.md) des objets [Connection](../../reference/ado-api/connection-object-ado.md), [Command](../../reference/ado-api/command-object-ado.md)et [Recordset](../../reference/ado-api/recordset-object-ado.md) incluent des propriétés dynamiques spécifiques au fournisseur. Ces propriétés fournissent des informations sur les fonctionnalités spécifiques au fournisseur au-delà des propriétés intégrées qu’ADO prend en charge.

 Après avoir établi la connexion et créé ces objets, utilisez la méthode [Refresh](../../reference/ado-api/refresh-method-ado.md) sur la collection **Properties** de l’objet pour obtenir les propriétés spécifiques au fournisseur. Reportez-vous à la documentation du fournisseur et au [Guide du programmeur OLE DB](/previous-versions/windows/desktop/ms713643(v=vs.85)) pour plus d’informations sur ces propriétés dynamiques.

## <a name="service-providers"></a>Fournisseurs de services
 Pour utiliser un fournisseur de services, vous devez fournir un mot clé. Vous devez également connaître les propriétés dynamiques spécifiques au fournisseur associées à chaque fournisseur de services. Les détails spécifiques au fournisseur sont répertoriés pour chaque fournisseur de services actuellement disponible auprès de Microsoft :

-   [Microsoft Data Shaping Service pour OLE DB](./microsoft-data-shaping-service-for-ole-db-ado-service-provider.md)

-   [Fournisseur de persistance Microsoft OLE DB](./microsoft-ole-db-persistence-provider-ado-service-provider.md)

-   [Fournisseur Microsoft OLE DB Remoting](./microsoft-ole-db-remoting-provider-ado-service-provider.md)

## <a name="service-components"></a>Composants du service
 Le [service de curseur pour](./microsoft-cursor-service-for-ole-db-ado-service-component.md) le composant de service OLE DB complète les fonctions de prise en charge de curseur des fournisseurs de données. Elle requiert également un mot clé et des propriétés dynamiques.

 Pour plus d’informations sur les fournisseurs de OLE DB, consultez [Microsoft OLE DB](/previous-versions/windows/desktop/ms722784(v=vs.85)).

## <a name="provider-commands"></a>Commandes du fournisseur
 Pour chaque fournisseur répertorié ici, si vos applications permettent aux utilisateurs d’entrer des instructions SQL en tant que commandes du fournisseur, vous devez toujours valider l’entrée utilisateur et faire vigilance les attaques de pirates potentielles à l’aide d’instructions SQL potentiellement dangereuses, telles que `DROP TABLE t1` , dans le cadre de l’entrée de l’utilisateur.

## <a name="see-also"></a>Voir aussi
 [Command, objet (ADO)](../../reference/ado-api/command-object-ado.md) [Connection Object (ado)](../../reference/ado-api/connection-object-ado.md) [Microsoft OLE DB fournisseur pour la publication Internet](./microsoft-ole-db-provider-for-internet-publishing.md) [Microsoft OLE DB Provider pour Microsoft Active Directory Service](./microsoft-ole-db-provider-for-microsoft-active-directory-service.md) [Microsoft OLE DB Provider pour microsoft Indexing Service](./microsoft-ole-db-provider-for-microsoft-indexing-service.md) fournisseur [Microsoft OLE DB pour ODBC](./microsoft-ole-db-provider-for-odbc.md) [fournisseur Microsoft OLE DB pour Oracle](./microsoft-ole-db-provider-for-oracle.md) fournisseur Microsoft OLE DB pour [SQL Server](./microsoft-ole-db-provider-for-sql-server.md) fournisseur [Microsoft OLE DB pour Microsoft Jet](./microsoft-ole-db-provider-for-microsoft-jet.md) [Properties collection (](../../reference/ado-api/properties-collection-ado.md) ADO) [objet Recordset (ADO](../../reference/ado-api/recordset-object-ado.md) ), [méthode Refresh (RDS)](../../reference/rds-api/refresh-method-rds.md)