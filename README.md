## Setup

```bash
export PATH=$PATH:/media/data/development/web/website-simgunz/hugo_0.87.0_Linux-64bit/
```



---

- Update the theme

```bash
git subtree pull --prefix themes/hugo-theme-bootstrap https://github.com/razonyang/hugo-theme-bootstrap.git vX.Y.Z --squash
```

## Preparing the images

```bash
convert -flatten -resize "400x" -quality "80" source_image.png dest_image.jpg
```

