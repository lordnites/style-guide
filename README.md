# Funkhaus Programming Style Guide

This is the Funkhaus programming style guide and WordPress template theme.

The purpose of this style guide is to get you started in our Vuehaus system and to get you acquainted with our coding conventions, styles, and best practices.

## Table of Contents

1. **Setting Up**
   - [Theme Setup](#theme-setup)
   - [JavaScript Setup](#javascript-setup)
   - [CSS Setup](#css-setup)
   - [Markup Guidelines](#markup-guidelines)
   - [Plugins](#plugins)
   - [Template Routing](#template-routing)
   - [Image Sizes](#image-sizes)
   - [Open Graph Tags](#open-graph-tags)
1. **Front-End Guidelines**
   - [Whitespace](#whitespace)
   - [Z-Index](#z-index)
   - [SVGs](#svgs)
   - [Mobile & Responsive](#mobile--responsive)
1. **Working with Wordpress**
   - [Custom Functions](#custom-functions)
   - [Enqueue Scripts](#enqueue-scripts)
   - [Menus](#menus)
   - [Metaboxes](#metaboxes)
   - [Loops](#loops)
   - [Admin & Login Pages](#admin-login)
1. **Common Elements**
   - [Vimeo](#vimeo)
   - [Galleries](#galleries)
   - [Slideshows](#slideshows)
   - [Contact Pages](#contact-pages)
   - [Advanced Pagination](#advanced-pagination)
   - [GeoIP Detect](#geoip-detect)
1. **Testing**
   - [Basic Checklist](#basic-checklist)
   - [Code Checklist](#code-checklist)
1. **Reference**
   - [HTML and CSS guides](#html-and-css-guides)
1. **[To Do List](#to-do-list)**

---

## Setting Up

Below are some general guidelines to follow while you get started:

### Theme Setup

The theme directory name should be something short and indicative of the client's name, with the year the theme was built. For example, `bullitt2018`, or `storyattic2018`. We do it this way so we can quickly tell how old a code base is, and easily build a new theme years later, and not worry about local caching of files.

#### Steps To Make a New Site

1. Install Flywheel Local // Drew will fill this in
1. Open Flywheel Local and pull down the website you want.
1. Clone Vuehaus into the `/ClientNamedWordpress/app/public/wp-content/themes` folder you just pulled from Flywheel. Once you've done that, rename it using our naming convention of `clientname2018`.
1. `cd` into that folder and run `npm install` to get all the npm modules.
1. Run `npm run dev` to get the Vuehaus theme running.
1. On Flywheel, run the website by pressing the play button to get the Wordpress install running locally. Open `http://clientname2018.local/wp-login` or use the Flywheel `ADMIN` button to get to the Wordpress backend. Log in.
1. Navigate to `Appearance -> Themes` in the Wordpress dashboard and enable the `clientname2018` theme you installed earlier and then it'll prompt you to install the required plugins that accompany Vuehaus. Install these plugins and activate them.

#### Common Next Steps

1. Set up your Wordpress view/page structure with the `Nested Pages` plugin and then replicate that structure in `/ClientNamedWordpress/app/public/wp-content/themes/clientname2018/functions/router.php`
1. Create a Vue view for each of the required pages in your application's structure in `/ClientNamedWordpress/app/public/wp-content/themes/clientname2018/src/views`

#### Vuehaus File Structure

Generally, the structure of your theme directory will look like this:

```
clientname2018
	/functions -> php utilities including routing and rest_easy
	/static -> This folder contains all the relevant backend Wordpress styling and images
	/src
		/components -> Where all the sites reusable components live
		/styles -> Where global style files like _vars and _base live
		/svgs
		/utils -> Where reusable helper/utility scripts live like the Vue router and store
		/views -> Where the apps pages live
		App.js -> Controller for all the pages and components where you handle any top level logic
		main.js -> Where all the includes and utilities are tied into the app
	.deploy.dev.js -> This file allows you to deploy your Vuehaus theme to a flywheel live site with npm run deploy
```

#### File Naming

All view and component files should be located in the `/views` and `/components` directories. Common examples:

- Folders are always lowercase names. Vue files are always Title Cased.
- `views/GridDirector.vue` -> This will be interpreted in KebabCase by Vue. E.g. `GridDirector` becomes `grid-director`.
- Components inside nested folders will have the folder name prepended. E.g. a file in `/components/block/Content.vue` becomes `<block-content>`.

Icons and image assets should be named describing what they are, not what they represent:

**Good:**

- `icon-envelope.svg`
- `icon-arrow-left.svg`

**Bad:**

- `icon-email.svg`
- `icon-previous.svg`

---

### HTML & CSS

#### General Rules

- Class names should all be lower case, with hyphens as spaces. So use `work-grid`, not `WorkGrid` or `work_grid`.

- For class names use is-{state} and has-{feature} or not-{type}. Like is-active, is-opened, has-video, not-case-study, is-active.

- Don't use ID's. Classes can be used interchangeably and aren't rigid like the convention for using ID's.

- Don't use element selectors unless there's no other alternative. Always use class selectors. This make our selectors descriptive and therefore readable.

- Don't use global CSS resets. Generally it becomes more annoying to add back in the default browser styles than it is to remvoe them.

- Don't use `:before`, `:after`, and `content` to add in HTML or text. These should only be used sparingly to add dynamic styling.

- Use a central z-index location for major structural components, increment by 100’s.

- For component level z-index set to 0 and increment by 10

- Use positioning sparingly. If you can reasonably make something look and function correctly with the default static positioning, do that.

- Avoid using wrappers for convenience. If you can reasonably make something look and function correctly without a wrapper, don't add it just for convenience.

- Alternate layouts, hover states, and breakpoints should all be at the bottom of your CSS: ```.contact-page {
  // Alternate Layout
  &.alternate {

      	}
      	// Hover State
      	@media (hover: hover) {

      	}
      	// Breakpoints
      	@media #{$lt-phone} {

      	}

  }```

- Transitions should generally be kept in the global \_transitons.scss file.

- When using scss if you’re going more than 2 levels deep, question yourself.

- Never use background-images, use object-fit with src-set instead. This keeps the images looking optimal.

- We care about Firefox, Chrome and Safari.

##### Markup Guidelines

All HTML should be concise, semantic, and use as few wrapping elements as possible. Here are a few strict guidelines we follow for specific tags:

- `H1`: Should almost never be used, H1 is reserved for the site title only.
- `H2`: This should be the page title for each page, i.e. "contact" or "Directors."
- `H3`: Should work well as a secondary headline on any page with body copy. In the design phase an H3 style will be mocked up inside a blog post.
- `div`: Should be the main building block of the site.
- `address`: Any street address areas should be wrapped in this tag.
- `HTML5 tags`: We don't use these very often, but feel free to use them in a way that makes sense.

#### Preprocessors and Resets

When it comes to CSS, our current philosophy is to avoid using any frameworks, preprocessors, or resets of any kind. We have a very minimal amount of boilerplate code to start with in our `_base` stylesheet.

#### CSS naming conventions

We like to use a semantic approach to CSS up to a certain point. The idea is for you to be able to read the CSS and get some idea of what the HTML would look like. In most cases we avoid making extremely general classes, doing things like `.three-col`, `.blue_font`, or `.largeText` is bad. We'd rather things be intuitive and easy to read when going through the stylesheet.

Here are some base style names we commonly use:

- `.section`
- `.detail`
- `.title`
- `.credit`
- `.meta`
- `.browse`
- `.component`
- `.nav`
- `.grid`
- `.block`
- `.image`
- `.grid`
- `.panel`
- `.menu`
- `.overlay`
- `.entry`

There are a few class names that should _always_ be used in certain situations:

- `.entry`
  - every time you use `wp-content` fh-component it should have this class.
- `.grid` or `.grid-X` where **X** is the type of grid being used.
  - any time you are displaying a loop of posts or pages, wrap the loop in this class
- `.block`
  - Use this for individual elements within a grid.
- `.template-name` where **template-name** describes what template is being used. E.g. `.contact-page` or `.video-detail`, describing what page is being made.

#### Style Sheet Struture

Our preferred approach with CSS is to structure it similar to the sites' visual structure. So things that appear at the top of the browser window, should be higher in the CSS document. This makes it faster to find a section of code, based on the visual hierarchy of the site.

This also applies to individual elements too, so when defining any elements try to keep the visual hierarchy in mind. For example:

**Example Markup**

```html
<div class="video-detail">
	<div class="media-player">
		<iframe>
	</div>
	<div class="meta">
		<h2 class="title">Steven Spielberg</h2>
		<h3 class="credit">Director</h3>
	</div>
	<div class="entry">
		Text in here.
	</div>
</div>
```

**Example CSS**

```css
.video-detail {
  margin: auto;
  max-width: 1100px;

  .media-player {
    height: 500px;
  }
  .meta {
    margin: auto;
    max-width: 800px;
  }
  .entry {
    margin-top: 50px;
  }
}
```

### Image sizes

When handeling images in WordPress, you'll generally need to define a set of sizes in `/functions/images.php` and then another set under `Settings > Media` in Wordpress.

```
set_post_thumbnail_size( 960, 540, true ); // Normal post thumbnails
add_image_size( 'social-preview', 600, 315, true ); // Square thumbnail used by sharethis and facebook
add_image_size( 'mobile', 750, 0, false );
add_image_size( 'mid-size', 960, 0, false );
add_image_size( 'desktop', 1200, 0, false );
add_image_size( 'fullscreen', 1920, 1080, false ); // Fullscreen image size
```

As for the sizes in Settings > Media, we set the width for both "medium" and "large" to be the maximum content width for the site. The height of "medium" will be a 16:9 ratio of that width, and the height of "large" will be unlimited (so set it to 0). Thumbnails we generally set to something small and square, like 250px X 250px.

Assuming that the max-width of .entry is 1200px, you'd have the following media settings in the backend:

```
Thumbnail size
Width: 250px | Height: 250px | Crop: True

Medium size
Max Width: 1200px | Max Height: 675px

Large size
Max Width: 1200px | Max Height: 0
```

#### Break Points

These are some typical breakpoints you might have declared as variables in your `_vars.scss`

```css
// Breakpoints
$gt-cinema: "only screen and (min-width: 1800px)";
$lt-desktop: "only screen and (max-width: 1200px)";
$lt-tablet: "only screen and (max-width: 900px)";
$lt-phone: "only screen and (max-width: 750px)";
$lt-phone-landscape: "only screen and (max-width: 750px) and (orientation: landscape)";
```
