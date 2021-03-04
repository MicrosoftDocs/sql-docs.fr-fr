---
title: Fournisseurs de magasins de clés personnalisés
description: Découvrez comment implémenter un fournisseur de magasins de clés personnalisé pour une utilisation avec le pilote ODBC pour SQL Server et la fonctionnalité Always Encrypted.
ms.custom: ''
ms.date: 07/12/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: a6166d7d-ef34-4f87-bd1b-838d3ca59ae7
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: f1f075c7ea2e80ec1760012c33ed9f719837c036
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837428"
---
# <a name="custom-keystore-providers"></a>Fournisseurs de magasins de clés personnalisés
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

## <a name="overview"></a>Vue d’ensemble

La fonctionnalité de chiffrement de colonne de SQL Server 2016 requiert que les clés de chiffrement de colonne chiffrées (ECEK) stockées sur le serveur soient récupérées par le client, puis déchiffrées en clés de chiffrement de colonne (CEK) afin d’accéder aux données stockées dans des colonnes chiffrées. Les clés ECEK sont chiffrées par les clés principales de colonne (CMK), et la sécurité de la clé CMK est importante pour la sécurité du chiffrement de colonne. Ainsi, la clé CMK doit être stockée dans un emplacement sécurisé. L’objectif d’un fournisseur de magasins de clés de chiffrement de colonne consiste à fournir une interface permettant au pilote ODBC d’accéder à ces clés CMK stockées en toute sécurité. Pour les utilisateurs disposant de leur propre stockage sécurisé, l’interface personnalisée du fournisseur de magasin de clés fournit une infrastructure afin d’implémenter l’accès au stockage sécurisé des clés CMK pour le pilote ODBC, qui permet alors d’effectuer le chiffrement et le déchiffrement de la clé CEK.

Chaque fournisseur de magasin de clés contient et gère une ou plusieurs clés CMK, identifiées par des chemins de clés - chaînes d’un format défini par le fournisseur. Ce CMK, avec l’algorithme de chiffrement, également une chaîne définie par le fournisseur, permet d’effectuer le chiffrement d’une clé CEK et le déchiffrement d’une clé ECEK. L’algorithme, ainsi que la clé ECEK et le nom du fournisseur sont stockés dans les métadonnées de chiffrement de la base de données. Pour plus d’informations, consultez [CREATE COLUMN MASTER KEY](../../t-sql/statements/create-column-master-key-transact-sql.md) et [CREATE COLUMN ENCRYPTION KEY](../../t-sql/statements/create-column-encryption-key-transact-sql.md). Ainsi, les deux opérations fondamentales de gestion des clés sont les suivantes :

```
CEK = DecryptViaCEKeystoreProvider(CEKeystoreProvider_name, Key_path, Key_algorithm, ECEK)

-and-

ECEK = EncryptViaCEKeystoreProvider(CEKeyStoreProvider_name, Key_path, Key_algorithm, CEK)
```

où `CEKeystoreProvider_name` sert à identifier le fournisseur de magasin de clés de chiffrement de colonne spécifique (CEKeystoreProvider), et les autres arguments sont utilisés par CEKeystoreProvider pour chiffrer/déchiffrer la clé (E)CEK. Le nom et le chemin de la clé sont fournis par les métadonnées CMK, tandis que l’algorithme et la valeur ECEK sont fournies par les métadonnées CEK. Plusieurs fournisseurs de magasin de clés peuvent être présents avec le ou les fournisseurs intégrés par défaut. Lors de l’exécution d’une opération qui requiert la clé CEK, le pilote utilise les métadonnées CMK pour rechercher le fournisseur de magasin de clés approprié par nom, puis exécute son opération de déchiffrement, qui peut être exprimée comme suit :

```
CEK = CEKeyStoreProvider_specific_decrypt(Key_path, Key_algorithm, ECEK)
```

Bien que le pilote n’ait pas besoin de chiffrer les clés CEK, un outil de gestion de clés peut nécessiter cette opération afin de mettre en œuvre des opérations telles que la création et la rotation CMK. Ces actions requièrent l’exécution de l’opération inverse :

```
ECEK = CEKeyStoreProvider_specific_encrypt(Key_path, Key_algorithm, CEK)
```

### <a name="cekeystoreprovider-interface"></a>Interface CEKeyStoreProvider

Ce document décrit en détail l’interface CEKeyStoreProvider. Un fournisseur de magasin de clés qui implémente cette interface peut être utilisé par Microsoft ODBC Driver pour SQL Server. Les implémenteurs CEKeyStoreProvider peuvent utiliser ce guide pour développer des fournisseurs de clés personnalisés utilisables par le pilote.

Une bibliothèque de fournisseurs de magasin de clés (« bibliothèque de fournisseur ») est une bibliothèque de liens dynamiques qui peut être chargée par le pilote ODBC et qui contient un ou plusieurs fournisseurs de magasin de clés. Le symbole `CEKeystoreProvider` doit être exporté par une bibliothèque de fournisseur et représenter l’adresse d’un tableau de pointeurs se terminant par un caractère Null vers des structures `CEKeystoreProvider`, une pour chaque fournisseur de magasin de clés dans la bibliothèque.

Une structure `CEKeystoreProvider` définit les points d’entrée d’un fournisseur de magasin de clés unique :

```
typedef struct CEKeystoreProvider {
    wchar_t *Name;
    int (*Init)(CEKEYSTORECONTEXT *ctx, errFunc *onError);
    int (*Read)(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int *len);
    int (*Write)(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int len);
    int (*DecryptCEK)(  CEKEYSTORECONTEXT *ctx,
                        errFunc *onError,
                        const wchar_t *keyPath,
                        const wchar_t *alg,
                        unsigned char *ecek,
                        unsigned short ecekLen,
                        unsigned char **cekOut,
                        unsigned short *cekLen);
    int (*EncryptCEK)(  CEKEYSTORECONTEXT *ctx,
                        errFunc *onError,
                        const wchar_t *keyPath,
                        const wchar_t *alg,
                        unsigned char *cek,
                        unsigned short cekLen,
                        unsigned char **ecekOut,
                        unsigned short *ecekLen);
    void (*Free)();
} CEKEYSTOREPROVIDER;
```

|Nom du champ|Description|
|:--|:--|
|`Name`|Nom du fournisseur de magasins de clés. Il ne doit pas être identique à celui d'un autre fournisseur de clés préalablement chargé par le pilote ou présent dans cette bibliothèque. Chaîne à caractères larges* se terminant par le caractère Null.|
|`Init`|Fonction d’initialisation. Si aucune fonction d’initialisation n’est requise, ce champ peut avoir la valeur Null.|
|`Read`|Fonction de lecture du fournisseur. Peut avoir la valeur Null si pas obligatoire.|
|`Write`|Fonction d’écriture du fournisseur. Obligatoire si Read n’a pas la valeur Null. Peut avoir la valeur Null si pas obligatoire.|
|`DecryptCEK`|Fonction de déchiffrement ECEK. Cette fonction est la raison d'être d'un fournisseur de clés, et ne doit pas avoir la valeur Null.|
|`EncryptCEK`|Fonction de chiffrement CEK. Le pilote n'appelle pas cette fonction, mais elle est fournie pour permettre un accès par programmation à la création de clés ECEK par les outils de gestion des clés. Peut avoir la valeur Null si pas obligatoire.|
|`Free`|Fonction d'arrêt. Peut avoir la valeur Null si pas obligatoire.|

À l’exception de Free, les fonctions de cette interface comportent toutes une paire de paramètres, **CTX** et **onError**. Le premier identifie le contexte dans lequel la fonction est appelée, tandis que le second est utilisé pour signaler les erreurs. Pour plus d’informations, consultez [Contextes](#context-association) et [Gestion des erreurs](#error-handling) ci-dessous.

```
int Init(CEKEYSTORECONTEXT *ctx, errFunc onError);
```
Nom de l’espace réservé d’une fonction d'initialisation définie par le fournisseur. Le pilote appelle cette fonction une fois, après le chargement d'un fournisseur, mais avant la première fois qu'il en a besoin pour déchiffrer des clés ECEK ou exécuter des requêtes Read()/Write(). Utilisez cette fonction pour effectuer l’initialisation requise.

|Argument|Description|
|:--|:--|
|`ctx`|[Input] Contexte d'opération.|
|`onError`|[Input] Fonction de création de rapports d’erreurs.|
|`Return Value`|Retourne une valeur différente de zéro pour indiquer la réussite, ou zéro pour indiquer l’échec.|

```
int Read(CEKEYSTORECONTEXT *ctx, errFunc onError, void *data, unsigned int *len);
```

Nom de l’espace réservé d’une fonction de communication définie par le fournisseur. Le pilote appelle cette fonction lorsque l'application demande à lire les données d'un fournisseur (précédemment écrites) en utilisant l'attribut de connexion SQL_COPT_SS_CEKEYSTOREDATA, permettant à l'application de lire des données arbitraires du fournisseur. Pour plus d’informations, consultez [Communication avec les fournisseurs de magasins de clés](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#communicating-with-keystore-providers).

|Argument|Description|
|:--|:--|
|`ctx`|[Input] Contexte d'opération.|
|`onError`|[Input] Fonction de création de rapports d’erreurs.|
|`data`|[Output] Pointeur vers une mémoire tampon dans laquelle le fournisseur écrit des données à lire par l'application. Ce tampon correspond au champ data de la structure CEKEYSTOREDATA.|
|`len`|[InOut] Pointeur vers une valeur de longueur ; lors de l’entrée, il s’agit de la longueur maximale de la mémoire tampon de données, et le fournisseur ne doit pas y écrire plus de *len octets. Lors du retour, le fournisseur doit mettre à jour la valeur *len avec le nombre d’octets écrits.|
|`Return Value`|Retourne une valeur différente de zéro pour indiquer la réussite, ou zéro pour indiquer l’échec.|

```
int Write(CEKEYSTORECONTEXT *ctx, errFunc onError, void *data, unsigned int len);
```
Nom de l’espace réservé d’une fonction de communication définie par le fournisseur. Le pilote appelle cette fonction lorsque l'application demande à écrire les données vers un fournisseur en utilisant l'attribut de connexion SQL_COPT_SS_CEKEYSTOREDATA, permettant à l'application d’écrire des données arbitraires vers le fournisseur. Pour plus d’informations, consultez [Communication avec les fournisseurs de magasins de clés](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#communicating-with-keystore-providers).

|Argument|Description|
|:--|:--|
|`ctx`|[Input] Contexte d'opération.|
|`onError`|[Input] Fonction de création de rapports d’erreurs.|
|`data`|[Input] Pointeur vers une mémoire tampon contenant les données que le fournisseur doit lire. Ce tampon correspond au champ data de la structure CEKEYSTOREDATA. Le fournisseur ne doit pas lire plus de len octets à partir de cette mémoire tampon.|
|`len`|[Input] Nombre d’octets disponibles dans les données. Cela correspond au champ dataSize de la structure CEKEYSTOREDATA.|
|`Return Value`|Retourne une valeur différente de zéro pour indiquer la réussite, ou zéro pour indiquer l’échec.|

```
int (*DecryptCEK)( CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg, unsigned char *ecek, unsigned short ecekLen, unsigned char **cekOut, unsigned short *cekLen);
```
Nom de l’espace réservé d’une fonction de déchiffrement ECEK définie par le fournisseur. Le pilote appelle cette fonction pour déchiffrer une clé ECEK chiffrée par une clé CMK associée à ce fournisseur dans une clé CEK.

|Argument|Description|
|:--|:--|
|`ctx`|[Input] Contexte d'opération.|
|`onError`|[Input] Fonction de création de rapports d’erreurs.|
|`keyPath`|[Input] Valeur de l'attribut de métadonnées [KEY_PATH](../../t-sql/statements/create-column-master-key-transact-sql.md) pour la clé CMK référencée par la clé ECEK donnée. Chaîne à caractères larges* se terminant par le caractère Null. Cette valeur vise à identifier une clé CMK gérée par ce fournisseur.|
|`alg`|[Input] Valeur de l’attribut de métadonnées [ALGORITHM](../../t-sql/statements/create-column-encryption-key-transact-sql.md) pour la clé ECEK donnée. Chaîne à caractères larges* se terminant par le caractère Null. Cette valeur vise à identifier l’algorithme de chiffrement utilisé pour chiffrer la clé ECEK donnée.|
|`ecek`|[Input] Pointeur vers la clé ECEK à déchiffrer.|
|`ecekLen`|[Input] Longueur de la clé ECEK.|
|`cekOut`|[Output] Le fournisseur doit allouer de la mémoire pour la clé ECEK déchiffrée et écrire son adresse sur le pointeur pointé par cekOut. Il doit être possible de libérer ce bloc de mémoire à l’aide de la fonction [LocalFree](/windows/desktop/api/winbase/nf-winbase-localfree) (Windows) ou Free (Linux/macOS). Si aucune mémoire n’a été allouée en raison d’une erreur ou d’un autre problème, le fournisseur doit définir *cekOut sur un pointeur Null.|
|`cekLen`|[Output] Le fournisseur doit écrire dans l’adresse pointée par cekLen la longueur de la clé ECEK déchiffrée dans laquelle il a écrit **cekOut.|
|`Return Value`|Retourne une valeur différente de zéro pour indiquer la réussite, ou zéro pour indiquer l’échec.|

```
int (*EncryptCEK)( CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg, unsigned char *cek,unsigned short cekLen, unsigned char **ecekOut, unsigned short *ecekLen);
```
Nom de l’espace réservé d’une fonction de chiffrement CEK définie par le fournisseur. Le pilote n'appelle pas cette fonction ni n’expose sa fonctionnalité via l’interface ODBC, mais elle est fournie pour permettre un accès par programmation à la création de clés ECEK par les outils de gestion des clés.

|Argument|Description|
|:--|:--|
|`ctx`|[Input] Contexte d'opération.|
|`onError`|[Input] Fonction de création de rapports d’erreurs.|
|`keyPath`|[Input] Valeur de l'attribut de métadonnées [KEY_PATH](../../t-sql/statements/create-column-master-key-transact-sql.md) pour la clé CMK référencée par la clé ECEK donnée. Chaîne à caractères larges* se terminant par le caractère Null. Vise à identifier une clé CMK gérée par ce fournisseur.|
|`alg`|[Input] Valeur de l’attribut de métadonnées [ALGORITHM](../../t-sql/statements/create-column-encryption-key-transact-sql.md) pour la clé ECEK donnée. Chaîne à caractères larges* se terminant par le caractère Null. Cette valeur vise à identifier l’algorithme de chiffrement utilisé pour chiffrer la clé ECEK donnée.|
|`cek`|[Input] Pointeur vers la clé CEK à chiffrer.|
|`cekLen`|[Input] Longueur de la clé CEK.|
|`ecekOut`|[Output] Le fournisseur doit allouer de la mémoire pour la clé CEK chiffrée et écrire son adresse sur le pointeur pointé par ecekOut. Il doit être possible de libérer ce bloc de mémoire à l’aide de la fonction [LocalFree](/windows/desktop/api/winbase/nf-winbase-localfree) (Windows) ou Free (Linux/macOS). Si aucune mémoire n’a été allouée en raison d’une erreur ou d’un autre problème, le fournisseur doit définir *ecekOut sur un pointeur Null.|
|`ecekLen`|[Output] Le fournisseur doit écrire dans l’adresse pointée par ecekLen la longueur de la clé CEK chiffrée dans laquelle il a écrit **ecekOut.|
|`Return Value`|Retourne une valeur différente de zéro pour indiquer la réussite, ou zéro pour indiquer l’échec.|

```
void (*Free)();
```
Nom de l’espace réservé d’une fonction d’arrêt définie par le fournisseur. Le pilote peut appeler cette fonction à l’arrêt normal du processus.

> [!NOTE]
> *Les chaînes à caractères larges sont des caractères à 2 octets (UTF-16) en raison de la façon dont SQL Server les stocke.*


### <a name="error-handling"></a>Gestion des erreurs

Des erreurs pouvant se produire pendant le traitement d’un fournisseur, un mécanisme est fourni pour lui permettre de signaler des erreurs au pilote dans des détails plus spécifiques qu’une valeur booléenne de réussite/échec. La plupart des fonctions disposent d’une paire de paramètres, **CTX** et **onError**, utilisés conjointement à cet effet, en plus de la valeur de retour de réussite/échec.

Le paramètre **ctx** identifie le contexte dans lequel se déroule une opération du fournisseur.

Le paramètre **onError** pointe vers une fonction de rapport d’erreurs, avec le prototype suivant :

`typedef void errFunc(CEKEYSTORECONTEXT *ctx, const wchar_t *msg, ...);`

|Argument|Description|
|:--|:--|
|`ctx`|[Input] Contexte du signalement de l’erreur.|
|`msg`|[Input] Message d'erreur à signaler. Chaîne à caractères larges se terminant par le caractère Null. Pour permettre la présence d’informations paramétrables, cette chaîne peut contenir des séquences de mise en forme d’insertion acceptées par la fonction [FormatMessage](/windows/desktop/api/winbase/nf-winbase-formatmessage). La fonctionnalité étendue peut être spécifiée par ce paramètre, comme décrit ci-dessous.|
|...|[Input] Paramètres variadiques supplémentaires pour ajuster les spécificateurs de format dans le message, le cas échéant.|

Pour signaler qu’une erreur s’est produite, le fournisseur appelle onError, en spécifiant le paramètre de contexte passé dans la fonction de fournisseur par le pilote ainsi qu’un message d’erreur avec des paramètres supplémentaires facultatifs à mettre en forme. Le fournisseur peut appeler cette fonction plusieurs fois pour envoyer consécutivement plusieurs messages d’erreur dans un appel de fonction de fournisseur. Par exemple :

```
    if (!doSomething(...))
    {
        onError(ctx, L"An error occurred in doSomething.");
        onError(ctx, L"Additional error message with more details.");
        return 0;
    }
```


Le paramètre `msg` représente généralement une chaîne de caractères larges, mais des extensions supplémentaires sont disponibles :

En utilisant l’une des valeurs prédéfinies spéciales avec la macro IDS_MSG, vous pouvez utiliser des messages d’erreur génériques déjà existants et sous une forme localisée dans le pilote. Par exemple, si un fournisseur ne parvient pas à allouer de la mémoire, le message `IDS_S1_001` « Échec de l'allocation de mémoire » peut être utilisé :

`onError(ctx, IDS_MSG(IDS_S1_001));`

Pour que l’erreur soit reconnue par le pilote, la fonction du fournisseur doit retourner un échec. En cas d’échec dans le contexte d’une opération ODBC, les erreurs publiées deviennent accessibles sur la connexion ou le descripteur d’instruction par le biais du mécanisme de diagnostic ODBC standard (`SQLError`, `SQLGetDiagRec`et `SQLGetDiagField`).


### <a name="context-association"></a>Association de contextes

La structure `CEKEYSTORECONTEXT`, en plus de fournir le contexte pour le rappel d’erreur, permet également de déterminer le contexte ODBC dans lequel une opération de fournisseur est exécutée. Ce contexte permet à un fournisseur d’associer des données à chacun de ces contextes, par exemple pour implémenter une configuration par connexion. À cet effet, la structure contient trois pointeurs opaques correspondant à l’environnement, à la connexion et au contexte d’instruction :

```
typedef struct CEKeystoreContext
{
void *envCtx;
void *dbcCtx;
void *stmtCtx;
} CEKEYSTORECONTEXT;
```

|Champ|Description|
|:--|:--|
|`envCtx`|Contexte de l’environnement.|
|`dbcCtx`|Contexte de la connexion.|
|`stmtCtx`|Contexte de l’instruction.|

Chacun de ces contextes représente une valeur opaque qui, bien qu’elle ne corresponde pas au descripteur ODBC correspondant, peut être utilisée comme identificateur unique pour le descripteur : si le descripteur *X* est associé à la valeur de contexte *Y*, aucun autre environnement, descripteur d’instruction ni connexion qui existe simultanément avec *X* n’aura une valeur de contexte *Y*, et aucune autre valeur de contexte ne sera associée au descripteur *X*. Si l’opération du fournisseur en cours d’exécution ne dispose pas d’un contexte de descripteur particulier, (par exemple, les appels SQLSetConnectAttr pour charger et configurer des fournisseurs, dans lesquels il n’y a aucun descripteur d’instruction), la valeur de contexte correspondante dans la structure est Null.


## <a name="example"></a>Exemple

### <a name="keystore-provider"></a>Fournisseurs de magasins de clés

Le code suivant est un exemple d’implémentation minimale d’un fournisseur de magasin de clés.

```
/* Custom Keystore Provider Example

Windows:   compile with cl MyKSP.c /LD /MD /link /out:MyKSP.dll
Linux/macOS: compile with gcc -fshort-wchar -fPIC -o MyKSP.so -shared MyKSP.c

 */

#ifdef _WIN32
#include <windows.h>
#else
#define __stdcall
#endif

#include <stdlib.h>
#include <sqltypes.h>
#include "msodbcsql.h"
#include <sql.h>
#include <sqlext.h>

int __stdcall KeystoreInit(CEKEYSTORECONTEXT *ctx, errFunc *onError) {
    printf("KSP Init() function called\n");
    return 1;
}

static unsigned char *g_encryptKey;
static unsigned int g_encryptKeyLen;

int __stdcall KeystoreWrite(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int len) {
    printf("KSP Write() function called (%d bytes)\n", len);
    if (len) {
        if (g_encryptKey)
            free(g_encryptKey);
        g_encryptKey = malloc(len);
        if (!g_encryptKey) {
            onError(ctx, L"Memory Allocation Error");
            return 0;
        }
        memcpy(g_encryptKey, data, len);
        g_encryptKeyLen = len;
    }
    return 1;
}

// Very simple "encryption" scheme - rotating XOR with the key
int __stdcall KeystoreDecrypt(CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg,
    unsigned char *ecek, unsigned short ecekLen, unsigned char **cekOut, unsigned short *cekLen) {
    unsigned int i;
    printf("KSP Decrypt() function called (keypath=%S alg=%S ecekLen=%u)\n", keyPath, alg, ecekLen);
    if (wcscmp(keyPath, L"TheOneAndOnlyKey")) {
        onError(ctx, L"Invalid key path");
        return 0;
    }
    if (wcscmp(alg, L"none")) {
        onError(ctx, L"Invalid algorithm");
        return 0;
    }
    if (!g_encryptKey) {
        onError(ctx, L"Keystore provider not initialized with key");
        return 0;
    }
#ifndef _WIN32
    *cekOut = malloc(ecekLen);
#else
    *cekOut = LocalAlloc(LMEM_FIXED, ecekLen);
#endif
    if (!*cekOut) {
        onError(ctx, L"Memory Allocation Error");
        return 0;
    }
    *cekLen = ecekLen;
    for (i = 0; i < ecekLen; i++)
        (*cekOut)[i] = ecek[i] ^ g_encryptKey[i % g_encryptKeyLen];
    return 1;
}

// Note that in the provider interface, this function would be referenced via the CEKEYSTOREPROVIDER
// structure. However, that does not preclude keystore providers from exporting their own functions,
// as illustrated by this example where the encryption is performed via a separate function (with a
// different prototype than the one in the KSP interface.)
#ifdef _WIN32
__declspec(dllexport)
#endif
int KeystoreEncrypt(CEKEYSTORECONTEXT *ctx, errFunc *onError,
    unsigned char *cek, unsigned short cekLen,
    unsigned char **ecekOut, unsigned short *ecekLen) {
    unsigned int i;
    printf("KSP Encrypt() function called (cekLen=%u)\n", cekLen);
    if (!g_encryptKey) {
        onError(ctx, L"Keystore provider not initialized with key");
        return 0;
    }
    *ecekOut = malloc(cekLen);
    if (!*ecekOut) {
        onError(ctx, L"Memory Allocation Error");
        return 0;
    }
    *ecekLen = cekLen;
    for (i = 0; i < cekLen; i++)
        (*ecekOut)[i] = cek[i] ^ g_encryptKey[i % g_encryptKeyLen];
    return 1;
}

CEKEYSTOREPROVIDER MyCustomKSPName_desc = {
    L"MyCustomKSPName",
    KeystoreInit,
    0,
    KeystoreWrite,
    KeystoreDecrypt,
    0
};

#ifdef _WIN32
__declspec(dllexport)
#endif
CEKEYSTOREPROVIDER *CEKeystoreProvider[] = {
    &MyCustomKSPName_desc,
    0
};
```

### <a name="odbc-application"></a>Application ODBC

Le code suivant est une démo d’application qui utilise le fournisseur de magasin de clés ci-dessus. Lors de son exécution, assurez-vous que la bibliothèque du fournisseur se trouve dans le même répertoire que le binaire de l’application, et que la chaîne de connexion spécifie (ou spécifie un DSN qui contient) le paramètre `ColumnEncryption=Enabled`.

```
/*
 Example application for demonstration of custom keystore provider usage

Windows:   compile with cl /MD kspapp.c /link odbc32.lib
Linux/macOS: compile with gcc -o kspapp -fshort-wchar kspapp.c -lodbc -ldl
 
 usage: kspapp connstr

 */

#define KSPNAME L"MyCustomKSPName"
#define PROV_ENCRYPT_KEY "JHKCWYT06N3RG98J0MBLG4E3"

#include <stdio.h>
#include <stdlib.h>
#ifdef _WIN32
#include <windows.h>
#else
#define __stdcall
#include <dlfcn.h>
#endif
#include <sql.h>
#include <sqlext.h>
#include "msodbcsql.h"

/* Convenience functions */

int checkRC(SQLRETURN rc, char *msg, int ret, SQLHANDLE h, SQLSMALLINT ht) {
    if (rc == SQL_ERROR) {
        fprintf(stderr, "Error occurred upon %s\n", msg);
        if (h) {
            SQLSMALLINT i = 0;
            SQLSMALLINT outlen = 0;
            char errmsg[1024];
            while ((rc = SQLGetDiagField(
                ht, h, ++i, SQL_DIAG_MESSAGE_TEXT, errmsg, sizeof(errmsg), &outlen)) == SQL_SUCCESS
                || rc == SQL_SUCCESS_WITH_INFO) {
                fprintf(stderr, "Err#%d: %s\n", i, errmsg);
            }
        }
        if (ret)
            exit(ret);
        return 0;
    }
    else if (rc == SQL_SUCCESS_WITH_INFO && h) {
        SQLSMALLINT i = 0;
        SQLSMALLINT outlen = 0;
        char errmsg[1024];
        printf("Success with info for %s:\n", msg);
        while ((rc = SQLGetDiagField(
            ht, h, ++i, SQL_DIAG_MESSAGE_TEXT, errmsg, sizeof(errmsg), &outlen)) == SQL_SUCCESS
            || rc == SQL_SUCCESS_WITH_INFO) {
            fprintf(stderr, "Msg#%d: %s\n", i, errmsg);
        }
    }
    return 1;
}

void postKspError(CEKEYSTORECONTEXT *ctx, const wchar_t *msg, ...) {
    if (msg > (wchar_t*)65535)
        wprintf(L"Provider emitted message: %s\n", msg);
    else
        wprintf(L"Provider emitted message ID %d\n", msg);
}

int main(int argc, char **argv) {
    char sqlbuf[1024];
    SQLHENV env;
    SQLHDBC dbc;
    SQLHSTMT stmt;
    SQLRETURN rc;
    unsigned char CEK[32];
    unsigned char *ECEK;
    unsigned short ECEKlen;
#ifdef _WIN32
    HMODULE hProvLib;
#else
    void *hProvLib;
#endif
    CEKEYSTORECONTEXT ctx = {0};
    CEKEYSTOREPROVIDER **ppKsp, *pKsp;
    int(__stdcall *pEncryptCEK)(CEKEYSTORECONTEXT *, errFunc *, unsigned char *, unsigned short, unsigned char **, unsigned short *);
    int i;
    if (argc < 2) {
        fprintf(stderr, "usage: kspapp connstr\n");
        return 1;
    }

    /* Load the provider library */
#ifdef _WIN32
    if (!(hProvLib = LoadLibrary("MyKSP.dll"))) {
#else
    if (!(hProvLib = dlopen("./MyKSP.so", RTLD_NOW))) {
#endif
        fprintf(stderr, "Error loading KSP library\n");
        return 2;
    }
#ifdef _WIN32
    if (!(ppKsp = (CEKEYSTOREPROVIDER**)GetProcAddress(hProvLib, "CEKeystoreProvider"))) {
#else
    if (!(ppKsp = (CEKEYSTOREPROVIDER**)dlsym(hProvLib, "CEKeystoreProvider"))) {
#endif
        fprintf(stderr, "The export CEKeystoreProvider was not found in the KSP library\n");
        return 3;
    }
    while (pKsp = *ppKsp++) {
        if (!memcmp(KSPNAME, pKsp->Name, sizeof(KSPNAME)))
            goto FoundProv;
    }
    fprintf(stderr, "Could not find provider in the library\n");
    return 4;
FoundProv:
    if (pKsp->Init && !pKsp->Init(&ctx, postKspError)) {
        fprintf(stderr, "Could not initialize provider\n");
        return 5;
    }
#ifdef _WIN32
    if (!(pEncryptCEK = (LPVOID)GetProcAddress(hProvLib, "KeystoreEncrypt"))) {
#else
    if (!(pEncryptCEK = dlsym(hProvLib, "KeystoreEncrypt"))) {
#endif
        fprintf(stderr, "The export KeystoreEncrypt was not found in the KSP library\n");
        return 6;
    }
    if (!pKsp->Write) {
        fprintf(stderr, "Provider does not support configuration\n");
        return 7;
    }

    /* Configure the provider with the key */
    if (!pKsp->Write(&ctx, postKspError, PROV_ENCRYPT_KEY, strlen(PROV_ENCRYPT_KEY))) {
        fprintf(stderr, "Error writing to KSP\n");
        return 8;
    }

    /* Generate a CEK and encrypt it with the provider */
    srand(time(0) ^ getpid());
    for (i = 0; i < sizeof(CEK); i++)
        CEK[i] = rand();

    if (!pEncryptCEK(&ctx, postKspError, CEK, sizeof(CEK), &ECEK, &ECEKlen)) {
        fprintf(stderr, "Error encrypting CEK\n");
        return 9;
    }

    /* Connect to Server */
    rc = SQLAllocHandle(SQL_HANDLE_ENV, NULL, &env);
    checkRC(rc, "allocating environment handle", 2, 0, 0);
    rc = SQLSetEnvAttr(env, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, 0);
    checkRC(rc, "setting ODBC version to 3.0", 3, env, SQL_HANDLE_ENV);
    rc = SQLAllocHandle(SQL_HANDLE_DBC, env, &dbc);
    checkRC(rc, "allocating connection handle", 4, env, SQL_HANDLE_ENV);
    rc = SQLDriverConnect(dbc, 0, argv[1], strlen(argv[1]), NULL, 0, NULL, SQL_DRIVER_NOPROMPT);
    checkRC(rc, "connecting to data source", 5, dbc, SQL_HANDLE_DBC);
    rc = SQLAllocHandle(SQL_HANDLE_STMT, dbc, &stmt);
    checkRC(rc, "allocating statement handle", 6, dbc, SQL_HANDLE_DBC);

    /* Create a CMK definition on the server */
    {
        static char cmkSql[] = "CREATE COLUMN MASTER KEY CustomCMK WITH ("
            "KEY_STORE_PROVIDER_NAME = 'MyCustomKSPName',"
            "KEY_PATH = 'TheOneAndOnlyKey')";
        printf("Create CMK: %s\n", cmkSql);
        SQLExecDirect(stmt, cmkSql, SQL_NTS);
    }

    /* Create a CEK definition on the server */
    {
        const char cekSqlBefore[] = "CREATE COLUMN ENCRYPTION KEY CustomCEK WITH VALUES ("
            "COLUMN_MASTER_KEY = CustomCMK,"
            "ALGORITHM = 'none',"
            "ENCRYPTED_VALUE = 0x";
        char *cekSql = malloc(sizeof(cekSqlBefore) + 2 * ECEKlen + 2); /* 1 for ')', 1 for null terminator */
        strcpy(cekSql, cekSqlBefore);
        for (i = 0; i < ECEKlen; i++)
            sprintf(cekSql + sizeof(cekSqlBefore) - 1 + 2 * i, "%02x", ECEK[i]);
        strcat(cekSql, ")");
        printf("Create CEK: %s\n", cekSql);
        SQLExecDirect(stmt, cekSql, SQL_NTS);
        free(cekSql);
#ifdef _WIN32
        LocalFree(ECEK);
#else
        free(ECEK);
#endif
    }

#ifdef _WIN32
    FreeLibrary(hProvLib);
#else
    dlclose(hProvLib);
#endif

    /* Create a table with encrypted columns */
    {
        static char *tableSql = "CREATE TABLE CustomKSPTestTable ("
            "c1 int,"
            "c2 varchar(255) COLLATE Latin1_General_BIN2 ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = CustomCEK, ENCRYPTION_TYPE = DETERMINISTIC, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'))";
        printf("Create table: %s\n", tableSql);
        SQLExecDirect(stmt, tableSql, SQL_NTS);
    }

    /* Load provider into the ODBC Driver and configure it */
    {
        unsigned char ksd[sizeof(CEKEYSTOREDATA) + sizeof(PROV_ENCRYPT_KEY) - 1];
        CEKEYSTOREDATA *pKsd = (CEKEYSTOREDATA*)ksd;
        pKsd->name = L"MyCustomKSPName";
        pKsd->dataSize = sizeof(PROV_ENCRYPT_KEY) - 1;
        memcpy(pKsd->data, PROV_ENCRYPT_KEY, sizeof(PROV_ENCRYPT_KEY) - 1);
#ifdef _WIN32
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREPROVIDER, "MyKSP.dll", SQL_NTS);
#else
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREPROVIDER, "./MyKSP.so", SQL_NTS);
#endif
        checkRC(rc, "Loading KSP into ODBC Driver", 7, dbc, SQL_HANDLE_DBC);
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREDATA, (SQLPOINTER)pKsd, SQL_IS_POINTER);
        checkRC(rc, "Configuring the KSP", 7, dbc, SQL_HANDLE_DBC);
    }

    /* Insert some data */
    {
        int c1;
        char c2[256];
        rc = SQLBindParameter(stmt, 1, SQL_PARAM_INPUT, SQL_C_LONG, SQL_INTEGER, 0, 0, &c1, 0, 0);
        checkRC(rc, "Binding parameters for insert", 9, stmt, SQL_HANDLE_STMT);
        rc = SQLBindParameter(stmt, 2, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_VARCHAR, 255, 0, c2, 255, 0);
        checkRC(rc, "Binding parameters for insert", 9, stmt, SQL_HANDLE_STMT);
        for (i = 0; i < 10; i++) {
            c1 = i * 10 + i + 1;
            sprintf(c2, "Sample data %d for column 2", i);
            rc = SQLExecDirect(stmt, "INSERT INTO CustomKSPTestTable (c1, c2) values (?, ?)", SQL_NTS);
            checkRC(rc, "Inserting rows query", 10, stmt, SQL_HANDLE_STMT);
        }
        printf("(Encrypted) data has been inserted into the [CustomKSPTestTable]. You may inspect the data now.\n"
            "Press Enter to continue...\n");
        getchar();
    }

    /* Retrieve the data */
    {
        int c1;
        char c2[256];
        rc = SQLBindCol(stmt, 1, SQL_C_LONG, &c1, 0, 0);
        checkRC(rc, "Binding columns for select", 11, stmt, SQL_HANDLE_STMT);
        rc = SQLBindCol(stmt, 2, SQL_C_CHAR, c2, sizeof(c2), 0);
        checkRC(rc, "Binding columns for select", 11, stmt, SQL_HANDLE_STMT);
        rc = SQLExecDirect(stmt, "SELECT c1, c2 FROM CustomKSPTestTable", SQL_NTS);
        checkRC(rc, "Retrieving rows query", 12, stmt, SQL_HANDLE_STMT);
        while (SQL_SUCCESS == (rc = SQLFetch(stmt)))
            printf("Retrieved data: c1=%d c2=%s\n", c1, c2);
        SQLFreeStmt(stmt, SQL_CLOSE);
        printf("Press Enter to clean up and exit...\n");
        getchar();
    }

    /* Clean up */
    {
        SQLExecDirect(stmt, "DROP TABLE CustomKSPTestTable", SQL_NTS);
        SQLExecDirect(stmt, "DROP COLUMN ENCRYPTION KEY CustomCEK", SQL_NTS);
        SQLExecDirect(stmt, "DROP COLUMN MASTER KEY CustomCMK", SQL_NTS);
        printf("Removed table, CEK, and CMK\n");
    }
    SQLDisconnect(dbc);
    SQLFreeHandle(SQL_HANDLE_DBC, dbc);
    SQLFreeHandle(SQL_HANDLE_ENV, env);
    return 0;
}

```

## <a name="see-also"></a>Voir aussi

[Utilisation d’Always Encrypted avec ODBC Driver](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)
