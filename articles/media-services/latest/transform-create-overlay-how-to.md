---
title: Comment créer une superposition à l’aide de Media Encoder Standard
description: Découvrez comment créer une superposition à l’aide de Media Encoder Standard.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: how-to
ms.date: 08/31/2020
ms.openlocfilehash: 6d68f35cd7eff745a3d17009b65b1ed73a190173
ms.sourcegitcommit: bfa7d6ac93afe5f039d68c0ac389f06257223b42
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2021
ms.locfileid: "106490248"
---
# <a name="how-to-create-an-overlay-with-media-encoder-standard"></a>Comment créer une superposition à l’aide de Media Encoder Standard

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Media Encoder Standard vous permet de superposer une image, un fichier audio ou une autre vidéo sur une autre vidéo. L’entrée doit spécifier un seul fichier. Vous pouvez spécifier un fichier image au format JPG, PNG, GIF ou BMP, un fichier audio (par exemple un fichier WAV, MP3, WMA ou M4A), ou un fichier vidéo.


## <a name="prerequisites"></a>Conditions préalables requises

* Collectez les informations de compte dont vous avez besoin pour configurer le fichier *appsettings.json* dans l’exemple. Si vous n’êtes pas sûr de savoir comment procéder, consultez [Démarrage rapide : Inscrire une application avec la plateforme des identités Microsoft](../../active-directory/develop/quickstart-register-app.md). Les valeurs suivantes sont attendues dans le fichier *appsettings.json*.

    ```json
    {
    "AadClientId": "",
    "AadEndpoint": "https://login.microsoftonline.com",
    "AadSecret": "",
    "AadTenantId": "",
    "AccountName": "",
    "ArmAadAudience": "https://management.core.windows.net/",
    "ArmEndpoint": "https://management.azure.com/",
    "Location": "",
    "ResourceGroup": "",
    "SubscriptionId": ""
    }
    ```

Si vous n’êtes pas déjà familiarisé avec les transformations, il est recommandé d’effectuer les activités suivantes :

* Lisez [Encodage vidéo et audio avec Media Services](encode-concept.md).
* Lisez [Comment encoder avec une transformation personnalisée – .NET](transform-custom-presets-how-to.md). Suivez les étapes de cet article pour configurer le .NET nécessaire à l’utilisation des transformations, puis revenez ici pour tester un exemple de présélection de superpositions.
* Consultez le [document de référence sur les transformations](/rest/api/media/transforms).

Une fois que vous êtes familiarisé avec les transformations, téléchargez l’exemple de superpositions.

## <a name="overlays-preset-sample"></a>Exemple de présélection de superpositions

Téléchargez l’[exemple media-services-overlay](https://github.com/Azure-Samples/media-services-overlays) pour prendre en main les superpositions.

## <a name="next-steps"></a>Étapes suivantes

[!INCLUDE [transforms next steps](./includes/transforms-next-steps.md)]
