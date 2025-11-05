---
theme: uncover
paginate: true
class:
  - lead

size: 16:9
style: |
  img[alt~="centre"] {
    display: block;
    margin: 0 auto;
    max-width: 80%;
  }
  .smaller-text {
    font-size: 0.75rem;
  }
  .small-text {
    font-size: 0.5rem;
  }
  p {
    text-align: left;
  }
  p.centre {
    text-align: center;
  }
  .hidden-bullets li {
    list-style-type: none
  }
  .left {
    width: 50%;
    float: left;
  }
  .right {
    width: 50%;
    float: right;
  }
footer: "Philip Norton [hashbangcode.com](https://www.hashbangcode.com) [fosstodon.org@hashbangcode](https://fosstodon.org/@hashbangcode) [fosstodon.org@philipnorton42](https://fosstodon.org/@philipnorton42)"
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
- Doing Drupal for 15 years.
- Programming in general for 20.
- This is a picture of a Lily58 mechanical keyboard.
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
# What Is Storybook?

- Storybook is a frontend tool for creating UI components.
- Mature product with a plugin system.

---

![centre](../src/assets/images/storybook_jpl.png)

<!--

I've been looking at Storybook for a few years, but there's always been a disconnect between Storybook and Drupal. I could build the stories in Storybook, but I'd then have to rebuild them in Drupal.

That changed with SDC and is facilitied by the Storybook module.

Here is an example of Storybook used by the JPL.

-->
---

# Why Talk About Single Directory Components?

- An important part of modern Drupal theme development.
- Can be added to themes or modules.
- Integrates with **Drupal Canvas**.

<!-- 

Now is a good time to get into SDC.

-->
---

# Contents

- Introduction To **Single Directory Components**
- Installing the **Storybook Module**
- Installing and running **Storybook**

---

## Single Directory Components (SDC)

A _very_ short introduction.

<!--
From this point on SDC will be Single Directory Components.
-->

---
<!-- _footer: '' -->
## SDC

![centre](../src/assets/images/edinburgh_university.png)

---
<!-- _footer: '' -->
## SDC

![centre](../src/assets/images/edinburgh_university_blocks.png)

---

## SDC

- They exist in a single directory<br>(hence the name).

```
example_component/
    example_component.component.yml
    example_component.css
    example_component.js
    example_component.twig
```
<!--
This directory can exist in a theme or module.
Let's look at each of the files.
-->
---

## SDC - *.component.yml

- The `*.component.yml` file is used to define the structure of the component.

```yml
name: My Component

props:
  type: object
  properties:
    # ...

slots:
  # ...
```

<!--

Note that when you create a component in a module this file is heavily validated. In the theme, you can 'get away' with a bit more.

-->
---
<!-- _footer: '' -->
## SDC - *.component.yml

- `props` have defined properties with set data types.

```yml
props:
  type: object
  properties:
    extra_classes:
      type: ['string', 'null']
      title: Extra classes
      examples:
        - extrawide, dark
```

- `{{ extra_classes }}` can now be used in the template.

---

## SDC - *.component.yml

- `slots` are for printing content and othe SDCs.
- They are much simpler to define.

```yml
slots:
  body:
    title: "Body copy."
```

- `{{ body }}` is now available in the template.

---

## SDC - Props vs Slots

- `props` - Properties or simple elements that the component has. Title, classes, menu depth, display options, etc.
- `slots` - Objects or more complex elements that the component can render. Body copy, images, nested SDCs etc.

---

## SDC - *.component.yml

- Use `libraryOverrides` to inject Drupal libraries into your SDC.

```yml
libraryOverrides:
  dependencies:
    - core/drupal
    - core/once
```

---
<!-- _footer: '' -->
## SDC - *.twig

- A normal twig template that renders the defined structure of the component.

- Example `slot` in a `*.component.yml` file:

```yml
slots:
  body:
    title: Body copy
    description: The HTML body copy of the element.
```

Twig template:

```html
{{ body }}
```

---

## SDC - *.twig

- You should probably add `slots` using the `block` Twig tag.

```twig
{% block body %}
  {{ body }}
{% endblock %}
```

- This allows you to override the value easily.
- You can inject the value in Storybook without having to add `|raw` to your Twig template.

---

## SDC - Other files

- Any `*.css` or `*.js` files will be automatically inclued as attachments.
- The libraries attached to the theme will also be included when the SDC is rendered in Storybook.

---

## SDC - Other files

- Template and CSS can reference files inside the SDC.

CSS:
```css
background-image: url('./assets/background.png');
```

Image:
```twig
<img src="/{{ componentMetadata.path }}/assets/image.png">
```
<!--
The componentMetadata object contains some information about the component.
In this case we are using the .path property that contains the current path of the SDC.
-->
---

## SDC - Usage

- With the SDC created we can now add it to our Drupal template.

```twig
{{ include('my_theme:example_component', {
  extra_classes: 'some-class',
  body: content.body,
}) }}
```

---

## SDC - Usage

- An example of rendering an Author content type via an SDC.

```twig
{{ include('my_theme:author', {
  author_url: url,
  name: label, 
  bio: content.field_author_short_bio,
  avatar: content.field_author_avatar
}) }}
```

---

# Storybook Module

---

## Storybook Module

- Require and install the Storybook module

```
composer require drupal/storybook
drush en storybook
```

---

## Storybook Module - Permissions

- Ensure that the correct permissions are set.

```
drush role:perm:add anonymous 'render storybook stories'
```

---

<!-- _footer: '' -->
## Storybook Module

- The whole mid section here is an iframe that points to Drupal.
- The route is provided by the Storybook module.

![centre](../src/assets/images/storybook_iframe.png)

---

## Storybook Module - Stories

- You need a `*.stories.twig` file for every component you want to preview.
- This file contains a number of examples of the SDC in use.
- You can also add additional markup or other components to the story.

---
<!-- _footer: '' -->
## Storybook Module - Stories

```twig
{% stories exampleComponent with { title: 'Components/Example Component' } %}

  {# Define story name and args: #}
  {% story default with {
    name: '1. Default',
    args: {
      example_property: 'Example property',
      example_slot: 'Example slot',
    }
  } %}

  {# Embed component: #}
  {% embed 'my_theme:example_component' %}
  {% endembed %}

  {% endstory %}
  
{% endstories %}
```

---

## Storybook Module - Stories

```twig
{% stories exampleComponent with { title: 'Components/Example Component' } %}
```

![centre](../src/assets/images/storybook_path.png)

---

## Storybook Module - Stories

```twig
{% stories exampleComponent with { title: 'Components/Examples/Example Component' } %}
```

![centre](../src/assets/images/storybook_path_deeper.png)

---
<!-- _footer: '' -->
## Storybook Module - Stories

- Any args passed to the story are editable properties.

```twig
  {% story default with {
    name: '1. Default',
    args: {
      example_property: 'Example property',
      example_slot: 'Example slot',
    }
  } %}
```

![centre](../src/assets/images/storybook_args.png)
<!-- 

The data types of the args are pulled from the *.component.yml file.

-->
---

## Storybook Module - Stories

- Use `include` for simple components with only `props`.

<div>
<div class="left">
<p class="small-text">example_component.twig</p>

```twig
<div>
  <h2>{{ title }}</h2>
</div>
```

</div>

<div class="right">
<p class="small-text">example_component.stories.twig</p>

```twig
  {% story default with {
    name: '1. Default',
    args: {
      title: 'The Title'
    }
  } %}
  {{ include('my_theme:example_component', {  
    title
  }) }}
```
</div>
</div>
<!-- 
Here, the title prop is being injected into the template.
-->

---
<!-- _footer: '' -->
## Storybook Module - Stories

- Use `embed` with more complex components that have `slots`.

<div>
<div class="left">
<p class="small-text">example_component.twig</p>

```twig
<div>
{% block body %}
  {{ body }}
{% endblock %}
</div>
```
<!-- 
Note that in this case any `props` we have defined will be passed to the template automatically.
-->
</div>

<div class="right">
<p class="small-text">example_component.stories.twig</p>

```twig
  {% story default with {
    name: '1. Default',
    args: {
      body: '<p>Some body copy.</p>'
    }
  } %}
  {% embed 'my_theme:example_component' %}
    {% block body %}
      {{ body|raw }}
    {% endblock %}
  {% endembed %}
```

</div>
</div>

---
<!-- _footer: '' -->
## Storybook Module - Stories

- You can add multiple stories for your component.
 
 <div class="smaller-text">

```twig
  {% story wrapped with {
    name: '2. Wrapped',
    args: {
      example_property: 'Example property',
      example_slot: '<p>Example slot with lots of content.</p>',
    }
  } %}
    <div style="border:1px solid red; width: 365px; height: 500px;">
    {% embed 'my_theme:example_component' %}
      {% block example_slot %}
        {{ example_slot|raw }}
      {% endblock %}
    {% endembed %}
    </div>
  {% endstory %}
```
</div>

<!--
This shows a second story where the element in question is wrapped in a restrictive red box.
You can add stories to show different content for your component, but also think about the surrounding elements on the page. Adding in some example markup to show the component in situ can be really useful.
-->
--- 

## Storybook Module - Drush

- Storybook doesn't understand `*.stories.twig` files.
- They need converting into `*.stories.json`.
- This Storybook module comes with a couple of Drush commands to do this.

---

## Storybook Module - Drush

- Convert all stories in your site.

```
drush storybook:generate-all-stories
```

- This command will only generate the file if nothing has changed.

---

## Storybook Module - Drush

- Watch for changes in your stories and re-generate the files.

```
watch --color drush storybook:generate-all-stories
```

---

## Storybook Module - Drush

- Convert a single story.

```
drush storybook:generate-stories themes/custom/my_theme/components/author/example_component.stories.twig
```

---

## Storybook Module - Stories

- Don't commit the `*.stories.json` files, they are created for the current site and will contain URLs for that site.

---

# Drupal Setup

- Some settings are needed to allow Storybook to connect to Drupal.

---
<!-- _footer: '' -->
## Drupal Setup - CORS Prevention

- Add these rules to `development.services.yml` file.

```yml
parameters:
  storybook.development: true
  cors.config:
    enabled: true
    allowedHeaders: [ '*' ]
    allowedMethods: [ '*' ]
    allowedOrigins: [ '*' ]
    exposedHeaders: false
    maxAge: false
    supportsCredentials: true
```
<!--
CORS = Cross-Origin Resource Sharing 
Prevents websites from serving content to sites that they haven't previouly agreed to.
This creates a problem when we do want to share content.
-->
---

## Drupal Setup - CORS Prevention

- Add a reference to the services file from the `settings.php` file.

```php
$settings['container_yamls'][] = DRUPAL_ROOT . '/sites/development.services.yml';
```

- Don't put these rules on your production instance!

---

## Drupal Setup - Twig Debug

Turn on the theme development settings (via Drush).

```yml
drush state:set twig_debug 1
drush state:set twig_cache_disable 1
drush state:set disable_rendered_output_cache_bins 1
```

---

<!-- _footer: '' -->
## Drupal Setup - Twig Debug

![centre](../src/assets/images/twig_dev_mode.png)

---

# Storybook

---

## Storybook - Install

- Install Storybook in a separate directory, away from your Drupal root.

```
mkdir tests && cd tests
npm init -y
npx storybook init --type server
```

---


## Storybook - Install

- This will give you a directory containing the following.

```
.storybook
stories
node_modules
package.json
package-lock.json
```
<!-- 
- .storybook is the storybook configuration directory.
- stories contains some example stories.
- The other directories are standard node directories.
-->

---
## Storybook - Configure

- Edit the `.storybook/main.js` file to point Storybook at your stories.

```json
"stories": [
    "../../web/themes/custom/my_theme/components/**/*.stories.@(json|yaml|yml)"
  ],
```
<!--
Remember that you can have stories in themes and modules, so you can have multiple lines here pointing at different directories.
The modules and themes don't need to be active, as long as they have a *.stories.twig
-->

---

## Running Storybook

- Storybook is a JavaScript application that can be run on its own.

```
npm run storybook
```

- This will open a browser window with the address http://localhost:6006.

---
<!-- _footer: '' -->
## Running Storybook - DDEV

- If you are using DDEV then you can run Storybook from inside the web container, with some config.

```yml
web_extra_exposed_ports:
  - name: storybook
    container_port: 6006
    http_port: 6007
    https_port: 6006
web_extra_daemons:
  - name: node.js
    command: "tail -F package.json > /dev/null"
    directory: /var/www/html/tests
```
<!--
Add this to your .ddev/config.yml file and restart ddev.
-->
--- 

## Running Storybook - DDEV

- To stop DDEV complaining you may want to add <br> `--no-open` flag to the relevant line in the `package.json` file to prevent the browser opening.

```
  "scripts": {
    "storybook": "storybook dev -p 6006 --no-open",
    "build-storybook": "storybook build"
  },
```

---

## Building Storybook

- Storybook can also be built as a static website.

```
npm run build-storybook
```

- The static site can be deployed and served as a site.
- This means that you can host Storybook and demo your SDCs to clients before they are written into Drupal template.

---

## Building Storybook - Hosting

- Use the following nginx configuration.

```nginx
location ^~ /storybook-static {
  add_header "Access-Control-Allow-Origin" *;
  add_header "Access-Control-Allow-Methods" "GET, POST, OPTIONS";
  disable_symlinks off;
  alias /your/project/directory/tests/storybook-static;
  index index.html;
}
```
---
<!-- _footer: "" -->

## Building Storybook - Hosting

- Extra configuration is needed for external files in your Drupal site.

```nginx
location @rewrite {
    rewrite ^ /index.php;
}

location ~* \.(js|css|woff|woff2|ttf)$ {
    add_header Access-Control-Allow-Origin '*';
    add_header X-Content-Type-Options nosniff;
    try_files $uri @rewrite;
    expires -1;
    log_not_found off;
}
```


---

# Workflow

- A workflow for building a component.

  - Start building your SDC
  * Create a story for the component
  * Build (or run) Storybook
  * Develop the SDC inside Storybook 
  * Once the SDC is complete, add it to your Drupal template.

---

# Demo

---

# Tips

* Look for CORS errors.
* If you only see a blank screen, check Drupal logs.
* Don't commit your `*.stories.json` files, build them on deploy.
* Storybook loves to cache the sidebar.
* Storybook has lots of plugins, check a few of them out.

<!-- 
CORS errors will likey be your biggest headache.
Make notes as you solve them as you can use these notes on other projects.
-->
---

# Tips

* Be aware that Drupal will wrap some things with extra elements. That might throw off your positioning and margins.
* Understand `props` and `slots`.
* Setup a styleguide storybook.
* Don't abstract too much with SDC.
* Don't try to cover too much with SDC.

---

# Alternatives

---
# Alternatives

<!-- _footer: '' -->
- [SDC - Component library](https://www.drupal.org/project/sdc_component_library)

![centre](../src/assets/images/drupal_sdc_preview_created.png)

<!--
A good alternative if you don't want to muck about with storybook.
Plus, comes with an accessibility scanner.
-->

---
<!-- _footer: '' -->
# Alternatives

- [Single Directory Components Styleguide](https://www.drupal.org/project/sdc_styleguide)

![centre](../src/assets/images/sdc_styleguide.png)

<!--
A bit unstable in places.
-->
---

# Alternatives

- [Storybook SDC Addon](storybook.js.org/addons/storybook-addon-sdc)
- Plugin for Storybook that allows you to build SDCs as native Storybook stories.
<!--

This seems a bit backwards to me.

Whilst theoretically possible, no SDC is an island. You will find that Drupal has injected some markup or styles into your theme.

-->
---

# Resources

- [Storybook](https://storybook.js.org/)
- [Storybook Module](https://www.drupal.org/project/storybook)
- [Drupal Documentation on SDC](https://www.drupal.org/docs/develop/theming-drupal/using-single-directory-components)
- [Drupal 11: Using Storybook To Preview Single Directory Components](https://www.hashbangcode.com/article/drupal-11-using-storybook-preview-single-directory-components)

---

## Questions?

- Slides: https://github.com/hashbangcode/drupal-storybook-talk

![bg h:50% right:40%](../src/assets/images/qr_slides.png)

---

## Thanks!

- Slides: https://github.com/hashbangcode/drupal-storybook-talk

![bg h:50% right:40%](../src/assets/images/qr_slides.png)
