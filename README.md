# select-in-order

## Reusable component
```vue
<select-in-order />
```

**Desktop:**
![DEKSTOP](.\static\images\markdown\menu.png)

**Mobile:**
![DEKSTOP](.\static\images\markdown\menu-mobile.png)

## Adding custom sequence topic

If you want to add your own sequence game, create an array containing image sources (URLs). The order of the image sources provided will be the basis for the correct order/sequence in the game.

```vue
<template>
  <select-in-order :sequence="testSequence"/>
</template>

<script>
export default {
  data() {
    return {
      testSequence: [
        "https://www.astro-seek.com/seek-images/fb/seek_fb_moon_new_1200.jpg",
        "https://i.redd.it/8o9j69nh19a11.jpg",
        "https://images.fineartamerica.com/images-medium-large-5/first-quarter-moon-nasas-scientific-visualization-studioscience-photo-library.jpg",
        "https://live.staticflickr.com/3874/19223685325_0b9a3b0ab9.jpg",
        "https://live.staticflickr.com/3874/19223685325_0b9a3b0ab9.jpg",
      ]
    }
  }
}
</script>
```
**_NOTE:_**  If the number of image sources in the provided array is less than 2 or more than 8, your custom topic will not be rendered.


**Output:**
![DEKSTOP](.\static\images\markdown\menu-custom.png)

As you can see, a new topic has been added to the options menu. By default, it has its own default topic `thumbnail` and topic `title` if not provided in the reusable component.

## Setting up title
You can pass your desired `title` inside the title prop to change the title of your custom topic:

```vue
<select-in-order :sequence="testSequence" title="Sample topic"/>
```

**Output:**

![DEKSTOP](.\static\images\markdown\title.png)

## Setting up thumbnail

If you want to change the thumbnail of your topic, simply pass an image source in the `thumbnail` prop:

```vue
<select-in-order ... thumbnail="\images\markdown\evolution.jpg"/>
```

**Output:**

![DEKSTOP](.\static\images\markdown\thumbnail.png)

## Removing default topics

If you only want your custom topic to be shown in the options menu, you can add the `hide-options` attribute to the component:

```vue
<select-in-order ... hide-options/>
```
**Output:**

![DEKSTOP](.\static\images\markdown\hide-options.png)
