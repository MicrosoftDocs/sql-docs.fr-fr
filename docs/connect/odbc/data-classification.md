---
description: Utilisation de la classification de données avec Microsoft ODBC Driver for SQL Server
title: Utilisation de la classification de données avec Microsoft ODBC Driver for SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
author: v-makouz
ms.author: v-makouz
manager: kenvh
ms.openlocfilehash: 3b97e6407f541657acee2b15097d3fbb877854a9
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837413"
---
# <a name="data-classification"></a>Classification des données
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

## <a name="overview"></a>Vue d’ensemble
Dans le cadre de la gestion des données sensibles, SQL Server et Azure SQL Server ont introduit la possibilité de fournir des colonnes de base de données avec des métadonnées de sensibilité permettant à l’application cliente de gérer différents types de données sensibles (telles que la santé, la finance, etc.) conformément aux stratégies de protection des données.

Pour plus d’informations sur la façon d’attribuer une classification à des colonnes, consultez [Découverte et classification des données SQL](../../relational-databases/security/sql-data-discovery-and-classification.md).

Le pilote Microsoft ODBC 17.2 permet la récupération de ces métadonnées via SQLGetDescField à l’aide de l’identificateur de champ SQL_CA_SS_DATA_CLASSIFICATION.

## <a name="format"></a>Format
SQLGetDescField a la syntaxe suivante :

```  
SQLRETURN SQLGetDescField(  
     SQLHDESC        DescriptorHandle,  
     SQLSMALLINT     RecNumber,  
     SQLSMALLINT     FieldIdentifier,  
     SQLPOINTER      ValuePtr,  
     SQLINTEGER      BufferLength,  
     SQLINTEGER *    StringLengthPtr);  
```
*DescriptorHandle*  
 [Entrée] Descripteur de ligne d’implémentation (IRD). Peut être récupéré par un appel à SQLGetStmtAttr avec l’attribut d’instruction SQL_ATTR_IMP_ROW_DESC
  
 *RecNumber*  
 [Entrée] 0
  
 *FieldIdentifier*  
 [Entrée] SQL_CA_SS_DATA_CLASSIFICATION
  
 *ValuePtr*  
 [Sortie] Mémoire tampon de sortie
  
 *BufferLength*  
 [Entrée] Longueur de la mémoire tampon de sortie en octets

 *StringLengthPtr* [Sortie] Pointeur vers la mémoire tampon dans laquelle renvoyer le nombre total d’octets disponibles à renvoyer dans *ValuePtr*.
 
> [!NOTE]
> Si la taille de la mémoire tampon est inconnue, elle peut être déterminée en appelant SQLGetDescField avec *ValuePtr* avec la valeur NULL et en examinant la valeur de *StringLengthPtr*.
 
Si les informations de classification des données ne sont pas disponibles, une erreur *Champ de descripteur non valide* est renvoyée.

Lors d’un appel réussi à SQLGetDescField, la mémoire tampon vers laquelle pointe *ValuePtr* contient les données suivantes :

 `nn nn [n sensitivitylabels] tt tt [t informationtypes] cc cc [c columnsensitivitys]`

> [!NOTE]
> `nn nn`, `tt tt` et `cc cc` sont des entiers multioctets, qui sont stockés avec l’octet le moins significatif à l’adresse la plus basse.

*`sensitivitylabel`* et *`informationtype`* sont au format

 `nn [n bytes name] ii [i bytes id]`

*`columnsensitivity`* est au format

 `nn nn [n sensitivityprops]`

Pour chaque colonne *(c)* , *n* *`sensitivityprops`* de 4 octets sont présents :

 `ss ss tt tt`

s - index dans le tableau *`sensitivitylabels`* , `FF FF` s’il n’est pas étiqueté

t - index dans le tableau *`informationtypes`* , `FF FF` s’il n’est pas étiqueté


<br><br>
Le format des données peut être exprimé sous la forme des pseudo-structures suivantes :

```
struct IDnamePair {
 BYTE nameLen;
 USHORT name[nameLen];
 BYTE idLen;
 USHORT id[idLen];
};

struct SensitivityProp {
 USHORT labelIdx;
 USHORT infoTypeIdx;
};

USHORT nLabels;
struct IDnamePair labels[nLabels];
USHORT nInfoTypes;
struct IDnamePair infotypes[nInfoTypes];
USHORT nColumns;
struct {
 USHORT nProps;
 struct SensitivityProp[nProps];
} columnClassification[nColumns];
```


## <a name="code-sample"></a>Exemple de code
Application de test qui montre comment lire les métadonnées de classification des données. Sur Windows, elle peut être compilée à l’aide de `cl /MD dataclassification.c /I (directory of msodbcsql.h) /link odbc32.lib` et exécutée avec une chaîne de connexion et une requête SQL (qui renvoie des colonnes classifiées) en tant que paramètres :

```
#ifdef _WIN32
#include <windows.h>
#endif
#include <sql.h>
#include <sqlext.h>
#include <msodbcsql.h>
#include <stdio.h>
SQLHANDLE env, dbc, stmt;
void checkRC_exit(SQLRETURN rc, SQLHANDLE hand, SQLSMALLINT htype, int retcode, char *action)
{
    if ((rc == SQL_ERROR || rc == SQL_SUCCESS_WITH_INFO) && hand)
    {
        char msg[1024], state[6];
        int i = 0;
        SQLRETURN rc2;
        SQLINTEGER err;
        SQLSMALLINT lenout;
        while ((rc2 = SQLGetDiagRec(htype, hand, ++i, state, &err, msg, sizeof(msg), &lenout)) == SQL_SUCCESS ||
            rc2 == SQL_SUCCESS_WITH_INFO)
            printf("%d (%d)[%s]%s\n", i, err, state, msg);
    }
    if (rc == SQL_ERROR && retcode)
    {
        printf("Error occurred%s%s\n", action ? " upon " : "", action ? action : "");
        exit(retcode);
    }
}
void printLabelInfo(char *type, char **pptr)
{
    char *ptr = *pptr;
    unsigned short nlabels;
    printf("----- %s(%u) -----\n", type, nlabels = *(unsigned short*)ptr);
    ptr += sizeof(unsigned short);
    while (nlabels--)
    {
        int namelen, idlen;
        char *nameptr, *idptr;
        namelen = *ptr++;
        nameptr = ptr;
        ptr += namelen * 2;
        idlen = *ptr++;
        idptr = ptr;
        ptr += idlen * 2;
        wprintf(L"Name: \"%.*s\" Id: \"%.*s\"\n", namelen, nameptr, idlen, idptr);
    }
    *pptr = ptr;
}
int main(int argc, char **argv)
{
    unsigned char *dcbuf;
    unsigned int dclen = 0;
    SQLRETURN rc;
    SQLHANDLE ird;
    if (argc < 3)
    {
        fprintf(stderr, "usage: dataclassification connstr query\n");
        return 1;
    }
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_ENV, 0, &env), 0, 0,
        2, "allocate environment");
    checkRC_exit(SQLSetEnvAttr(env, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, 0), env, SQL_HANDLE_ENV,
        3, "set ODBC version");
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_DBC, env, &dbc), env, SQL_HANDLE_ENV,
        4, "allocate connection");
    checkRC_exit(SQLDriverConnect(dbc, 0, argv[1], SQL_NTS, 0, 0, 0, SQL_DRIVER_NOPROMPT), dbc, SQL_HANDLE_DBC,
        5, "connect to server");
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_STMT, dbc, &stmt), dbc, SQL_HANDLE_DBC,
        6, "allocate statement");
    checkRC_exit(SQLExecDirect(stmt, argv[2], SQL_NTS), stmt, SQL_HANDLE_STMT,
        7, "execute query");
    checkRC_exit(SQLGetStmtAttr(stmt, SQL_ATTR_IMP_ROW_DESC, (SQLPOINTER)&ird, SQL_IS_POINTER, 0), stmt, SQL_HANDLE_STMT,
        8, "get IRD handle");
    rc = SQLGetDescFieldW(ird, 0, SQL_CA_SS_DATA_CLASSIFICATION, dcbuf, 0, &dclen);
    if (rc != SQL_SUCCESS && rc != SQL_SUCCESS_WITH_INFO)
    {
        checkRC_exit(rc, ird, SQL_HANDLE_DESC, 0, 0);
        printf("Error reading SQL_CA_SS_DATA_CLASSIFICATION IRD field. Ensure that the driver and\n"
            "server both support the Data Classification feature, and that the query returns columns\n"
            "with classification information.\n");
    }
    else
    {
        SQLINTEGER dclenout;
        unsigned char *dcptr;
        unsigned short ncols;
        printf("Data Classification information (%u bytes):\n", dclen);
        if (!(dcbuf = malloc(dclen)))
        {
            printf("Memory Allocation Error");
            return 9;
        }
        checkRC_exit(SQLGetDescFieldW(ird, 0, SQL_CA_SS_DATA_CLASSIFICATION, dcbuf, dclen, &dclenout),
            ird, SQL_HANDLE_DESC, 10, "reading SQL_CA_SS_DATA_CLASSIFICATION");
        dcptr = dcbuf;
        printLabelInfo("Labels", &dcptr);
        printLabelInfo("Information Types", &dcptr);
        printf("----- Column Sensitivities(%u) -----\n", ncols = *(unsigned short*)dcptr);
        dcptr += sizeof(unsigned short);
        while (ncols--)
        {
            unsigned short nprops = *(unsigned short*)dcptr;
            dcptr += sizeof(unsigned short);
            while (nprops--)
            {
                unsigned short labelidx, typeidx;
                labelidx = *(unsigned short*)dcptr; dcptr += sizeof(unsigned short);
                typeidx = *(unsigned short*)dcptr; dcptr += sizeof(unsigned short);
                printf(labelidx == 0xFFFF ? "(none) " : "%u ", labelidx);
                printf(typeidx == 0xFFFF ? "(none)\n" : "%u\n", typeidx);
            }
            printf("-----\n");
        }
        if (dcptr != dcbuf + dclen)
        {
            printf("Error: unexpected parse of DATACLASSIFICATION data\n");
            return 11;
        }
        free(dcbuf);
    }
    return 0;
}
```

## <a name="supported-version"></a><a name="bkmk-version"></a>Version prise en charge
Microsoft ODBC Driver 17.2 permet d’extraire les informations de classification des données via `SQLGetDescField` si`FieldIdentifier` est défini sur `SQL_CA_SS_DATA_CLASSIFICATION` (1237). 

À compter de Microsoft ODBC Driver 17.4.1.1, il est possible de récupérer la version de classification des données prise en charge par un serveur par le biais de `SQLGetDescField` à l’aide de l’identificateur de champ `SQL_CA_SS_DATA_CLASSIFICATION_VERSION` (1238). Dans la version 17.4.1.1, la version de classification des données prise en charge est définie sur « 2 ».

 

La version 17.4.2.1 a introduit la version par défaut de la classification des données avec la valeur « 1 », et est la version signalée à SQL Server comme étant prise en charge. Le nouvel attribut de connexion `SQL_COPT_SS_DATACLASSIFICATION_VERSION` (1400) peut permettre à l’application de modifier la version prise en charge de la classification des données de « 1 » jusqu’à la valeur maximale prise en charge.  

Exemple : 

Pour définir la version, cet appel doit être effectué juste avant l’appel à SQLConnect ou SQLDriverConnect :
```
ret = SQLSetConnectAttr(dbc, SQL_COPT_SS_DATACLASSIFICATION_VERSION, (SQLPOINTER)2, SQL_IS_INTEGER);
```

La valeur de la version actuellement prise en charge de la classification des données peut être récupérée via l’appel SQLGetConnectAttr : 
```
ret = SQLGetConnectAttr(dbc, SQL_COPT_SS_DATACLASSIFICATION_VERSION, (SQLPOINTER)&dataClassVersion, SQL_IS_INTEGER, 0);
```