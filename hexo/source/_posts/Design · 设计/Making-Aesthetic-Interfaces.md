---
title: Making Aesthetic Interfaces
date: 2019-11-25 00:23:04
author: FadingWinds
img: 
categories: 
tags:
    - CT courses
    - UI
    - notes
summary: About how to design interfaces that are at least not ugly.
top:
hidden:
password:
---
Expert designers usually do not solve every problem, instead, they **reuse** solutions that worked before, aka. design patterns. 

[Material Design](https://material.io/) is always a good reference resource.

### Hierarchy

Decide the content to present, level of importance for each element and organize them.

**Steps** to implement visual hierarchy: collect -> group -> prioritize.

**Aspects**: scale, color, contrast, alignment, proximity.

**Components**: buttons, cards, lists, tabs, menus, etc.

### Typography

#### Definition

Two easily mixed up terms:

- A **typeface** is a style of type design which includes a complete scope of characters in all sizes and weight. e.g. Noto Sans SC. Usually could be divided into **Sans-serif** and **Serif**.
- A **font** is a graphical representation of text character usually introduced in one particular typeface, size, and weight. e.g. Multiple files of Noto Sans SC with different suffixes when you download it from Google Fonts.

#### Parameters

Consider the common small notebooks with four lines for each row used in elementary schools...it will help understand some of the concepts below.

- Mean line and baseline
  
  The two lines in the middle.

- X-height (x)
  
  The height between the two lines, which equals to the height of a lowercased *x*.

- Ascender and Descender
  
  The part above the mean line is ascender. Correspondingly, descender is the part below the baseline.

- Alignment
  
  Besides left, right and center, there's a **justified** alignment for English which could be seen used in newspaper. 

- Line length
  
  A perfect line length is like row numbers when writing codes.

- Tracking and Kerning
  
  Space between letters. The former refers to general ones while the latter refers to specific ones.

- Leading
  
  The spacing between the baselines of copy.

- Indent and Line Space
  
  Paragraph-wise concepts.


#### Rules

- One typeface for headline and one for body text is usually enough.
- Never use both indent and line space.
- In design, the standard leading is 120% the point size of the font.

### Color Palettes

#### Models

- **Additive / RGB**: use for digital mediums
- **Subtractive / CMYK**: use for paint and print

#### Concepts

- Chroma
  
  Purity of a color. A hue with high chroma has no black, white or gray added.

  Use hues with chromas that are either exactly the same or a few steps away from each other on the color wheel.

- Hue
  
  Just color, like purple, blue, etc.

- Saturation
  
  How a hue appears under particular lighting conditions.

- Value
  
  How light or dark a color is.

#### Schemes

- Monochromatic
  
  Different shades of a specific hue.

  Hard to make mistakes.

  Adding a strong neutral (e.g. black or white) could avoid boredom.

- Analogous
  
  Use colors located right next to each other on the color wheel.

- Complementary
  
  Combine colors from the opposite site of the color wheel.

  Provides high contrast.

- Triadic
  
  Based on three separate colors which are equidistant on the color wheel.

<br>
<br>




*Ref:*

https://designshack.net/articles/ux-design/google-material-design-everything-you-need-to-know/

https://uxplanet.org/typography-in-ui-guide-for-beginners-7ee9bdbc4833

https://uxplanet.org/color-theory-brief-guide-for-designers-76e11c57eaa