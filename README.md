This is NOT theme that is ready to be used, it's a starter for anyone wanting to style their own site. 

## Getting Started


__1) Clone or download this Repository__

Once you have the folder locally on your machine open up the project in VS Code.

__2) Open a terminal window__

In VS Code you can open an integrated [terminal window](https://code.visualstudio.com/docs/terminal/basics) using the ⌃` keyboard shortcut.

__3) Install dependencies__

```
npm install
```

__4) Run Eleventy__

Our site will be generated in a docs folder (see step 5 for the reason about the name). Try deleting the docs folder you see now, then generate it again by running one of the commands below.

Generate a production-ready build to the `docs` folder:

```
npx @11ty/eleventy
```

Or build and host on a local development server:

```
npx @11ty/eleventy --serve
```

Or you can run [debug mode](https://www.11ty.dev/docs/debugging/) to see all the internals.

__5) Get ready to host on Github pages__

We are making use of the Github pages ability to serve a site from the docs folder on main as this is the most painless way to quickly get a site live. You can also create a gh-pages branch and/or [use github actions](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site).

Before you push up any changes you have made to github pages, in package.json make sure that you have added the name of your repo to the line below, e.g. my repos is called "eleventy-portfolio"

```
"build-ghpages": "npx @11ty/eleventy --pathprefix=/eleventy-portfolio/",
```

So you would simply replace "eleventy-portfolio" with your github repo name.

Once you have done this and are ready to push your site to github pages in terminal run:

```
npm run build-ghpages
```

__6) Push up your repo to Github & convert it into a github page__

- Use the github app to create a new repo. 
- Convert the repo into a Github page. Expect instead of hosting main, select the docs folder.
- If you need a reminder on how to convert your repo you can go start from step 3-8 from [here](https://docs.github.com/en/pages/quickstart). 

How else can you host this static site? You can host the contents of your docs folder (the generated static site) on any server!!! Using FTP you can upload the contents to a hosting service you buy, or you can use Netlify, or a number of other service providers. 


## What is generated

- Generated Pages (found in the docs folder after you have run the project)
	- Home, Portfolio, and About pages.
	- `sitemap.xml`
	- Zero-maintenance tag pages
	- Content not found (404) page
	- Automated next/previous links to projects
- Project Drafts
	- Draft projects: use `draft: true` to mark a project as a draft. Drafts are **only** included during `--serve`/`--watch` and are excluded from full builds. Configured in eleventy.config.drafts.js

Review `eleventy.config.js` and `_data/metadata.js` to configure the site’s options and data.


## Links to features & resources


This site was modified from [eleventy-base-blog](https://github.com/11ty/eleventy-base-blog) where you can find full documentation on features, with the addition of Bootstrap. Below are some notes on implementation and links to features / resources. 

- Bootstrap has been added via [downloading the minified css and js](https://getbootstrap.com/docs/5.0/getting-started/download/) which has been added in `public/bootstrap-5.0.2-dist`
- Using [Eleventy v2.0](https://www.11ty.dev/blog/eleventy-v2/) with zero-JavaScript output.
	- Content is exclusively pre-rendered (this is a static site).
	- Can easily [deploy to a subfolder without changing any content](https://www.11ty.dev/docs/plugins/html-base/)
	- All URLs are decoupled from the content’s location on the file system.
	- Configure templates via the [Eleventy Data Cascade](https://www.11ty.dev/docs/data-cascade/)
- Local development live reload provided by [Eleventy Dev Server](https://www.11ty.dev/docs/dev-server/).
- Content-driven [navigation menu](https://www.11ty.dev/docs/plugins/navigation/)
- [Image optimization](https://www.11ty.dev/docs/plugins/image/) via the `{% image %}` shortcode.
	- Zero-JavaScript output.
	- Support for modern image formats automatically (e.g. AVIF and WebP)
	- Prefers `<img>` markup if possible (single image format) but switches automatically to `<picture>` for multiple image formats.
	- Automated `<picture>` syntax markup with `srcset` and optional `sizes`
	- Includes `width`/`height` attributes to avoid [content layout shift](https://web.dev/cls/).
	- Includes `loading="lazy"` for native lazy loading without JavaScript.
	- Includes [`decoding="async"`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/decoding)
	- Images can be co-located with project files.
	- View the [Image plugin source code](https://github.com/11ty/eleventy-base-blog/blob/main/eleventy.config.images.js)
- Per page CSS bundles [via `eleventy-plugin-bundle`](https://github.com/11ty/eleventy-plugin-bundle).

If your site enforces a Content Security Policy (as public-facing sites should), either, in `base.njk`, disable
```html
<style>{% getBundle "css" %}</style>
```
and enable
```html
<link rel="stylesheet" href="{% getBundleFileUrl "css" %}">
```
or configure the server with the CSP directive `style-src: 'unsafe-inline'` (which is less secure).


## Implementation Notes 

- `content/about/index.md` is an example of a content page.
- `content/projects/` has the projects but really they can live in any directory. They need only the `projects` tag to be included in the projects [collection](https://www.11ty.dev/docs/collections/).
- Use the `eleventyNavigation` key (via the [Eleventy Navigation plugin](https://www.11ty.dev/docs/plugins/navigation/)) in your front matter to add a template to the top level site navigation. This is in use on `content/index.njk` and `content/about/index.md`.
- Content can be in _any template format_ (projects needn’t exclusively be markdown, for example). Configure your project’s supported templates in `eleventy.config.js` -> `templateFormats`.
- The `public` folder in your input directory will be copied to the output folder (via `addPassthroughCopy` in the `eleventy.config.js` file). This means `./public/css/*` will live at `./docs/css/*` after your build completes.
- This project uses three [Eleventy Layouts](https://www.11ty.dev/docs/layouts/):
	- `_includes/layouts/base.njk`: the top level HTML structure
	- `_includes/layouts/home.njk`: the home page template (wrapped into `base.njk`)
	- `_includes/layouts/project.njk`: the project template (wrapped into `base.njk`)
- `_includes/projectlist.njk` is a Nunjucks include and is a reusable component used to display a list of all the projects. `content/index.njk` has an example of how to use it.
- `_includes/nav.njk`is an include that gets included into `base.njk`. It's an example of a Bootsrap [navBar](https://getbootstrap.com/docs/5.0/components/navbar/) that uses [ Bootstrap navs](https://getbootstrap.com/docs/5.0/components/navs/)
- `_includes/slideshow.njk` an example of a Bootsrap [carousel](https://getbootstrap.com/docs/5.0/components/carousel/) 


## Other Eleventy Themes 

- [Elevnty Theme that uses Bootstrap](https://github.com/mandrasch/11ty-plain-bootstrap5/tree/main)
- [Eleventy Starter Projects](https://www.11ty.dev/docs/starter/)
- [Elevent Themes on Jamstack](https://jamstackthemes.dev/ssg/eleventy/)