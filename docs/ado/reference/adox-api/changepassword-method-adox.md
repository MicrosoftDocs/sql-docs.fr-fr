---
description: ChangePassword, méthode (ADOX)
title: ChangePassword, méthode (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _User25::raw_ChangePassword
- _User25::ChangePassword
helpviewer_keywords:
- ChangePassword method [ADOX]
ms.assetid: d187fbc6-5fac-4abb-803d-bf344dcf0302
author: rothja
ms.author: jroth
ms.openlocfilehash: 4347cb856b1fcb4d76ef6133024335404e32ad5b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164289"
---
# <a name="changepassword-method-adox"></a>ChangePassword, méthode (ADOX)
Modifie le mot de passe d’un compte d' [utilisateur](./user-object-adox.md) .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
User.ChangePassword OldPassword, NewPassword  
```  
  
#### <a name="parameters"></a>Paramètres  
 *OldPassword*  
 Valeur de **chaîne** qui spécifie le mot de passe existant de l’utilisateur. Si l’utilisateur n’a pas de mot de passe, utilisez une chaîne vide ("") pour *oldPassword*.  
  
 *NewPassword*  
 Valeur de **chaîne** qui spécifie le nouveau mot de passe.  
  
## <a name="remarks"></a>Notes  
 Pour des raisons de sécurité, l’ancien mot de passe doit être spécifié en plus du nouveau mot de passe.  
  
 Une erreur se produit si le fournisseur ne prend pas en charge l’administration des propriétés du tiers de confiance.  
  
## <a name="applies-to"></a>S'applique à  
 [User, objet (ADOX)](./user-object-adox.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Append (méthode) sur les collections Groups et Users, ChangePassword, exemple de méthodes (VB)](./groups-and-users-append-changepassword-methods-example-vb.md)