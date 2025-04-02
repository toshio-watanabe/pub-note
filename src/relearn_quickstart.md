# Getting Started

Here’s how to start your new website. If you’re new to Hugo, we recommend learning more about it in its excellent [starter’s guide](https://gohugo.io/getting-started/).

<details open="" class=" box cstyle notices note" style="box-sizing: border-box; display: block; background-color: var(--VARIABLE-BOX-color); border-color: var(--VARIABLE-BOX-color); border-style: solid; border-width: 1px; margin: 1.5rem 0px; outline: none; pointer-events: none; --VARIABLE-BOX-color: var(--INTERNAL-BOX-NOTE-color); --VARIABLE-BOX-CAPTION-color: var(--INTERNAL-BOX-CAPTION-color); --VARIABLE-BOX-BG-color: var(--INTERNAL-BOX-BG-color); --VARIABLE-BOX-TEXT-color: var(--INTERNAL-BOX-NOTE-TEXT-color); -webkit-print-color-adjust: exact; color: rgb(224, 224, 224); font-family: &quot;Roboto Flex&quot;, Helvetica, Tahoma, Geneva, Arial, sans-serif; font-size: 16.25px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 300; letter-spacing: 0.2275px; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary class="box-label" tabindex="-1" style="box-sizing: border-box; display: block; color: var(--VARIABLE-BOX-CAPTION-color); font-weight: 500; margin-left: 0.6rem; margin-right: 0.6rem; padding: 0.2rem 0px;"><i class="fa-fw fas fa-exclamation-circle" style="box-sizing: border-box; -webkit-font-smoothing: antialiased; display: var(--fa-display,inline-block); font-style: normal; font-variant: normal; line-height: 1; text-rendering: auto; font-family: &quot;Font Awesome 6 Free&quot;; text-align: center; width: 1.25em; font-weight: 900;"></i><span>&nbsp;</span>Note</summary><div class="box-content" style="box-sizing: border-box; background-color: var(--VARIABLE-BOX-BG-color); color: var(--VARIABLE-BOX-TEXT-color); padding: 1rem; pointer-events: auto;"><p style="box-sizing: border-box; margin: 0px;">This quickstart is not meant for a production setup. In production you always want to use specific versions of the theme and Hugo. See the<span>&nbsp;</span><a href="https://mcshelby.github.io/hugo-theme-relearn/introduction/upgrade/index.html" class="highlight" style="box-sizing: border-box; background: 0px 0px; text-decoration: none; color: var(--INTERNAL-MAIN-LINK-color); margin-top: 0px; margin-bottom: 0px;">upgrade page</a><span>&nbsp;</span>to learn how to obtain a specific version of the theme.</p></div></details>

## Install Hugo

Download and install Hugo 0.126.0 or newer for your operating system [following the instructions](https://gohugo.io/installation/).

The *standard* edition of Hugo is sufficient but you can also use the *extended* edition.

## Create your Project

Use Hugo’s `new site` command to make a new website

```shell
hugo new site my-new-site
```

Then move into the new directory

```shell
cd my-new-site
```

Run all following commands from this directory.

## Install the Theme

### Download as a Zip File

You can [download the theme as a .zip file](https://github.com/McShelby/hugo-theme-relearn/archive/main.zip) and unzip it into the `themes/hugo-theme-relearn` directory.

Then add this at the top of your `hugo.toml`

hugo.

tomlyamljson

```toml
theme = 'hugo-theme-relearn'
```

### Use Hugo’s Module System

Install the Relearn theme using [Hugo’s module system](https://gohugo.io/hugo-modules/use-modules/#use-a-module-for-a-theme)

```shell
hugo mod init example.com
```

Then add this at the end of your `hugo.toml`

hugo.

tomlyamljson

```toml
[module]
  [[module.imports]]
    path = 'github.com/McShelby/hugo-theme-relearn'
```

### Use as a Git Submodule

If you’re using [Git](https://git-scm.com/) for your project, you can create a repository now

```shell
git init
```

Add the theme as a Git submodule

```shell
git submodule add --depth 1 https://github.com/McShelby/hugo-theme-relearn.git themes/hugo-theme-relearn
```

Then add this at the top of your `hugo.toml`

hugo.

tomlyamljson

```toml
theme = 'hugo-theme-relearn'
```

## Create Content

### Your Home Page

Start by making a home page

```shell
hugo new --kind home _index.md
```

The new home page file `content/_index.md` has two parts: the page info (like `title`) at the top, called [front matter](https://gohugo.io/content-management/front-matter/), and the page content below.

### Your First Chapter Page

Chapters are top-level pages that contain other pages. They have a special layout.

Make your first chapter page

```shell
hugo new --kind chapter log/_index.md
```

The new file `content/log/_index.md` has a `weight` number in the front matter. By the themes default settings this sets the chapter’s subtitle and its order in the menu.

### Your First Content Pages

Now make content pages inside the chapter. Here are three ways to do this

```shell
hugo new log/first-day/_index.md
hugo new log/second-day/index.md
hugo new log/third-day.md
```

Hugo treats these files differently based on their file names. Learn more in [Hugo’s guide](https://gohugo.io/content-management/).

Feel free to edit these files. Change the `title`, add a `weight` if you want, and write your content.

## Test your Website

Start your new website on your computer with this command

```shell
hugo serve
```

Open [http://localhost:1313](http://localhost:1313/) in your web browser.

You can keep the server running while you edit. The browser will update automatically when you save changes.



It’s a kind of magic

## Build and Deploy

When your site is ready to go live, run this command

```shell
hugo
```

This creates a `public` directory with all your website files.

You can upload this directory to any web server, or use one of [Hugo’s many other ways to publish](https://gohugo.io/hosting-and-deployment/).

## Next Steps

Your site is now fully functional.

You can continue [configuring your site](https://mcshelby.github.io/hugo-theme-relearn/configuration/index.html) to your needs.

Or just start [authoring content](https://mcshelby.github.io/hugo-theme-relearn/authoring/index.html) and discover what’s possible.