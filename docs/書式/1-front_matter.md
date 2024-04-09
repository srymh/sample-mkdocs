# Front Matter

## サイドバーを隠す
[https://squidfunk.github.io/mkdocs-material/setup/setting-up-navigation/#hiding-the-sidebars](https://squidfunk.github.io/mkdocs-material/setup/setting-up-navigation/#hiding-the-sidebars)

- [https://squidfunk.github.io/mkdocs-material/setup/setting-up-site-search/#search-boosting](https://squidfunk.github.io/mkdocs-material/setup/setting-up-site-search/#search-boosting)
- [https://squidfunk.github.io/mkdocs-material/setup/setting-up-site-search/#search-exclusion](https://squidfunk.github.io/mkdocs-material/setup/setting-up-site-search/#search-exclusion)

``` md
---
hide:
  - navigation
  - toc
search:
  boost: 2
  exclude: true
---

```

## フッターの前/次のリンクを隠す

``` md
---
hide:
  - footer
---

```
