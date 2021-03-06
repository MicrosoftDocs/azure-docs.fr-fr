---
title: 'Résoudre les problèmes de connexion point à site : clients Mac OS X'
titleSuffix: Azure VPN Gateway
description: Découvrez comment résoudre les problèmes de connectivité point à site à partir de Mac OS X à l’aide du client VPN natif et d’IKEv2.
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: troubleshooting
ms.date: 02/10/2021
ms.author: alzam
ms.openlocfilehash: d0b8cb4b75bfa162c392549b21f4d09743c37f8e
ms.sourcegitcommit: 49bd8e68bd1aff789766c24b91f957f6b4bf5a9b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2021
ms.locfileid: "108229537"
---
# <a name="troubleshoot-point-to-site-vpn-connections-from-mac-os-x-vpn-clients"></a>Résoudre les problèmes de connexion VPN point à site à partir de clients Mac OS X

Cet article vous aidera à résoudre les problèmes de connectivité point à site à partir de Mac OS X à l’aide du client VPN natif et d’IKEv2. Très basique, le client VPN disponible dans Mac pour IKEv2 n’offre pas beaucoup de possibilités de personnalisation. Il y a seulement quatre paramètres à vérifier :

* Adresse du serveur
* Identifiant distant
* Identifiant local
* Paramètres d’authentification
* Version du système d’exploitation (10.11 ou ultérieure)


## <a name="troubleshoot-certificate-based-authentication"></a><a name="VPNClient"></a> Résoudre les problèmes d’authentification par certificat
1. Vérifiez les paramètres du client VPN. Accédez à la fenêtre **Réseau** en appuyant sur Commande + Maj, puis tapez « VPN » pour vérifier les paramètres du client VPN. Dans la liste, cliquez sur l’entrée VPN à examiner.

   ![Authentification par certificat IKEv2](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2cert1.jpg)
2. Vérifiez que l’**adresse du serveur** correspond au FQDN complet et inclut cloudapp.net.
3. L’**identifiant distant** doit être identique à l’adresse du serveur (FQDN de la passerelle).
4. L’**identifiant local** doit être identique à l’**objet** du certificat client.
5. Cliquez sur **Réglages d’authentification** pour ouvrir la page Réglages d’authentification.

   ![Capture d'écran représentant une boîte de dialogue Paramètres d'authentification dans laquelle Certificat est sélectionné.](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth2.jpg)
6. Vérifiez que **Certificat** est sélectionné dans la liste déroulante.
7. Cliquez sur le bouton **Sélectionner** et vérifiez que le certificat approprié est sélectionné. Cliquez sur **OK** pour enregistrer les éventuelles modifications apportées.

## <a name="troubleshoot-username-and-password-authentication"></a><a name="ikev2"></a>Résoudre les problèmes d’authentification par nom d’utilisateur et mot de passe

1. Vérifiez les paramètres du client VPN. Accédez à la fenêtre **Réseau** en appuyant sur Commande + Maj, puis tapez « VPN » pour vérifier les paramètres du client VPN. Dans la liste, cliquez sur l’entrée VPN à examiner.

   ![Nom d’utilisateur et mot de passe IKEv2](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2user3.jpg)
2. Vérifiez que l’**adresse du serveur** correspond au FQDN complet et inclut cloudapp.net.
3. L’**identifiant distant** doit être identique à l’adresse du serveur (FQDN de la passerelle).
4. L’**identifiant local** peut être vide.
5. Cliquez sur le bouton **Réglages d’authentification** et vérifiez que « Nom d’utilisateur » est sélectionné dans la liste déroulante.

   ![Capture d'écran représentant une boîte de dialogue Paramètres d'authentification dans laquelle Nom d'utilisateur est sélectionné.](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth4.png)
6. Vérifiez que les informations d’identification saisies sont correctes.

## <a name="additional-steps"></a><a name="additional"></a>Étapes supplémentaires

Si vous avez suivi les étapes précédentes et que tout est correctement configuré, téléchargez [Wireshark](https://www.wireshark.org/#download) et effectuez une capture de paquets.

1. Filtrez sur *isakmp* et examinez les paquets **IKE_SA**. Les détails de la proposition de l’association de sécurité doivent figurer sous **Payload: Security Association** (Charge utile : association de sécurité). 
2. Vérifiez que le client et le serveur présentent un jeu commun.

   ![Paquet](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/packet5.jpg) 
  
3. S’il n’existe aucune réponse du serveur sur les traces réseau, vérifiez que vous avez activé le protocole IKEv2 dans la page Configuration de la passerelle Azure sur le site web du Portail Azure.

## <a name="next-steps"></a>Étapes suivantes
Pour obtenir de l’aide supplémentaire, consultez [Support Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
