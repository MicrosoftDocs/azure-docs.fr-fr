---
title: Exemple CLI - Groupe de basculement - Azure SQL Managed Instance
description: Exemple de script Azure CLI permettant de créer une instance Azure SQL Managed Instance, l’ajouter à un groupe de basculement, puis tester le basculement.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: BustosMSFT
ms.author: robustos
ms.reviewer: mathoma
ms.date: 07/16/2019
ms.openlocfilehash: fe24deb694a23e216ebcad430d87c62aa6cb2cfd
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2021
ms.locfileid: "110698084"
---
# <a name="use-cli-to-add-an-azure-sql-managed-instance-to-a-failover-group"></a>Utiliser l’interface CLI pour ajouter une instance Azure SQL Managed Instance à un groupe de basculement

Cet exemple Azure CLI crée deux instances managées, les ajoute à un groupe de basculement, puis teste le basculement de l’instance managée principale vers l’instance managée secondaire.

Si vous choisissez d’installer et d’utiliser l’interface de ligne de commande localement, vous devez exécuter Azure CLI version 2.0 ou une version ultérieure pour poursuivre la procédure décrite dans cet article. Exécutez `az --version` pour trouver la version. Si vous devez effectuer une installation ou une mise à niveau, consultez [Installer Azure CLI](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Exemple de script

### <a name="sign-in-to-azure"></a>Connexion à Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Exécuter le script

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/failover-groups/add-managed-instance-to-failover-group-az-cli.sh "Add managed instance to a failover group")]

### <a name="clean-up-deployment"></a>Nettoyer le déploiement

Utilisez la commande suivante pour supprimer le groupe de ressources et toutes les ressources associées. Vous devez supprimer le groupe de ressources à deux reprises. La première suppression du groupe de ressources entraîne la suppression de l’instance gérée et des clusters virtuels, mais échouera avec le message d’erreur `az group delete : Long running operation failed with status 'Conflict'.`. Exécutez la commande az group delete une deuxième fois pour supprimer toutes les ressources résiduelles ainsi que le groupe de ressources.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Informations de référence sur l’exemple

Ce script utilise les commandes suivantes. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Description |
|---|---|
| [az network vnet](/cli/azure/network/vnet) | Commandes de réseau virtuel.  |
| [az network vnet subnet](/cli/azure/network/vnet/subnet) | Commandes de sous-réseau de réseau virtuel. |
| [az network nsg](/cli/azure/network/nsg) | Commandes de groupe de sécurité réseau. |
| [az network route-table](/cli/azure/network/route-table) | Commandes de table de route. |
| [az sql mi](/cli/azure/sql/mi) | Commandes SQL Managed Instance. |
| [az network public-ip](/cli/azure/network/public-ip) | Commandes d’adresse IP publique réseau. |
| [az network vnet-gateway](/cli/azure/network/vnet-gateway) | Commandes de passerelle de réseau virtuel. |
| [az sql instance-failover-group](/cli/azure/sql/instance-failover-group) | Commandes de groupe de basculement SQL Managed Instance. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](/cli/azure).

Vous trouverez des exemples supplémentaires de scripts CLI SQL Database dans [Documentation Azure SQL Database](../../azure-sql/database/az-cli-script-samples-content-guide.md).
