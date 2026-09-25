# tjerandsilde.no

Source for the personal academic website of Tjerand Silde, Associate Professor in Cryptology at NTNU: <https://tjerandsilde.no>.

The site is built with [Jekyll](https://jekyllrb.com) on a trimmed-down version of the [AcademicPages](https://github.com/academicpages/academicpages.github.io) template (itself a fork of [Minimal Mistakes](https://mademistakes.com/work/minimal-mistakes-jekyll-theme/)). It is deployed to GitHub Pages by the workflow in `.github/workflows/jekyll.yml` on every push to `master`.

## Structure

| Path | Contents |
| --- | --- |
| `_config.yml` | Site settings and sidebar profile (name, bio, links) |
| `_data/navigation.yml` | Top navigation menu |
| `_pages/` | One Markdown file per page (`about.md` is the front page) |
| `files/` | PDFs of slides, theses and articles, served at `/files/<name>` |
| `images/` | Profile photo and page images |
| `_layouts/`, `_includes/`, `_sass/`, `assets/` | Theme templates, styles, fonts and JavaScript |

## Editing

- Add a page by creating `_pages/<name>.md` with `title` and `permalink` in the front matter, and link it from `_data/navigation.yml` if it should appear in the menu.
- Link to uploaded files and other pages with root-relative paths, e.g. `[Slides](/files/Talk.pdf)` or `[Research](/research/)`.
- Lists are in reverse chronological order.

## Running locally

```sh
bundle install
bundle exec jekyll serve
```

The site is then available at <http://localhost:4000>.

## License

The theme code is released under the MIT License (see `LICENSE`). Page content and files are © Tjerand Silde.
