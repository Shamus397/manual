
# The Ardour Manual

This is the project that generates the static ardour manual website available at [manual.ardour.org](http://manual.ardour.org). The site is built using python 3.

### Get the code

    git clone <repo-url> ardour-manual
    cd ardour-manual

## Structure of the content

There are 2 different types of content:

- a master document which describes the overall structure of the manual
- normal content, which is described in the master document

### The Master Document

This is a text file (master-doc.txt) which describes the structure of the manual. It does this
through headers which tell the build script where the content lives, what its
relationship to the overall structure is, as well as a few other things.

All headers have a similar structure, and have to have at least the following
minimal structure:

    ---
    title: Some Wordy and Expressive Title
    part: part
    ---

Keywords that go into the header are of the form:

	keyword: value

Here are the keywords you can put in, and a brief description of what they do:


| Keyword | Meaning  |
| ------- | -------- |
| title   | Sets the title for the content that follows |
| menu_title | Sets the title for the content that follows which will appear in the menu link sidebar. If this is not specified, it defaults to the value of the `title` keyword |
| part    | Sets the hierarchy for the content that follows. It must be one of the following (listed in order of lowering hierarchy): part, chapter, subchapter, section, subsection.  |
| link    | Sets the unbreakable link to the content that follows. Links in the *content* should be prefixed with a double at-sign (@@) to tell the build system that the link is an internal one |
| include | Tells the build system that the content lives in an external file; these normally live in the `include/` directory. Note that the filename should **not** be prefixed with `include/` |
| exclude | Tells the `implode` and `explode` scripts that file referred to by the `include` keyword should be ignored. Note that the value of this keyword is ignored |
| style   | Sets an alternate CSS stylesheet; the name should match the one referred to (sans the `.css` suffix) in the `source/css` directory |
| uri     | Sets an absolute URI where this page will go in the hierachy of the created website. It does *not* change the document structure |

### Normal content

Manual content goes into the `include/` directory (or in the Master Document itself); and consists of normal HTML, sans the usual headers that is normally seen in regular HTML web pages. Any other content, such as css files, images, files and fixed pages goes into the `source/` directory.

Adding `source/images/horse.png` makes it available at the url `/images/horse.png` after publishing it; things work similarly for `source/files/` and `source/css/`.

## More Advanced Stuff

You probably don't want or need to do any of this, but here are some
notes just in case you decide to anyway.

### Run it locally

You may want the manual available on a machine that doesn't have constant
internet access. You will need `git`, and `python` installed.

1. Download code and build manual

  ```
  git clone <repo-url> ardour-manual
  cd ardour-manual
  ./build.py
  ```

2. open `ardour-manual/website/index.html` in your favorite web browser

  If this page doesn't open and function correctly, follow these optional steps to serve up the page with nginx.
  N.B.: Step 2 will *never* work; you *must* install nginx if you want to see it!

3. Install [nginx](http://wiki.nginx.org/Install)
4. Configure nginx server block in `/etc/nginx/sites-available/default`

  ```
  server {
      listen 80;
      server_name localhost;

      root ...path_to_.../ardour-manual/website;
      index index.html;
  }
  ```

5. Restart nginx server

        service nginx restart

6. The manual will now be available at http://localhost

### Helper scripts: `implode` and `explode`

The `implode` and `explode` scripts exist in order to accomodate different working styles. `implode` takes all the files referenced by the `include` keywords in the headers in the Master Document and automagically puts them into the Master Document in their proper places. Note that any header that has an `exclude` keyword will remain in the `include/` directory. `explode` does the inverse of `implode`; it takes all the content in the Master Document and blows it into individual files in the `include/` directory.

