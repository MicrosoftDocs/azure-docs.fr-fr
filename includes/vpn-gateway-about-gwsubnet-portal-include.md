---
title: Fichier include
description: Fichier include
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/27/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 83bc155050c0c2aa27c565c757d4cb8a38046af6
ms.sourcegitcommit: 1b698fb8ceb46e75c2ef9ef8fece697852c0356c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/28/2021
ms.locfileid: "110658199"
---
La passerelle de réseau virtuel utilise un sous-réseau spécifique, appelé sous-réseau de passerelle. Le sous-réseau de passerelle fait partie de la plage d’adresses IP du réseau virtuel que vous spécifiez lors de la configuration de votre réseau virtuel. Il contient les adresses IP utilisées par les ressources et les services de passerelle de réseau virtuel. 


Lorsque vous créez le sous-réseau de passerelle, vous spécifiez le nombre d’adresses IP que contient le sous-réseau. Le nombre d’adresses IP requises varie selon la configuration de la passerelle VPN que vous souhaitez créer. Certaines configurations nécessitent plus d’adresses IP que d’autres. Nous vous recommandons de créer un sous-réseau de passerelle dont la taille est définie sur /27 ou /28.


Si une erreur s’affiche indiquant que l’espace d’adressage chevauche un sous-réseau, ou que le sous-réseau n’est pas contenu dans l’espace d’adressage de votre réseau virtuel, vérifiez la plage d’adresses de votre réseau virtuel. Vous n’avez peut-être pas suffisamment d’adresses IP disponibles dans la plage d’adresses que vous avez créée pour votre réseau virtuel. Par exemple, si votre sous-réseau par défaut englobe la plage d’adresses complète, il n’y a plus d’adresses IP disponibles pour créer des sous-réseaux supplémentaires. Vous pouvez ajuster vos sous-réseaux au sein de l’espace d’adressage existant pour libérer des adresses IP, ou spécifier une plage d’adresses supplémentaire pour y créer le sous-réseau de passerelle.