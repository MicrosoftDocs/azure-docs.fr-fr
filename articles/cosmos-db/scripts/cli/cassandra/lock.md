---
title: Créer un verrou de ressource pour un espace de clés et une table Cassandra pour Azure Cosmos DB
description: Créer un verrou de ressource pour un espace de clés et une table Cassandra pour Azure Cosmos DB
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: b2c1e663c659935e7e820f30a281099058a4cf34
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/20/2021
ms.locfileid: "107770990"
---
# <a name="create-a-resource-lock-for-azure-cosmos-cassandra-api-keyspace-and-table-using-azure-cli"></a>Créer un verrou de ressource pour un espace de clés et une table d’API Cassandra Azure Cosmos avec Azure CLI
[!INCLUDE[appliesto-cassandra-api](../../../includes/appliesto-cassandra-api.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- Cet article nécessite la version 2.9.1 ou ultérieure d’Azure CLI. Si vous utilisez Azure Cloud Shell, la version la plus récente est déjà installée.

> [!IMPORTANT]
> Les verrous de ressources ne fonctionnent pas pour les modifications apportées par les utilisateurs qui se connectent à l’aide d’un SDK Cassandra, de l’interpréteur de commandes CQL ou du portail Azure, sauf si le compte Cosmos DB est d’abord verrouillé avec la propriété `disableKeyBasedMetadataWriteAccess` activée. Pour en savoir plus sur l’activation de cette propriété, consultez [Prévention des modifications à partir des SDK](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Exemple de script

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/cassandra/lock.sh "Create a resource lock for an Azure Cosmos DB Cassandra API keyspace, and table.")]

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Notes |
|---|---|
| [az lock create](/cli/azure/lock#az_lock_create) | Crée un verrou. |
| [az lock list](/cli/azure/lock#az_lock_list) | Liste les informations sur les verrous. |
| [az lock show](/cli/azure/lock#az_lock_show) | Affiche les propriétés d’un verrou. |
| [az lock delete](/cli/azure/lock#az_lock_delete) | Supprime un verrou. |

## <a name="next-steps"></a>Étapes suivantes

- [Verrouiller les ressources pour empêcher les modifications inattendues](../../../../azure-resource-manager/management/lock-resources.md)

- [Documentation sur l’interface CLI Azure Cosmos DB](/cli/azure/cosmosdb).

- [Dépôt GitHub de l’interface CLI Azure Cosmos DB](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
