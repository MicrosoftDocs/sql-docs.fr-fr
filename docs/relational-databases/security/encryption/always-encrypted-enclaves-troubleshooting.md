---
title: Résoudre les problèmes courants relatifs à Always Encrypted avec enclaves sécurisées
description: Résoudre les problèmes courants relatifs à Always Encrypted avec enclaves sécurisées
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: how-to
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: dc6bcbecdb29cdf0cf1fca8c41e971463bb0d6b9
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236357"
---
# <a name="troubleshoot-common-issues-for-always-encrypted-with-secure-enclaves"></a>Résoudre les problèmes courants relatifs à Always Encrypted avec enclaves sécurisées

Cet article explique comment identifier et résoudre les problèmes courants qui peuvent se présenter lors de l’exécution d’instructions TSQL (Transact-SQL) utilisant [Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md).

Pour plus d’informations sur l’exécution de requêtes utilisant des enclaves sécurisées, consultez [Exécuter des instructions Transact-SQL utilisant des enclaves sécurisées](always-encrypted-enclaves-query-columns.md).

## <a name="database-connection-errors"></a>Database connection errors (Erreurs de connexion de base de données)

Pour exécuter des instructions utilisant une enclave sécurisée, vous devez activer Always Encrypted et spécifier une URL d’attestation pour la connexion de base de données, comme cela est expliqué dans les [prérequis pour l’exécution d’instructions utilisant des enclaves sécurisées](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves). Toutefois, si vous spécifiez une URL d’attestation, mais que votre base de données dans [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] ou votre instance [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] cible ne prend pas en charge les enclaves sécurisées ou n’est pas configurée correctement, votre connexion échoue.

- Si vous utilisez [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], vérifiez que votre base de données utilise la configuration matérielle de la [série DC](https://docs.microsoft.com/azure/azure-sql/database/service-tiers-vcore?tabs=azure-portal#dc-series). Pour plus d’informations, consultez [Activer Intel SGX pour votre base de données Azure SQL](/azure/azure-sql/database/always-encrypted-enclaves-enable-sgx).
- Si vous utilisez [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)], vérifiez que l’enclave sécurisée est correctement configurée pour votre instance. Pour plus d’informations, consultez [Configurer l’enclave sécurisée dans SQL Server](always-encrypted-enclaves-configure-enclave-type.md).

## <a name="attestation-errors-when-using-microsoft-azure-attestation"></a>Erreurs d’attestation lors de l’utilisation de Microsoft Azure Attestation

> [!NOTE]
> Cette section s’applique uniquement à [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

Avant d’envoyer une instruction T-SQL au serveur logique Azure SQL pour exécution, le pilote client déclenche le workflow d’attestation d’enclave suivant à l’aide de Microsoft Azure Attestation.

1. Le pilote client passe l’URL d’attestation, spécifiée dans la connexion de base de données, au serveur logique Azure SQL.
2. Le serveur logique Azure SQL collecte les preuves relatives à l’enclave, à son environnement d’hébergement et au code s’exécutant dans l’enclave. Le serveur envoie ensuite une demande d’attestation au fournisseur d’attestation, référencé dans l’URL d’attestation.
3. Le fournisseur d’attestation valide les preuves par rapport à la stratégie configurée et émet un jeton d’attestation au serveur logique Azure SQL. Le fournisseur d’attestation signe le jeton d’attestation avec sa clé privée.
4. Le serveur logique Azure SQL envoie le jeton d’attestation au pilote client.
5. Le client contacte le fournisseur d’attestation à l’URL d’attestation spécifiée pour récupérer sa clé publique et vérifie la signature dans le jeton d’attestation.

Des erreurs peuvent se produire à différentes étapes du workflow ci-dessus en raison de configurations incorrectes. Voici les erreurs d’attestation courantes, leurs causes principales et les étapes de résolution des problèmes recommandées :

- Votre serveur logique Azure SQL ne peut pas se connecter au fournisseur d’attestation spécifié dans l’URL d’attestation dans Azure Attestation (étape 2 du workflow ci-dessus). Les causes probables sont les suivantes :
  - L’URL d’attestation est incorrecte ou incomplète. Pour plus d’informations, consultez [Déterminer l’URL d’attestation de votre stratégie d’attestation](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation#determine-the-attestation-url-for-your-attestation-policy).
  - Le fournisseur d’attestation a été accidentellement supprimé.
  - Le pare-feu a été configuré pour le fournisseur d’attestation, mais il n’autorise pas l’accès aux services Microsoft.
  - Une erreur réseau intermittente entraîne l’indisponibilité du fournisseur d’attestation.
- Votre serveur logique Azure SQL n’est pas autorisé à envoyer des demandes d’attestation au fournisseur d’attestation. Vérifiez que l’administrateur de votre fournisseur d’attestation a ajouté le serveur de base de données au rôle Lecteur d’attestation. Pour plus d’informations, consultez [Accorder à votre serveur Azure SQL Database l’accès à votre fournisseur d’attestation](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation#grant-your-azure-sql-database-server-access-to-your-attestation-provider).
- La validation de la stratégie d’attestation échoue (étape 3 du workflow ci-dessus).
  - La cause racine probable est une stratégie d’attestation incorrecte. Veillez à utiliser la stratégie recommandée par Microsoft. Pour plus d’informations, consultez [Créer et configurer un fournisseur d’attestation](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation#create-and-configure-an-attestation-provider).
  - La validation de la stratégie peut également échouer en raison d’une violation de sécurité compromettant l’enclave côté serveur.
- Votre application cliente ne peut pas se connecter au fournisseur d’attestation et récupérer la clé de signature publique (étape 5). Les causes probables sont les suivantes :
  - La configuration de pare-feux entre votre application et le fournisseur d’attestation peut bloquer les connexions. Pour résoudre les problèmes de connexion bloquée, vérifiez que vous pouvez vous connecter au point de terminaison OpenId de votre fournisseur d’attestation. Par exemple, utilisez un navigateur web sur l’ordinateur hébergeant votre application pour voir si vous pouvez vous connecter au point de terminaison OpenID. Pour plus d’informations, consultez [Configuration des métadonnées - Get](https://docs.microsoft.com/rest/api/attestation/metadataconfiguration/get).

## <a name="attestation-errors-when-using-host-guardian-service"></a>Erreurs d’attestation lors de l’utilisation du service Guardian hôte

> [!NOTE]
> Cette section s’applique uniquement à [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)].

Avant d’envoyer une instruction T-SQL à [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] pour exécution, le pilote client déclenche le workflow d’attestation d’enclave suivant à l’aide du service Guardian hôte (SGH).

1. Le pilote client appelle [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] pour lancer l’attestation.
2. [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] collecte les preuves relatives à son enclave, à son environnement d’hébergement et au code s’exécutant dans l’enclave. [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] demande un certificat d’intégrité à l’instance SGH auprès de laquelle la machine hébergeant [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] est inscrite. Pour plus d’informations, consultez [Inscrire un ordinateur auprès du service Guardian hôte](always-encrypted-enclaves-host-guardian-service-register.md).
3. SGH valide les preuves et émet le certificat d’intégrité à [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. SGH signe le certificat d’intégrité avec sa clé privée.
4. [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] envoie le certificat d’intégrité au pilote client.
5. Le pilote client contacte SGH à l’URL d’attestation, spécifiée pour la connexion de base de données, afin de récupérer la clé publique SGH. Le pilote client vérifie la signature dans le certificat d’intégrité.

Des erreurs peuvent se produire à différentes étapes du workflow ci-dessus en raison de configurations incorrectes. Voici les erreurs d’attestation courantes, leurs causes racines et les étapes de résolution recommandées :

- [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] ne peut pas se connecter à SGH (étape 2 du workflow ci-dessus) en raison d’une erreur réseau intermittente. Pour résoudre le problème de connexion, l’administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] doit vérifier que l’ordinateur peut se connecter à la machine SGH.
- La validation à l’étape 3 échoue. Pour résoudre le problème de validation :
  - L’administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] doit travailler avec l’administrateur de l’application cliente pour vérifier que l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] est inscrit auprès de la même instance SGH que celle référencée dans l’URL d’attestation côté client.
  - L’administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] doit confirmer que l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] peut effectuer correctement une attestation selon les instructions fournies à l’[Étape 5 : Confirmer que l’hôte peut effectuer correctement une attestation](always-encrypted-enclaves-host-guardian-service-register.md#step-5-confirm-the-host-can-attest-successfully).
- Votre application cliente ne peut pas se connecter à SGH et récupérer sa clé de signature publique (étape 5). La cause la plus probable est la suivante :
  - La configuration de l’un des pare-feux entre votre application et le fournisseur d’attestation peut bloquer les connexions. Vérifiez que la machine hébergeant votre application peut se connecter à la machine SGH.

## <a name="in-place-encryption-errors"></a>Erreurs de chiffrement sur place

Cette section liste les erreurs courantes que vous pouvez rencontrer lors de l’utilisation de `ALTER TABLE`/`ALTER COLUMN` pour le chiffrement sur place (en plus des erreurs d’attestation décrites dans les sections précédentes). Pour plus d’informations, consultez [Configurer le chiffrement de colonne sur place en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-configure-encryption.md).

- La clé de chiffrement de colonne que vous essayez d’utiliser pour chiffrer, déchiffrer ou rechiffrer des données n’est pas une clé prenant en charge les enclaves. Pour plus d’informations sur les prérequis du chiffrement sur place, consultez les [prérequis](always-encrypted-enclaves-configure-encryption.md#prerequisites). Pour plus d’informations sur le provisionnement des clés prenant en charge les enclaves, consultez [Provisionner des clés prenant en charge les enclaves](always-encrypted-enclaves-provision-keys.md).
- Vous n’avez pas activé Always Encrypted et les calculs d’enclave pour la connexion de base de données. Consultez les [prérequis pour l’exécution d’instructions utilisant des enclaves sécurisées](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves).
- Votre instruction `ALTER TABLE`/`ALTER COLUMN` déclenche une opération de chiffrement et change le type de données de la colonne ou définit un classement avec une page de codes différente de celle du classement actuel. La combinaison d’opérations de chiffrement avec des changements de type de données ou de page de classement n’est pas autorisée. Pour résoudre le problème, utilisez des instructions distinctes : une pour changer le type de données ou la page de codes de classement, une autre pour le chiffrement sur place.

## <a name="errors-when-running-confidential-dml-queries-using-secure-enclaves"></a>Erreurs lors de l’exécution de requêtes DML confidentielles utilisant des enclaves sécurisées

Cette section liste les erreurs courantes que vous pouvez rencontrer lors de l’utilisation de requêtes DML confidentielles utilisant des enclaves sécurisées (en plus des erreurs d’attestation décrites dans les sections précédentes).

- La clé de chiffrement de colonne configurée pour la colonne que vous interrogez n’est pas une clé prenant en charge les enclaves.
- Vous n’avez pas activé Always Encrypted et les calculs d’enclave pour la connexion de base de données. Pour plus d’informations, consultez les [prérequis pour l’exécution d’instructions utilisant des enclaves sécurisées](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves).
- La colonne que vous interrogez utilise le chiffrement déterministe. Les requêtes DML confidentielles utilisant des enclaves sécurisées ne sont pas prises en charge avec le chiffrement déterministe. Pour plus d’informations sur la façon de remplacer le type de chiffrement par un chiffrement aléatoire, consultez [Activer Always Encrypted avec enclaves sécurisées pour les colonnes chiffrées existantes](always-encrypted-enclaves-enable-for-encrypted-columns.md).
- La colonne de chaîne que vous interrogez utilise un classement autre que BIN2 ou UTF-8. Remplacez le classement par BIN2 ou UTF-8. Pour plus d’informations, consultez [Instructions DML utilisant des enclaves sécurisées](always-encrypted-enclaves-query-columns.md#dml-statements-using-secure-enclaves).
- Votre requête déclenche une opération non prise en charge. Pour obtenir la liste des opérations prises en charge dans les enclaves, consultez [Instructions DML utilisant des enclaves sécurisées](always-encrypted-enclaves-query-columns.md#dml-statements-using-secure-enclaves).
## <a name="next-steps"></a>Étapes suivantes

- [Développer des applications en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>Voir aussi

- [Exécuter des instructions Transact-SQL utilisant des enclaves sécurisées](always-encrypted-enclaves-query-columns.md)
- [Configurer le chiffrement de colonne sur place en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-configure-encryption.md)
- [Créer et utiliser des index sur des colonnes à l’aide d’Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-create-use-indexes.md)