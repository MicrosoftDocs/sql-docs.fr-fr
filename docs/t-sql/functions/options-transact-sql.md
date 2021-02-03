---
description: '&#x40;&#x40;OPTIONS (Transact-SQL)'
title: '@@OPTIONS (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@OPTIONS'
- '@@OPTIONS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- SET statement, current SET options
- '@@OPTIONS function'
- current SET options
ms.assetid: 3d5c7f6e-157b-4231-bbb4-4645a11078b3
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 43da748085a31a9d9f25ef20bdb690f2b94e4cb9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183944"
---
# <a name="x40x40options-transact-sql"></a>&#x40;&#x40;OPTIONS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne des informations sur les options SET actuelles.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
@@OPTIONS  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 **integer**  
  
## <a name="remarks"></a>Remarques  
 Les options peuvent provenir de l’utilisation de la commande **SET** ou de la valeur **sp_configure user options**. Les valeurs de session configurées avec la commande **SET** remplacent les options **sp_configure**. De nombreux outils (tels que [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]) configurent automatiquement les options de définition. Chaque utilisateur dispose d’une fonction @@OPTIONS qui représente la configuration.  
  
 Vous pouvez modifier les options linguistiques et de traitement des requêtes pour une session utilisateur spécifique à l'aide de l'instruction SET. **\@\@OPTIONS** peut détecter uniquement les options qui ont la valeur ON ou OFF.  
  
 La fonction **\@\@OPTIONS** renvoie une bitmap des options, convertie en entier de base 10 (décimal). Les paramètres de bit sont stockés dans les emplacements décrits dans une table de la rubrique [Configurer l’option de configuration de serveur user options](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md).  
  
 Pour décoder la valeur **\@\@OPTIONS**, convertissez l’entier renvoyé par **\@\@OPTIONS** en binaire, puis recherchez les valeurs dans le tableau, dans [Configurer l’option de configuration de serveur user options](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md). Par exemple, si `SELECT @@OPTIONS;` renvoie la valeur `5496`, utilisez la calculatrice de programmation Windows (**calc.exe**) pour convertir la valeur décimale `5496` au format binaire. Le résultat est `1010101111000`. Les caractères situés les plus à droite (1, 2 et 4 binaires) sont 0, ce qui indique que les trois premiers éléments de la table sont désactivés. En consultant la table, vous voyez qu’il s’agit de **DISABLE_DEF_CNST_CHK**, de **IMPLICIT_TRANSACTIONS** et de **CURSOR_CLOSE_ON_COMMIT**. L’élément suivant (**ANSI_WARNINGS** dans la position `1000`) est activé. Continuez à parcourir la bitmap vers la gauche et la liste d’options vers le bas. Quand les options les plus à gauche sont 0, elles sont tronquées par la conversion de type. Le bitmap `1010101111000` est en fait `001010101111000` et représente les 15 options.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-demonstration-of-how-changes-affect-behavior"></a>R. Démonstration de l'impact des modifications sur le comportement  
 L’exemple suivant illustre la différence de comportement de concaténation avec deux paramétrages différents de l’option **CONCAT_NULL_YIELDS_NULL**.  
  
```sql  
SELECT @@OPTIONS AS OriginalOptionsValue;  
SET CONCAT_NULL_YIELDS_NULL OFF;  
SELECT 'abc' + NULL AS ResultWhen_OFF, @@OPTIONS AS OptionsValueWhen_OFF;  
  
SET CONCAT_NULL_YIELDS_NULL ON;  
SELECT 'abc' + NULL AS ResultWhen_ON, @@OPTIONS AS OptionsValueWhen_ON;  
```  
  
### <a name="b-testing-a-client-nocount-setting"></a>B. Test du paramètre NOCOUNT d'un client  
 L’exemple suivant définit `NOCOUNT``ON`, puis teste la valeur de `@@OPTIONS`. L’option `NOCOUNT``ON` empêche le message relatif au nombre de lignes affectées d’être renvoyé au client demandeur pour chaque instruction d’une session. La fonction `@@OPTIONS` a pour valeur `512` (0x0200). Ceci représente l'option NOCOUNT. Cet exemple teste si l'option NOCOUNT est activée sur le client. Vous pouvez vous en servir pour rechercher les différences de performances sur un ordinateur client.  
  
```sql  
SET NOCOUNT ON  
IF @@OPTIONS & 512 > 0   
RAISERROR ('Current user has SET NOCOUNT turned on.', 1, 1)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de configuration &#40;Transact-SQL&#41;](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Configurer l'option de configuration de serveur user options](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md)  
  
  
