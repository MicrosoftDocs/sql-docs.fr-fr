---
description: Fonction LocalDBFormatMessage
title: LocalDBFormatMessage fonction) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
apiname:
- LocalDBFormatMessage
apilocation:
- sqluserinstance.dll
apitype: DLLExport
ms.assetid: 31b3152a-94cf-4f75-a31b-296d7dd16dbe
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a7be5ebfad2430b1161d5af8c1a61697aacd568c
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89544056"
---
# <a name="localdbformatmessage-function"></a>Fonction LocalDBFormatMessage
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Retourne la description textuelle localisée pour l'erreur SQL Server Express LocalDB spécifiée.  
  
 **Fichier d'en-tête :** sqlncli.h  
  
## <a name="syntax"></a>Syntaxe  
  
```  
HRESULT LocalDBFormatMessage(  
           HRESULT hrLocalDB,  
           DWORD dwFlags,   
           DWORD dwLanguageId,   
           LPWSTR wszMessage,   
           LPDWORD lpcchMessage   
);  
```  
  
## <a name="parameters"></a>Paramètres  
 *hrLocalDB*  
 [Entrée] Code d'erreur de LocalDB.  
  
 *dwFlags*  
 [Entrée] Indicateurs spécifiant le comportement de cette fonction.  
  
 Indicateurs disponibles :  
  
 LOCALDB_TRUNCATE_ERR_MESSAGE  
 Si la mémoire tampon d'entrée est trop courte, le message d'erreur sera tronqué pour s'adapter à la mémoire tampon.  
  
 *dwLanguageId*  
 [Entrée] Langue souhaitée (LANGID) ou 0, auquel cas l'ordre du langage Win32 FormatMessage est utilisé.  
  
 *wszMessage*  
 [Sortie] Mémoire tampon pour stocker le message d'erreur de LocalDB.  
  
 *lpcchMessage*  
 [Entrée/sortie] En entrée contient la taille de la mémoire tampon de *wszMessage* en caractères. En sortie, si la taille de la mémoire tampon donnée est trop petite, contient la taille de la mémoire tampon requise en caractères, y compris les zéros de fin. Si la fonction réussit, contient le nombre de caractères du message, à l'exception des zéros de fin.  
  
## <a name="returns"></a>Retours  
 S_OK  
 La fonction a réussi.  
  
 [LOCALDB_ERROR_NOT_INSTALLED](../../relational-databases/express-localdb-error-messages/localdb-error-not-installed.md)  
 SQL Server Express LocalDB n'est pas installé sur l'ordinateur.  
  
 [LOCALDB_ERROR_INVALID_PARAMETER](../../relational-databases/express-localdb-error-messages/localdb-error-invalid-parameter.md)  
 Un ou plusieurs paramètres d'entrée spécifiés ne sont pas valides.  
  
 [LOCALDB_ERROR_UNKNOWN_ERROR_CODE](../../relational-databases/express-localdb-error-messages/localdb-error-unknown-error-code.md)  
 Le message demandé n'existe pas.  
  
 [LOCALDB_ERROR_UNKNOWN_LANGUAGE_ID](../../relational-databases/express-localdb-error-messages/localdb-error-unknown-language-id.md)  
 Le message n'est pas disponible dans la langue demandée.  
  
 [LOCALDB_ERROR_INSUFFICIENT_BUFFER](../../relational-databases/express-localdb-error-messages/localdb-error-insufficient-buffer.md)  
 Le tampon d'entrée *wszMessage* est trop court, et la troncation n'est pas demandée.  
  
 [LOCALDB_ERROR_INTERNAL_ERROR](../../relational-databases/express-localdb-error-messages/localdb-error-internal-error.md)  
 Une erreur inattendue s’est produite. Pour plus d'informations, consultez le journal des événements.  
  
## <a name="remarks"></a>Notes  
 Pour un exemple de code qui utilise l'API LocalDB, consultez [SQL Server Express LocalDB Reference](../../relational-databases/sql-server-express-localdb-reference.md).  
  
## <a name="see-also"></a>Voir aussi  
 [En-tête et informations de version SQL Server Express LocalDB](../../relational-databases/express-localdb-instance-apis/sql-server-express-localdb-header-and-version-information.md)  
  
  
