---
description: Utilisation d’Always Encrypted avec enclaves sécurisées avec le pilote JDBC
title: Utilisation d’Always Encrypted avec enclaves sécurisées avec le pilote JDBC | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 271c0438-8af1-45e5-b96a-4b1cabe32707
author: reneye
ms.author: v-reye
ms.openlocfilehash: 4016f3eb5d725673b1e4149d43dc21d20cdc627f
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534638"
---
# <a name="using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver"></a>Utilisation d’Always Encrypted avec enclaves sécurisées avec le pilote JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Cette page fournit des informations sur la façon de développer des applications Java à l’aide d’[Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves.md) et de Microsoft JDBC Driver 8.2 (ou version ultérieure) pour SQL Server.

Les enclaves sécurisées sont un ajout à la fonctionnalité [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) existante. Les enclaves sécurisées visent à résoudre les problèmes de limitations rencontrées lors de l’utilisation de données Always Encrypted. Avant, les utilisateurs pouvaient uniquement effectuer des comparaisons d’égalité sur des données Always Encrypted ; ils devaient récupérer et déchiffrer les données pour faire d’autres opérations. Les enclaves sécurisées suppriment ces limitations en autorisant les calculs sur les données de texte en clair à l’intérieur d’une enclave sécurisée côté serveur. Une enclave sécurisée est une région de mémoire protégée dans le processus SQL Server qui agit comme un environnement d’exécution approuvé pour le traitement des données sensibles dans le moteur SQL Server. Une enclave sécurisée apparaît sous la forme d’une boîte noire pour le reste de SQL Server et des autres processus sur l’ordinateur d’hébergement. Il n’existe aucun moyen d’afficher les données ou le code à l’intérieur de l’enclave depuis l’extérieur, même avec un débogueur.

## <a name="prerequisites"></a>Prérequis
- Assurez-vous que Microsoft JDBC Driver 8.2 (ou version ultérieure) pour SQL Server est installé sur votre machine de développement.
- Vérifiez que les dépendances de l’environnement telles que les DLL, les magasins de clés, etc. se trouvent dans les bons chemins. Always Encrypted avec enclaves sécurisées, module complémentaire de la fonctionnalité [Always Encrypted](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md) existante, partage des prérequis similaires.

> [!Note]
> Si vous utilisez une version antérieure de JDK 8, vous devrez peut-être télécharger et installer les fichiers de stratégie Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction. Veillez à lire le fichier Lisez-moi inclus dans le fichier zip pour obtenir les instructions d’installation et des informations pertinentes sur les éventuels problèmes d’importation/exportation.  
>
> Vous pouvez télécharger les fichiers de stratégie à partir de la page web [Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files 8 Download](https://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html).

## <a name="setting-up-secure-enclaves"></a>Configuration des enclaves sécurisées
Vous pouvez suivre le [Tutoriel : Démarrage avec Always Encrypted avec enclaves sécurisées dans SQL Server](../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) ou le [Tutoriel : Démarrage avec Always Encrypted avec enclaves sécurisées dans Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started) pour bien démarrer avec les enclaves sécurisées. Pour avoir des informations plus complètes, consultez [Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

## <a name="connection-string-properties"></a>Propriétés de chaîne de connexion

Pour activer les calculs d’enclave pour une connexion de base de données, vous devez définir les mots clés de chaînes de connexion suivants, en plus de l’activation d’Always Encrypted.

- **enclaveAttestationProtocol** : spécifie un protocole d’attestation. 
  - Si vous utilisez [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] et le service Guardian hôte (SGH), la valeur de ces mots clés doit être `HGS`.
  - Si vous utilisez [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et Microsoft Azure attestation, la valeur de ces mots clés doit être `AAS`.

- **enclaveAttestationUrl:**  : spécifie une URL d’attestation (un point de terminaison de service d’attestation). Vous devez obtenir une URL d’attestation pour votre environnement auprès de votre administrateur de services fédérés d’attestation.
  - Si vous utilisez [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] et le service Guardian hôte (SGH), consultez [Déterminer et partager l’URL d’attestation SGH](../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Si vous utilisez [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et Microsoft Azure Attestation, consultez [Déterminer l’URL d’attestation de votre stratégie d’attestation](/azure-sql/database/always-encrypted-enclaves-configure-attestation#determine-the-attestation-url-for-your-attestation-policy).

Les utilisateurs doivent activer **columnEncryptionSetting** et configurer correctement les **deux** propriétés de chaîne de connexion ci-dessus pour activer Always Encrypted avec enclaves sécurisées à partir de [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)].

## <a name="working-with-secure-enclaves"></a>Utilisation d’enclaves sécurisées
Une fois que les propriétés de connexion aux enclaves ont été correctement définies, la fonctionnalité est transparente pour l’utilisateur. Le pilote détermine automatiquement si la requête nécessite l’utilisation d’une enclave sécurisée. Les exemples ci-dessous montrent des requêtes qui déclenchent des calculs d’enclave. Vous pouvez trouver la configuration de la base de données et de la table dans le [Tutoriel : Démarrage avec Always Encrypted avec enclaves sécurisées dans SQL Server](../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) ou le [Tutoriel : Démarrage avec Always Encrypted avec enclaves sécurisées dans Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started).


Les requêtes enrichies déclenchent des calculs d’enclave :
```java
private static final String URL = "jdbc:sqlserver://<server>:<port>;user=<username>;password=<password>;databaseName=ContosoHR;columnEncryptionSetting=enabled;enclaveAttestationUrl=<attestation-url>;enclaveAttestationProtocol=<attestation-protocol>;";
try (Connection c = DriverManager.getConnection(URL)) {
    try (PreparedStatement p = c.prepareStatement("SELECT * FROM Employees WHERE SSN LIKE ?")) {
        p.setString(1, "%6818");
        try (ResultSet rs = p.executeQuery()) {
            while (rs.next()) {
                // Do work with data
            }
        }
    }
    
    try (PreparedStatement p = c.prepareStatement("SELECT * FROM Employees WHERE SALARY > ?")) {
        ((SQLServerPreparedStatement) p).setMoney(1, new BigDecimal(0));
        try (ResultSet rs = p.executeQuery()) {
            while (rs.next()) {
                // Do work with data
            }
        }
    }
}
```

L’activation du chiffrement sur une colonne déclenche également des calculs d’enclave :
```java
private static final String URL = "jdbc:sqlserver://<server>:<port>;user=<username>;password=<password>;databaseName=ContosoHR;columnEncryptionSetting=enabled;enclaveAttestationUrl=<attestation-url>;enclaveAttestationProtocol=<attestation-protocol>;";
try (Connection c = DriverManager.getConnection(URL);Statement s = c.createStatement()) {
    s.executeUpdate("ALTER TABLE Employees ALTER COLUMN SSN CHAR(11) NULL WITH (ONLINE = ON)");
}
```

## <a name="java-8-users"></a>Utilisateurs de Java 8
Cette fonctionnalité requiert l’algorithme de signature RSASSA-PSA. Cet algorithme a été ajouté dans JDK 11, mais il n’a pas été rétroporté dans JDK 8. Les utilisateurs qui souhaitent utiliser cette fonctionnalité avec la version JDK 8 de [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] ont deux solutions : charger leur propre fournisseur qui prend en charge l’algorithme de signature RSASSA-PSA ou inclure la dépendance facultative BouncyCastleProvider. La dépendance sera supprimée à une date ultérieure si l’algorithme de signature est rétroporté dans JDK 8 ou si le cycle de vie de la prise en charge de JDK 8 se termine.

## <a name="see-also"></a>Voir aussi
[Utilisation d'Always Encrypted avec le pilote JDBC](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md)  
