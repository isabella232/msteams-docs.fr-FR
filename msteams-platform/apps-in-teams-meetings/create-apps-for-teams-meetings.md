---
title: Créer des applications pour les réunions teams
author: laujan
description: créer des applications pour les réunions teams
ms.topic: conceptual
ms.author: lajanuar
keywords: applications Team Apps Meeting User Role Role API
ms.openlocfilehash: 83e0a5b53e363a090935b4afa9840dd96c5f7381
ms.sourcegitcommit: b01986739a05c65094618fbe76aeb53d038b1c74
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2020
ms.locfileid: "48181987"
---
# <a name="create-apps-for-teams-meetings-preview"></a><span data-ttu-id="fbda0-104">Créer des applications pour les réunions Teams (aperçu)</span><span class="sxs-lookup"><span data-stu-id="fbda0-104">Create apps for Teams meetings (Preview)</span></span>

>[!IMPORTANT]
> <span data-ttu-id="fbda0-105">Les fonctionnalités incluses dans l’aperçu de Microsoft teams sont fournies à des fins d’accès anticipé, de test et de commentaires uniquement.</span><span class="sxs-lookup"><span data-stu-id="fbda0-105">Features included in Microsoft Teams preview are provided for early-access, testing, and feedback purposes only.</span></span> <span data-ttu-id="fbda0-106">Ils peuvent être soumis à des modifications avant de devenir disponibles dans la publication publique et ne doivent pas être utilisés dans les applications de production.</span><span class="sxs-lookup"><span data-stu-id="fbda0-106">They may undergo changes before becoming available in the public release and should not be used in production applications.</span></span>

## <a name="prerequisites-and-considerations"></a><span data-ttu-id="fbda0-107">Conditions préalables et considérations</span><span class="sxs-lookup"><span data-stu-id="fbda0-107">Prerequisites and considerations</span></span>

1. <span data-ttu-id="fbda0-108">Les applications dans les réunions nécessitent des connaissances de base sur le [développement d’applications teams](../overview.md).</span><span class="sxs-lookup"><span data-stu-id="fbda0-108">Apps in meetings require some basic knowledge of [Teams app development](../overview.md).</span></span> <span data-ttu-id="fbda0-109">Une application dans une réunion peut comprendre des fonctionnalités d' [onglets](../tabs/what-are-tabs.md), de [robots](../bots/what-are-bots.md)et d' [extensions de messagerie](../messaging-extensions/what-are-messaging-extensions.md) , et nécessitera des mises à jour du manifeste d' [application](#update-your-app-manifest) teams pour indiquer que l’application est disponible pour les réunions.</span><span class="sxs-lookup"><span data-stu-id="fbda0-109">An app in a meeting can comprise of [tabs](../tabs/what-are-tabs.md), [bots](../bots/what-are-bots.md), and [messaging extensions](../messaging-extensions/what-are-messaging-extensions.md) features and will require updates to the Teams [app manifest](#update-your-app-manifest) to indicate that the app is available for meetings</span></span>

1. <span data-ttu-id="fbda0-110">Pour que votre application fonctionne dans le cycle de vie de la réunion sous la forme d’un onglet, elle doit prendre en charge les onglets configurables dans l' [étendue groupchat](../resources/schema/manifest-schema.md#configurabletabs).</span><span class="sxs-lookup"><span data-stu-id="fbda0-110">For your app to function in the meeting lifecycle as a tab, it must support configurable tabs in the [groupchat scope](../resources/schema/manifest-schema.md#configurabletabs).</span></span> <span data-ttu-id="fbda0-111">*Voir* [étendre votre application teams avec un onglet personnalisé](../tabs/how-to/add-tab.md). La prise en charge de l' `groupchat` étendue activera votre application dans les conversations [pré-réunion](teams-apps-in-meetings.md#pre-meeting-app-experience) et [post-réunion](teams-apps-in-meetings.md#post-meeting-app-experience) .</span><span class="sxs-lookup"><span data-stu-id="fbda0-111">*See* [Extend your Teams app with a custom tab](../tabs/how-to/add-tab.md). Supporting the `groupchat` scope will enable your app in [pre-meeting](teams-apps-in-meetings.md#pre-meeting-app-experience) and [post-meeting](teams-apps-in-meetings.md#post-meeting-app-experience) chats.</span></span>

1. <span data-ttu-id="fbda0-112">Les paramètres de l’URL de l’API de réunion peuvent exiger `meetingId` , `userId` et les [tenantId](/onedrive/find-your-office-365-tenant-id) disponibles dans le cadre du kit de développement logiciel client teams et de l’activité du robot.</span><span class="sxs-lookup"><span data-stu-id="fbda0-112">Meeting API URL parameters may require `meetingId`, `userId`, and the [tenantId](/onedrive/find-your-office-365-tenant-id) These are available as part of the Teams Client SDK and bot activity.</span></span> <span data-ttu-id="fbda0-113">En outre, des informations fiables pour l’ID d’utilisateur et l’ID de client peuvent être récupérées à l’aide de [l’authentification SSO de tabulation](../tabs/how-to/authentication/auth-aad-sso.md).</span><span class="sxs-lookup"><span data-stu-id="fbda0-113">Additionally, reliable information for user ID and tenant ID can be retrieved using [Tab SSO authentication](../tabs/how-to/authentication/auth-aad-sso.md).</span></span>

1. <span data-ttu-id="fbda0-114">Certaines API de réunion, telles que `GetParticipant` nécessiteront un [ID d’enregistrement et d’application bot](../bots/how-to/create-a-bot-for-teams.md#with-an-azure-subscription) pour générer des jetons d’authentification.</span><span class="sxs-lookup"><span data-stu-id="fbda0-114">Some meeting APIs, such as `GetParticipant` will require a [bot registration and bot app ID](../bots/how-to/create-a-bot-for-teams.md#with-an-azure-subscription) to generate auth tokens.</span></span>

1. <span data-ttu-id="fbda0-115">Les développeurs doivent respecter les [conseils généraux de conception des onglets teams](../tabs/design/tabs.md) pour les scénarios antérieurs et postérieurs à la réunion, ainsi que les [recommandations](designing-in-meeting-dialog.md) pour la boîte de dialogue de réunion déclenchée lors d’une réunion Teams.</span><span class="sxs-lookup"><span data-stu-id="fbda0-115">Developers must adhere to general [Teams tab design guidelines](../tabs/design/tabs.md) for pre- and post-meeting scenarios as well as the [in-meeting dialog guidelines](designing-in-meeting-dialog.md) for in-meeting dialog triggered during a Teams meeting.</span></span>

## <a name="meeting-apps-api-reference"></a><span data-ttu-id="fbda0-116">Référence de l’API des applications de réunion</span><span class="sxs-lookup"><span data-stu-id="fbda0-116">Meeting apps API reference</span></span>

|<span data-ttu-id="fbda0-117">API</span><span class="sxs-lookup"><span data-stu-id="fbda0-117">API</span></span>|<span data-ttu-id="fbda0-118">Description</span><span class="sxs-lookup"><span data-stu-id="fbda0-118">Description</span></span>|<span data-ttu-id="fbda0-119">Demande</span><span class="sxs-lookup"><span data-stu-id="fbda0-119">Request</span></span>|<span data-ttu-id="fbda0-120">Source</span><span class="sxs-lookup"><span data-stu-id="fbda0-120">Source</span></span>|
|---|---|----|---|
|<span data-ttu-id="fbda0-121">**GetUserContext**</span><span class="sxs-lookup"><span data-stu-id="fbda0-121">**GetUserContext**</span></span>| <span data-ttu-id="fbda0-122">Obtenir des informations contextuelles pour afficher le contenu pertinent dans un onglet Teams.</span><span class="sxs-lookup"><span data-stu-id="fbda0-122">Get contextual information to display relevant content in a Teams tab.</span></span> |<span data-ttu-id="fbda0-123">_**microsoftTeams. getContext (() => {/*...\* / } )*\*_</span><span class="sxs-lookup"><span data-stu-id="fbda0-123">_**microsoftTeams.getContext( ( ) => {  /*...*/ } )**_</span></span>|<span data-ttu-id="fbda0-124">Kit de développement logiciel client Microsoft teams</span><span class="sxs-lookup"><span data-stu-id="fbda0-124">Microsoft Teams client SDK</span></span>|
|<span data-ttu-id="fbda0-125">**GetParticipant**</span><span class="sxs-lookup"><span data-stu-id="fbda0-125">**GetParticipant**</span></span>|<span data-ttu-id="fbda0-126">Cette API permet à un bot d’extraire des informations sur les participants par ID de réunion et ID de participant.</span><span class="sxs-lookup"><span data-stu-id="fbda0-126">This API allows a bot to fetch a participant information by meeting id and participant id.</span></span>|<span data-ttu-id="fbda0-127">**Obtenir** _ **/v1/meetings/{meetingId}/participants/{participantId} ? tenantId = {tenantId}**_</span><span class="sxs-lookup"><span data-stu-id="fbda0-127">**GET** _**/v1/meetings/{meetingId}/participants/{participantId}?tenantId={tenantId}**_</span></span> |<span data-ttu-id="fbda0-128">Kit de développement logiciel (SDK) Microsoft bot Framework</span><span class="sxs-lookup"><span data-stu-id="fbda0-128">Microsoft Bot Framework SDK</span></span>|
|<span data-ttu-id="fbda0-129">**NotificationSignal**</span><span class="sxs-lookup"><span data-stu-id="fbda0-129">**NotificationSignal**</span></span> |<span data-ttu-id="fbda0-130">Les signaux de réunion seront remis à l’aide de l’API de notification de conversation existante suivante (pour la conversation utilisateur-bot).</span><span class="sxs-lookup"><span data-stu-id="fbda0-130">Meeting signals will be delivered using the following existing conversation notification API (for user-bot chat).</span></span> <span data-ttu-id="fbda0-131">Cette API permet aux développeurs de signaler en fonction de l’action de l’utilisateur final à afficher-case une bulle de dialogue de réunion.</span><span class="sxs-lookup"><span data-stu-id="fbda0-131">This API allows developers to signal based on end-user action to show-case an in-meeting dialog bubble.</span></span>|<span data-ttu-id="fbda0-132">**Post** _ **/v3/conversations/{conversationId}/Activities**_</span><span class="sxs-lookup"><span data-stu-id="fbda0-132">**POST** _**/v3/conversations/{conversationId}/activities**_</span></span>|<span data-ttu-id="fbda0-133">Kit de développement logiciel (SDK) Microsoft bot Framework</span><span class="sxs-lookup"><span data-stu-id="fbda0-133">Microsoft Bot Framework SDK</span></span>|

### <a name="getusercontext"></a><span data-ttu-id="fbda0-134">GetUserContext</span><span class="sxs-lookup"><span data-stu-id="fbda0-134">GetUserContext</span></span>

<span data-ttu-id="fbda0-135">Pour obtenir des instructions sur l’identification et la récupération des informations contextuelles pour votre contenu d’onglet, consultez notre [contexte de l’onglet teams](../tabs/how-to/access-teams-context.md#getting-context-by-using-the-microsoft-teams-javascript-library) .</span><span class="sxs-lookup"><span data-stu-id="fbda0-135">Please refer to our [Get context for your Teams tab](../tabs/how-to/access-teams-context.md#getting-context-by-using-the-microsoft-teams-javascript-library) documentation for guidance on identifying and  retrieving contextual information for your tab content.</span></span> <span data-ttu-id="fbda0-136">Dans le cadre de l’extensibilité des réunions, une nouvelle valeur a été ajoutée pour la charge utile de la réponse :</span><span class="sxs-lookup"><span data-stu-id="fbda0-136">As part of meetings extensibility, a new value has been added for the response payload:</span></span>

<span data-ttu-id="fbda0-137">✔ **meetingId**: utilisé par un onglet lors de l’exécution dans le contexte de la réunion.</span><span class="sxs-lookup"><span data-stu-id="fbda0-137">✔ **meetingId**: used by a tab when running in the meeting context.</span></span>

### <a name="getparticipant-api"></a><span data-ttu-id="fbda0-138">API GetParticipant</span><span class="sxs-lookup"><span data-stu-id="fbda0-138">GetParticipant API</span></span>

> [!NOTE]
>
> * <span data-ttu-id="fbda0-139">Ne pas mettre en cache les rôles des participants étant donné que l’organisateur de la réunion peut modifier un rôle à tout moment.</span><span class="sxs-lookup"><span data-stu-id="fbda0-139">Do not cache participant roles since the meeting organizer can change a role at any point in time.</span></span>
>
> * <span data-ttu-id="fbda0-140">Teams ne prend actuellement pas en charge les listes de distribution volumineuses ni les tailles de liste de plus de 350 participants pour l' `GetParticipant` API.</span><span class="sxs-lookup"><span data-stu-id="fbda0-140">Teams does not currently support large distribution lists or roster sizes of more than 350 participants for the `GetParticipant` API.</span></span>
>
> * <span data-ttu-id="fbda0-141">La prise en charge du kit de développement logiciel (SDK) de robot sera bientôt disponible.</span><span class="sxs-lookup"><span data-stu-id="fbda0-141">Support for the Bot Framework SDK is coming soon.</span></span>

#### <a name="request"></a><span data-ttu-id="fbda0-142">Demande</span><span class="sxs-lookup"><span data-stu-id="fbda0-142">Request</span></span>

```http
GET /v3/meetings/{meetingId}/participants/{participantId}?tenantId={tenantId}
```

<span data-ttu-id="fbda0-143">*Voir* la référence de l’API de l' [infrastructure bot](/azure/bot-service/rest-api/bot-framework-rest-connector-api-reference?view=azure-bot-service-4.0&preserve-view=true).</span><span class="sxs-lookup"><span data-stu-id="fbda0-143">*See* the [Bot Framework API reference](/azure/bot-service/rest-api/bot-framework-rest-connector-api-reference?view=azure-bot-service-4.0&preserve-view=true).</span></span>

<!-- markdownlint-disable MD025 -->

<span data-ttu-id="fbda0-144">**Exemple C#**</span><span class="sxs-lookup"><span data-stu-id="fbda0-144">**C# Example**</span></span>

```csharp
string meetingId = "meetingid?";
string participantId = "participantidhere";
var connectorClient = turnContext.TurnState.Get<IConnectorClient>();
var creds = connectorClient.Credentials as AppCredentials;
var bearerToken = await creds.GetTokenAsync().ConfigureAwait(false);
var request = new HttpRequestMessage();
request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
request.Method = new HttpMethod("GET");
request.RequestUri = new System.Uri(Path.Combine(connectorClient.BaseUri.OriginalString, $"/meetings/{meetingId}/participants/{participantId}"));
HttpResponseMessage response = await (connectorClient as ServiceClient<ConnectorClient>).HttpClient.SendAsync(request, cancellationToken).ConfigureAwait(false);
if (response.StatusCode == System.Net.HttpStatusCode.OK)
{
    var content = await response.Content.ReadAsStringAsync().ConfigureAwait(false);
    var theObject = Rest.Serialization.SafeJsonConvert.DeserializeObject<WhateverObjectIsReturned>(content, connectorClient.DeserializationSettings);
```

* * *
<!-- markdownlint-disable MD001 -->

#### <a name="query-parameters"></a><span data-ttu-id="fbda0-145">Paramètres de requête</span><span class="sxs-lookup"><span data-stu-id="fbda0-145">Query parameters</span></span>

<span data-ttu-id="fbda0-146">**meetingId**.</span><span class="sxs-lookup"><span data-stu-id="fbda0-146">**meetingId**.</span></span> <span data-ttu-id="fbda0-147">L’identificateur de la réunion est obligatoire.</span><span class="sxs-lookup"><span data-stu-id="fbda0-147">The meeting identifier is required.</span></span>  
<span data-ttu-id="fbda0-148">**participantId**.</span><span class="sxs-lookup"><span data-stu-id="fbda0-148">**participantId**.</span></span> <span data-ttu-id="fbda0-149">L’identificateur du participant est obligatoire.</span><span class="sxs-lookup"><span data-stu-id="fbda0-149">The participant identifier is required.</span></span>  
<span data-ttu-id="fbda0-150">**tenantId**.</span><span class="sxs-lookup"><span data-stu-id="fbda0-150">**tenantId**.</span></span> <span data-ttu-id="fbda0-151">[ID de client](/onedrive/find-your-office-365-tenant-id) du participant.</span><span class="sxs-lookup"><span data-stu-id="fbda0-151">[Tenant id](/onedrive/find-your-office-365-tenant-id) of the participant.</span></span> <span data-ttu-id="fbda0-152">Obligatoire pour l’utilisateur client.</span><span class="sxs-lookup"><span data-stu-id="fbda0-152">Required for tenant user.</span></span>

#### <a name="response-payload"></a><span data-ttu-id="fbda0-153">Charge utile de réponse</span><span class="sxs-lookup"><span data-stu-id="fbda0-153">Response Payload</span></span>
<!-- markdownlint-disable MD036 -->

<span data-ttu-id="fbda0-154">**Exemple 1**</span><span class="sxs-lookup"><span data-stu-id="fbda0-154">**Example 1**</span></span>

```json
{
    "meetingRole":"Presenter",
    "conversation":{
            "isGroup": true,
            "id": "19:meeting_NDQxMzg1YjUtMGIzNC00Yjc1LWFmYWYtYzk1MGY2MTMwNjE0@thread.v2"
        }
}
```

<span data-ttu-id="fbda0-155">**meetingRole** peut être *organisateur*, *présentateur*ou *participant*.</span><span class="sxs-lookup"><span data-stu-id="fbda0-155">**meetingRole** can be *Organizer*, *Presenter*, or *Attendee*.</span></span>

<span data-ttu-id="fbda0-156">**Exemple 2**</span><span class="sxs-lookup"><span data-stu-id="fbda0-156">**Example 2**</span></span>

```json
{
    "meetingRole": "Attendee",
    "conversation": {
        "isGroup": true,
        "id": "19:meeting_OWIyYWVhZWMtM2ExMi00ZTc2LTg0OGEtYWNhMTM4MmZlZTNj@thread.v2"
        }
}
```

#### <a name="response-codes"></a><span data-ttu-id="fbda0-157">Codes de réponse</span><span class="sxs-lookup"><span data-stu-id="fbda0-157">Response Codes</span></span>

<span data-ttu-id="fbda0-158">**403**: l’application n’est pas autorisée à obtenir des informations sur les participants.</span><span class="sxs-lookup"><span data-stu-id="fbda0-158">**403**: the app is not allowed to get participant information.</span></span> <span data-ttu-id="fbda0-159">Il s’agit de la réponse d’erreur la plus courante, qui est déclenchée lorsque l’application n’est pas installée dans la réunion, par exemple lorsque l’application est désactivée par l’administrateur client ou bloquée lors de l’atténuation du site actif.</span><span class="sxs-lookup"><span data-stu-id="fbda0-159">This will be the most common error response and is triggered when the app is not installed in the meeting such as when the app is disabled by tenant admin or blocked during live site mitigation.</span></span>  
<span data-ttu-id="fbda0-160">**200**: informations sur les participants récupérées avec succès</span><span class="sxs-lookup"><span data-stu-id="fbda0-160">**200**: participant information successfully retrieved</span></span>  
<span data-ttu-id="fbda0-161">**401**: jeton non valide</span><span class="sxs-lookup"><span data-stu-id="fbda0-161">**401**: invalid token</span></span>  
<span data-ttu-id="fbda0-162">**404**: la réunion n’existe pas ou le participant est introuvable.</span><span class="sxs-lookup"><span data-stu-id="fbda0-162">**404**: the meeting doesn't exist or participant can’t be found.</span></span>

<!-- markdownlint-disable MD024 -->
### <a name="notificationsignal-api"></a><span data-ttu-id="fbda0-163">API NotificationSignal</span><span class="sxs-lookup"><span data-stu-id="fbda0-163">NotificationSignal API</span></span>

> [!NOTE]
> <span data-ttu-id="fbda0-164">Lorsqu’une boîte de dialogue de réunion est appelée, le même contenu s’affiche également sous la forme d’un message de conversation.</span><span class="sxs-lookup"><span data-stu-id="fbda0-164">When an in-meeting dialog is invoked, the same content will also be presented as a chat message.</span></span>

#### <a name="request"></a><span data-ttu-id="fbda0-165">Demande</span><span class="sxs-lookup"><span data-stu-id="fbda0-165">Request</span></span>

```http
POST /v3/conversations/{conversationId}/activities
```

#### <a name="query-parameters"></a><span data-ttu-id="fbda0-166">Paramètres de requête</span><span class="sxs-lookup"><span data-stu-id="fbda0-166">Query parameters</span></span>

<span data-ttu-id="fbda0-167">**conversationId**: identificateur de conversation.</span><span class="sxs-lookup"><span data-stu-id="fbda0-167">**conversationId**: The conversation identifier.</span></span> <span data-ttu-id="fbda0-168">Obligatoire</span><span class="sxs-lookup"><span data-stu-id="fbda0-168">Required</span></span>

#### <a name="request-payload"></a><span data-ttu-id="fbda0-169">Charge utile de la demande</span><span class="sxs-lookup"><span data-stu-id="fbda0-169">Request Payload</span></span>

# <a name="json"></a>[<span data-ttu-id="fbda0-170">JSON</span><span class="sxs-lookup"><span data-stu-id="fbda0-170">JSON</span></span>](#tab/json)

```json
{
    "type": "message",
    "text": "John Phillips assigned you a weekly todo",
    "summary": "Don't forget to meet with Marketing next week",
    "channelData": {
    "notification": {
    "alert": true,
    "externalResourceUrl": "https://teams.microsoft.com/l/bubble/APP_ID?url=&height=&width=&title=<TaskInfo.title>"
    }
},
    "replyToId": "1493070356924"
    }
```

# <a name="cnet"></a>[<span data-ttu-id="fbda0-171">C#/.NET</span><span class="sxs-lookup"><span data-stu-id="fbda0-171">C#/.NET</span></span>](#tab/dotnet)

```csharp
Activity activity = MessageFactory.Text("This is a meeting signal test");
MeetingNotification notification = new MeetingNotification
{
    AlertInMeeting = true,
    ExternalResourceUrl = "https://teams.microsoft.com/l/bubble/APP_ID?url=&height=&width=&title=<TaskInfo.title>"
};
activity.ChannelData = new TeamsChannelData
{
    Notification = notification
};
await turnContext.SendActivityAsync(activity).ConfigureAwait(false);
```

# <a name="javascript"></a>[<span data-ttu-id="fbda0-172">JavaScript</span><span class="sxs-lookup"><span data-stu-id="fbda0-172">JavaScript</span></span>](#tab/javascript)

```javascript

const replyActivity = MessageFactory.text('Hi'); // this could be an adaptive card instead
        replyActivity.channelData = {
            notification: {
                alertInMeeting: true,
                externalResourceUrl: 'https://teams.microsoft.com/l/bubble/APP_ID?url=<TaskInfo.url>&height=<TaskInfo.height>&width=<TaskInfo.width>&title=<TaskInfo.title>’
            }
        };
        await context.sendActivity(replyActivity);
```

* * *

> [!IMPORTANT]
> <span data-ttu-id="fbda0-173">L’URL dans la bulle de contenu (URL de taskInfo) doit être incluse dans la liste des [domaines valide](../resources/schema/manifest-schema.md#validdomains) incluse dans le manifeste de l’application Teams.</span><span class="sxs-lookup"><span data-stu-id="fbda0-173">The URL in the content bubble (taskInfo URL) must be included in the [valid domains](../resources/schema/manifest-schema.md#validdomains) list included in the Teams app manifest.</span></span>

#### <a name="response-codes"></a><span data-ttu-id="fbda0-174">Codes de réponse</span><span class="sxs-lookup"><span data-stu-id="fbda0-174">Response Codes</span></span>

<span data-ttu-id="fbda0-175">**201**: l’activité avec le signal est correctement envoyée</span><span class="sxs-lookup"><span data-stu-id="fbda0-175">**201**: activity with signal is successfully sent</span></span>  
<span data-ttu-id="fbda0-176">**401**: jeton non valide</span><span class="sxs-lookup"><span data-stu-id="fbda0-176">**401**: invalid token</span></span>  
<span data-ttu-id="fbda0-177">**403**: l’application n’est pas autorisée à envoyer le signal.</span><span class="sxs-lookup"><span data-stu-id="fbda0-177">**403**: the app is not allowed to send the signal.</span></span> <span data-ttu-id="fbda0-178">Dans ce cas, la charge utile doit contenir plus de détails sur les messages d’erreur.</span><span class="sxs-lookup"><span data-stu-id="fbda0-178">In this case, the payload should contain more detail error message.</span></span> <span data-ttu-id="fbda0-179">Il peut y avoir plusieurs raisons : l’application est désactivée par l’administrateur client, bloquée lors de la limitation du site actif, etc.</span><span class="sxs-lookup"><span data-stu-id="fbda0-179">There can be many reasons: app disabled by tenant admin, blocked during live site mitigation, etc.</span></span>  
<span data-ttu-id="fbda0-180">**404**: la conversation de réunion n’existe pas</span><span class="sxs-lookup"><span data-stu-id="fbda0-180">**404**: meeting chat doesn't exist</span></span>  

## <a name="enable-your-app-for-teams-meetings"></a><span data-ttu-id="fbda0-181">Activer votre application pour les réunions teams</span><span class="sxs-lookup"><span data-stu-id="fbda0-181">Enable your app for Teams meetings</span></span>

### <a name="update-your-app-manifest"></a><span data-ttu-id="fbda0-182">Mettre à jour le manifeste de votre application</span><span class="sxs-lookup"><span data-stu-id="fbda0-182">Update your app manifest</span></span>

<span data-ttu-id="fbda0-183">Les fonctionnalités des applications de réunion sont déclarées dans le manifeste **configurableTabs**de votre application via les  ->  **étendues** configurableTabs et les tableaux de **contexte** .</span><span class="sxs-lookup"><span data-stu-id="fbda0-183">The meetings app capabilities are declared in your app manifest via the **configurableTabs** -> **scopes** and **context** arrays.</span></span> <span data-ttu-id="fbda0-184">L' *étendue* définit la personne et le *contexte* définissant où votre application sera disponible.</span><span class="sxs-lookup"><span data-stu-id="fbda0-184">*Scope* defines to whom and *context* defines where your app will be available.</span></span>

```json
"configurableTabs": [
    {
      "configurationUrl": "https://contoso.com/teamstab/configure",
      "canUpdateConfiguration": true,
      "scopes": [
        "team",
        "groupchat"
      ],
      "context":[
        "channelTab",
        "privateChatTab",
        "meetingChatTab",
        "meetingDetailsTab",
        "meetingSidePanel"
     ]
    }
  ]
```

### <a name="context-property"></a><span data-ttu-id="fbda0-185">Context, propriété</span><span class="sxs-lookup"><span data-stu-id="fbda0-185">Context property</span></span>

<span data-ttu-id="fbda0-186">L’onglet `context` et les `scopes` propriétés fonctionnent en harmonie pour vous permettre de déterminer où vous souhaitez que votre application apparaisse.</span><span class="sxs-lookup"><span data-stu-id="fbda0-186">The tab `context` and `scopes` properties work in harmony to allow you to determine where you want your app to appear.</span></span> <span data-ttu-id="fbda0-187">Alors que les onglets de l' `personal` étendue ne peuvent avoir qu’un seul contexte, c’est-à-dire, `personalTab`  `team` ou les `groupchat` onglets de portée peuvent contenir plusieurs contextes.</span><span class="sxs-lookup"><span data-stu-id="fbda0-187">While tabs in the `personal` scope can only have one context, i.e., `personalTab`,  `team` or `groupchat` scoped tabs can have more than one context.</span></span> <span data-ttu-id="fbda0-188">Les valeurs possibles pour la propriété Context sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="fbda0-188">The possible values for the context property are as follows:</span></span>

* <span data-ttu-id="fbda0-189">**channelTab**: onglet de l’en-tête d’un canal d’équipe.</span><span class="sxs-lookup"><span data-stu-id="fbda0-189">**channelTab**: a tab in the header of a team channel.</span></span>
* <span data-ttu-id="fbda0-190">**privateChatTab**: onglet de l’en-tête d’une conversation de groupe entre un ensemble d’utilisateurs qui n’est pas dans le contexte d’une équipe ou d’une réunion.</span><span class="sxs-lookup"><span data-stu-id="fbda0-190">**privateChatTab**: a tab in the header of a group chat between a set of users not in the context of a team or meeting.</span></span>
* <span data-ttu-id="fbda0-191">**meetingChatTab**: onglet de l’en-tête d’une conversation de groupe entre un ensemble d’utilisateurs dans le contexte d’une réunion planifiée.</span><span class="sxs-lookup"><span data-stu-id="fbda0-191">**meetingChatTab**: a tab in the header of a group chat between a set of users in the context of a scheduled meeting.</span></span>
* <span data-ttu-id="fbda0-192">**meetingDetailsTab**: onglet de l’en-tête de l’affichage Détails de la réunion du calendrier.</span><span class="sxs-lookup"><span data-stu-id="fbda0-192">**meetingDetailsTab**: a tab in the header of the meeting details view of the calendar.</span></span>
* <span data-ttu-id="fbda0-193">**meetingSidePanel**: un panneau de réunion ouvert par le biais de la barre unifiée (u-bar).</span><span class="sxs-lookup"><span data-stu-id="fbda0-193">**meetingSidePanel**: an in-meeting panel opened via the unified bar (u-bar).</span></span>

## <a name="configure-your-app-for-meeting-scenarios"></a><span data-ttu-id="fbda0-194">Configurer votre application pour les scénarios de réunion</span><span class="sxs-lookup"><span data-stu-id="fbda0-194">Configure your app for meeting scenarios</span></span>

> [!NOTE]
> <span data-ttu-id="fbda0-195">Pour que votre application soit visible dans la Galerie d’onglets, elle doit **prendre en charge les onglets configurables** et l' **étendue de conversation de groupe**.</span><span class="sxs-lookup"><span data-stu-id="fbda0-195">For your app to be visible in the tab gallery it needs to **support configurable tabs** and the **group chat scope**.</span></span>

### <a name="pre-meeting"></a><span data-ttu-id="fbda0-196">Pré-réunion</span><span class="sxs-lookup"><span data-stu-id="fbda0-196">Pre-meeting</span></span>

<span data-ttu-id="fbda0-197">Les utilisateurs disposant de rôles d’organisateur et/ou de présentateur ajoutent des onglets à une réunion en utilisant le bouton plus ➕ dans les pages **conversation** de réunion et **Détails** de la réunion.</span><span class="sxs-lookup"><span data-stu-id="fbda0-197">Users with organizer and/or presenter roles add tabs to a meeting using the plus ➕ button in the meeting **Chat** and meeting **details** pages.</span></span> <span data-ttu-id="fbda0-198">Les extensions de messagerie sont ajoutées à via le menu ellipses/Overflow &#x25CF;&#x25CF;&#x25CF; située sous la zone de message composer dans la conversation.</span><span class="sxs-lookup"><span data-stu-id="fbda0-198">Messaging extensions are added to via the ellipses/overflow menu &#x25CF;&#x25CF;&#x25CF; located beneath the compose message area in the chat.</span></span> <span data-ttu-id="fbda0-199">Les robots sont ajoutés à une conversation de réunion à l’aide de la **@** touche «» et en sélectionnant **obtenir les bots**.</span><span class="sxs-lookup"><span data-stu-id="fbda0-199">Bots are added to a meeting chat using the "**@**" key and selecting **Get bots**.</span></span>

<span data-ttu-id="fbda0-200">✔ L’identité de l’utilisateur *doit* être confirmée via les [onglets SSO](../tabs/how-to/authentication/auth-aad-sso.md).</span><span class="sxs-lookup"><span data-stu-id="fbda0-200">✔ The user identity *must* be confirmed via [Tabs SSO](../tabs/how-to/authentication/auth-aad-sso.md).</span></span> <span data-ttu-id="fbda0-201">À la suite de cette authentification, l’application peut récupérer le rôle d’utilisateur via l’API GetParticipant.</span><span class="sxs-lookup"><span data-stu-id="fbda0-201">Following this authentication, the app can retrieve the user role via the GetParticipant API.</span></span>

 <span data-ttu-id="fbda0-202">✔ En fonction du rôle d’utilisateur, l’application est désormais capable de présenter des expériences spécifiques de rôle.</span><span class="sxs-lookup"><span data-stu-id="fbda0-202">✔ Based on the user role, the app will now have the capability to present role specific experiences.</span></span> <span data-ttu-id="fbda0-203">Par exemple, une application d’interrogation peut autoriser uniquement les organisateurs et les présentateurs à créer une nouvelle interrogation.</span><span class="sxs-lookup"><span data-stu-id="fbda0-203">For example, a polling app can allow only organizers and presenters to create a new poll.</span></span>

> <span data-ttu-id="fbda0-204">**Remarque**: les attributions de rôles peuvent être modifiées lorsqu’une réunion est en cours.</span><span class="sxs-lookup"><span data-stu-id="fbda0-204">**NOTE**: Role assignments can be changed while a meeting is in progress.</span></span>  <span data-ttu-id="fbda0-205">*Voir* [rôles dans une réunion teams](https://support.microsoft.com/office/roles-in-a-teams-meeting-c16fa7d0-1666-4dde-8686-0a0bfe16e019).</span><span class="sxs-lookup"><span data-stu-id="fbda0-205">*See* [Roles in a Teams meeting](https://support.microsoft.com/office/roles-in-a-teams-meeting-c16fa7d0-1666-4dde-8686-0a0bfe16e019).</span></span> 

### <a name="in-meeting"></a><span data-ttu-id="fbda0-206">Réunion</span><span class="sxs-lookup"><span data-stu-id="fbda0-206">In-meeting</span></span>

#### <a name="side-panel"></a><span data-ttu-id="fbda0-207">**panneau latéral**</span><span class="sxs-lookup"><span data-stu-id="fbda0-207">**side-panel**</span></span>

<span data-ttu-id="fbda0-208">✔ Dans votre manifeste d’application, ajoutez **sidePanel** au tableau **meetingSurfaces** , comme décrit ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="fbda0-208">✔ In your app manifest add **sidePanel** to the **meetingSurfaces** array as described above.</span></span>

<span data-ttu-id="fbda0-209">✔ Dans la réunion, ainsi que dans tous les scénarios, l’application sera affichée sous la forme d’un onglet dans la réunion 320 px.</span><span class="sxs-lookup"><span data-stu-id="fbda0-209">✔ In the meeting as well as in all scenarios, the app will be rendered in an in-meeting tab that is 320px in width.</span></span> <span data-ttu-id="fbda0-210">Pour ce faire, vous devez optimiser l’onglet.</span><span class="sxs-lookup"><span data-stu-id="fbda0-210">Your tab must be optimized for this.</span></span> <span data-ttu-id="fbda0-211">*Voir*, [interface FrameContext](/javascript/api/@microsoft/teams-js/microsoftteams.framecontext?view=msteams-client-js-latest&preserve-view=true)</span><span class="sxs-lookup"><span data-stu-id="fbda0-211">*See*, [FrameContext interface](/javascript/api/@microsoft/teams-js/microsoftteams.framecontext?view=msteams-client-js-latest&preserve-view=true)</span></span>

<span data-ttu-id="fbda0-212">✔ Reportez-vous au [Kit de développement logiciel teams](../tabs/how-to/access-teams-context.md#user-context) pour utiliser l’API **userContext** pour acheminer les demandes en conséquence.</span><span class="sxs-lookup"><span data-stu-id="fbda0-212">✔Refer to the [Teams SDK](../tabs/how-to/access-teams-context.md#user-context) to use the **userContext** API to route requests accordingly.</span></span>

<span data-ttu-id="fbda0-213">✔ Reportez-vous au [flux d’authentification teams pour les onglets](../tabs/how-to/authentication/auth-flow-tab.md).</span><span class="sxs-lookup"><span data-stu-id="fbda0-213">✔ Refer to the [Teams authentication flow for tabs](../tabs/how-to/authentication/auth-flow-tab.md).</span></span> <span data-ttu-id="fbda0-214">Le flux d’authentification des onglets est très similaire au flux d’authentification pour les sites Web.</span><span class="sxs-lookup"><span data-stu-id="fbda0-214">Authentication flow for tabs is very similar to the auth flow for websites.</span></span> <span data-ttu-id="fbda0-215">Par conséquent, les onglets peuvent utiliser OAuth 2,0 directement.</span><span class="sxs-lookup"><span data-stu-id="fbda0-215">Thus, tabs can use OAuth 2.0 directly.</span></span> <span data-ttu-id="fbda0-216">*Voir aussi*la [plateforme d’identité Microsoft et le flux de code d’autorisation OAuth 2,0](/azure/active-directory/develop/v2-oauth2-auth-code-flow).</span><span class="sxs-lookup"><span data-stu-id="fbda0-216">*See also*, [Microsoft identity platform and OAuth 2.0 authorization code flow](/azure/active-directory/develop/v2-oauth2-auth-code-flow).</span></span>

#### <a name="in-meeting-dialog"></a><span data-ttu-id="fbda0-217">**boîte de dialogue de réunion**</span><span class="sxs-lookup"><span data-stu-id="fbda0-217">**in-meeting dialog**</span></span>

<span data-ttu-id="fbda0-218">✔ Vous devez respecter les [instructions de conception de la boîte de dialogue de réunion](designing-in-meeting-dialog.md).</span><span class="sxs-lookup"><span data-stu-id="fbda0-218">✔ You must adhere to the [in-meeting dialog design guidelines](designing-in-meeting-dialog.md).</span></span>

<span data-ttu-id="fbda0-219">✔ Reportez-vous au [flux d’authentification teams pour les onglets](../tabs/how-to/authentication/auth-flow-tab.md).</span><span class="sxs-lookup"><span data-stu-id="fbda0-219">✔ Refer to the [Teams authentication flow for tabs](../tabs/how-to/authentication/auth-flow-tab.md).</span></span>

<span data-ttu-id="fbda0-220">✔ Utiliser l’API de [notification](/graph/api/resources/notifications-api-overview?view=graph-rest-beta&preserve-view=true) pour signaler qu’une notification de bulle doit être déclenchée.</span><span class="sxs-lookup"><span data-stu-id="fbda0-220">✔ Use the [notification](/graph/api/resources/notifications-api-overview?view=graph-rest-beta&preserve-view=true) API to signal that a bubble notification needs to be triggered.</span></span>

<span data-ttu-id="fbda0-221">✔ Dans le cadre de la charge utile de la demande de notification, incluez l’URL où le contenu à présenter est hébergé.</span><span class="sxs-lookup"><span data-stu-id="fbda0-221">✔ As part of the notification request payload, include the URL where the content to be showcased is hosted.</span></span>

> [!NOTE]
> <span data-ttu-id="fbda0-222">Ces notifications sont permanentes.</span><span class="sxs-lookup"><span data-stu-id="fbda0-222">These notifications are persistent in nature.</span></span> <span data-ttu-id="fbda0-223">Vous devez appeler la fonction [**submitTask ()**](../task-modules-and-cards/task-modules/task-modules-bots.md#submitting-the-result-of-a-task-module) pour faire une fermeture automatique après qu’un utilisateur a effectué une action dans l’affichage Web.</span><span class="sxs-lookup"><span data-stu-id="fbda0-223">You must invoke the [**submitTask()**](../task-modules-and-cards/task-modules/task-modules-bots.md#submitting-the-result-of-a-task-module) function to auto-dismiss after a user takes an action in the web-view.</span></span> <span data-ttu-id="fbda0-224">Il s’agit d’une condition requise pour l’envoi d’une application.</span><span class="sxs-lookup"><span data-stu-id="fbda0-224">This is a requirement for app submission.</span></span> <span data-ttu-id="fbda0-225">*Voir aussi*, [Kit de développement logiciel (SDK) teams : module de tâches](/javascript/api/@microsoft/teams-js/microsoftteams.tasks?view=msteams-client-js-latest#submittask-string---object--string---string---&preserve-view=true).</span><span class="sxs-lookup"><span data-stu-id="fbda0-225">*See also*, [Teams SDK: task module](/javascript/api/@microsoft/teams-js/microsoftteams.tasks?view=msteams-client-js-latest#submittask-string---object--string---string---&preserve-view=true).</span></span>

### <a name="post-meeting"></a><span data-ttu-id="fbda0-226">Post-réunion</span><span class="sxs-lookup"><span data-stu-id="fbda0-226">Post-meeting</span></span>

<span data-ttu-id="fbda0-227">Les configurations post-réunion et de réunion sont équivalentes.</span><span class="sxs-lookup"><span data-stu-id="fbda0-227">The post-meeting and pre-meeting configurations are equivalent.</span></span>

## <a name="meeting-app-sample"></a><span data-ttu-id="fbda0-228">Exemple d’application de réunion</span><span class="sxs-lookup"><span data-stu-id="fbda0-228">Meeting app sample</span></span>

 > [!div class="nextstepaction"]
> [<span data-ttu-id="fbda0-229">Application du générateur de jetons de réunion</span><span class="sxs-lookup"><span data-stu-id="fbda0-229">Meeting token generator app</span></span>](https://github.com/OfficeDev/microsoft-teams-sample-meetings-token)