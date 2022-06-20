# Hugo

## Basic commands

```bash
# To create new website
hugo new site <NAME>

# To summarize the website contents
hugo

# To build static files
hugo -D
```

You can download various themes and configuret it in `config.toml`

```env
ubuntu@ubuntu-g2-small3-sh-1:~/website$ cat config.toml 
baseURL = "http://example.org/"
languageCode = "en-us"
title = "My New Hugo Site"
theme = "ananke"
```

To run a server with live reload

```bash
# It will contain the drafts too.
hugo server -D 

# On a server you should bind to 0.0.0.0
hugo server -D --bind="0.0.0.0"
```

## Directories

### archtypes

You can create your own archtypes with custom preconfigured front matter fields as well.

### data

It stores some configuration files for Hugo. 

### content

It contains all of contents of your websites. You can create multiple directories under this directory if you have multiple kinds of posts and contents.

### layouts

Stores templates in the form of `.html` files that specify how views of your content will be rendered into a static website.

### static

Stores all the static content: images, CSS, JavaScript, etc.
