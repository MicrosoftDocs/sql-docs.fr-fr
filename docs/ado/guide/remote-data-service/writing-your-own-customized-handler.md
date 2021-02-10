---
description: Écriture d’un gestionnaire personnalisé
title: Écriture de votre propre gestionnaire personnalisé | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- DataFactory handler in RDS [ADO]
- customized handler in RDS [ADO]
ms.assetid: d447712a-e123-47b5-a3a4-5d366cfe8d72
author: rothja
ms.author: jroth
ms.openlocfilehash: f2c62c8b12435ec70166b66416f343fa835f6158
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032604"
---
# <a name="writing-your-own-customized-handler"></a>Écriture d’un gestionnaire personnalisé
Vous pouvez écrire votre propre gestionnaire si vous êtes un administrateur de serveur IIS qui souhaite la prise en charge de RDS par défaut, mais plus de contrôle sur les demandes utilisateur et les droits d’accès.  
  
 Le MSDFMAP. Le gestionnaire implémente l’interface **IDataFactoryHandler** .  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="idatafactoryhandler-interface"></a>Interface IDataFactoryHandler  
 Cette interface a deux méthodes, **GetRecordSet** et **reconnect**. Les deux méthodes requièrent que la propriété [CursorLocation](../../reference/ado-api/cursorlocation-property-ado.md) soit définie sur **adUseClient**.  
  
 Les deux méthodes acceptent les arguments qui apparaissent après la première virgule dans le mot clé «**handler =**». Par exemple, passera `"Handler=progid,arg1,arg2;"` une chaîne d’arguments de `"arg1,arg2"` et passera `"Handler=progid"` un argument null.  
  
## <a name="getrecordset-method"></a>Méthode GetRecordset  
 Cette méthode interroge la source de données et crée un nouvel objet [Recordset](../../reference/ado-api/recordset-object-ado.md) à l’aide des arguments fournis. Le **jeu d’enregistrements** doit être ouvert avec **adLockBatchOptimistic** et ne doit pas être ouvert de façon asynchrone.  
  
### <a name="arguments"></a>Arguments  
 ***conn***  Chaîne de connexion.  
  
 ***arguments***  Arguments pour le gestionnaire.  
  
 ***requête***  Texte de commande pour la création d’une requête.  
  
 ***PPRS** _ le pointeur où _ *Recordset** doit être retourné.  
  
## <a name="reconnect-method"></a>Reconnect, méthode  
 Cette méthode met à jour la source de données. Il crée un objet de [connexion](../../reference/ado-api/connection-object-ado.md) et attache le **jeu d’enregistrements** donné.  
  
### <a name="arguments"></a>Arguments  
 ***conn***  Chaîne de connexion.  
  
 ***arguments***  Arguments pour le gestionnaire.  
  
 ***pRS** _ A _ *Recordset** objet.  
  
## <a name="msdfhdlidl"></a>msdfhdl.idl  
 Il s’agit de la définition d’interface pour **IDataFactoryHandler** qui apparaît dans le fichier **msdfhdl. idl** .  
  
```cpp
[  
  uuid(D80DE8B3-0001-11d1-91E6-00C04FBBBFB3),  
  version(1.0)  
]  
library MSDFHDL  
{  
    importlib("stdole32.tlb");  
    importlib("stdole2.tlb");  
  
    // TLib : Microsoft ActiveX Data Objects 2.0 Library  
    // {00000200-0000-0010-8000-00AA006D2EA4}  
    #ifdef IMPLIB  
    importlib("implib\\x86\\release\\ado\\msado15.dll");  
    #else  
    importlib("msado20.dll");  
    #endif  
  
    [  
      odl,  
      uuid(D80DE8B5-0001-11d1-91E6-00C04FBBBFB3),  
      version(1.0)  
    ]  
    interface IDataFactoryHandler : IUnknown  
    {  
HRESULT _stdcall GetRecordset(  
      [in] BSTR conn,  
      [in] BSTR args,  
      [in] BSTR query,  
      [out, retval] _Recordset **ppRS);  
  
// DataFactory will use the ActiveConnection property  
// on the Recordset after calling Reconnect.  
   HRESULT _stdcall Reconnect(  
      [in] BSTR conn,  
      [in] BSTR args,  
      [in] _Recordset *pRS);  
    };  
};  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Section de connexion au fichier de personnalisation](./customization-file-connect-section.md)   
 [Section journaux des fichiers de personnalisation](./customization-file-logs-section.md)   
 [Section SQL du fichier de personnalisation](./customization-file-sql-section.md)   
 [Section UserList du fichier de personnalisation](./customization-file-userlist-section.md)   
 [Personnalisation de DataFactory](./datafactory-customization.md)   
 [Paramètres client requis](./required-client-settings.md)   
 [Présentation du fichier de personnalisation](./understanding-the-customization-file.md)