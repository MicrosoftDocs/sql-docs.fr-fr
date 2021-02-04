---
description: Méthode setTrustStore (SQLServerDataSource)
title: Méthode setTrustStore (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- setTrustStore Method (SQLServerDataSource)
apilocation:
- setTrustStore Method (SQLServerDataSource)
apitype: Assembly
ms.assetid: bab5485d-4547-426c-adbe-44e2b5702d1d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d537e17309db560b914ea4b9a71c726e9bfe2003
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172944"
---
# <a name="settruststore-method-sqlserverdatasource"></a>Méthode setTrustStore (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit le chemin d'accès (y compris le nom de fichier) au fichier trustStore de certificat.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setTrustStore(java.lang.String trustStore)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *trustStore*  
  
 Valeur **String** qui contient le chemin (notamment le nom de fichier) du fichier trustStore de certificat.  
  
## <a name="remarks"></a>Notes  
 Quand la propriété trustStore n’est pas spécifiée ou a la valeur Null, le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] se fie aux règles de recherche de la fabrication du gestionnaire d’approbation pour déterminer le magasin de certificats à utiliser. Le TrustManagerFactory SunX509 par défaut essaie de rechercher les informations approuvées aux emplacements suivants dans cet ordre :  
  
-   1. Un fichier spécifié par la propriété système de machine virtuelle Java (JVM) « javax.net.ssl.trustStore ».  
  
-   2. Fichier « \<java-home>/lib/security/jssecacerts ».  
  
-   3. Fichier « \<java-home>/lib/security/cacerts ».  
  
 Pour plus d'informations, consultez la documentation d'interface SunX509 TrustManager sur le site Web de Sun Microsystems.  
  
 Si la propriété trustStore est définie avec une chaîne ou une chaîne vide (""), le pilote utilise cette valeur pour rechercher le fichier trustStore afin de valider le certificat TLS/SSL du serveur.  
  
 La propriété trustStorePassword peut être spécifiée avec la propriété trustStore et sa valeur est utilisée pour ouvrir le fichier trustStore. Pour plus d’informations, consultez [setTrustStorePassword](../../../connect/jdbc/reference/settruststorepassword-method-sqlserverdatasource.md).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
