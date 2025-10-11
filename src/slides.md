---
theme: uncover
paginate: true
class:
  - lead
  - invert
size: 16:9
style: |
  .small-text {
    font-size: 0.75rem;
  }
footer: "Philip Norton [hashbangcode.com](https://www.hashbangcode.com) [@hashbangcode](https://twitter.com/hashbangcode) [@philipnorton42](https://twitter.com/philipnorton42)"
marp: true

---

# Using Storybook To Preview Single Directory Components

<p class="centre">Philip Norton<br>
<small>DrupalCamp Scotland 2025</small></p>
<!-- Speaker notes will appear here. -->

---

# Philip Norton
- Developer at Code Enigma
- Writer at `#! code` (www.hashbangcode.com)
- NWDUG co-organiser
![bg h:50% right:40%](../src/assets/images/lily58.png)

<!--
- Doing Drupal for about 15 years.
- Programming in general for about 20 now.
- That is a picture of a Lily58 mechanical keyboard.
-->

---

<!-- _footer: "" -->
## Source Code
- Talk is available at:
<small>https://github.com/hashbangcode/drupal-storybook-talk</small> or via QR code.
- All code seen can be found at <small>[http://bit.ly/3W1bnu0](https://github.com/hashbangcode/drupal-storybook)</small>
- <small>[www.hashbangcode.com](https://www.hashbangcode.com)</small>

![bg h:50% right:40%](../src/assets/images/qr_slides.png)

<!-- 
- Scan the QR code for the talk repo. Which has links to all of the resources you need.
- There's also a Drupal and Storybook article on my site.
-->

---

# Using Storybook To Preview Single Directory Components

- Single Directory Components = SDC
- Storybook Drupal Module 
- Storybook 

---

# Why Single Directory Components?

Drupal Canvas is just around the corner.
Single Directory Components can be used with it.
Now is as good a time as every to get into SDC.

---

# 

- Introduction To Single Directory Components
- Installing the Storybook module
- Install Storybook

<!--

I've been looking at Storybook for a few years, but there's always been a disconnect between Storybook and Drupal. I could build the stories in Storybook, but I'd then have to rebuild them in Drupal.

That changed with SDC and is facilitied by the Storybook module.

-->


---

## Single Directory Components (SDC)

A _very_ short introduction.

---

## SDC

- Exist in a single directory

```
example_component.component.yml
example_component.css
example_component.js
example_component.twig
```

---

## SDC - *.component.yml

---

## SDC -  Props vs Slots

- Props - Properties that the component has.

- Slots - Objects that the component can render.

---

## SDC - *.twig

- The meat of the componenet.

--- 

## SDC - Other files

- Any *.css or *.js files will be automatically inclued as attachments.

---

# Storybook Module

---

## Storybook Module

Require and install the Storybook module

```
composer require drupal/storybook
```

```
drush en storybook
```

---

## Storybook Module

Permissions

```
drush role:perm:add anonymous 'render storybook stories'
```

---

# Drupal Setup

---

## Drupal Setup

Additional Setup
- development.services.yml
- turn on twig debug

---

# Storybook

---

## Storybook

- Install

---

## Building Storybook
- hosting storybook

---

## Running Storybook
- locally
- ddev setup

---

## Creating Stories
- stories.twig

---

# Workflow

A workflow for building a component.

- Create story for component
- Build (or run) storybook
- Preview story

---

# Demo

---

# Resources

[Drupal 11: Using Storybook To Preview Single Directory Components](https://www.hashbangcode.com/article/drupal-11-using-storybook-preview-single-directory-components)


---

## Questions?

- Slides: https://github.com/hashbangcode/drupal-storybook-talk

![bg h:50% right:40%](../src/assets/images/qr_slides.png)

---

## Thanks!

- Slides: https://github.com/hashbangcode/drupal-storybook-talk

![bg h:50% right:40%](../src/assets/images/qr_slides.png)
