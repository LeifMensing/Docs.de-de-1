---
title: Externe Anmeldung Setup Twitter
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 11/1/2016
ms.topic: article
ms.assetid: E5931607-31C0-4B20-B416-85E3550F5EA8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/twitter-logins
ms.openlocfilehash: 800f98285859a54198b76411aea000384de05cd3
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2017
---
# <a name="configuring-twitter-authentication"></a><span data-ttu-id="afc7f-103">Konfigurieren von Twitter-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="afc7f-103">Configuring Twitter authentication</span></span>

<a name=security-authentication-twitter-logins></a>

<span data-ttu-id="afc7f-104">Durch [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="afc7f-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="afc7f-105">Diesem Lernprogramm erfahren Sie, wie Sie Ihren Benutzern ermöglichen [melden Sie sich mit ihren Twitter-Konto](https://dev.twitter.com/web/sign-in/desktop-browser) ein Beispielprojekt für ASP.NET Core 2.0 erstellt wurde, auf die [vorherige Seite](index.md).</span><span class="sxs-lookup"><span data-stu-id="afc7f-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="afc7f-106">Erstellen Sie die app in Twitter</span><span class="sxs-lookup"><span data-stu-id="afc7f-106">Create the app in Twitter</span></span>

* <span data-ttu-id="afc7f-107">Navigieren Sie zu [https://apps.twitter.com/](https://apps.twitter.com/) und melden Sie sich.</span><span class="sxs-lookup"><span data-stu-id="afc7f-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="afc7f-108">Wenn Sie einen Twitter-Konto noch nicht haben, verwenden die  **[jetzt registrieren](https://twitter.com/signup)**  Link, um eines zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="afc7f-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="afc7f-109">Nach der Anmeldung, die **Anwendungsverwaltung** Seite wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="afc7f-109">After signing in, the **Application Management** page is shown:</span></span>

![Twitter Application Management in Microsoft Edge öffnen](index/_static/TwitterAppManage.png)

* <span data-ttu-id="afc7f-111">Tippen Sie auf **neue App** , und füllen Sie die Anwendung **Namen**, **Beschreibung** und öffentlichen **Website** URI (Dies kann temporär sein bis Registrieren Sie den Domänennamen):</span><span class="sxs-lookup"><span data-stu-id="afc7f-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

![Erstellen einer Anwendungsseite](index/_static/TwitterCreate.png)

* <span data-ttu-id="afc7f-113">Geben Sie die Entwicklung URI mit */signin-twitter* angefügt, die in der **gültige OAuth-Umleitungs-URIs** Feld (z. B.: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="afc7f-113">Enter your development URI with */signin-twitter* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="afc7f-114">Verarbeiten der Twitter-Authentifizierungsschema, die weiter unten in diesem Lernprogramm konfiguriert automatisch Anforderungen, die bei */signin-twitter* Route zum Implementieren des OAuth-Fluss.</span><span class="sxs-lookup"><span data-stu-id="afc7f-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at */signin-twitter* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="afc7f-115">Füllen Sie den Rest des Formulars, und tippen Sie auf **die Twitter-Anwendung erstellen**.</span><span class="sxs-lookup"><span data-stu-id="afc7f-115">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="afc7f-116">Anwendungsdetails des neuen werden angezeigt:</span><span class="sxs-lookup"><span data-stu-id="afc7f-116">New application details are displayed:</span></span>

![Registerkarte "Details" auf der Seite "Anwendung"](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="afc7f-118">Bei der Bereitstellung der Website müssen Sie rufen die **Anwendungsverwaltung** Seite, und registrieren Sie einen neuen öffentlichen URI.</span><span class="sxs-lookup"><span data-stu-id="afc7f-118">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="afc7f-119">Speichern von Twitter ConsumerKey und ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="afc7f-119">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="afc7f-120">Verknüpfen Sie die sensiblen Einstellungen wie Twitter `Consumer Key` und `Consumer Secret` zu Ihrer Anwendungskonfigurationsdatei mithilfe der [geheimen Manager](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="afc7f-120">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="afc7f-121">Für die Zwecke dieses Lernprogramms, benennen Sie die Token `Authentication:Twitter:ConsumerKey` und `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="afc7f-121">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="afc7f-122">Diese Token befinden sich auf die **Schlüssel und Zugriffstoken** Registerkarte nach dem Erstellen einer neuen Twitter-Anwendung:</span><span class="sxs-lookup"><span data-stu-id="afc7f-122">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Registerkarte "Schlüssel und Zugriffstoken"](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="afc7f-124">Twitter-Authentifizierung konfigurieren</span><span class="sxs-lookup"><span data-stu-id="afc7f-124">Configure Twitter Authentication</span></span>

<span data-ttu-id="afc7f-125">Die Projektvorlage verwendet, die in diesem Lernprogramm wird sichergestellt, dass [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) Paket ist bereits installiert.</span><span class="sxs-lookup"><span data-stu-id="afc7f-125">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="afc7f-126">Zum Installieren dieses Pakets mit Visual Studio 2017 Maustaste auf das Projekt, und wählen **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="afc7f-126">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="afc7f-127">Führen Sie die folgenden im Projektverzeichnis, um die mit .NET Core CLI installieren:</span><span class="sxs-lookup"><span data-stu-id="afc7f-127">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="afc7f-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="afc7f-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="afc7f-129">Hinzufügen den Twitter-Dienst in der `ConfigureServices` Methode im *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="afc7f-129">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

<span data-ttu-id="afc7f-130">Die `AddAuthentication` Methode sollte nur einmal beim Hinzufügen mehrerer Authentifizierungsanbieter aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="afc7f-130">The `AddAuthentication` method should only be called once when adding multiple authentication providers.</span></span> <span data-ttu-id="afc7f-131">Nachfolgende Aufrufe haben das volle Potenzial von überschreiben alle zuvor konfigurierten [authenticationoptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="afc7f-131">Subsequent calls to it have the potential of overriding any previously configured [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) properties.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="afc7f-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="afc7f-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="afc7f-133">Hinzufügen die Twitter-Middleware in der `Configure` Methode im *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="afc7f-133">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

<span data-ttu-id="afc7f-134">Finden Sie unter der [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) API-Referenz für Weitere Informationen zu den Konfigurationsoptionen von Twitter-Authentifizierung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="afc7f-134">See the [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="afc7f-135">Dies kann verwendet werden, um unterschiedliche Informationen über den Benutzer anzufordern.</span><span class="sxs-lookup"><span data-stu-id="afc7f-135">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="afc7f-136">Melden Sie sich mit Twitter</span><span class="sxs-lookup"><span data-stu-id="afc7f-136">Sign in with Twitter</span></span>

<span data-ttu-id="afc7f-137">Führen Sie die Anwendung, und klicken Sie auf **melden Sie sich**.</span><span class="sxs-lookup"><span data-stu-id="afc7f-137">Run your application and click **Log in**.</span></span> <span data-ttu-id="afc7f-138">Eine Option zur Anmeldung mit Twitter angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="afc7f-138">An option to sign in with Twitter appears:</span></span>

![-Webanwendung: nicht authentifizierte Benutzer](index/_static/DoneTwitter.png)

<span data-ttu-id="afc7f-140">Durch Klicken auf **Twitter** leitet an Twitter für die Authentifizierung:</span><span class="sxs-lookup"><span data-stu-id="afc7f-140">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Twitter-Seite "Authentifizierung"](index/_static/TwitterLogin.png)

<span data-ttu-id="afc7f-142">Nach der Eingabe der Anmeldeinformationen für Twitter, werden Sie zurück zur Website umgeleitet, wo Sie Ihre e-Mail-Adresse festlegen können.</span><span class="sxs-lookup"><span data-stu-id="afc7f-142">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="afc7f-143">Sie sind jetzt angemeldet mit Ihren Twitter-Anmeldeinformationen:</span><span class="sxs-lookup"><span data-stu-id="afc7f-143">You are now logged in using your Twitter credentials:</span></span>

![-Webanwendung: Authentifizierte Benutzer](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="afc7f-145">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="afc7f-145">Troubleshooting</span></span>

* <span data-ttu-id="afc7f-146">**ASP.NET Core 2.x nur:** Wenn Identität ist nicht konfiguriert, durch den Aufruf `services.AddIdentity` in `ConfigureServices`, Authentifizierungsversuch führt zu *ArgumentException: die Option "SignInScheme" muss angegeben werden*.</span><span class="sxs-lookup"><span data-stu-id="afc7f-146">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="afc7f-147">Die Projektvorlage, die in diesem Lernprogramm verwendete wird sichergestellt, dass dies geschehen ist.</span><span class="sxs-lookup"><span data-stu-id="afc7f-147">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="afc7f-148">Wenn die Standortdatenbank nicht erstellt wurde, indem der anfänglichen Migration anwenden, erhalten Sie *Fehler bei ein Datenbankvorgang beim Verarbeiten der Anforderung* Fehler.</span><span class="sxs-lookup"><span data-stu-id="afc7f-148">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="afc7f-149">Tippen Sie auf **gelten Migrationen** der Datenbank zu erstellen und aktualisieren, um den Fehler hinaus zu fortfahren.</span><span class="sxs-lookup"><span data-stu-id="afc7f-149">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="afc7f-150">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="afc7f-150">Next steps</span></span>

* <span data-ttu-id="afc7f-151">In diesem Artikel wurde gezeigt, wie Sie mit Twitter authentifizieren können.</span><span class="sxs-lookup"><span data-stu-id="afc7f-151">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="afc7f-152">Führen Sie einen ähnlichen Ansatz für die Authentifizierung bei anderen Anbietern aufgeführt, auf die [vorherige Seite](index.md).</span><span class="sxs-lookup"><span data-stu-id="afc7f-152">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="afc7f-153">Nachdem Sie Ihre Website in Azure-Web-app veröffentlichen, sollten Sie Zurücksetzen der `ConsumerSecret` im Entwicklerportal Twitter.</span><span class="sxs-lookup"><span data-stu-id="afc7f-153">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="afc7f-154">Legen Sie die `Authentication:Twitter:ConsumerKey` und `Authentication:Twitter:ConsumerSecret` als Anwendungseinstellungen im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="afc7f-154">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="afc7f-155">Das Konfigurationssystem wird beim Lesen von Schlüsseln von Umgebungsvariablen eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="afc7f-155">The configuration system is set up to read keys from environment variables.</span></span>