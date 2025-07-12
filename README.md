# Template FluSight ILI Sandbox Dashboard Repository

This is a template for creating a dashboard repository for a hubverse-style modeling hub based around FluSight ILI forecasting.
It is designed to be used only with the [FluSight ILI Template Hub](https://github.com/reichlab/FluSight-ILI-template-hub), 
which is in turn based on [the archive of FluSight ILI forecasts](https://github.com/hubverse-org/flusight_hub_archive).

To be clear, **THIS IS NOT A REAL-TIME FORECASTING HUB.** 
It is a hub that is designed to be used for teaching, training and research purposes.

To set up this hub for your use, please [read the quickstart instructions below](#quickstart).


## Quickstart

1. Clone the [FluSight ILI Template Hub](https://github.com/reichlab/FluSight-ILI-template-hub) by clicking the "use this template" button on the repo homepage.
1. Clone this hub using the "use this template" button on the main page.
1. Add any special markdown content to `pages/`
1. Update `site-config.yml`
    i. `hub` is the github repository (`owner/repo`) for your active hub.
    ii. `title` is the title of your dashboard
    iii. `pages` is a list of additional optional pages you want included in the top bar after the
         home page (index.html) and forecasts (forecast.html).
3. Update `predtimechart-config.yml` according to the instructions at
   [hub_predtimechart](https://github.com/hubverse-org/hub-dashboard-predtimechart/tree/main?tab=readme-ov-file#required-hub-configuration).
4. (Optional) Add `predevals-config.yml` if you have oracle output that you can
   use to generate predevals data. (See below for a link to the documentation and
   [reichlab/flusight-dashboard](https://github.com/reichlab/flusight-dashboard/blob/main/predevals-config.yml)
   for an example).
5. (Optional) If your hub has an S3 bucket associated with it (you can find
   this in the `cloud.host.name` property of the `hub-config/admin.json` file of your hub), and you are including a `data.qmd` page, you can add this
   information to the `hub-bucket-name` key in the YAML header of
   [pages/data.qmd](pages/data.qmd). This will automatically populate the data
   template with the s3 bucket name in the appropriate locations in the page.

Once these steps are performed, the workflows will automatically generate the
website on the `gh-pages` branch on your behalf. Once this branch is created,
you can activate your website to deploy from this branch.

> [!NOTE]
>
> At the moment, the first time you create your repository, you will need to
> manually switch on your github pages by going to `<repo>/settings/pages` and
> selecting `gh-pages` as the branch to deploy from:
>
> ![screenshot of the "Build and Deployment" section of the pages setting. There are two sub-headings that say "source" and "branch". The Source heading has a dropdown that is selected to "Deploy from a branch". The Branch heading shows a dropdown with `gh-pages`, `main`, `ptc/data`, and `None` as options for the "branch" dropdown. A red arrow is pointing to the `gh-pages` option, which is highlighted.](pages.png)

## Configuration

### PredTimeChart Forecasts Visualization

Edit the [`predtimechart-config.yml`](predtimechart-config.yml) file to match your hub schema.
You can find instructions to do so [in the PredTimeChart visualization guide](https://docs.hubverse.io/en/latest/user-guide/dashboards.html#dashboard-ptc).

If you do not want a forecasts visualization on your site, you can remove the
`predtimechart-config.yml` file.

### PredEvals Visualization

Edit the [`predevals-config.yml`](predevals-config.yml) file to match your hub schema.
You can find instructions to do so [in the PredEvals visualization guide](https://docs.hubverse.io/en/latest/user-guide/dashboards.html#dashboard-predevals).


If you do not want an evaluations visualization on your site, you can remove the
`predevals-config.yml` file.

### Dashboard Website

#### Configuration

The [`site-config.yml`](site-config.yml) Is a simplified form of [A Quarto Website](https://quarto.org/docs/websites/#config-file). This simplified form is intended to allow you to set up a dashboard website in a manner of minutes while allowing for flexibility of theme.

A simple configuration is presented in [the template `site-config.yml`](https://github.com/hubverse-org/hub-dashboard-template/blob/main/site-config.yml) file
with three keys:

 - hub: the GitHub slug to your active hub that contains quantile forecast data
 - title: the title of your hub dashboard website
 - pages: a [YAML array](https://www.commonwl.org/user_guide/topics/yaml-guide.html#arrays) that lists files _relative to [the `pages` directory](pages/)_ that should be included in the dashboard site. The name of each page is encoded in the `title:` element of the file header (but this can be overridden with [site customization](#customization)).

Other than the `hub` field all remaining fields have the following mapping equivalents in the Quarto configuration file:

| `site-config.yml`  | `_quarto.yml` |
| ------------------ | ------------- |
| `.title`           | `.website.title` |
| `.pages`           | [`.website.navbar.left`](https://quarto.org/docs/websites/website-navigation.html#top-navigation) |
| `.html` (optional) | [`.format.html`](https://quarto.org/docs/reference/formats/html.html#format-options) |

#### Pages

In the `pages/` directory, you will see three pages:

1. `index.qmd` this is the home page for the dashboard website. It is required.
2. `data.qmd` this is an _optional_ template that provides instructions for how
   to access data for a hub locally or via S3. If you do not have an S3-enabled
   hub, you can remove this page (and remove it from the `site.yml` file). If
   you have an S3-enabled hub, add your hub's bucket name to the
   `hub-bucket-name` key. For example, if your are building a dashboard for the
   [CDC flusight
   hub](https://hubverse.io/community/hubs.html#flusight-forecast-hub), then
   you would use `hub-bucket-name: "cdcepi-flusight-forecast-hub"`
3. `about.md` this _optional_ page demonstrates that you can add any extra
   pages to the dashboard site. You can either replace the page with useful
   information or you can remove it altogether (and remove it from the
   `site.yml` file).

#### Customization

When the page is built with [the hub dashboard site builder](https://github.com/hubverse-org/hub-dash-site-builder), this configuration file is merged with [the default quarto config file](https://github.com/hubverse-org/hub-dash-site-builder/blob/main/static/_quarto.yml). This allows for customization of the page. Below
are examples of customization.

##### Icons added to pages

You can add icons to the page title bars with a YAML map. If you wanted to add an icon of people for the "about" page, you would use `.pages.icon: "people-fill"`

```yaml
pages:
  - icon: "people-fill"
    href: "about.md"
  - icon: "mortarboard-fill"
    href: "citation.md"
```

##### Theme

The default site is built on top of the [Bootstrip yeti theme](https://bootswatch.com/yeti/) with [custom CSS](https://github.com/hubverse-org/hub-dash-site-builder/blob/main/static/resources/css/styles.css).

If you wanted to use [a different theme](https://quarto.org/docs/output-formats/html-themes.html), you can change it by setting `.html.theme`. You can reset the css by setting `.html.css: null`

```yaml
html:
  theme: "litera"
  css: null
```

##### Contents

If you wanted to add custom HTML to appear at the bottom or top of every page,
you can use `.html.include-after-body` or `.html.include-before-body`. Remeber
that all resources are _relative to the `pages/` directory_, so if you wanted
to include an HTML snippet at the end of every page you would:

1. add a file called `resources/after-body.html` into `pages/`
2. add this to your yaml:
   ```yaml
   html:
     include-after-body: "resources/after-body.html"
   ```
