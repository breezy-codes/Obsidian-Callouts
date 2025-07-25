# Obsidian Custom Callouts

This repository contains a growing collection of custom callouts for use in Obsidian. These are defined using CSS snippets and are intended to improve visual clarity and structure when writing in Markdown.

![figure](https://briannalaird.com/_images/fig6.png)

## About

Obsidian supports callouts using syntax like `> [!note]`. While useful, the default styles are limited. This repository offers custom alternatives tailored for a cleaner, more expressive note-taking experience.

The styles follow a consistent colour scheme, use Lucide icons, and rely on core Obsidian features only, no plugins are required. Where possible, theme variables are used so that colours and layout integrate cleanly with existing themes.

My blog post on [Making Callouts in Obsidian](https://briannalaird.com/content/blog-posts/2025-06-17-making-callouts-obsidian.html) provides additional context and examples of how these callouts can enhance your notes.

## Theming

The callouts are themed to match the **Dracula** colour palette, which I use across my entire operating system. However, the structure is designed to be easily customisable.

At the top of the CSS, you’ll find the core palette defined using variables:

```css
:root {
    /* Dracula base palette */
    --drac-bg: #282A37;
    --drac-pink-rgb: 255, 121, 198;
    --drac-cyan-rgb: 132, 222, 240;
    --drac-purple-rgb: 189, 147, 249;
    --drac-green-rgb: 80, 250, 123;
    --drac-yellow-rgb: 241, 250, 140;
    --drac-red-rgb: 255, 85, 85;
}
```

You can modify these values to suit your preferred aesthetic without needing to alter the rest of the snippet.

The following block controls the overall appearance of each callout, ensuring the styling works cleanly across light and dark modes and maintains readability:

```css
/* Base background for all callouts */
.callout {
    background-color: var(--drac-bg) !important;
    position: relative;
    overflow: hidden;
}

/* Overlay using callout colour with opacity */
.callout::before {
    content: "";
    position: absolute;
    inset: 0;
    background-color: rgb(var(--callout-color), 0.3);
    pointer-events: none;
    z-index: 0;
    border-radius: var(--radius-s);
}

/* Ensure content stays above the overlay */
.callout > * {
    position: relative;
    z-index: 1;
}
```

This layered structure creates a subtle overlay effect based on the specific callout’s highlight colour while preserving text readability.

## Blank Callout

I added a blank (neutral) callout style that lets you group a section of text into a block without adding any special styling or title. This is helpful when you want to link to an entire section of text from other notes, using the neutral callout makes it easy to reference the whole block, while keeping the visual style minimal.

```css
/* === NEUTRAL (unstyled, no title, no icon, no styling) === */
.callout[data-callout="neutral"] {
    --callout-color: 255, 255, 255;
    --callout-icon: none;
    background-color: unset !important;
    box-shadow: none !important;
    border: none !important;
    padding: 0 !important;
    margin: 0 !important;
}

.callout[data-callout="neutral"]::before {
    display: none !important;
}

/* Hide the title bar completely */
.callout[data-callout="neutral"] .callout-title {
    display: none !important;
}
```

This allows you to create a clean, unstyled block that can be used for grouping content without any additional visual noise. It also hides the title bar and icon, making it truly neutral.

## Scrollable Callouts

You can also create scrollable callouts by adding some custom CSS, this is useful for large blocks of text or code that you want to keep within a defined area without breaking the flow of your notes.

```css
/* === SHARED SCROLL BEHAVIOUR === */
.callout[data-callout$="-scrollable"] .callout-content {
    --shown-line-count: 8;
    max-height: calc(var(--line-height-normal) * 1rem * var(--shown-line-count));
    overflow-y: auto;
}
```

Here you can define a callout that will only show a certain number of lines before it becomes scrollable. This is particularly useful for keeping your notes tidy while still allowing access to larger blocks of text. Whats cool is that you can combine this with any of the custom callouts, so long as you append `-scrollable` to the callout type.

### How to preserve styling for scrollable callouts

If you want to preserve the styling of a callout while making it scrollable, you just need to make a small adjustment to the CSS. For example, if you want to make a `note` callout scrollable, then you just need to add a `^` before the `=` in the selector:

```css
/* === NOTE === */
.callout[data-callout^="note"] {
    --callout-color: var(--drac-pink-rgb);
    --callout-icon: lucide-pencil;
    background-color: rgba(var(--drac-pink-rgb), 0.2);
}
```

Now you can use the scrollable version of the note callout like this:

```
> [!note-scrollable] Note
> This is a scrollable note callout. It will only show a limited number of lines before becoming scrollable.
```

## Features

* Custom callout types (e.g. `figure`, `quote`, `info-block`)
* Nesting-compatible styling
* Designed to look good in both light and dark mode
* Easily themeable via a single set of CSS variables
* Does not override the default callouts unless you want it to

## Usage

1. Open Obsidian settings and navigate to **Appearance**
2. Scroll to **CSS snippets**
3. Click **Open snippets folder**
4. Create a file (e.g. `custom-callouts.css`)
5. Paste the callout CSS into the file
6. Return to Obsidian and enable the snippet from the list

Once enabled, you can use callouts like:

```
> [!figure] Figure 1
> This is a stylised figure box.
```

Or a collapsible version:

```
> [!figure]- Figure 1
> This version can be expanded or collapsed.
```

## Notes

* All styles are built using the standard Obsidian callout system
* CSS variables ensure colours work across themes and can be easily swapped
* Dracula-themed by default, but adaptable to any style
* Tested with both light and dark modes

## Contributions

Contributions are welcome. Feel free to open an issue or submit a pull request with improvements, fixes, or new callout styles.
