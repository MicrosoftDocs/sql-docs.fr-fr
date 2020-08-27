---
description: Dialect, propriété
title: Dialect, propriété | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- _Command::Dialect
helpviewer_keywords:
- Dialect property
ms.assetid: 329c3a71-ba88-4009-b04f-2f52195a5957
author: rothja
ms.author: jroth
ms.openlocfilehash: be93ceb76aa0aadba5b28673c16704b0881186f9
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88973900"
---
# <a name="dialect-property"></a>Dialect, propriété
Indique le dialecte des propriétés [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) ou [CommandStream](../../../ado/reference/ado-api/commandstream-property-ado.md) . Le dialecte définit la syntaxe et les règles générales que le fournisseur utilise pour analyser la chaîne ou le flux.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 La propriété **Dialect** contient un GUID valide qui représente le dialecte du texte ou du flux de commande. La valeur par défaut de cette propriété est {C8B521FB-5CF3-11CE-ADE5-00AA0044773D}, qui indique que le fournisseur doit choisir comment interpréter le texte de la commande ou le flux.  
  
## <a name="remarks"></a>Notes  
 ADO n’interroge pas le fournisseur lorsque l’utilisateur lit la valeur de cette propriété ; elle retourne une représentation sous forme de chaîne de la valeur actuellement stockée dans l’objet [Command](../../../ado/reference/ado-api/command-object-ado.md) .  
  
 Lorsque l’utilisateur définit la propriété **dialecte** , ADO valide le GUID et génère une erreur si la valeur fournie n’est pas un GUID valide. Consultez la documentation de votre fournisseur pour déterminer les valeurs GUID prises en charge par la propriété **dialecte** .  
  
## <a name="applies-to"></a>S'applique à  
 [Command, objet (ADO)](../../../ado/reference/ado-api/command-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Execute, méthode (commande ADO)](../../../ado/reference/ado-api/execute-method-ado-command.md)
