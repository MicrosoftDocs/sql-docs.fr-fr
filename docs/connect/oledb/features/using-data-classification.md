---
title: Utilisation de la classification de données avec Microsoft OLE DB Driver pour SQL Server | Microsoft Docs
description: Découvrez comment utiliser Microsoft OLE DB Driver pour SQL Server pour obtenir des informations de classification.
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- OLE DB Driver for SQL Server, data classification
author: bazizi
ms.author: v-beaziz
manager: kenvh
ms.openlocfilehash: f289b12800c7ce88607c587e31c3f99887c8d9bb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347891"
---
# <a name="using-data-classification"></a>Utilisation de la classification des données
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../../../includes/applies-to-version/sql-asdb-asa.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

## <a name="overview"></a>Vue d’ensemble
[Découverte et classification des données SQL](../../../relational-databases/security/sql-data-discovery-and-classification.md) est un ensemble de services avancés de découverte, de classification, d’étiquetage et de génération de rapports des informations sensibles dans vos bases de données. Microsoft OLE DB Driver pour SQL Server (version [18.5.0](../release-notes-for-oledb-driver-for-sql-server.md#1850)) ajoute le support de l’extraction des métadonnées de classification lorsque la source de données sous-jacente prend en charge la fonctionnalité. Ces informations sont accessibles via l’interface [ISSDataClassification](../ole-db-interfaces/issdataclassification-ole-db.md).

Pour plus d’informations sur la façon d’attribuer une classification à des colonnes, consultez [Découverte et classification des données SQL](../../../relational-databases/security/sql-data-discovery-and-classification.md).

## <a name="code-samples"></a>Exemples de code

Les requêtes [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivantes peuvent être exécutées dans SSMS pour configurer les conditions préalables pour l’exemple d'application C++ :

```sql
CREATE DATABASE [mydb]
GO

USE [mydb]
GO

CREATE TABLE [dbo].[mytable](
    [col1] [int] NULL,
    [col2] [int] NULL
)
GO

ADD SENSITIVITY CLASSIFICATION TO [dbo].[mytable].[col1] WITH (label = 'Label1', label_id = 'LabelId1', information_type = 'Type1', information_type_id = 'TypeId1', rank = Medium)
GO

ADD SENSITIVITY CLASSIFICATION TO [dbo].[mytable].[col2] WITH (label = 'Label2', label_id = 'LabelId2', information_type = 'Type2', information_type_id = 'TypeId2', rank = High)
```

Le code C++ suivant utilise le pilote Microsoft OLE DB pour obtenir les informations de classification générées à l’aide des requêtes [!INCLUDE[tsql](../../../includes/tsql-md.md)] ci-dessus :
```cpp
#include <atlbase.h>
#include <msdasc.h>
#include <exception>
#include <iostream>
#include <string>
#include "msoledbsql.h"

void Connect(CComPtr<IDBInitialize>& pIDBInitialize, const wchar_t* server, const wchar_t* database);
SENSITIVITYCLASSIFICATION* GetSensitivityClassificationInfo(CComPtr<IDBInitialize>& pIDBInitialize, const wchar_t* query);
void PrintSensitivityClassificationInfo(SENSITIVITYCLASSIFICATION* pSensitivityClassification);

int main()
{
    const wchar_t server[] = L"myserver";
    const wchar_t database[] = L"mydb";
    const wchar_t query[] = L"SELECT col1, col2, col1 + col2 FROM mytable";

    CoInitialize(nullptr);

    try
    {
        // Connect to data source
        CComPtr<IDBInitialize> pIDBInitialize;
        Connect(pIDBInitialize, server, database);

        // Obtain sensitivity classification info
        SENSITIVITYCLASSIFICATION* pSensitivityClassification = GetSensitivityClassificationInfo(pIDBInitialize, query);

        // Print sensitivity classification info
        PrintSensitivityClassificationInfo(pSensitivityClassification);

        if (pSensitivityClassification)
        {
            CComPtr<IMalloc> pIMalloc;
            if (FAILED(CoGetMalloc(1, &pIMalloc)))
            {
                throw std::exception("CoGetMalloc call failed.");
            }

            // Release memory
            pIMalloc->Free(pSensitivityClassification);
        }
    }
    catch (std::exception& e)
    {
        std::cerr << "Exception caught: " << e.what() << std::endl;
        return 1;
    }

    CoUninitialize();
    return 0;
}

void Connect(CComPtr<IDBInitialize>& pIDBInitialize, const wchar_t* server, const wchar_t* database)
{
    // Construct the connection string.
    std::wstring connString = L"Provider=MSOLEDBSQL;Data Source=" + std::wstring(server) + L";Database=" +
        std::wstring(database) + L";Authentication=ActiveDirectoryIntegrated;Use Encryption for Data=true;";

    CComPtr<IDataInitialize> pIDataInitialize;
    if (FAILED(CoCreateInstance(CLSID_MSDAINITIALIZE, nullptr, CLSCTX_INPROC_SERVER, IID_IDataInitialize, reinterpret_cast<LPVOID*>(&pIDataInitialize))))
    {
        throw std::exception("CoCreateInstance call failed.");
    }

    if (FAILED(pIDataInitialize->GetDataSource(nullptr, CLSCTX_INPROC_SERVER, connString.c_str(), IID_IDBInitialize, reinterpret_cast<IUnknown**>(&pIDBInitialize))))
    {
        throw std::exception("GetDataSource call failed.");
    }

    if (FAILED(pIDBInitialize->Initialize()))
    {
        throw std::exception("Initialize call failed.");
    }
}

SENSITIVITYCLASSIFICATION* GetSensitivityClassificationInfo(CComPtr<IDBInitialize>& pIDBInitialize, const wchar_t* query)
{
    CComPtr<IDBCreateSession> pIDBCreateSession;
    if (FAILED(pIDBInitialize.QueryInterface<IDBCreateSession>(&pIDBCreateSession)))
    {
        throw std::exception("QueryInterface call failed.");
    }

    CComPtr<IDBCreateCommand> pIDBCreateCommand;
    if (FAILED(pIDBCreateSession->CreateSession(nullptr, IID_IDBCreateCommand, reinterpret_cast<IUnknown**>(&pIDBCreateCommand))))
    {
        throw std::exception("CreateSession call failed.");
    }

    CComPtr<ICommandText> pICommandText;
    if (FAILED(pIDBCreateCommand->CreateCommand(nullptr, IID_ICommandText, reinterpret_cast<IUnknown**>(&pICommandText))))
    {
        throw std::exception("CreateCommand call failed.");
    }

    if (FAILED(pICommandText->SetCommandText(DBGUID_DBSQL, query)))
    {
        throw std::exception("SetCommandText call failed.");
    }

    CComPtr<ISSDataClassification> pISSDataClassification;
    if (FAILED(pICommandText->Execute(nullptr, IID_ISSDataClassification, nullptr, nullptr, reinterpret_cast<IUnknown**>(&pISSDataClassification))))
    {
        throw std::exception("Execute call failed.");
    }

    SENSITIVITYCLASSIFICATION* pSensitivityClassification = nullptr;
    if (FAILED(pISSDataClassification->GetSensitivityClassification(&pSensitivityClassification)))
    {
        throw std::exception("GetSensitivityClassification call failed.");
    }

    return pSensitivityClassification;
}

void PrintSensitivityClassificationInfo(SENSITIVITYCLASSIFICATION* pSensitivityClassification)
{
    if (!pSensitivityClassification)
    {
        return;
    }

    if (pSensitivityClassification->eQuerySensitivityRank != SENSITIVITYRANK_NOT_DEFINED)
    {
        std::wcout << L"Query sensitivity rank: " << pSensitivityClassification->eQuerySensitivityRank << L"\n\n";
    }

    for (USHORT colIdx = 0; colIdx < pSensitivityClassification->cColumnSensitivityMetadata; ++colIdx)
    {
        const COLUMNSENSITIVITYMETADATA& columnMetadata = pSensitivityClassification->rgColumnSensitivityMetadata[colIdx];

        std::wcout << L"Sensitivity classification for column #" << colIdx << L":" << std::endl;
        for (USHORT propIdx = 0; propIdx < columnMetadata.cSensitivityProperties; ++propIdx)
        {
            const SENSITIVITYPROPERTY& prop = columnMetadata.rgSensitivityProperties[propIdx];

            std::wcout << L"Property #" << propIdx << L":" << std::endl;

            if (prop.eSensitivityRank != SENSITIVITYRANK_NOT_DEFINED)
            {
                std::wcout << L"\tSensitivity rank: \t" << prop.eSensitivityRank << std::endl;
            }

            if (prop.pSensitivityLabel)
            {
                if (prop.pSensitivityLabel->pwszId)
                {
                    std::wcout << L"\tSensitivity label id: \t" << prop.pSensitivityLabel->pwszId << std::endl;
                }
                if (prop.pSensitivityLabel->pwszName)
                {
                    std::wcout << L"\tSensitivity label name: " << prop.pSensitivityLabel->pwszName << std::endl;
                }
            }

            if (prop.pInformationType)
            {
                if (prop.pInformationType->pwszId)
                {
                    std::wcout << L"\tInformation type id: \t" << prop.pInformationType->pwszId << std::endl;
                }
                if (prop.pInformationType->pwszName)
                {
                    std::wcout << L"\tInformation type name: \t" << prop.pInformationType->pwszName << std::endl;
                }
            }

            std::wcout << std::endl;
        }
    }
}
```

## <a name="see-also"></a>Voir aussi
 [Interfaces &#40;OLE DB&#41;](../ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)  
 [ISSDataClassification](../ole-db-interfaces/issdataclassification-ole-db.md)