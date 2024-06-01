Sidecar
=======

A plain but pretty [Pelican](https://getpelican.com/) theme with a little sidebar.

Features
--------

* **Nice-looking text** with lots of extra typographical features.

  Sidecar uses Oatcake, my CSS typography stylesheet.
  See [Oatcake's site](https://www.seanh.cc/oatcake/) for all the details.

* **Responsive design**: works great on both desktop and mobile.

* **Supports most of Pelican's features**:
  all the different pages (static pages, archives and period archives, category and categories pages, tag and tags pages, author and authors pages),
  syntax highlighting,
  Atom and RSS feeds (with feed autodiscovery links in the HTML `<head>`),
  pagination.
  Also responds to many of Pelican's default settings (see [Settings](#settings) below).

  Support for a couple of Pelican features is still missing, including
  [translations](https://github.com/seanh/sidecar/issues/1)
  and [author, category and tag feeds](https://github.com/seanh/sidecar/issues/18).

* **Standard, semantic HTML**:
  `<main>` for the main content of index and static pages,
  `<article>` for articles,
  `<footer>` for article footers,
  `<time>` for the publication dates in article footers,
  `rel="author"` for article author links,
  `rel="bookmark"` for article permalinks,
  `rel="tag"` for tag links,
  `rel="next"` and `rel="prev"` for pagination links.
  Links have nice `title`'s for tooltips.
  Pages have nice `<title>`'s for tab titles.

* Optional **tables of contents** for static pages and articles.

Usage
-----

1. Clone the theme to a local directory:

   ```terminal
   $ git clone https://github.com/seanh/sidecar.git
   ```

2. Set the [`THEME`](https://docs.getpelican.com/en/latest/settings.html#THEME)
   setting in your Pelican config to the path to your local clone of Sidecar.
   It can be either an absolute path or a path relative to your Pelican config
   file:

   ```python
   # pelicanconf.py

   THEME = "../sidecar"
   ```

Removing the archives page
--------------------------

Sidecar differs from Pelican's built-in themes in that it shows only the titles
of articles on index pages (like the front page). Sidecar doesn't support
showing full article contents or summaries on index pages.

This can make Pelican's archives page unnecessary, especially if you have
pagination disabled on the front page: the archives page is just the same as
the front page. So you might want to remove your site's archives page.
You can do this with a couple of settings in your Pelican config:

```python
# pelicanconf.py

# Disable the archives page.
ARCHIVES_SAVE_AS = ""
ARCHIVES_URL = ""
```

Article summaries are still used in RSS and Atom feeds.

<details>
  <summary><h4>Why Sidecar doesn't support article summaries or full bodies on index pages</h4></summary>

Pelican's built-in `simple` and `notmyidea` themes show summaries of articles
on index pages (like the front page) by default.
If an article has a handwritten `summary` in its metadata then that's used as
the article's summary.
Otherwise a summary is generated by truncating the article body after
`SUMMARY_MAX_LENGTH` (50) words and appending `SUMMARY_END_SUFFIX` (…).
If `SUMMARY_MAX_LENGTH` is set to `None` then the full bodies of articles are
shown on index pages, instead of just summaries.

Hand-writing a summary adds extra work to posting an article.

Automatically generating summaries by truncating articles after a fixed number
of words gives bad results, cutting off articles in the middle of a sentence,
list, heading, code block, or who knows what, sometimes catching an image or
video in the summary, etc.

Showing full article bodies on index pages also works poorly with my content:
many of my articles are very long and would push earlier posts many screenfuls
down. With so much text to scroll past it would be difficult to spot when one
article ends and the next begins.

I didn't want to offer an option of full bodies, summaries, or only titles
because this would make for more design work, having to come up with
good-looking index page designs for all three options when I'm only actually
going to use one of them on my site.

So I ended up deciding to support only article titles on index pages. I like
how quick and easy it is to find articles and navigate between them.  Tag and
category pages that're just a list of article titles are particularly nice for
getting an overview of everything posted in that tag or category.
</details>

Tables of contents
------------------

Sidecar uses [Tocbot](https://tscanlin.github.io/tocbot/) to generate tables of
contents with anchor links from
[AnchorJS](https://www.bryanbraun.com/anchorjs/).
I prefer this approach rather than using a Pelican plugin or Markdown extension
to generate tables of contents because it's portable to other site generators:
just include AnchorJS and Tocbot in your pages and your tables of contents will
work.

Insert a div with the CSS class `toc` anywhere in a page or article and it'll
be turned into a table of contents based on the page or article's headers:

```html
<!-- Tocbot will turn this div into a table of contents. -->
<div class="toc"></div>
```

If you want to omit a particular heading from the table of contents add the CSS
class `notoc` to it:

```html
<h1 class="notoc">Heading</h1>
```

If you don't want a heading to have an anchor link either, add `noanchor` to it
(this will also remove the heading from the table of contents):

```html
<h1 class="noanchor">Heading</h1>
```

Settings
--------

### `SITENAME`

Sets the name of your site in tab and feed titles:

```python
# pelicanconf.py

SITENAME = "A Pelican Blog"
```

### SITEURL

You must set this to the base URL of your site with no trailing slash.
It's used to generate URLs in feeds, links in the sidebar, etc.

```python
# pelicanconf.py

SITEURL = "http://blog.notmyidea.org"
```

### DEFAULT_LANG

Your site's default language, used for the standard `lang` attribute on the
root `<html>` element. If not set this defaults to `"en"`.

```python
# pelicanconf.py

DEFAULT_LANG = "en"
```

### `ARTICLE_SOURCE_URL` and `PAGE_SOURCE_URL`

If [Pelican's `OUTPUT_SOURCES` setting](https://docs.getpelican.com/en/latest/settings.html#basic-settings)
is enabled Pelican copies the plain text source files of
your articles and pages into your site's output directory.

On article and static pages, if `OUTPUT_SOURCES` is enabled, Sidecar inserts a
link to the article or page's plain text source file into the HTML `<head>`
(using `<link rel="alternate" type="text/plain" href="...">`).

If you've changed [Pelican's `ARTICLE_SAVE_AS` and `ARTICLE_URL` or `PAGE_SAVE_AS` and `PAGE_URL` settings](https://docs.getpelican.com/en/latest/settings.html#url-settings)
from the defaults then you need to add `ARTICLE_SOURCE_URL` and
`PAGE_SOURCE_URL` settings to your Pelican config to tell Sidecar how to
generate the URLs to your article and page source files. For example:

  ```python
  # pelicanconf.py

  ARTICLE_SAVE_AS = '{date:%Y}/{date:%m}/{date:%d}/{slug}/index.html'
  ARTICLE_URL = '{date:%Y}/{date:%m}/{date:%d}/{slug}/'
  ARTICLE_SOURCE_URL = "{article.url}index{OUTPUT_SOURCES_EXTENSION}"

  PAGE_SAVE_AS = '{slug}/index.html'
  PAGE_URL = '{slug}/'
  PAGE_SOURCE_URL = "{page.url}index{OUTPUT_SOURCES_EXTENSION}"
  ```

### `GITHUB_URL`

If Pelican's [`GITHUB_URL`](https://docs.getpelican.com/en/latest/settings.html#GITHUB_URL)
setting is set in your Pelican config then a GitHub ribbon linking to
`GITHUB_URL` will be added to the top-right corner of your site.
The idea is that you set `GITHUB_URL` to your GitHub profile page.
For example:

```python
# pelicanconf.py

GITHUB_URL = "https://github.com/seanh"
```

### `GITHUB_REPO_URL`

If you keep your site's source files in a GitHub repo you can set
`GITHUB_REPO_URL` to the URL of that GitHub repo. If you use `GITHUB_REPO_URL`
instead of `GITHUB_URL` then on your static pages and articles the GitHub
ribbon will link directly to the page or article's source file on GitHub:

```python
# pelicanconf.py

GITHUB_REPO_URL = "https://github.com/seanh/seanh.github.io"
```

This can be particularly useful for you as the author of the site because, if
you're logged in to GitHub, the GitHub pages for your files have
<kbd>Edit</kbd> buttons that you can use to edit your source files right in the
GitHub web interface. So if you spot a typo in a page or article, you can
quickly click on the GitHub ribbon then on GitHub's <kbd>Edit</kbd> button and
correct it.

### `CONTENT_PATH`

If using `GITHUB_REPO_URL` and your site content directory isn't a `content`
directory in the root of your GitHub repo you need to add a `CONTENT_PATH`
setting to your Pelican config to help Sidecar to generate the GitHub links for
your pages and articles. This should be the path to your content directory
relative to the root of your GitHub repo:

```python
# pelicanconf.py

CONTENT_PATH = "path/to/your/content"
```

### `SIDECAR_MENU`

You can customize the contents of the sidebar by adding a `SIDECAR_MENU`
setting (list of strings) to your Pelican config. For example:

```python
# pelicanconf.py

SIDECAR_MENU = [
    "HOME",
    "MENUITEMS",
    "PAGES",
    "CATEGORIES",
    "TAGS",
    "AUTHORS",
    "ARCHIVES",
    '<a rel="external" href="https://example.com">Custom Link</a>',
]
```

Certain string values have special meanings in `SIDECAR_MENU`:

* `HOME`: inserts a link to your site's home page (uses Pelican's [`SITEURL` setting](https://docs.getpelican.com/en/latest/settings.html#SITEURL))

* `MENUITEMS`: inserts the items from Pelican's [`MENUITEMS` setting](https://docs.getpelican.com/en/latest/settings.html#MENUITEMS)

* `PAGES`: inserts links to each of your site's static pages

* `CATEGORIES`: inserts a link to your site's categories page.

  For this to work you must add matching `CATEGORIES_SAVE_AS`
  and `CATEGORIES_URL` settings to your Pelican config.
  For example:

  ```python
  # pelicanconf.py

  CATEGORIES_SAVE_AS = "categories/index.html"
  CATEGORIES_URL = "categories/"
  ```

* `TAGS`: inserts a link to your site's tags page.

  For this to work you must add matching `TAGS_SAVE_AS`
  and `TAGS_URL` settings to your Pelican config.
  For example:

  ```python
  # pelicanconf.py

  TAGS_SAVE_AS = "tags/index.html"
  TAGS_URL = "tags/"
  ```

* `AUTHORS`: inserts a link to your site's authors page.

  For this to work you must add matching `AUTHORS_SAVE_AS`
  and `AUTHORS_URL` settings to your Pelican config.
  For example:

  ```python
  # pelicanconf.py

  AUTHORS_SAVE_AS = "authors/index.html"
  AUTHORS_URL = "authors/"
  ```

* `ARCHIVES`: inserts a link to your site's archives page.

  For this to work you must add matching `ARCHIVES_SAVE_AS`
  and `ARCHIVES_URL` settings to your Pelican config.
  For example:

  ```python
  # pelicanconf.py

  ARCHIVES_SAVE_AS = "archives/index.html"
  ARCHIVES_URL = "archives/"
  ```

Items in `SIDECAR_MENU` that don't match any of the special strings above are
rendered directly. This lets you include your own raw HTML strings as menu
items. For example you could include a custom link. This lets you use HTML
attributes other than `href` in your sidebar links, which you can't do with
Pelican's `MENUITEMS`:

```python
# pelicanconf.py

SIDECAR_MENU = [
    ...
    '<a rel="external" href="https://example.com">Custom Link</a>',
]
```

The substring `{SITEURL}` will be replaced with Pelican's
[`SITEURL` setting](https://docs.getpelican.com/en/latest/settings.html#SITEURL).
For example:

```python
# pelicanconf.py

SIDECAR_MENU = [
    ...
    '<a rel="license" href="{SITEURL}/license/">License</a>',
]
```

### `DISPLAY_CATEGORIES_ON_MENU`

If [Pelican's `DISPLAY_CATEGORIES_ON_MENU` setting](https://docs.getpelican.com/en/latest/settings.html#basic-settings)
is set in your Pelican config then a list of links to each of your site's
categories will be included in the sidebar.

```python
# pelicanconf.py

DISPLAY_CATEGORIES_ON_MENU = True
```

### `DISPLAY_TAGS_ON_MENU`

Include a list of links to each of your site's tags in the sidebar.

```python
# pelicanconf.py

DISPLAY_TAGS_ON_MENU = True
```

### `DISPLAY_AUTHORS_ON_MENU`

Include a list of links to each of your site's authors in the sidebar.

```python
# pelicanconf.py

DISPLAY_AUTHORS_ON_MENU = True
```

### `LINKS` and `LINKS_WIDGET_NAME`

If [Pelican's `LINKS` setting](https://docs.getpelican.com/en/latest/settings.html#LINKS)
is set in your Pelican config the list of links will be added to the sidebar.
`LINKS` should be a list of `(title, URL)` tuples. For example:

```python
# pelicanconf.py

LINKS = [
  ("Link One", "https://example.com/link/1"),
  ("Link Two", "https://example.com/link/2"),
  ("Link Three", "https://example.com/link/3"),
]
LINKS_WIDGET_NAME = "My Links"
```

[Pelican's `LINKS_WIDGET_NAME` setting](https://docs.getpelican.com/en/latest/settings.html#LINKS_WIDGET_NAME)
sets the title of the links menu (defaults to "Links").

### `SOCIAL` and `SOCIAL_WIDGET_NAME`

If [Pelican's `SOCIAL` setting](https://docs.getpelican.com/en/latest/settings.html#SOCIAL)
is set in your Pelican config the list of social media links will be added to the sidebar.
`SOCIAL` should be a list of `(title, URL)` tuples. For example:

```python
# pelicanconf.py

SOCIAL = [
  ("Mastodon", "https://mastodon.social/@YOUR_USERNAME"),
  ("Tumblr", "https://www.tumblr.com/YOUR_USERNAME"),
]
SOCIAL_WIDGET_NAME = "Social Media"
```

[Pelican's `SOCIAL_WIDGET_NAME` setting](https://docs.getpelican.com/en/latest/settings.html#SOCIAL_WIDGET_NAME)
sets the title of the social menu (defaults to "Social").

### `SIDECAR_ARTICLE_FOOTER`

You can customize the contents of the footers at the bottoms of articles by
adding a `SIDECAR_ARTICLE_FOOTER` setting (list of strings) to your Pelican
config. For example:

```python
# pelicanconf.py

SIDECAR_ARTICLE_FOOTER = [
    "AUTHORS",
    "TIME",
    "SOURCE",
    "TAGS",
]
```

Certain string values have special meanings in `SIDECAR_ARTICLE_FOOTER`:

* `AUTHORS`: insert links to Pelican's author pages for the article's authors.

* `TIME`: inserts the article's publication date/time.

  You can customize the format of dates with Pelican's [`DEFAULT_DATE_FORMAT` and `DATE_FORMATS` settings](https://docs.getpelican.com/en/latest/settings.html#time-and-date).

* `SOURCE`: inserts a link to the article's plain text source file, if [Pelican's `OUTPUT_SOURCES` setting](https://docs.getpelican.com/en/latest/settings.html#basic-settings) is enabled.

  If you've changed Pelican's `ARTICLE_SAVE_AS` and `ARTICLE_URL` settings from
  the defaults then you need to add an `ARTICLE_SOURCE_URL` setting to your
  Pelican config to tell Sidecar how to generate the URLs to your article
  source files. See [`ARTICLE_SOURCE_URL` and `PAGE_SOURCE_URL`](#article_source_url-and-page_source_url) above.

* `GITHUB`: inserts a link to the article's source file on GitHub, if [`GITHUB_REPO_URL`](#github_repo_url) (see above) is set.

* `CATEGORY`: inserts a link to Pelican's category page for the article's category.

* `TAGS`: inserts links to Pelican's tag pages for each of the article's tags, if any.

Items in `SIDECAR_ARTICLE_FOOTER` that don't match any of the special strings
above are rendered directly, so you include your own raw HTML strings in
article footers.
