---
title: clr enabled (option de configuration de serveur) | Microsoft Docs
description: Découvrez comment utiliser l’option CLR activé pour spécifier si SQL Server peut exécuter des assemblys utilisateur. Découvrez quand l’exécution de CLR (Common Language Runtime) n’est pas prise en charge.
ms.custom: ''
ms.date: 06/20/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- assemblies [CLR integration], verifying can run
- clr enabled option
ms.assetid: 0722d382-8fd3-4fac-b4a8-cd2b7a7e0293
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 600b70e751fe4fcd3d80c7e86bd494ecc8cf1758
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100347034"
---
# <a name="clr-enabled-server-configuration-option"></a>clr enabled (option de configuration de serveur)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  L'option clr enabled vous permet de spécifier si des assemblys utilisateur peuvent être exécutés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L’option clr enabled fournit les valeurs suivantes : 
  
|Valeur|Description|  
|-----------|-----------------|  
|0|L'exécution de l'assembly n'est pas autorisée sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|1|L'exécution de l'assembly est autorisée sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
WOW64 uniquement. Redémarrez les serveurs WOW64 pour appliquer les modifications des paramètres. Le redémarrage n’est pas nécessaire pour les autres types de serveurs.  

Quand vous exécutez l’instruction RECONFIGURE et que la valeur d’exécution de l’option clr enabled est 0 et non plus 1, tous les domaines d’application contenant des assemblys utilisateur sont immédiatement déchargés.  
  
>  **L’exécution de CLR (Common language runtime) n’est pas prise en charge sous le regroupement léger** Désactivez l’une des deux options : « clr activé » ou « regroupement léger ». Les fonctionnalités qui reposent sur le CLR et qui ne fonctionnent pas correctement en mode fibre incluent le type de données **hierarchyid**, la fonction `FORMAT`, la réplication et la gestion basée sur des stratégies.  
> 
> [!WARNING]
>  CLR utilise la sécurité d’accès du code (CAS) dans le .NET Framework, qui n’est plus pris en charge comme limite de sécurité. Un assembly CLR créé avec `PERMISSION_SET = SAFE` peut être en mesure d’accéder à des ressources système externes, d’appeler du code non managé et d’acquérir des privilèges sysadmin. À compter de [!INCLUDE[sssql17](../../includes/sssql17-md.md)], une option de `sp_configure` appelée `clr strict security` est introduite pour renforcer la sécurité des assemblys CLR. `clr strict security` est activée par défaut et traite les assemblys `SAFE` et `EXTERNAL_ACCESS` comme s’ils étaient marqués `UNSAFE`. L’option `clr strict security` peut être désactivée pour assurer une compatibilité descendante, mais ceci n’est pas recommandé. Microsoft recommande que tous les assemblys soient signés par un certificat ou une clé asymétrique avec une connexion correspondante à laquelle a été accordée l’autorisation `UNSAFE ASSEMBLY` dans la base de données master. Les administrateurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent également ajouter des assemblys à une liste d’assemblys, que le moteur de base de données doit approuver. Pour plus d’informations, consultez [sys.sp_add_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-add-trusted-assembly-transact-sql.md).
  
## <a name="example"></a>Exemple  
 L'exemple suivant déclare indique d'abord le paramètre actuel de l'option clr enabled, puis active l'option en lui attribuant la valeur 1. Pour désactiver l'option, affectez-lui la valeur 0.  
  
```sql  
EXEC sp_configure 'clr enabled';  
EXEC sp_configure 'clr enabled' , '1';  
RECONFIGURE;    
```  
  
## <a name="see-also"></a>Voir aussi  
 [lightweight pooling (option de configuration de serveur)](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [lightweight pooling (option de configuration de serveur)](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)  
  
  
