---
title: Établir une connexion série à votre DK Azure Percept
description: Comment configurer une connexion série à votre DK Azure Percept avec un câble série USB à TTL
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/03/2021
ms.custom: template-how-to
ms.openlocfilehash: b1cb975c65f2234892cfef919220c68ebf5b2dca
ms.sourcegitcommit: 1fbd591a67e6422edb6de8fc901ac7063172f49e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2021
ms.locfileid: "109488264"
---
# <a name="connect-to-your-azure-percept-dk-over-serial"></a>Établir une connexion série à votre DK Azure Percept

Effectuez les étapes ci-dessous pour configurer une connexion série à votre DK Azure Percept avec [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).

> [!WARNING]
> **N’essayez pas** d’établir une connexion série à votre devkit, sauf en cas de panne extrême (par exemple, vous avez « brické » votre appareil). Il est très difficile de démonter le boîtier de la carte de base pour connecter le câble série, ce qui a pour effet de casser les câbles de l’antenne Wi-Fi.

## <a name="prerequisites"></a>Prérequis

- [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- PC hôte
- DK Azure Percept
- [Câble série USB à TTL](https://www.adafruit.com/product/954)

    :::image type="content" source="./media/how-to-connect-to-percept-dk-over-serial/usb-serial-cable.png" alt-text="Câble série USB à TTL.":::

## <a name="start-the-serial-connection"></a>Lancer la connexion série

1. Si votre carte de base est connectée à un rail 80/20, retirez-la du rail à l’aide de la clé hexagonale (incluse dans la carte de bienvenue du devkit).

1. Retirez les vis situées sous le boîtier de la carte de base et extrayez la carte mère.

    > [!WARNING]
    > Le retrait de la carte mère entraîne la rupture des câbles de l’antenne Wi-Fi. **Ne procédez pas** à la connexion série, sauf s’il s’agit de votre dernier recours pour récupérer l’appareil.

1. Retirez le dissipateur thermique.

1. Retirez la carte de connexion des broches GPIO.

    > [!TIP]
    > Notez l’orientation de la carte de connexion avant de la retirer. Par exemple, dessinez une flèche ou collez un autocollant sur la carte de connexion pointant vers la circuiterie pour référence. La carte de connexion n’est pas détrompée et peut être accidentellement connectée à l’envers quand vous remontez votre carte de base.

1. Connectez le [câble série USB à TTL](https://www.adafruit.com/product/954) aux broches GPIO sur la carte mère comme indiqué ci-dessous.

    - Connectez le câble noir (GND) à la broche 6.
    - Connectez le câble blanc (RX) à la broche 8.
    - Connectez le câble vert (TX) à la broche 10.

    :::image type="content" source="./media/how-to-connect-to-percept-dk-over-serial/serial-connection-carrier-board.png" alt-text="Connexions des broches série de la carte de base.":::

1. Mettez sous tension votre devkit et connectez la partie USB du câble série à votre PC.

1. Dans Windows, accédez à **Démarrer** -> **Paramètres de Windows Update** -> **Afficher les mises à jour facultatives** -> **Mises à jour de pilotes** Recherchez une mise à jour Série à USB dans la liste, cochez la case en regard de celle-ci, puis sélectionnez **Télécharger et installer**.  

1. Ensuite, ouvrez le gestionnaire de périphériques Windows (**Démarrer** -> **Gestionnaire de périphériques**). Accédez à **Ports**, puis sélectionnez **USB à UART** pour ouvrir **Propriétés**. Notez le port COM auquel votre appareil est connecté.

1. Sélectionnez l’onglet **Paramètres du port**. Vérifiez que **Bits par seconde** a la valeur 115200.

1. Ouvrez PuTTY. Entrez les informations suivantes, puis sélectionnez **Ouvrir** pour établir une connexion série à votre devkit :

    1. Ligne série : COM[port #]
    1. Vitesse : 115200
    1. Type de connexion : Série

    :::image type="content" source="./media/how-to-connect-to-percept-dk-over-serial/putty-serial-session.png" alt-text="Fenêtre de session PuTTY avec les paramètres de connexion série sélectionnés.":::
