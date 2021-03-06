---
title: Conseils et incidents fréquemment ayant échoué
description: Décrit des conseils pour l’envoi et la plupart des stratégies ayant échoué
author: laujan
ms.author: lajanuar
ms.topic: how to
keywords: Applications teams validation de la plupart des cas de test ayant échoué AppSource publier
ms.openlocfilehash: 180a12563d11f9089f8350521a40937edb2e6b3e
ms.sourcegitcommit: fdb53284a20285f7e8a7daf25e85cb5d06c52b95
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/11/2020
ms.locfileid: "48992650"
---
# <a name="tips-for-a-successful-app-submission"></a>Conseils relatifs à la soumission d’une application réussie

Cet article aborde les principales raisons d’échec de validation des applications soumises. Bien qu’il ne soit pas prévu d’être une liste exhaustive de tous les problèmes potentiels liés à votre application, ce guide augmente la probabilité que la soumission de votre application passe la première fois. Pour obtenir une liste complète des stratégies de validation, *consultez la rubrique* relative aux [stratégies de certification Marketplace](/legal/marketplace/certification-policies) .

>[!NOTE]
>La **[section 1140](/legal/marketplace/certification-policies#1140-teams)** est spécifique à Microsoft teams et à la **[sous-section 1140,4](https://docs.microsoft.com/legal/marketplace/certification-policies#11404-functionality)** traite des fonctionnalités requises pour les applications Teams.

## <a name="validation-guidelines--most-failed-test-cases"></a>Instructions de validation & la plupart des cas de test ayant échoué

### <a name="9989-general-considerations"></a>Considérations d’ordre général sur les &#9989;

*Voir aussi* la [section 100 — Généralités](/legal/marketplace/certification-policies#100-general)

* Vérifiez que vous utilisez la version 1.4.1 ou une version ultérieure du [Kit de développement logiciel (SDK) de Microsoft teams](https://www.npmjs.com/package/@microsoft/teams-js).
* N’effectuez pas de modifications dans votre application pendant que le processus de validation est en cours. Cette opération nécessite une revalidation complète de votre application.
* Votre application ne doit pas cesser de répondre, se terminer de manière inopinée ou contenir des erreurs de programmation. Si un problème est rencontré, votre application doit échouer et fournir un message de transfert valide à l’utilisateur.
* Votre application ne doit pas télécharger, installer ou lancer automatiquement un code exécutable dans l’environnement de l’utilisateur. Tous les téléchargements doivent demander une autorisation explicite à l’utilisateur.
* Tous les éléments que vous associez à votre expérience, tels que les descriptions et la documentation de support, doivent être précis. Utilisez correctement l'orthographe, les majuscules, la ponctuation et la grammaire.
* Fournir des informations d’aide et de support. Il est vivement recommandé que votre application comprenne un lien Aide/FAQ pour l’expérience utilisateur de première exécution. Pour toutes les applications personnelles, nous vous recommandons de fournir votre page d’aide sous la forme d’un onglet personnel pour une meilleure expérience utilisateur.
* Les applications ne doivent pas déconnecter l’utilisateur de teams pour les scénarios d’utilisateur de base. L’utilisation des onglets/modules de tâches est recommandée pour afficher des informations pour les utilisateurs dans Teams.
* Incrémentez le numéro de version de votre application dans le manifeste si vous effectuez des modifications de manifeste de votre envoi.
* L’application ne doit pas détenir les utilisateurs de teams pour les scénarios d’utilisateur de base. Les cibles de lien dans les applications ne doivent pas se lier à un navigateur externe, mais doivent être liées à des éléments div contenus dans Teams, par exemple à l’intérieur des onglets et des modules tâches.
* Les applications personnelles permettent aux utilisateurs de partager du contenu d’une expérience d’application personnelle avec d’autres membres de l’équipe.

### <a name="9989--provide-a-clear-and-simple-sign-insign-out-and-sign-up-experience"></a>&#9989; fournir une expérience de connexion/déconnexion claire et simple d’accès

*Voir aussi* la [section 1100,5 — Customer Control](/legal/marketplace/certification-policies#11005-customer-control)

* Si votre application ou complément dépend d’un compte ou d’un service externe, l’expérience de connexion/déconnexion et d’inscription doit être visible et accessible sur toutes les fonctionnalités de votre application.
* S’il existe une option de connexion explicite fournie à l’utilisateur, il doit y avoir une option de déconnexion correspondante (même si l’application utilise l' [authentification sans assistance](../../../../tabs/how-to/authentication/auth-silent-aad.md)).
* L’option de déconnexion doit signer l’utilisateur en dehors de la fonctionnalité de votre application et ne pas se connecter au client Teams.
* Au minimum, l’option de déconnexion doit signer l’utilisateur des mêmes fonctionnalités accessibles avec l’option de connexion. Par exemple, si l’option de connexion comprend un onglet et une extension de messagerie, l’option de déconnexion doit inclure l’extension de messagerie et l’onglet.

* Assurez-vous qu’il existe toujours un moyen d’inverser les comportements suivants (ou similaires) :
  * Connexion => déconnexion.
  * Lier un compte/service => rompre le lien d’un compte/service.
  * Connecter un compte/service => déconnecter un compte/service.
  * Autoriser un compte/service => Désautoriser/refuser un compte/service.
  * Inscrire un compte/service => désinscrire/annuler l’abonnement à un compte/service.
* Si votre application nécessite un compte ou un service, vous devez permettre à l’utilisateur de s’inscrire ou de créer une demande d’abonnement. Une exception peut être accordée si votre application requiert une licence d’utilisation. Toutefois, de tels scénarios, un transfert clair pour un nouvel utilisateur doit être fourni.
* Assurez-vous que vous fournissez des instructions de transfert clair à un nouvel utilisateur pour vous inscrire afin d’utiliser vos services d’application. Si un lien de connexion prêt n’est pas disponible, un transfert clair peut être fourni dans les domaines suivants :

> [!div class="checklist"]
>
> * dans les sections de description de votre application ;
> * dans le message d’accueil de votre application ;
> * dans le message d’aide de votre application ;
> * dans la fenêtre où vous demandez à un utilisateur de se connecter à vos services ;

* Les applications qui ne disposent pas d’un flux d’inscription simple peuvent également inclure un onglet d’aide ou un lien vers une page Web où un nouvel utilisateur peut consulter des instructions détaillées sur la configuration de votre application avec Microsoft Teams.  Cela permet de s’assurer qu’un nouvel utilisateur n’est pas bloqué lorsque vous essayez d’effectuer votre application pour la première fois.
* La fonctionnalité de connexion/déconnexion doit fonctionner sur les clients mobiles. Assurez-vous que vous utilisez le [Kit de développement logiciel Microsoft teams](https://www.npmjs.com/package/@microsoft/teams-js) version 1.4.1 ou ultérieure.

Pour plus d’informations sur l’authentification, consultez la rubrique suivante :

* [Documentation de l’authentification](../../../authentication/authentication.md)
* [Exemple d’authentification bot dans le nœud](https://github.com/OfficeDev/microsoft-teams-sample-auth-node)
* [Exemple d’authentification d’onglet dans le nœud](https://github.com/OfficeDev/microsoft-teams-sample-complete-node)
* [Authentification Tab/bot dans C#/.NET](https://github.com/OfficeDev/microsoft-teams-sample-complete-csharp)

### <a name="9989-response-times-must-be-reasonable"></a>Les temps de réponse &#9989; doivent être raisonnables

* **Onglets**. Si une réponse à une action prend plus de trois secondes, vous devez fournir un message de chargement ou un avertissement.
* **Robots**. Une réponse à une commande utilisateur doit se produire dans les deux secondes. Si un traitement plus long est requis, votre application doit afficher un indicateur de saisie.
* **Extensions de composition**. Une réponse à une commande utilisateur doit se produire dans les cinq secondes.

> [!TIP]
> Assurez-vous que votre application affiche un indicateur de chargement ou une forme d’avertissement lorsque votre application prend plus de temps que prévu.

### <a name="9989-tab-content-must-not-have-excessive-chrome-or-layered-navigation"></a>&#9989; le contenu de l’onglet ne doit pas avoir une navigation en couches ou en chrome excessive

* Les onglets doivent fournir du contenu ciblé et éviter des éléments d’interface utilisateur inutiles. En règle générale, il s’agit généralement d’une navigation imbriquée/superposée inutile, d’une interface utilisateur superflue ou inappropriée en regard du contenu, ou de liens permettant à l’utilisateur d’accéder à du contenu non lié. Par exemple, voici un affichage d’onglet qui omet les menus de navigation et qui présente le contenu principal uniquement :

![Vue Web SharePoint](../../../../assets/images/faq/web-sp.png)  
![Affichage de l’onglet SharePoint](../../../../assets/images/faq/tab-sp.png)

* Les tabulations doivent être claires et ne pas inclure une navigation complexe.
* Les onglets de canal qui ont des fonctionnalités de modification complexes dans l’application doivent ouvrir la vue de l’éditeur dans une fenêtre à plusieurs fenêtres plutôt qu’un onglet.
* Les onglets de canal ne doivent pas fournir de barre d’application avec des icônes dans le rail gauche qui entrent en conflit avec la navigation principale de teams.
* Les onglets ne doivent pas présenter de barre d’application avec des icônes qui entrent en conflit avec la navigation principale de teams.
* Les onglets disposant de fonctionnalités d’édition complexes dans l’application doivent ouvrir la vue de l’éditeur dans une fenêtre multiple et non dans l’onglet.
* S’il existe plusieurs options d’affichage, vous pouvez choisir d’utiliser un menu de configuration d’onglet pour l’utilisateur. Par exemple, au lieu d’incorporer un menu à l’intérieur de l’onglet, placez le menu dans la page de configuration de sorte que la vue réelle de l’onglet soit propre et centrée.
* Incluez un onglet *d’aide* sous la forme d’un onglet statique pour informer les utilisateurs de la configuration, de l’inscription et de l’utilisation de votre application.
* Veuillez inclure un onglet *paramètres* qui est disponible à partir de l’en-tête de l’application.

![Page de configuration de l’idée large](../../../../assets/images/faq/wideidea.png)

### <a name="9989-tab-configuration-must-happen-in-the-configuration-screen"></a>La configuration de l’onglet &#9989; doit avoir lieu dans l’écran de configuration

* L’écran de configuration doit expliquer clairement la valeur de l’expérience et comment configurer l’onglet.
* Le processus de configuration doit toujours permettre aux utilisateurs de continuer à ne pas inactifr l’expérience de l’utilisateur. Par exemple, ne pas afficher une carte vide une fois que l’utilisateur a configuré l’onglet
* Le processus de connexion de l’utilisateur doit faire partie du processus de configuration et doit se terminer dans l’interface utilisateur de l’onglet. Une fois que l’utilisateur a terminé la configuration et chargé votre onglet, aucune autre action n’est nécessaire.
* Ne pas afficher l’intégralité de votre page Web dans la fenêtre contextuelle de configuration de connexion.
* Un utilisateur doit toujours pouvoir terminer l’expérience de configuration, même s’il ne parvient pas à trouver immédiatement le contenu qu’il recherche.
* L’expérience de configuration doit fournir des options permettant à l’utilisateur de trouver son contenu, d’épingler une URL ou de créer du contenu s’il n’existe pas.
* L’expérience de configuration doit rester dans le contexte de teams. L’utilisateur ne doit pas laisser l’expérience de configuration pour créer du contenu, puis revenir à teams pour l’épingler.
* Utilisez efficacement la zone de la fenêtre d’affichage disponible. Ne les gaspillez pas à l’aide de logos énormes dans la fenêtre contextuelle configuration

![OneNote permet aux utilisateurs de coller un lien OneNote dans les notes de cas est introuvable](../../../../assets/images/faq/tab-onenote-config.png)

![Les utilisateurs peuvent toujours créer un plan sur le planificateur pour les cas où il n’en existe pas.](../../../../assets/images/faq/tab-planner-config.png)

![SharePoint permet également à l’utilisateur de coller directement une liaison SharePoint](../../../../assets/images/faq/tab-sp-config.png)

### <a name="9989-bots-must-always-be-responsive-and-fail-gracefully"></a>Les robots de &#9989; doivent toujours être réactifs et échouer correctement

Votre robot doit répondre à n’importe quelle commande et ne pas être inactif. Voici quelques conseils pour aider votre robot à répondre intelligemment aux utilisateurs :

* **Utilisez des listes de commandes**. L’analyse de l’entrée utilisateur ou la prévision de l’intention de l’utilisateur est difficile. Au lieu de laisser les utilisateurs deviner ce que votre robot peut faire, fournissez une liste de commandes compréhensibles par votre robot.

![Liste de commandes de flux](../../../../assets/images/faq/flow-bot.png)

* **Inclut une commande aide**. Les utilisateurs sont susceptibles de taper « aide » lorsqu’ils sont perdus ou lorsque votre bot ne répond pas comme prévu. Incluez une commande help qui décrit comment la valeur de votre application sera utilisée avec toutes les commandes valides.

![Commande d’aide au flux](../../../../assets/images/faq/flow-help.png)

* **Inclure le contenu de l’aide ou des conseils en cas de perte de votre bot**. Lorsque votre bot ne peut pas comprendre la saisie de l’utilisateur, elle doit suggérer une autre action. Par exemple, *«je suis désolée, je ne comprends pas. Tapez « aide » pour plus d’informations.* Ne répondez pas avec un message d’erreur ou simplement, *« je ne comprends pas »*. Utilisez cette chance pour enseigner vos utilisateurs.

* **Utilisation de cartes adaptatives et de modules de tâches pour que la réponse de votre bot soit claire et exploitable** 
 [Des cartes adaptatives avec des boutons appelant des modules de tâches](/task-modules-and-cards/task-modules/task-modules-bots) améliorent l’expérience utilisateur du robot. Ces cartes et boutons sont plus faciles à utiliser dans un appareil mobile que si votre utilisateur tape les commandes. De même, les réponses de bot ne doivent pas être textuelles avec du texte long. Les robots doivent utiliser des cartes adaptatives & des modules de tâches au lieu de l’interface utilisateur basée sur conversation conversation et des longues réponses texte

* **Examinez toutes les étendues**. Assurez-vous que votre robot fournit les réponses appropriées lorsqu’il est mentionné ( `@*botname*` ) dans un canal et dans les conversations personnelles. Si votre bot ne fournit pas de contexte explicite au sein de l’étendue personnelle ou Teams, désactivez cette étendue via le manifeste. (Consultez le `bots` bloc dans la [Référence du schéma de manifeste de Microsoft teams](../../../../resources/schema/manifest-schema.md#bots).)

* **Inclure une conversation d’équipe, de groupe ou de conversation 1:1**. Les notifications de bot doivent inclure une équipe, une conversation de groupe ou une conversation un-à-un avec le contenu pertinent pour votre public.

* **Ne pas transmettre les données sensibles**. Les robots ne doivent pas transmettre les données sensibles à une équipe, une conversation de groupe ou une conversation 1:1 où il existe une audience qui ne doit pas être en mesure d’afficher ces données.

* **Fournissez un message de bienvenue**. Le bot doit fournir un message d’accueil FRE incluant un didacticiel interactif avec des cartes de carrousel ou des boutons « essayez » pour encourager l’engagement.

### <a name="9989-personal-bots-must-always-send-a-welcome-message-on-first-launch"></a>&#9989; les robots personnels doivent toujours envoyer un message de bienvenue lors du premier lancement

Un message de bienvenue est le meilleur moyen de définir la tonalité pour votre robot personnel/de conversation. Il s’agit de la première interaction d’un utilisateur avec le bot. Un message de bienvenue peut inciter l’utilisateur à continuer à explorer l’application. Si le message de bienvenue ou d’introduction est confus ou non clair, les utilisateurs ne verront pas immédiatement la valeur de l’application et perdront des intérêts.
Consultez la section ci-dessous pour connaître les conditions requises pour les messages d’accueil.

> [!Note]
> Un message de bienvenue est facultatif pour un robot de canal.

### <a name="welcome-message-requirements"></a>Conditions requises pour les messages d’accueil

* Incluez une proposition de valeur dans la visite d’accueil.
* Fournissez des instructions pour l’utilisation de l’application.
* Inclure des instructions sur la manière de vous inscrire et de configurer votre application
* Présenter un texte facile à lire et un dialogue simple, de préférence une carte avec un bouton de présentation de l’accueil actionnable qui charge un module de tâches.
* Restez facile et utilisable avec des boutons et des cartes : Évitez la boîte de dialogue de texte long.
* Incluez des cartes et des boutons adaptatifs pour faciliter l’utilisation du message de bienvenue.
* Invoquer le message de bienvenue avec une commande ping, pas deux ou plusieurs pings simultanés.
* Un message de bienvenue doit uniquement être affiché à l’utilisateur qui a configuré l’application, de préférence dans une conversation personnelle 1:1.
* Les applications personnelles doivent toujours fournir un message de bienvenue à un utilisateur.
* Ne jamais envoyer de conversation personnelle à chaque membre de l’équipe : il s’agit du courrier indésirable.
* Ne jamais envoyer le message de bienvenue plusieurs fois. Le fait de répéter le même message d’accueil à intervalles réguliers n’est pas autorisé et est considéré comme du courrier indésirable.

#### <a name="avoid-welcome-message-spamming"></a>Éviter le courrier indésirable des messages de bienvenue

* **Canal message par robot**. Ne pas échanger des courriers indésirables en créant des billets de conversation distincts. Créez un billet de thread unique avec des réponses dans le même thread.
* **Conversation personnelle par robot**. N’envoyez pas plusieurs messages. Envoyer un message avec des informations complètes. Le fait de répéter le même message d’accueil à intervalles réguliers n’est pas autorisé et est considéré comme du courrier indésirable.

#### <a name="notification-only-bot-welcome-messages"></a>Messages d’accueil des robots uniquement

Les robots de notification uniquement doivent envoyer un message de bienvenue comprenant un message indiquant *« je suis un robot de notification uniquement et ne pourra pas répondre à vos conversations »*.

#### <a name="welcome-messages-in-the-personal-scope"></a>Messages de bienvenue dans l’étendue personnelle

* **Rendez vos messages concis et instructifs**.  Il est très probable que l’expérience utilisateur et les connaissances de votre application varient. Un utilisateur a peut-être utilisé votre application sur une autre plateforme ou n’a rien à faire sur votre application. Vous souhaitez personnaliser votre message pour toutes les audiences et en quelques phrases Expliquez ce que fait votre bot et les moyens d’interagir avec celui-ci. Vous devez également expliquer la valeur de l’application et la façon dont les utilisateurs peuvent l’utiliser.
![Café et robot dinning](../../../../assets/images/faq/cafe-bot.png)

* **Rendez votre message exploitable**. Réfléchissez à la première chose que les utilisateurs doivent faire après l’installation de votre application. S’agit-il d’une commande intéressante qu’il faut essayer ? Existe-t-il une autre expérience d’intégration ? Doivent-ils se connecter ? Vous pouvez ajouter des actions sur une carte adaptative ou fournir des exemples spécifiques tels que *« essayez de vous demander.... »* , *« Voici ce que je peux faire.*.. ».

#### <a name="welcome-messages-in-the-teamchannel--scope"></a>Messages de bienvenue dans l’étendue de l’équipe/du canal

Les choses sont un peu différentes lorsque le bot est d’abord ajouté à un canal. Normalement, vous ne devez pas envoyer un message 1:1 à tous les membres de l’équipe, mais le bot peut envoyer un message de bienvenue dans le canal.

### <a name="9989-mobile-responsiveness-no-direct-upsell-or-payment"></a>&#9989; la réactivité mobile, pas de promotion directe ni paiement

* Vos onglets, cartes adaptatives, messages robots et contenu dans les modules de tâches doivent être réactifs pour un grand nombre de tailles d’écrans d’appareil mobile.
* Les applications qui prennent en charge iOS doivent être entièrement fonctionnelles sur le dernier appareil iPad à l’aide de la dernière version d’iOS.
* Ne doit pas inclure de référence directe à des achats dans l’application, des offres d’essai, des offres de versions payantes ou des liens vers des magasins en ligne dans lesquels les utilisateurs peuvent acheter ou acquérir d’autres contenus, applications ou compléments à partir de votre application de teams sur un système d’exploitation mobile (Android, iOS).
* La version iOS ou Android du complément ne doit pas afficher l’interface utilisateur ou la langue ni le lien vers les autres applications, compléments ou sites Web qui demandent à l’utilisateur de payer.
* Les pages stratégie de confidentialité et conditions d’utilisation associées doivent également être dépourvues de liens de l’interface utilisateur ou de la boutique de commerce.

### <a name="9989-do-not-post-sensitive-data-to-an-audience-not-intended-to-view-the-data"></a>&#9989; ne publiez pas de données sensibles dans une audience qui n’est pas destinée à afficher les données

Votre application Teams ne doit pas publier de données sensibles telles que l’instrument de paiement par carte de crédit, d’informations d’identification personnelle, d’intégrité ou de contact pour un public non destiné à afficher ces données.

### <a name="9989-do-not-transmit-financial-payment-details-or-complete-financial-transactions-via-your-teams-app"></a>&#9989; ne pas transmettre de détails sur les paiements financiers ni effectuer des transactions financières complètes via votre application teams

* Votre application Teams ne doit pas demander aux utilisateurs d’effectuer un paiement directement dans l’interface teams
* Il se peut que les applications ne transmettent pas les détails de l’instrument financier via l’interface de l’utilisateur. Les applications peuvent uniquement transmettre des liens vers des services de paiement sécurisés aux utilisateurs si cela est indiqué dans les conditions d’utilisation de l’application, la politique de confidentialité, ainsi que les pages de profil ou le site Web de l’application avant qu’un utilisateur accepte l’utilisation de l’application.

### <a name="9989-clear-warning-before-downloading-any-files-or-executable-exe-into-a-users-environment"></a>&#9989; avertissement de suppression avant de télécharger des fichiers ou des fichiers exécutables ( `.exe` ) dans l’environnement d’un utilisateur

Avertissez les utilisateurs avant que votre application télécharge des fichiers ou des fichiers exécutables ( `.exe`  ) sur l’ordinateur ou l’environnement de l’utilisateur.

### <a name="9989-messaging-extensions-should-provide-help-text-and-be-easy-to-read"></a>Les extensions de messagerie &#9989; doivent fournir un texte d’aide et être faciles à lire

* L’extension de messagerie basée sur la recherche doit fournir un texte d’aide sur la façon de rechercher efficacement (par exemple, afficher un exemple d’entrée).
* Les modules de tâches doivent inclure une icône et un nom court qu’ils sont contenus ou créés à partir de l’application.
* Les `@mention` fichiers exécutables d’extension de message doivent être clairs, faciles à comprendre et faciles à lire.
![Extension de message](../../../../assets/images/faq/message-extension.png)

## <a name="app-category-mapping"></a>Mappage des catégories d’application

| Catégorie teams       | Catégories de PC  |
|:---------------------|:---------------|
| Analyse et BI | Analyse, visualisation des données et BI |
| Développeur et informatique | Outils de développement, administrateur informatique |
| Éducation | Éducation |
| ressources humaines ; | Ressources humaines et recrutement |
| Productivité | Gestion de contenu, fichiers et documents, productivité, formation et didacticiels et utilitaires |
| Gestion de projets | Communication, gestion de projet, flux de travail et gestion des activités |
| Ventes et support | Gestion des clients et des contacts, support technique, gestion financière, ventes et marketing |
| Sociales et loisirs | Galeries d’images et de vidéos, style de vie, Actualités et météo, réseaux sociaux, voyages et navigation |

> [!div class="nextstepaction"]
> [En savoir plus sur les stratégies d’approbation des applications teams](/legal/marketplace/certification-policies#1140-teams)
