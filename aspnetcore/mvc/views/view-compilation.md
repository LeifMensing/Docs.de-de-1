---
title: Razor-Ansicht-Kompilierung und Vorkompilierung
author: rick-anderson
description: "Ein Verweis Dokument erläutert, wie die MVC Razor-Ansicht-Kompilierung und Vorkompilierung in ASP.NET Core-Anwendungen zu ermöglichen."
ms.author: riande
manager: wpickett
ms.date: 12/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 87989455c2fb6b5a922c7fb6133aa3e8cef42c88
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Razor-Ansicht-Kompilierung und Vorkompilierung in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Razor-Ansichten werden zur Laufzeit kompiliert, wenn die Ansicht aufgerufen wird. ASP.NET Core 1.1.0 und höheren Versionen kann optional kompilieren Razor-Ansichten und diese mit der app bereitstellen&mdash;genannten Vorkompilierung. Die Projektvorlagen für ASP.NET Core 2.x aktiviert Vorkompilierung standardmäßig.

> [!IMPORTANT]
> Vorkompilierung der Razor-Ansicht ist zurzeit nicht verfügbar, beim Ausführen einer [eigenständige Bereitstellung (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0. Die Funktion wird für SCDs verfügbar sein, wenn 2.1 freigibt. Weitere Informationen finden Sie unter [Ansicht Kompilierung schlägt fehl, beim übergreifenden für Linux auf Windows Kompilieren](https://github.com/aspnet/MvcPrecompilation/issues/102).

Vorkompilierung Aspekte:

* Vorkompilieren von Ansichten führt zu einer kleineren veröffentlichte Paket und schneller gestartet.
* Razor-Dateien kann nicht bearbeitet werden, nachdem Sie Ansichten vorkompilieren. Die bearbeiteten Sichten wird nicht in das veröffentlichte Paket vorhanden sein. 

Vorkompilierte Sichten bereitstellen:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Wenn Ihr Projekt auf .NET Framework abzielt, enthalten einen Verweis Paket auf [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Wenn das Projekt .NET Core als Ziel verwendet werden, sind keine Änderungen erforderlich.

Legen Sie die Projektvorlagen von ASP.NET Core 2.x implizit `MvcRazorCompileOnPublish` auf `true` standardmäßig, d. h. dieser Knoten kann sicher entfernt werden, aus der *csproj* Datei. Wenn Sie es vorziehen, die explizit sein, besteht keine Schäden in der Setting der `MvcRazorCompileOnPublish` Eigenschaft `true`. Die folgenden *csproj* Beispiel beschreibt diese Einstellung:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Legen Sie `MvcRazorCompileOnPublish` auf `true`, und schließen Sie einen Paket Verweis auf `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Die folgenden *csproj* Beispiel beschreibt diese Einstellungen:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

Vorbereiten der Anwendung für eine [Framework abhängiges Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd) durch Ausführen eines Befehls wie im folgenden Stammverzeichnis Projekt:

```console
dotnet publish -c Release
```

Ein *< Project_name >. PrecompiledViews.dll* Datei, mit der kompilierten Razor-Ansichten wird ausgelöst, wenn Vorkompilierung erfolgreich ausgeführt wird. Der folgende Screenshot zeigt z. B. den Inhalt des *Index.cshtml* innerhalb eines *WebApplication1.PrecompiledViews.dll*:

![Razor-Ansichten in DLL-Datei](view-compilation/_static/razor-views-in-dll.png)
