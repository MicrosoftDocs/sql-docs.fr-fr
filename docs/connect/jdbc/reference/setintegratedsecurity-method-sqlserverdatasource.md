---
description: Méthode setIntegratedSecurity (SQLServerDataSource)
title: Méthode setIntegratedSecurity (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerDataSource.setIntegratedSecurity
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 4c968ee4-b041-424a-bf69-cc2c4a4f51c6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b172631e774f5ea9da15873a1e8a00121ab46faf
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88431851"
---
# <a name="setintegratedsecurity-method-sqlserverdatasource"></a>Méthode setIntegratedSecurity (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit une valeur **booléenne** qui indique si la propriété integratedSecurity est activée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setIntegratedSecurity(boolean enable)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *enable*  
  
 **true** si integratedSecurity est activé. Dans le cas contraire, la valeur est **false**.  
  
## <a name="remarks"></a>Notes  
 Définissez sa valeur sur **true** pour indiquer que les informations d’identification Windows seront utilisées par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour authentifier l’utilisateur de l’application. Si la valeur est **true**, le [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] recherche les informations d’identification déjà fournies lors de la connexion à l’ordinateur ou au réseau dans le cache des informations d’identification de l’ordinateur local. Si la valeur est **false**, le nom d’utilisateur et le mot de passe doivent être fournis.  
  
> [!NOTE]  
>  Cette propriété de connexion est prise en charge seulement sur les systèmes d’exploitation [!INCLUDE[msCoName](../../../includes/msconame_md.md)] Windows.  
  
 Pour plus d’informations sur l’authentification intégrée, consultez [Générer l’URL de connexion](../../../connect/jdbc/building-the-connection-url.md).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
