---
layout: post
title: "How to add epub to SuperMemo"
tags: macOS SuperMemo epub
---

## Intro

I'm a rather quick learner. However, I've been struggling with actually remembering the material for a longer period. Recently I've found [Anki](https://apps.ankiweb.net) and [SuperMemo](https://super-memo.com). Both applications are wonderful, but ultimately I've chosen SuperMemo, mainly because of its Incremental Reading feature. I'm still very early in my journey to learn using it proficiently. (SuperMemo is a Windows-only software that I'm using with [Parallels Desktop for Mac](https://www.parallels.com/eu/products/desktop/))

I'm planning to read a lot of books with [incremental reading](https://help.supermemo.org/wiki/Incremental_learning) but one of the first problems I've encountered is that SuperMemo doesn't support `.epub` natively. This short guide presents my findings on importing a book into SuperMemo.

## Main steps

1. Have a DRM free `epub`
2. Import it to Calibre and convert it from `epub` to `epub`
3. Convert to `html`
4. Clean up `html` and optimize images
5. Import to SuperMemo
6. Fix images, set up references
7. Split & Spread

## Have a DRM free `.epub`

In my experience, most of the `epub`s out there are suitable candidates for conversion. There are also ways to remove DRM from az epub, but due to my understanding, it's a grey area.

> I've wanted to import the official Swift book, _The Swift Programming Language_. This book is written by Apple and is available for free in the official Apple Books application. Exporting from Books is as simple as dragging the book to Finder. However, I had a small hiccup importing this book to Calibre because Apple uses a non-standard epub format. [Erica Sadun has found a simple solution to resolve this issue.](https://ericasadun.com/2015/10/28/converting-the-swift-programming-language-to-pdf-2/)

## Import to Calibre and convert

Calibre is a wonderful piece of software full of well-written features. I've found that even if I already have an `.epub` it might be worth converting it from `epub` to `epub` as it seems to be producing a somewhat cleaner result.

## Convert to `html`

Having an `.epub` then from Calibre, I'm using Pandoc, the universal document converter to convert it to html. Install Pandoc on macOS via Homebrew:

```sh
brew install pandoc
```

SuperMemo's support for styles and images is somewhat limited. Even if the `.epub` has some fancy fonts and colors it would probably look bad in SuperMemo. This is why we have to take a minimalistic approach during the conversion.

```sh
pandoc -M document-css=false -s --extract-media=Apple-TheSwift55ProgrammingLanguage Apple-TheSwift55ProgrammingLanguage.epub -o Apple-TheSwift55ProgrammingLanguage.html
```

This command upon success will generate an `html` file and a folder full of images.

## Clean up `html` and optimize images

Opening the `html` file in our favorite editor we can see a lot of noise/clutter that seamingly doesn't provide anything valuable. I got rid of them by searching for the following **regular expressions** and replacing them with an empty string:

- `( class="[^"]+")` and `(class="[^"]+")`
- `( id="[^"]+")` and `(id="[^"]+")`

As a final step before the import, I ran all images through [ImageOptim](https://imageoptim.com/mac) saving a little space without compromising on the quality.

## Import to SuperMemo

- Copy the `.html` file and the folder full of images to the root of `C:\`.
- Open the `.html` file with **Internet Explorer** (😢)
- Select all (`Ctrl + A`) and copy (`Ctrl + C`)
- Open SuperMemo, create a new article/topic (`Ctrl + N`) and paste (`Ctrl + V`)
- Remove anything that you deem unnecessary. (Table of Contents, Index, Acknowledgements, etc.)
- Using `Alt + Q` set the **Reference fields** accordingly. For me, the bare minimum is to set the
  - author
  - title
  - date

## Fix images, set up references

The book is imported, we could jump to the next, final step, however, the image references are currently pointing to the root where we've copied the folder. (e.g.: `C:/Apple-TheSwift55ProgrammingLanguage/`) If the images are removed from there they won't show up in SuperMemo again. I haven't found a standard solution for this, but I wanted to know these images in safety so I did the following.

- Depending on where SuperMemo is installed (for me it's `C:/SuperMemo`) inside `systems`, next to my collection I've created a new `Images` folder.
- I've copied all the images here: `C:/SuperMemo/systems/Images/Books/Apple-TheSwift55ProgrammingLanguage/`
- The last step is to open the book's `.HTM` file from SuperMemo in an external editor (`Ctrl + F9`) and replace the image paths.

```
file:///C:/Apple-TheSwift55ProgrammingLanguage/
file:///C:/SuperMemo/systems/Images/Books/Apple-TheSwift55ProgrammingLanguage/
```

> Later on it's recommended to gradually import the images into image components (that will be saved to the image registry) instead of leaving it in the html with reference.

> Parallels Desktop for Mac is amazing, somehow it integrates the Mac apps into Windows's interface. **I could set Visual Studio Code (Mac) as a [default app for `.HTM` file extension](https://support.microsoft.com/en-us/windows/change-default-programs-in-windows-10-e5d82cad-17d1-c53b-3505-f10a32e1894d) on Windows!**

## Split & Spread

The final step is to **split** the book up and schedule it for later review. I've mostly followed [Pleasurable Learning SuperMemo's great step-by-step video](https://www.youtube.com/watch?v=whRhtXMlaaM) to do this.

One of the most useful tricks I've learned is to NOT to use SuperMemo's default splitting as it messes up the topics a little. Instead specify a custom tag, that's usually `<h1>, <h2>, <h3>, <h4>` with `epub`s converted to `html`.

Splitting up this way is a little cumbersome, but doable in a relatively short time. After the split, we can **spread** the material according to our schedule. I'm still figuring out how to do this last step effectively.

## References

- [Anki](https://apps.ankiweb.net)
- [SuperMemo](https://super-memo.com)
- [Parallels Desktop for Mac](https://www.parallels.com/eu/products/desktop/)
- [Incremental leading](https://help.supermemo.org/wiki/Incremental_learning)
- [How to import and process an epub book with inline images using Pandoc: A Comprehensive guide](https://www.youtube.com/watch?v=whRhtXMlaaM&t=602s)
- [Converting the Swift Programming Language to PDF](https://ericasadun.com/2015/10/28/converting-the-swift-programming-language-to-pdf-2/)
- [Change default programs in Windows 10](https://support.microsoft.com/en-us/windows/change-default-programs-in-windows-10-e5d82cad-17d1-c53b-3505-f10a32e1894d)
- [Calibre](https://calibre-ebook.com)
- [Image Optim](https://imageoptim.com/mac)
