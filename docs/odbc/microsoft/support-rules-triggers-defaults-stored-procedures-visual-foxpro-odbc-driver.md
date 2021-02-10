---
description: Prise en charge des règles, des déclencheurs, des valeurs par défaut et des procédures stockées (pilote ODBC Visual FoxPro)
title: Prise en charge des règles, des déclencheurs, des valeurs par défaut et des procédures stockées | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- Visual FoxPro ODBC driver [ODBC], stored procedures
- FoxPro ODBC driver [ODBC], commands and functions
- commands for FoxPro ODBC driver [ODBC]
- FoxPro ODBC driver [ODBC], default values
- FoxPro ODBC driver [ODBC], triggers
- stored procedures [ODBC], Visual FoxPro ODBC driver
- Visual FoxPro ODBC driver [ODBC], rules
- FoxPro commands and functions [ODBC]
- rules [ODBC]
- Visual FoxPro ODBC driver [ODBC], default values
- FoxPro ODBC driver [ODBC], rules
- functions [ODBC], Visual FoxPro
- default values [ODBC]
- Visual FoxPro ODBC driver [ODBC], commands and functions
- Visual FoxPro ODBC driver [ODBC], triggers
- FoxPro ODBC driver [ODBC], stored procedures
- Visual FoxPro commands and functions [ODBC]
ms.assetid: e449de20-d6ca-4902-9f8e-814eb6e86650
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ea801de44e1fce175ac549402e17b9fcc2e9f020
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061124"
---
# <a name="support-for-rules-triggers-default-values-and-stored-procedures-visual-foxpro-odbc-driver"></a>Prise en charge des règles, des déclencheurs, des valeurs par défaut et des procédures stockées (pilote ODBC Visual FoxPro)
Vous ne pouvez pas créer des règles, des déclencheurs, des valeurs par défaut ou des procédures stockées Visual FoxPro à l’aide du pilote ODBC Visual FoxPro. Toutefois, votre application peut interagir avec les règles, les déclencheurs, les valeurs par défaut ou les procédures stockées existants lors de l’insertion, de la mise à jour ou de la suppression de données Visual FoxPro stockées dans une base de données.  
  
 Le tableau suivant répertorie les commandes et les fonctions Visual FoxPro prises en charge par le pilote ODBC Visual FoxPro lorsque les commandes ou fonctions existent dans des règles, des déclencheurs, des valeurs par défaut ou des procédures stockées.  
  
 Si votre application interagit avec des données dont les règles, les déclencheurs, les valeurs par défaut ou les procédures stockées appellent d’autres commandes ou fonctions Visual FoxPro, le pilote génère une erreur. Pour obtenir la liste des commandes et fonctions non prises en charge par le pilote, consultez [commandes et fonctions Visual FoxPro non prises en charge](../../odbc/microsoft/unsupported-visual-foxpro-commands-and-functions-visual-foxpro-odbc-driver.md) .  
  
> [!TIP]  
>  Si vous souhaitez insérer du code conditionnel dans vos règles, déclencheurs ou procédures stockées qui déterminent les commandes à exécuter quand il est appelé par le pilote, vous pouvez utiliser la fonction **version ()** . La fonction **version ()** retourne « pilote ODBC Visual FoxPro *\<version>* » lorsqu’elle est appelée par le pilote.  
  
## <a name="visual-foxpro-commands-and-functions-supported-in-rules-triggers-default-values-and-stored-procedures"></a>Commandes et fonctions Visual FoxPro prises en charge dans les règles, les déclencheurs, les valeurs par défaut et les procédures stockées  

:::row:::
    :::column:::
        $, Opérateur  
        %, opérateur  
    :::column-end:::
    :::column:::
        Commande &  
        Commande &&   
    :::column-end:::
    :::column:::
        \* Command  
        =, Commande  
    :::column-end:::
:::row-end:::

## <a name="a"></a>Un  

:::row:::
    :::column:::
        Fonction ABS ()  
        ACOPY () (fonction)  
        Fonction ACOS ()  
        ADATABASES () (fonction)  
        ADBOBJECTS () (fonction)  
        Ajouter une TABLE, commande  
        ADEL () (fonction)  
        AELEMENT () (fonction)  
        AERROR () (fonction)  
        AFIELDS () (fonction)  
        AINS () (fonction)  
        ALEN () (fonction)  
        Fonction ALIAS ()  
    :::column-end:::
    :::column:::
        ALLTRIM () (fonction)  
        ALTER TABLE, commande SQL  
        Opérateur AND  
        Ajouter une commande  
        Ajouter à partir de la commande  
        Ajouter à partir d’une commande de tableau  
        Ajouter une commande générale  
        Ajouter un mémo, commande  
        Commande APPEND PROCEDUREs  
        ASC (), fonction  
        ASCAN () (fonction)  
        Fonction ASIN ()  
        Fonction ASORt ()  
    :::column-end:::
    :::column:::
        ASUBSCRIPT () (fonction)  
        Fonction AT ()  
        Fonction AT_C ()  
        Fonction ATAN ()  
        Fonction ATC ()  
        ATCC () (fonction)  
        ATCLINE () (fonction)  
        ATLINE () (fonction)  
        Fonction ATN2 ()  
        AUSED () (fonction)  
        MOYENNE, commande  
    :::column-end:::
:::row-end:::

## <a name="b"></a>B  

:::row:::
    :::column:::
        Commande BEGIN TRANSACTION  
        BETWEEN () (fonction)  
        BITAND () (fonction)  
        BITCLEAR () (fonction)  
        BITLSHIFT () (fonction)  
    :::column-end:::
    :::column:::
        BITNOT () (fonction)  
        BITOR () (fonction)  
        BITRSHIFT () (fonction)  
        BITSET () (fonction)  
        Fonction BITTEst ()  
    :::column-end:::
    :::column:::
        BITXOR () (fonction)  
        Commande vide  
        BOF (), fonction  
    :::column-end:::
:::row-end:::

## <a name="c"></a>C  

:::row:::
    :::column:::
        Commande CALCULATE  
        Fonction CANDIDATe ()  
        CDOW () (fonction)  
        CDX (), fonction  
        Fonction CEILING ()  
        Fonction CHR ()  
        CHRTRAN () (fonction)  
        CHRTRANC () (fonction)  
        Commandes fermer  
        CMONTH () (fonction)  
    :::column-end:::
    :::column:::
        CONTINUER, commande  
        COPIER les INDEX, commande  
        Commande de copie de procédures  
        COPIER la STRUCTURE, commande  
        COPIER la STRUCTURE-commande étendue  
        COPIER la BALIse, commande  
        COPIER dans la commande  
        COPIER dans le tableau, commande  
        COS () (fonction)  
        COUNT (commande)  
    :::column-end:::
    :::column:::
        CPCONVERT () (fonction)  
        CPCURRENT () (fonction)  
        CPDBF () (fonction)  
        CTOD () (fonction)  
        CTOT () (fonction)  
        CURSORGETPROP () (fonction)  
        CURSORSETPROP () (fonction)  
        Fonction COURBUREal ()  
    :::column-end:::
:::row-end:::

## <a name="d"></a>D  

:::row:::
    :::column:::
        Fonction DATE ()  
        Fonction DATETIME ()  
        Fonction DAY ()  
        Fonction DBC ()  
        Fonction DBF ()  
        DBGETPROP () (fonction)  
        Fonction DBUSed ()  
        DELETE, commande SQL  
    :::column-end:::
    :::column:::
        Commande DELETE  
        DELETE TAG, commande  
        Fonction DELETEd ()  
        Fonction décroissant ()  
        Fonction DIFFERENCE ()  
        DIMENSION, commande  
        Fonction d’insuffisant ()  
        DMY () (fonction)  
    :::column-end:::
    :::column:::
        Commande DO  
        CASSE... Commande ENDCASE  
        DO WHILE... Commande ENDDO  
        Fonction DOW ()  
        DTOC () (fonction)  
        Fonction DTOR ()  
        Fonction DTO ()  
        DTOT () (fonction)  
    :::column-end:::
:::row-end:::

## <a name="e"></a>E  

:::row:::
    :::column:::
        EMPTY (), fonction  
        Commande END TRANSACTION  
        Fonction EOF ()  
    :::column-end:::
    :::column:::
        ERROR () (fonction)  
        Fonction EVALUATe ()  
        EXIT (commande)  
    :::column-end:::
    :::column:::
        EXP () (fonction)  
    :::column-end:::
:::row-end:::

## <a name="f"></a>F  

:::row:::
    :::column:::
        FCOUNT () (fonction)  
        FDATE () (fonction)  
        FIELD (), fonction  
        Fonction FILE ()  
        Fonction FILTER ()  
        FLDLIST () (fonction)  
    :::column-end:::
    :::column:::
        Fonction troupeau ()  
        FLOOR (), fonction  
        FLUSH (commande)  
        Fonction FOR ()  
        POUR... Commande ENDFOR  
        Fonction FOUND ()  
    :::column-end:::
    :::column:::
        Commande FREE TABLE  
        FSIZE (), fonction  
        FTIME () (fonction)  
        Fonction FULLPATH ()  
        Commande de fonction  
        FV (), fonction  
    :::column-end:::
:::row-end:::

## <a name="g"></a>G  

:::row:::
    :::column:::
        Commande GATHER  
        GETCP () (fonction)  
        GETENV () (fonction)  
    :::column-end:::
    :::column:::
        GETFLDSTATE () (fonction)  
        GETNEXTMODIFIED () (fonction)  
        Commande GO/GOTO  
    :::column-end:::
    :::column:::
        GOMONTH () (fonction)  
    :::column-end:::
:::row-end:::

## <a name="h"></a>H  

:::row:::
    :::column:::
        HEADer () (fonction)
    :::column-end:::
    :::column:::
        Fonction HOUR ()
    :::column-end:::
:::row-end:::

## <a name="i"></a>I  

:::row:::
    :::column:::
        IDXCOLLATE () (fonction)  
        IF... Commande ENDIF  
        Fonction IIF ()  
        INDBC () (fonction)  
        INDEX, commande  
        Fonction inList ()  
    :::column-end:::
    :::column:::
        Commande INSERT-SQL  
        Fonction INT ()  
        ISALPHA () (fonction)  
        ISBLANK () (fonction)  
        ISDIGIT () (fonction)  
        ISEXCLUSIVE () (fonction)  
    :::column-end:::
    :::column:::
        ISLEADBYTE () (fonction)  
        ISLOWER () (fonction)  
        ISNULL () (fonction)  
        Fonction ISREADONLY ()  
        ISUPPER () (fonction)  
    :::column-end:::
:::row-end:::

## <a name="k"></a>K  

:::row:::
    :::column:::
        Fonction KEY ()
    :::column-end:::
    :::column:::
        Keymatch (), fonction
    :::column-end:::
:::row-end:::

## <a name="l"></a>L  

:::row:::
    :::column:::
        Fonction LEFT ()  
        LEFTC () (fonction)  
        LEN (), fonction  
        LENC () (fonction)  
        LIKE () (fonction)  
        LIKEC () (fonction)  
    :::column-end:::
    :::column:::
        Commande locale  
        Rechercher, commande  
        LOCK () (fonction)  
        Fonction LOG ()  
        LOG10 (), fonction  
        Fonction LOOKUP ()  
    :::column-end:::
    :::column:::
        Fonction LOWER ()  
        Commande LPARAMETERS  
        Fonction LTRIM ()  
        LUPDATE () (fonction)  
    :::column-end:::
:::row-end:::

## <a name="m"></a>M  

:::row:::
    :::column:::
        Fonction MAX ()  
        Fonction MDX ()  
        MJA () (fonction)  
        MEMLINES () (fonction)  
    :::column-end:::
    :::column:::
        Fonction MESSAGE ()  
        Fonction MIN ()  
        Fonction MINUTE ()  
        _MLINE variable de mémoire système  
    :::column-end:::
    :::column:::
        MLINE () (fonction)  
        Fonction MOD ()  
        Fonction MONTH ()  
        MTON () (fonction)  
    :::column-end:::
:::row-end:::

## <a name="n"></a>N  

:::row:::
    :::column:::
        NDX () (fonction)  
        Fonction NORMALIZe ()  
    :::column-end:::
    :::column:::
        Opérateur NOT  
        Remarque (commande)  
    :::column-end:::
    :::column:::
        NTOM () (fonction)  
        NVL () (fonction)  
    :::column-end:::
:::row-end:::

## <a name="o"></a>O  

:::row:::
    :::column:::
        Fonction se produit ()  
        OLDVAL () (fonction)  
        ON () (fonction)  
    :::column-end:::
    :::column:::
        Commande en cas d’erreur  
        Commande ON KEY  
        OUVRIR une base de données, commande  
    :::column-end:::
    :::column:::
        Opérateur OR  
        ORDER () (fonction)  
        OS () (fonction)  
    :::column-end:::
:::row-end:::

## <a name="p"></a>P  

:::row:::
    :::column:::
        Commande PACK  
        Fonctions PADL () &#124; PADR () &#124; PADC ()  
        PARAMÈTRES, commande  
        Fonction Parameters ()  
        Fonction PAYMENT ()  
    :::column-end:::
    :::column:::
        Fonction PI ()  
        Fonction principale ()  
        Commande privée  
        Commande de procédure  
        Fonction PROGRAM ()  
    :::column-end:::
    :::column:::
        Fonction NOMPROPRE ()  
        Commande publique  
        Fonction PV ()  
    :::column-end:::
:::row-end:::

## <a name="r"></a>R  

:::row:::
    :::column:::
        Fonction RAND ()  
        Fonction RAT ()  
        RATC () (fonction)  
        RATLINE () (fonction)  
        Commande de rappel  
        RECCOUNT () (fonction)  
        RECNO () (fonction)  
        RECSIZE () (fonction)  
    :::column-end:::
    :::column:::
        Commande régionale  
        RELATION () (fonction)  
        Commande REMOVE TABLE  
        Replace, commande  
        REMPLACER à partir d’une commande de tableau  
        Fonction REPLICATE ()  
        Nouvelle tentative, commande  
        Return, commande  
    :::column-end:::
    :::column:::
        Fonction RIGHT ()  
        RIGHTC () (fonction)  
        RLOCK () (fonction)  
        Commande ROLLBACK  
        Fonction ROUND ()  
        RTOD () (fonction)  
        RTRIM () (fonction)  
    :::column-end:::
:::row-end:::

## <a name="s"></a>S  

:::row:::
    :::column:::
        RECHERCHER... Commande ENDSCAN  
        Commande nuages de points  
        Fonction SEC ()  
        Fonction SECONDS ()  
        Commande SEEK  
        Fonction SEEK ()  
        SÉLECTIONNER une commande  
        SELECT () (fonction)  
        SELECT-commande SQL  
        SET () (fonction)  
        SET BLOCKSIZE, commande  
        SET CARRY, commande  
        SET CENTURY, commande  
        SET COLLATE, commande  
        SET DATABASE, commande  
        SET DATE, commande  
        DÉFINIR la commande par défaut  
        SET DELETED, commande  
        SET EXACT, commande  
        SET EXCLUSIVE, commande  
        DÉFINIR la commande FDOW  
    :::column-end:::
    :::column:::
        Commande SET FIELDs  
        DÉFINIR la commande de filtre  
        DÉFINIR la commande fixe  
        DÉFINIR la commande FULLPATH  
        DÉFINIR la commande FWEEK  
        Commande SET HOURs  
        SET INDEX, commande  
        SET LOCK, commande  
        DÉFINIR la commande de multiverrouillages  
        DÉFINIR la commande NEAR  
        DÉFINIR la commande NOCPTRANS  
        DÉFINIR la commande NOTIFY  
        SET NULL, commande  
        SET OPTIMIZE, commande  
        Commande SET ORDER  
        SET PATH, commande  
        Commande SET PROCEDURE  
        DÉFINIR la RELATION, commande  
        SET RELATION OFF (commande)  
        SET REPROCESS, commande  
        SET SKIP, commande  
    :::column-end:::
    :::column:::
        DÉFINIR la commande UDFPARMS  
        SET UNIQUE, commande  
        SET VOLUME, commande  
        SETFLDSTATE () (fonction)  
        SIGN () (fonction)  
        SIN () (fonction)  
        SKIP (commande)  
        Commande de tri  
        Fonction SPACE ()  
        Fonction SQRT ()  
        Commande STORE  
        Fonction STR ()  
        Fonction STRCONV ()  
        STRTRAN () (fonction)  
        Fonction STUFF ()  
        STUFFC () (fonction)  
        SUBSTR () (fonction)  
        SUBSTRC () (fonction)  
        SUM, commande  
        Fonction SYS (2011)  
    :::column-end:::
:::row-end:::

## <a name="t"></a>T  

:::row:::
    :::column:::
        TABLEREVERT () (fonction)  
        TABLEUPDATE () (fonction)  
        TAG () (fonction)  
        TAGCOUNT () (fonction)  
        TAGNO () (fonction)  
        _TALLY variable de mémoire système  
    :::column-end:::
    :::column:::
        TAN (), fonction  
        Fonction TARGET ()  
        Fonction TIME ()  
        TOTAL, commande  
        _TRIGGERLEVEL variable de mémoire système  
        Fonction TRIM ()  
    :::column-end:::
    :::column:::
        TTOC () (fonction)  
        TTOD () (fonction)  
        TXNLEVEL () (fonction)  
        Fonction TYPE ()  
    :::column-end:::
:::row-end:::

## <a name="u"></a>U  

:::row:::
    :::column:::
        Fonction UNIQUE ()  
        Commande UNLOCK  
        Commande UPDATE  
    :::column-end:::
    :::column:::
        UPDATE, commande SQL  
        Fonction UPPER ()  
        UTILISER la commande  
    :::column-end:::
    :::column:::
        Fonction USED ()  
    :::column-end:::
:::row-end:::

## <a name="v"></a>V  

:::row:::
    :::column:::
        Fonction VAL ()
    :::column-end:::
    :::column:::
        VERSION () (fonction)
    :::column-end:::
:::row-end:::

## <a name="w"></a>W  

Fonction WEEK ()

## <a name="y"></a>O  

Fonction YEAR ()

## <a name="z"></a>Z  

Commande ZAP
