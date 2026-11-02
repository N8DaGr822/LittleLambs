# Miss Mary's Little Lambs

A standalone Blazor WebAssembly website for Miss Mary's Little Lambs, built with C# and .NET 10. The site introduces Miss Mary, her teaching philosophy, childcare and preschool offerings, curriculum, and enrollment interest forms.

**Website:** https://n8dagr822.github.io/LittleLambs/

## Programs

- **Childcare:** 7:30 am–5:30 pm.
- **Preschool:** 8:00 am–11:00 am for ages 3–5.

Childcare and preschool have separate navigation links and pages. The previous `/programs` route remains available as an alias for the preschool page.

## Run locally

Install the .NET 10 SDK, then run these commands from the repository root:

```sh
dotnet restore LittleLamb/LittleLamb.csproj
dotnet run --project LittleLamb/LittleLamb.csproj --launch-profile http
```

Open http://localhost:5036 in your browser.

For development with automatic updates:

```sh
dotnet watch --project LittleLamb/LittleLamb.csproj run --launch-profile http
```

The app runs in the browser. There is no application backend or database to configure; enrollment and childcare inquiries link to external Google Forms.

## Build and publish

```sh
dotnet build LittleLamb/LittleLamb.csproj
dotnet publish LittleLamb/LittleLamb.csproj --configuration Release
```

The published static site is generated in `LittleLamb/bin/Release/net10.0/publish/wwwroot/`. Build output is excluded from Git.

## Project structure

| Path | Purpose |
| --- | --- |
| `LittleLamb/Pages/` | Page content and routes |
| `LittleLamb/Pages/Programs.razor` | Preschool page and legacy `/programs` route |
| `LittleLamb/Layout/` | Shared layout and main navigation |
| `LittleLamb/wwwroot/css/app.css` | Shared site styling |
| `LittleLamb/wwwroot/images/` | Site images |
| `LittleLamb/wwwroot/index.html` | App entry point |
| `.github/workflows/deploy-pages.yml` | GitHub Pages build and deployment |

Main navigation lives in `Layout/NavMenu.razor`. The homepage compares preschool and childcare and links to their separate interest forms. Keep internal links relative so they work locally and under the GitHub Pages repository path.

## GitHub Pages deployment

Pushes to `main` trigger the [Deploy to GitHub Pages workflow](https://github.com/N8DaGr822/LittleLambs/actions/workflows/deploy-pages.yml). It can also be started manually from the repository's Actions tab. In repository settings, **Pages → Build and deployment → Source** must be set to **GitHub Actions**.

The workflow:

1. Publishes the app in Release mode with the .NET 10 SDK.
2. Changes the published base path to `/LittleLambs/`, leaving local development at `/`.
3. Copies the published `index.html` to `404.html` so direct visits and refreshes of Blazor routes can load the app.
4. Adds `.nojekyll`, uploads the static files, and deploys them to GitHub Pages.

The route fallback still returns an HTTP 404 status for direct visits to client routes, even though Blazor can render the requested page. Client-side navigation from the home page does not require this fallback.

If the repository name or hosting path changes, update the deployment base path in the workflow.
