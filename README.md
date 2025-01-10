## Installation
Install Ruby
and then execute:

    $ bundle install
    $ bundle exec jekyll serve

### Layouts

Refers to files within the `_layouts` directory, that define the markup for your theme.

  - `base.html` &mdash; The base layout that lays the foundation for subsequent layouts. The derived layouts inject their
    contents into this file at the line that says ` {{ content }} ` and are linked to this file via
    [FrontMatter](https://jekyllrb.com/docs/frontmatter/) declaration `layout: base`.
  - `home.html` &mdash; The layout for your landing-page / home-page / index-page. [[More Info.](#home-layout)]
  - `page.html` &mdash; The layout for your documents that contain FrontMatter, but are not posts.
  - `post.html` &mdash; The layout for your posts.


### Includes

Refers to snippets of code within the `_includes` directory that can be inserted in multiple layouts (and another include-file as well) within the same theme-gem.

  - `disqus_comments.html` &mdash; Code to markup disqus comment box.
  - `footer.html` &mdash; Defines the site's footer section.
  - `google-analytics.html` &mdash; Inserts Google Analytics module (active only in production environment).
  - `head.html` &mdash; Code-block that defines the `<head></head>` in *default* layout.
  - `custom-head.html` &mdash; Placeholder to allow users to add more metadata to `<head />`.
  - `header.html` &mdash; Defines the site's main header section. By default, pages with a defined `title` attribute will have links displayed here.
  - `social.html` &mdash; Renders social-media icons based on the `minima:social_links` data in the config file.
  - `social-item.html` &mdash; Template to render individual list-item containing graphic link to configured social-profile.
  - `social-links/*.svg` &mdash; SVG markup components of supported social-icons.


### Sass

Refers to `.scss` files within the `_sass` directory that define the theme's styles.

  - `minima/skins/classic.scss` &mdash; The "classic" skin of the theme. *Used by default.*
  - `minima/initialize.scss` &mdash; A component that defines the theme's *skin-agnostic* variable defaults and sass partials.
    It imports the following components (in the following order):
    - `minima/custom-variables.scss` &mdash; A hook that allows overriding variable defaults and mixins. (*Note: Cannot override styles*)
    - `minima/_base.scss` &mdash; Sass partial for resets and defines base styles for various HTML elements.
    - `minima/_layout.scss` &mdash; Sass partial that defines the visual style for various layouts.
    - `minima/custom-styles.scss` &mdash; A hook that allows overriding styles defined above. (*Note: Cannot override variables*)


### Assets

Refers to various asset files within the `assets` directory.

  - `assets/css/style.scss` &mdash; Imports sass files from within the `_sass` directory and gets processed into the theme's
    stylesheet: `assets/css/styles.css`.
  - `assets/minima-social-icons.html` &mdash; Imports enabled social-media icon graphic and gets processed into a composite SVG file.


### Plugins

Minima comes with [`jekyll-seo-tag`](https://github.com/jekyll/jekyll-seo-tag) plugin preinstalled to make sure your website gets the most useful meta tags. See [usage](https://github.com/jekyll/jekyll-seo-tag#usage) to know how to set it up.


### Customize navigation links

This allows you to set which pages you want to appear in the navigation area and configure order of the links.

For instance, to only link to the `about` and the `portfolio` page, add the following to your `_config.yml`:

```yaml
header_pages:
  - about.md
  - portfolio.md
```

### Enabling comments (via Disqus)

Optionally, if you have a Disqus account, you can tell Jekyll to use it to show a comments section below each post.

:warning: `url`, e.g. `https://example.com`, must be set in you config file for Disqus to work.

To enable it, after setting the url field, you also need to add the following lines to your Jekyll site:

```yaml
  disqus:
    shortname: my_disqus_shortname
```

You can find out more about Disqus' shortnames [here](https://help.disqus.com/installation/whats-a-shortname).

Comments are enabled by default and will only appear in production, i.e., `JEKYLL_ENV=production`

If you don't want to display comments for a particular post you can disable them by adding `comments: false` to that post's YAML Front Matter.


### Social networks

You can add links to the accounts you have on other sites, with respective icon as an SVG graphic, via the config file.
From `Minima-3.0` onwards, the social media data is sourced from config key `minima.social_links`. It is a list of key-value pairs, each entry
corresponding to a link rendered in the footer. For example, to render links to Jekyll GitHub repository and twitter account, one should have:

```yaml
minima:
  social_links:
    - { platform: github,  user_url: "https://github.com/jekyll/jekyll" }
    - { platform: twitter, user_url: "https://twitter.com/jekyllrb" }
```

Apart from the necessary keys illustrated above, `title` may also be defined to render a custom link-title. By default, the title is the same
as `platform`. The `platform` key corresponds to the SVG id of the sprite in the composite file at URL `/assets/minima-social-icons.svg`.

The theme ships with an icon for `rss` and icons of select social-media platforms:

- `devto`
- `dribbble`
- `facebook`
- `flickr`
- `github`
- `google_scholar`
- `instagram`
- `keybase`
- `linkedin`
- `microdotblog`
- `pinterest`
- `stackoverflow`
- `telegram`
- `twitter`
- `youtube`

To render a link to a platform not listed above, one should first create a file at path `_includes/social-icons/<PLATFORM>.svg` comprised of
graphic markup **without the top-level `<svg></svg>`**. The icon is expected to be centered within a viewbox of `"0 0 16 16"`. Then, make an
entry under key `minima.social_links`.

For example, to render a link to an account of user `john.doe` at platform `deviantart.com`, the steps to follow would be:
  - Get DeviantArt logo in SVG format.
  - Using a text-editor, open the downloaded file to inspect if the `viewBox` attribute is defined on the `<svg>` element and is set
    as `"0 0 16 16" (or similar "square" dimension)`.
  - If the `viewBox` attribute is non-square or undefined, the graphic *may optionally need* to be edited in a vector graphic editor such as
    *Inkscape* or *Adobe Illustrator* for properly aligned render on page.
  - Edit the SVG file in text-editor to delete everything **except** what is contained between `<svg></svg>` and save it into the Jekyll
    project at path `_includes/social-icons/deviantart.svg`.
  - Finally, edit the Jekyll config file to enable loading of new icon graphic with:
    ```yaml
    minima:
      social_links:
        - platform: deviantart  # same as SVG filename.
          user_url: "https://www.deviantart.com/john.doe"  # URL of profile page.
          title:  My profile at DeviantArt.com  # Optional. Text displayed on hovering over link.
    ```

**Notes:**
- The list of social-links is declarative. List-items are rendered in the order declared in the downstream configuration file and not merged
  with entries from upstream config file(s) such as theme-config-file or prior local config files.
- The `user_url` is rendered as given without handling any special characters within.


### Enabling Google Analytics

To enable Google Analytics, add the following lines to your Jekyll site:

```yaml
  google_analytics: UA-NNNNNNNN-N
```

Google Analytics will only appear in production, i.e., `JEKYLL_ENV=production`

### Enabling Excerpts on the Home Page

To display post-excerpts on the Home Page, simply add the following to your `_config.yml`:

```yaml
show_excerpts: true
```

## License

The theme is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
