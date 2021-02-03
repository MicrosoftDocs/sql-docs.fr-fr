---
description: ALTER ENDPOINT (Transact-SQL)
title: ALTER ENDPOINT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER ENDPOINT
- ALTER_ENDPOINT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER ENDPOINT statement
- modifying endpoints
- endpoints [SQL Server], modifying
ms.assetid: 70f35566-30cf-47c6-8394-dfe5d71629d3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cf0653529ab512cf65375022a809c2f34227069c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204287"
---
# <a name="alter-endpoint-transact-sql"></a>ALTER ENDPOINT (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Permet de modifier un point de terminaison existant selon plusieurs procédures :  
  
-   en ajoutant une nouvelle méthode à un point de terminaison existant ;  
  
-   en modifiant ou supprimant une méthode existante du point de terminaison ;  
  
-   en modifiant les propriétés d'un point de terminaison.  
  
> [!NOTE]  
>  Cette rubrique décrit la syntaxe et les arguments spécifiques à ALTER ENDPOINT. Pour obtenir la description des arguments communs à CREATE ENDPOINT et à ALTER ENDPOINT, consultez [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
 Les services Web XML natifs (points de terminaison HTTP/SOAP) sont supprimés à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
ALTER ENDPOINT endPointName [ AUTHORIZATION login ]  
[ STATE = { STARTED | STOPPED | DISABLED } ]  
[ AS { TCP } ( <protocol_specific_items> ) ]  
[ FOR { TSQL | SERVICE_BROKER | DATABASE_MIRRORING } (  
   <language_specific_items>  
        ) ]  
  
<AS TCP_protocol_specific_arguments> ::=  
AS TCP (  
  LISTENER_PORT = listenerPort  
  [ [ , ] LISTENER_IP = ALL | ( 4-part-ip ) | ( "ip_address_v6" ) ]  
)  
<FOR SERVICE_BROKER_language_specific_arguments> ::=  
FOR SERVICE_BROKER (  
   [ AUTHENTICATION = {   
      WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]  
      | CERTIFICATE certificate_name   
      | WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ] CERTIFICATE certificate_name   
      | CERTIFICATE certificate_name WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
    } ]  
   [ , ENCRYPTION = { DISABLED   
       |   
         {{SUPPORTED | REQUIRED }   
       [ ALGORITHM { RC4 | AES | AES RC4 | RC4 AES } ] }   
   ]  
  
  [ , MESSAGE_FORWARDING = {ENABLED | DISABLED} ]  
  [ , MESSAGE_FORWARD_SIZE = forwardSize  
)  
  
<FOR DATABASE_MIRRORING_language_specific_arguments> ::=  
FOR DATABASE_MIRRORING (  
   [ AUTHENTICATION = {   
      WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]  
      | CERTIFICATE certificate_name   
      | WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ] CERTIFICATE certificate_name   
      | CERTIFICATE certificate_name WINDOWS [ { NTLM | KERBEROS | NEGOTIATE } ]   
    } ]  
   [ , ENCRYPTION = { DISABLED   
       |   
         {{SUPPORTED | REQUIRED }   
       [ ALGORITHM { RC4 | AES | AES RC4 | RC4 AES } ] }   
    ]   
   [ , ] ROLE = { WITNESS | PARTNER | ALL }  
)  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
  
> [!NOTE]  
>  Les arguments suivants sont spécifiques à ALTER ENDPOINT. Pour obtenir une description des autres arguments, consultez [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
 **AS** { **TCP** }  
 Vous ne pouvez pas changer le protocole de transport avec **ALTER ENDPOINT**.  
  
 **AUTHORIZATION** *login*  
 L’option **AUTHORIZATION** n’est pas disponible dans **ALTER ENDPOINT**. La propriété ne peut être assignée que lorsque le point de terminaison est créé.  
  
 **FOR** { **TSQL** | **SERVICE_BROKER** | **DATABASE_MIRRORING** }  
 Vous ne pouvez pas changer le type de charge utile avec **ALTER ENDPOINT**.  
  
## <a name="remarks"></a>Notes  
 Lorsque vous utilisez ALTER ENDPOINT, spécifiez uniquement les paramètres que vous souhaitez mettre à jour. Toutes les propriétés d'un point de terminaison existant restent inchangées tant que vous ne les modifiez pas explicitement.  
  
 Il n'est pas possible d'exécuter d'instructions ENDPOINT DDL à l'intérieur d'une transaction utilisateur.  
  
 Pour obtenir des informations sur le choix d’un algorithme de chiffrement à utiliser avec un point de terminaison, consultez [Choisir un algorithme de chiffrement](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md).  
  
> [!NOTE]  
>  L'algorithme RC4 est uniquement pris en charge pour des raisons de compatibilité descendante. Le nouveau matériel ne peut être chiffré à l'aide de RC4 ou de RC4_128 que lorsque la base de données se trouve dans le niveau de compatibilité 90 ou 100. (Non recommandé.) Utilisez à la place un algorithme plus récent, tel qu'un des algorithmes AES. Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures, le matériel chiffré à l’aide de RC4 ou de RC4_128 peut être déchiffré dans n’importe quel niveau de compatibilité.  
>   
>  RC4 est un algorithme relativement faible et AES est un algorithme relativement fort. Toutefois, AES est considérablement plus lent que RC4. Si vous considérez que la sécurité est une priorité plus importante que la vitesse, nous vous recommandons d'utiliser AES.  
  
## <a name="permissions"></a>Autorisations  
 L’utilisateur doit être membre du rôle serveur fixe **sysadmin**, propriétaire du point de terminaison ou bénéficier de l’autorisation ALTER ANY ENDPOINT.  
  
 Pour modifier l'appartenance d'un point de terminaison existant, vous devez utiliser l'instruction ALTER AUTHORIZATION. Pour plus d’informations, consultez [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
 Pour plus d’informations, consultez [Autorisations de point de terminaison GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
 [DROP ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-endpoint-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
