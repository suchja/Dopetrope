# Dopetrope

This is the [HTML5 UP](https://html5up.net/) - Dopetrope theme adapted from https://html5up.net/dopetrope, for usage with the [STATIQ - site generator](https://www.statiq.dev). Many thanks to [AJ](https://twitter.com/ajlkn) from [HTML5 UP](https://html5up.net/) for providing the Dopetrope theme under the [Creative Commons Attribution 3.0 License](https://html5up.net/license/).

## Minimum Statiq Web Version

This theme requires Statiq Web 1.0.0-beta.36 or later.

## Installation

Statiq themes go in a `theme` folder alongside your `input` folder. If your site is inside a git repository, you can add the theme as a git submodule:

```shell
git submodule add https://github.com/suchja/statiq-dopetrope-theme.git theme
```

Alternatively, you can clone the theme directly:

```shell
git clone https://github.com/suchja/statiq-dopetrope-theme.git theme
```

Once inside the `theme` folder, Statiq will automatically recognize the theme.

If you're not going to modify theme files directly, you can leave the theme out of your repository and use the [Devlead.Statiq](https://www.nuget.org/packages/Devlead.Statiq) package to automatically download it as you build your site. See [this blog post](https://www.devlead.se/posts/2021/2021-04-10-devlead-statiq-part2-theme-from-uri) for details.

## Site Contents

You can add contents to your site using plain HTML and CSS, as well as any of the [template languages](https://www.statiq.dev/guide/content-and-data/template-languages/) supported by Statiq.Web. In the examples below, we will assume that you write your articles using Markdown, with [front matter](https://www.statiq.dev/guide/documents-and-metadata/front-matter) in YAML.

### Home Page

The theme comes **without** an initial home page. However, to see a similar home page as show cased by [Statiq Dopetrope Demo](https://github.com/suchja/statiq-dopetrope-demo), add a `index.cshtml` to your `input` folder (but **not** the `input` folder inside the `theme` folder) with the following content:

```html
@await Html.PartialAsync("_index-main")
```

This will be processed by [STATIQ - site generator](https://www.statiq.dev) as a single static page. That means:
1. It will apply the complete site layout (as defined in `theme/input/_layout.cshtml`)
2. Once it comes to calls like `@RenderSectionOrPartial` or `HTML.Partial...` in the `_layout.cshtml`, the specified partials will be applied.
3. Finally it will arrive at a `RenderBody` call in the `_layout.cshtml`. This is when the content of "your" `index.cshtml` will be used. In this example (`@await Html.PartialAsync("_index-main")`) you simply say that the content within `_index-main.cshtml` shall be used.

You can and probably will provide your own home page. The `_index-main.cshtml` is simply meant as starting point. More Details can be found in the chapters [Customization](#customization) and [Settings](#settings).

#### Sidebar

***TODO:*** See [this issue](https://github.com/suchja/statiq-dopetrope-theme/issues/13)

### Blog Posts

Add your blog posts to a `posts` folder under your own `input` folder. An example post might be named `input/posts/example.md` and contain the following content:

```text
Title: This Is An Example Post
Lead: Yay for examples!
Published: 12/13/2014
Tags:
  - Examples
  - Tag with spaces
---
This is my example blog post content.
```

The `---` line separates the _front matter_, i.e. YAML code containing page-specific settings, from the actual body of the post. YAML syntax is widely known and is usually intuitive; however, if you need a quick tutorial or a refresher you may find [this article](https://learnxinyminutes.com/docs/yaml/) useful.

As for the `.md` extension, it means that the article body is written with Markdown syntax. And yes, there's [a quick guide](https://learnxinyminutes.com/docs/markdown/) for it, too. See also [here](https://www.statiq.dev/guide/content-and-data/template-languages/markdown) for Markdown features specific to Statiq.Web.

The meaning of the different *front matter* variables used in the example post above, is described in the [settings chapter](#page).

#### Published Date

If you don't specify a `Published` date, Statiq will use the last modification date of the file. Since Git does not save and restore file times, you should always specify a `Published` date if you store your blog in a Git repository; otherwise, all your posts will appear to have been published at the date of checkout.

#### Tags

***TODO:*** see [this issue](https://github.com/suchja/statiq-dopetrope-theme/issues/14)

### Static pages

Static pages purpose is to provide information on a site-wide basis, for example an author's bio, portfolio, and/or resume; a privacy policy; a contact form; etc.

Add your static pages to a `pages` folder under your own `input` folder. An example page might be named `input/pages/about.md` and contain the following content:

```text
NavbarTitle: About
Order: 1
Title: About me
Description: The life and times of James D. Author
---
Hi there, I'm James and this is my blog.

Short bio; why I opened a blog; miscellaneous yada yada.
```

The meaning of the different *front matter* variables used in the example page above, is described in the [settings chapter](#page).

## Customization

This theme offers lots of customization options. Methods through which you can personalize your blog include:

- **Settings** - Quite a lot can be accomplished just by tweaking [settings](#settings), either in a global `settings.yml` file, in the front matter of documents, or both.
- **File addition** - Certain files in the theme are just empty placeholders. Just add a corresponding file in your `input` folder and it will be used instead of the empty file. This way you can add your own HTML, scripts, and styles, without having to modify the theme.
- **File replacement** - You can replace every theme file with your own. Just copy any file from the theme's `input` folder to your own, modify the copy at your leisure, and enjoy your fully personalized blog.

> **NOTE:** This theme uses [Razor](https://www.statiq.dev/guide/content-and-data/template-languages/razor) to build HTML pages and [SASS](https://sass-lang.com) to build CSS styles. You may want to familiarize with their syntax (besides, of course, HTML and CSS) before making additions and/or modifications.

The remainder of this section illustrates how to customize some specific areas of the theme. More exhaustive references for settings and placeholder files are in subsequent sections.

### Basic Configuration

Here's an example of a `settings.yml` file:

```yaml
# Hosting settings
Host: jamesdauthor.github.io # Host name - This example is for GitHub pages
LinksUseHttps: true # Whether to generate HTTPS links in archives etc. - usually true, of course

# Branding
SiteTitle: My awesome blog
SiteDescription: The blog no one but me was missing
Copyright: => @$"All content © {DateTime.Now.Year} James D. Author" # You can use C# expressions prefixed by =>

# Blog settings
DateTimeInputCulture: "" # This determines the expected format for Published dates in posts - Empty string means invariant culture (yyyy-MM-dd format)
DateTimeDisplayCulture: en-US # This determines the format for displayed dates, e.g. in post headers and archive pages
GenerateSearchIndex: true # Setting this to false disables both the search box in navigation bar and the search page
```

> [!WARNING]
> The original theme did not provide any visuals for a search functionality. This is currently under development (see [this issue](https://github.com/suchja/statiq-dopetrope-theme/issues/16)). Therefore the `GenerateSearchIndex` setting does not have any impact.

### The Navigation Bar

The navigation bar is a series of links that appears at the top of every page of your blog on desktop browsers, or can be displayed by touching a "Menu" button in the top right corner on mobile browsers.

The navigation bar includes: links to all static pages (by default, see below), a "Posts" link to a historical archive of posts, and a "Tags" link to an index of all tags.

> [!Important]
> The themes navigation shows which site is currently active. This might be confusing, if the homepage is **not** included in the navigation. Although the `SiteTitle` allows to go back to the homepage as well, you might want to add your homepage to navigation as well. See the [following section](#links-to-static-pages) for settings you need to add to your `index.cshtml` file to get hits working.

#### Links To Static Pages

If you don't want a page to be linked in the navigation bar, add a `ShowInNavbar: false` setting in its front matter.

If you don't specify a value for `NavbarTitle` in a page, `Title` will be used as the link text.

Links are sorted by the value of their `Order` setting. Pages with `Order` greater than or equal to 100 will appear after "Posts" and "Tags".

### Banner & Intro elements

The standard homepage shows an image (or color if no image is present) with a centered box containing a headline and some text. This is called *banner*. The *banner* is located immdiately below the [Navigation Bar](#the-navigation-bar).

Below the *banner* is a section called *intro*. This currently shows 3 Icons with some text. Here you could for example show the characteristics of your business, site, ...

There are several *front matter* variables you can use to add your content to the *banner*. Those are described in [settings](#page).

### Destination Paths

By default, posts and static pages are output using the same folder structure they were found in your `posts` and `pages` folders, respectively. If you want to modify that, you can use [directory metadata](https://www.statiq.dev/guide/web/directory-metadata) to specify a different destination. If you combine that with [computed values](https://www.statiq.dev/guide/documents-and-metadata/metadata-values#computed-values), you can programatically set the destination of your posts.

For example, the following in a `_directory.yml` file in the `posts` folder will result in outputting posts with a path of "yyyy/mm/post-title.html":

```yaml
DestinationPath: => $"{Document.GetDateTime("Published").ToString("yyyy/MM")}/{Document.Destination.FileName}"
```

Keep in mind that what determines if a document is a post or a static page is its _source_ location (unless, of course, you manually set `IsPost` or `IsPage` in front matter). You can go wild with computed destinations, even to the point where your resulting site has no `posts` or `pages` directory at all: the correct styles will still be applied to each post or static page, and of course all links (e.g. in archives and navigation bar) will point to your computed destinations.

## Settings

### Global

These can be set in a settings file (in addition to any of Statiq's [settings](https://statiq.dev/guide/configuration/settings) and [web settings](https://statiq.dev/guide/configuration/web-settings)).

- `SiteTitle`: The title of the site.
- `SiteDescription`: The subtitle shown on the home (index) page.
- `PostSources`: A globbing pattern where to find blog posts (defaults to `posts/**/*`).
- `PageSources`: A globbing pattern where to find static pages (defaults to `pages/**/*`).

It's also highly recommended to set `Host` and `LinksUseHttps` to match your hosting environment. This ensures that the RSS feeds and other artifacts that rely on absolute hosting information are correctly generated.

### Page

These can be set in front matter, a sidecar file, etc. (in addition to any of Statiq's [settings](https://statiq.dev/guide/configuration/settings) and [web settings](https://statiq.dev/guide/configuration/web-settings)).

- `Title`: The title of the page (or post).
- `NavbarTitle`: The title of the page to use in the top-level navbar (optional, `Title` will be used if not specified).
- `Description`: A description of the page; not displayed as a subtitle for posts, but used nonetheless to build a `<meta>` HTML tag.
- `Lead`: Descriptive text that expands on the title of a post.
- `Tags`: Tags for a blog post.
- `Published`: The date a page or post was published.
- `Image`: Path to an image for the page or post.
- `ImageAttribution`: Markdown of copyright attribution for the image.
- `ShowInNavbar`: Set to `false` to hide a static page in the top navigation bar.
- `IsPost`: Set to `false` to exclude the file from the set of posts. This will also disable post styling like displaying tags in the header.
- `IsPage`: Set to `false` to exclude the file from the set of static pages. This will also disable navigation bar linking.
- `Banner-heading`: The text that should be shown as heading in the banner box
- `Banner-description`: The text that should be shown below the heading in the banner box

> [!Note]
> Currently there is no support for a *modified* date of a page or post. This is under development (see [this issue](https://github.com/suchja/statiq-dopetrope-theme/issues/18)).

### Post comments

***TODO:*** See [this issue](https://github.com/suchja/statiq-dopetrope-theme/issues/15)
These can be set in a settings file (in addition to any of Statiq's [settings](https://statiq.dev/guide/configuration/settings) and [web settings](https://statiq.dev/guide/configuration/web-settings)).

- `CommentEngine`: The comment engine to use for posts (empty string or "none" to _not_ display comments).

Currently, the only available comment engine is [`giscus`](https://giscus/app).

You can interface to a comment engine of your choice without modifying this theme:

- create a Razor partial named `_post-comments-name_of_your_engine.cshtml` file in your `input` folder, containing the necessary HTML code;
- set `CommentEngine` to `name_of_your_engine` to have your partial automatically included.

#### Giscus

***TODO:*** See [this issue](https://github.com/suchja/statiq-dopetrope-theme/issues/15)

[`giscus`](https://giscus/app) uses GitHub Discussions as comment storage.

The following settings must be set in your `settings.yml` file when `CommentEngine` is set to `giscus`:

- `GiscusRepoName`: Name of the repository whose discussions act as storage for post comments.
- `GiscusRepoId`: ID of the repository whose discussions act as storage for post comments.
- `GiscusCategoryId`: ID of the discussion category where new discussions will be created. It is recommended to use a category with the Announcements type so that new discussions can only be created by maintainers and giscus.

To configure Giscus correctly in your blog, go to https://giscus.app and follow the configuration instructions, then copy and paste configuration values from the preconfigured `<script>` tag to your settings file.

Alternatively, or if you need to tweak some advanced Giscus setting, create a file named `_post-comments-giscus.cshtml` in your `input` folder and just copy the preconfigured script into it.

### Calculated Settings

The following keys are calculated in `settings.yml` and can be overridden by providing new values in your settings or front matter or used from your own pages.

- `PageTitle`: The title that's set for the current page and shown in the browser (by default this is "[Site Title] - [Document Title]").

## Partials

Replace or copy any of these Razor partials in your `input` folder to override sections of the site:

- `/_head.cshtml`: Additional content for the `<head>` tag.
- `/_navigation.cshtml`: The navigation bar at the top every page.
- `/_navbar.cshtml`: The items of the navigation bar at the top of the page.
- `/_header.cshtml`: The header section of the page. Replace this for a complete new header. This currently includes `/_navigation` on all pages. Additionally it calls `/_banner.cshtml` as well as `/_intro.cshtml`, if used for the homepage.
- `/_banner.cshtml`: The image with the box containing a heading and short description. Located immediately after the navigation. To modify the text of heading and description, simply use the variables as defined in [settings](#page).
- `/_intro.cshtml`: The section immediately below the *banner* (**only** visible on homepage!). This one you should replace with your own content or provide an empty file with this name to skip the *intro*.
- `/_index-main`: The main content of the homepage. This includes `/_index-portfolio.cshtml` as well as `/_index-blog.cshtml`. If the default homepage of the theme suits your needs, you might want to leave this as it is and change the either `/_index-portfolio.cshtml` and/or `/_index-blog.cshtml`. Otherwise you could also ignore this file completely and directly include `/_index-portfolio.cshtml` and/or `/_index-blog.cshtml` from your `/index.cshtml`, if that is what you fancy!
- `/_index-portfolio.cshtml`: The portfolio like section after the *intro* (**only** visible on homepage by default!). Currently you require a local copy of this file in your website to modify its content. As described in [this issue](https://github.com/suchja/statiq-dopetrope-theme/issues/20), there might be an easier "configuration" of the portfolio in the future.
- `/_index-blog.cshtml`: The section showing the two most recent blog posts (**only** visible on homepage by default!). As described in [this issue](https://github.com/suchja/statiq-dopetrope-theme/issues/2) the amount of posts shown is currently not configurable. This uses the `/_post-overview.cshtml` to visualise an overview of each post.
- `/_post-overview.cshtml`: A card like presentation with important information about a single post.
- `/_content-header.cshtml`: This relates to content in the body of a page or post. Replacing this you can change how pages or posts show their image, heading and description.
- `/_footer.cshtml`: The footer at the bottom of all pages. This file provides the basic layout with its rows and columns. The Content as well layout can be configured in the other footer related files (e.g. `/_footer-about.cshtml`).
- `/_footer-about.cshtml`: This shows a small about section in the footer. This fits into the overall footer layout as defined in `/_footer.cshtml`.
- `/_footer-contact.cshtml`: This shows a section with social media links and address in the footer. This fits into the overall footer layout as defined in `/_footer.cshtml`.
- `/_footer-copyright-credits.cshtml`: Displays a copyright attribution and credits for the blog. By default, this is shown as last entry in the footer.
- `/_footer-dates.cshtml`: This shows the dates section in the footer. This fits into the overall footer layout as defined in `/_footer.cshtml`. Based on [this issue](https://github.com/suchja/statiq-dopetrope-theme/issues/3) it is planned to handle these dates more easily for the user.
- `/_footer-first-link-section.cshtml`: This shows a list of links in the footer. This fits into the overall footer layout as defined in `/_footer.cshtml`.
- `/_footer-second-link-section.cshtml`: This shows another list of links in the footer. This fits into the overall footer layout as defined in `/_footer.cshtml`.
- `/_scripts.cshtml`: Additional scripts or other content at the bottom of the page.

### Homepage - modifying the default homepage

As detailed in the previous list about all [partials](#partials), there are a few parts on the homepage, that sets it apart from other pages and posts:
- *Banner*
- *Intro*
- *Portfolio*
- *Blog-Overview*

The theme provides partials for each of these parts. That means you can change the content of each of these parts by adding the related file to your `input` folder. You can copy the file from the theme's `input` folder to start with. Now you can change the content as you fancy. In case you like to completely skip a part, simply add its file to your `input` **without** any content.

## Sections

In addition to globally changing sections of the site using the partials above you can also add the following Razor sections to any given page to override them for that page (which will typically disable the use of the corresponding partial):

- `Head`: Additional content for the `<head>` tag (this section is additive with the content in the `_head.cshtml` partial).
- `Header`: The header section of the page (see [partials](#partials) for what is replaced by this section.).
- `Footer`: The footer section of the page (see [partials](#partials) for what is replaced by this section.).
- `Scripts`: Additional scripts or other content at the bottom of the page (this section is additive with the content in the `_scripts.cshtml` partial).

## Styles

There are three distinct extensibility points for styles:

- to override the style of a post (content within `<article>` tag styled by `box.post`), create an input file at `scss/_box-post-overrides.cshtml` and add the proper styling;
- to override theme variables, for example to change the primary color or the banner, create an input file at `scss/_variable-overrides.scss` and add your variables there;
- to override any styles, create an input file at `scss/_overrides.scss` and add your styles there.

You will find the definitions of overridable Sass variables in the following files:

- `input/scss/_variables.scss` (to customize this variation of the theme - primary color as well as banner styling is here).

