---
title: Paramètres de chaîne de requête API v3
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: include
ms.date: 06/30/2020
ms.openlocfilehash: 14380dd781fc1eebfe7edb0a816ff8af0f2f17b4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2021
ms.locfileid: "91309424"
---
Voici les paramètres de chaîne de requête API v3 :

|Paramètre de requête.|Nom du portail LUIS|Type|Version|Default|Objectif|
|--|--|--|--|--|--|
|`log`|Enregistrer les journaux|boolean|V2 et V3|false|Stocker la requête dans un fichier journal. La valeur par défaut est false.|
|`query`|-|string|V3 uniquement|Pas de valeur par défaut. Obligatoire dans la demande GET.|**Dans V2**, l’énoncé à prédire se trouve dans le paramètre `q`. <br><br>**Dans V3**, la fonctionnalité est transmise dans le paramètre `query`.|
|`show-all-intents`|Inclure les scores de toutes les intentions|boolean|V3 uniquement|false|Retourner toutes les intentions avec le score correspondant dans l’objet **prediction.intents**. Les intentions sont retournées en tant qu’objets dans un objet `intents` parent. Cela permet un accès programmatique sans qu’il soit nécessaire de rechercher l’intention dans un tableau : `prediction.intents.give`. Dans V2, elles étaient retournées dans un tableau. |
|`verbose`|Inclure plus de détails sur les entités|boolean|V2 et V3|false|**Dans V2**, quand la valeur est true, cela signifie que toutes les intentions prédites ont été retournées. Si vous avez besoin de toutes les intentions prédites, utilisez le paramètre `show-all-intents` de V3.<br><br>**Dans V3**, ce paramètre fournit uniquement les détails des métadonnées d’entité de la prédiction d’entité.  |
|`timezoneOffset`|-|string|V2|-|Fuseau horaire appliqué aux entités datetimeV2.|
|`datetimeReference`|-|string|V3|-|[Fuseau horaire](../luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) appliqué aux entités datetimeV2. Remplace `timezoneOffset` dans V2.|