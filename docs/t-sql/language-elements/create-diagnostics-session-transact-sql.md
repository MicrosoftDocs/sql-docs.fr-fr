---
description: CREATE DIAGNOSTICS SESSION (Transact-SQL)
title: CREATE DIAGNOSTICS SESSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 662d019e-f217-49df-9e2f-b5662fa0342d
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 14a913922b62500d527928ca37e5b10f50acb0da
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208685"
---
# <a name="create-diagnostics-session-transact-sql"></a>CREATE DIAGNOSTICS SESSION (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Les sessions de diagnostic vous permettent d’enregistrer des informations de diagnostic détaillées et définies par l’utilisateur sur les performances du système ou de la requête.  
  
 Elles sont généralement utilisées pour déboguer les performances d’une requête spécifique ou pour surveiller le comportement d’un composant d’appliance spécifique au cours d’une opération d’appliance.  
  
> [!NOTE]  
>  Vous devez connaître le langage XML pour pouvoir utiliser les sessions de diagnostic.  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Creating a new diagnostics session:  
CREATE DIAGNOSTICS SESSION diagnostics_name AS N'{<session_xml>}';  
  
<session_xml>::  
<Session>  
   [ <MaxItemCount>max_item_count_num</MaxItemCount> ]  
   <Filter>  
      { \<Event Name="event_name"/>  
         [ <Where>\<filter_property_name Name="value" ComparisonType="comp_type"/></Where> ] [ ,...n ]  
      } [ ,...n ]  
   </Filter> ]   
   <Capture>  
      \<Property Name="property_name"/> [ ,...n ]  
   </Capture>  
<Session>  
  
-- Retrieving results for a diagnostics session:  
SELECT * FROM master.sysdiag.diagnostics_name ;  
  
-- Removing results for a diagnostics session:  
DROP DIAGNOSTICS SESSION diagnostics_name ;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *diagnostics_name*  
 Nom de la session de diagnostic. Les noms des sessions de diagnostic peuvent uniquement inclure des caractères a-z, A-Z et 0-9. De plus, ils doivent commencer par un caractère. *diagnostics_name* est limité à 127 caractères.  
  
 *max_item_count_num*  
 Nombre d’événements devant être persistants dans une vue. Par exemple, si 100 est spécifié, les 100 derniers événements correspondant aux critères de filtrage sont rendus persistants dans la session de diagnostic. Si moins de 100 événements sont trouvés, la session de diagnostic contient moins de 100 événements. *max_item_count_num* doit être au moins égal à 100 et inférieur ou égal à 100 000.  
  
 *event_name*  
 Définit les événements réels à collecter dans la session de diagnostic.  *event_name* est l’un des événements répertoriés dans [sys.pdw_diag_events](../../relational-databases/system-catalog-views/sys-pdw-diag-events-transact-sql.md) où `sys.pdw_diag_events.is_enabled='True'`.  
  
 *filter_property_name*  
 Nom de la propriété sur laquelle restreindre les résultats. Par exemple, si vous voulez limiter par ID de session, *filter_property_name* doit avoir la valeur *SessionId*. Consultez *property_name* ci-dessous pour obtenir la liste de valeurs potentielles de *filter_property_name*.  
  
 *value*  
 Valeur à évaluer par rapport à *filter_property_name*. Le type de la valeur et le type de la propriété doivent être identiques. Par exemple, si le type de la propriété est décimal, celui de la *valeur* doit l’être aussi.  
  
 *comp_type*  
 Type de comparaison. Les valeurs possibles sont : Equals, EqualsOrGreaterThan, EqualsOrLessThan, GreaterThan, LessThan, NotEquals, Contains, RegEx  
  
 *property_name*  
 Propriété associée à l’événement.  Les noms de propriété peuvent faire partie de la balise de capture ou être utilisées dans le cadre des critères de filtrage.  
  
|Nom de la propriété|Description|  
|-------------------|-----------------|  
|UserName|Nom d’utilisateur (ID de connexion).|  
|SessionId|ID de session.|  
|QueryId|ID de requête.|  
|CommandType|Type de commande.|  
|CommandText|Texte dans une commande traitée.|  
|OperationType|Type d’opération de l’événement.|  
|Duration|Durée de l’événement.|  
|SPID|ID de processus de service.|  
  
## <a name="remarks"></a>Remarques  
 Chaque utilisateur a droit à un maximum de 10 sessions de diagnostic simultanées. Consultez [sys.pdw_diag_sessions](../../relational-databases/system-catalog-views/sys-pdw-diag-sessions-transact-sql.md) pour obtenir la liste des sessions actives et supprimer toutes les sessions inutiles à l’aide de `DROP DIAGNOSTICS SESSION`.  
  
 Les sessions de diagnostic continuent à collecter des métadonnées jusqu’à ce qu’elles soient supprimées.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **ALTER SERVER STATE**.  
  
## <a name="locking"></a>Verrouillage  
 Accepte un verrou partagé sur la table des sessions de diagnostic.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-diagnostics-session"></a>R. Création d’une session de diagnostic  
 Cet exemple crée une session de diagnostic permettant d’enregistrer des métriques des performances du moteur de base de données. L’exemple crée une session de diagnostic qui écoute les événements en cours d’exécution/de fin de requête de moteur et un événement DMS de blocage. Le texte de la commande, le nom de l’ordinateur, l’ID de la requête et la session sur laquelle l’événement a été créé sont ainsi retournés.  
  
```sql  
CREATE DIAGNOSTICS SESSION MYDIAGSESSION AS N'  
<Session>  
   <MaxItemCount>100</MaxItemCount>  
   <Filter>  
      <Event Name="EngineInstrumentation:EngineQueryRunningEvent" />  
      <Event Name="DmsCoreInstrumentation:DmsBlockingQueueEnqueueBeginEvent" />  
      <Where>  
         <SessionId Value="381" ComparisonType="NotEquals" />  
      </Where>  
   </Filter>  
   <Capture>  
      <Property Name="Query.CommandText" />  
      <Property Name="MachineName" />  
      <Property Name="Query.QueryId" />  
      <Property Name="Alias" />  
      <Property Name="Duration" />  
      <Property Name="Session.SessionId" />  
   </Capture>  
</Session>';  
```  
  
 Après avoir créé la session de diagnostic, exécutez une requête.  
  
```sql  
SELECT COUNT(EmployeeKey) FROM AdventureWorksPDW2012..FactSalesQuota;  
```  
  
 Ensuite, affichez les résultats de la session de diagnostic en faisant une sélection à partir du schéma sysdiag.  
  
```sql  
SELECT * FROM master.sysdiag.MYDIAGSESSION;  
```  
  
 Remarquez que le schéma sysdiag contient une vue qui porte le nom de votre session de diagnostic.  
  
 Pour afficher uniquement l’activité de votre connexion, ajoutez la propriété `Session.SPID` et ajoutez `WHERE [Session.SPID] = @@spid;` à la requête.  
  
 Lorsque vous avez terminé avec la session de diagnostic, supprimez-la à l’aide de la commande **DROP DIAGNOSTICS**.  
  
```sql  
DROP DIAGNOSTICS SESSION MYDIAGSESSION;  
```  
  
### <a name="b-alternative-diagnostic-session"></a>B. Autre session de diagnostic  
 Deuxième exemple avec des propriétés légèrement différentes.  
  
```sql  
-- Determine the session_id of your current session  
SELECT TOP 1 session_id();  
-- Replace \<*session_number*> in the code below with the numbers in your session_id  
CREATE DIAGNOSTICS SESSION PdwOptimizationDiagnostics AS N'  
<Session>  
   <MaxItemCount>100</MaxItemCount>  
   <Filter>  
      <Event Name="EngineInstrumentation:MemoGenerationBeginEvent" />  
      <Event Name="EngineInstrumentation:MemoGenerationEndEvent" />  
      <Event Name="DSQLInstrumentation:OptimizationBeginEvent" />  
      <Event Name="DSQLInstrumentation:OptimizationEndEvent" />  
      <Event Name="DSQLInstrumentation:BuildRelOpContextTreeBeginEvent" />  
      <Event Name="DSQLInstrumentation:PostPlanGenModifiersEndEvent" />  
      <Where>  
         <SessionId Value="\<*session_number*>" ComparisonType="Equals" />  
      </Where>  
   </Filter>  
   <Capture>  
      <Property Name="Session.SessionId" />  
      <Property Name="Query.QueryId" />  
      <Property Name="Query.CommandText" />  
      <Property Name="Name" />  
      <Property Name="DateTimePublished" />  
      <Property Name="DateTimePublished.Ticks" />  
  </Capture>  
</Session>';  
```  
  
 Exécutez une requête, comme :  
  
```sql  
USE ssawPDW;  
GO  
SELECT * FROM dbo.FactFinance;  
```  
  
 La requête suivante retourne la durée d’autorisation :  
  
```sql  
SELECT *   
FROM master.sysdiag.PdwOptimizationDiagnostics   
ORDER BY DateTimePublished;  
```  
  
 Lorsque vous avez terminé avec la session de diagnostic, supprimez-la à l’aide de la commande **DROP DIAGNOSTICS**.  
  
```sql  
DROP DIAGNOSTICS SESSION PdwOptimizationDiagnostics;  
```  
  
  
