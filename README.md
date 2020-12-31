# WindbreakerCSS

This is a css utility framework based on sass and inspired by [Bootstrap](https://getbootstrap.com/) and [TailwindCSS](https://tailwindcss.com/). It allows you to create utility classes easily by using sass mixins.

## Features

- One unified config
- State classes
- Group state classes
- Dark mode
- Multi property classes

## Roadmap

- Include example config
- Refactoring
- More comments

## Installation

```
npm install windbreakercss
```

or

```
yarn add windbreakercss
```

In your main scss file include the mixin:

```
@import '~windbreakercss/main';
```

## Usage

Create your utility config and pass it to the mixin `windbreakercss-utility` and it will genreate all the classes.

```scss
$separator: '\\:';

$config: (
    prefix: 'text-',
    dark-mode: true,
    group: hover active,
    property: color,
    states: hover focus active,
    values: (
        'white': #fff,
        'black': #000,
        'gray': #666
    )
);

@include windbreakercss-utility($config, $separator);
```

Example of the css that will be generated:

```css
.text-white {
    color: #fff;
}
...
```

You can wrap the mixin in a media query and add a query key if you would like to add responsive utilities, see example below:

```scss
$separator: '\\:';

$breakpoints: (
    sm: 576px,
    md: 768px,
    lg: 992px,
    xl: 1200px,
    2xl: 1600px
);

$config: (
    prefix: 'text-',
    dark-mode: true,
    group: hover active,
    property: color,
    states: hover focus active,
    values: (
        'white': #fff,
        'black': #000,
        'gray': #666
    )
);

@include windbreakercss-utility($config, $separator);
@each $bp, $size in $breakpoints {
    @media screen and (min-width: $size) {
        @include windbreakercss-utility($config, $separator, $bp);
    }
}
```

Example of the css that will be generated:

```css
.text-white {
    color: #fff;
}

@media screen and (min-width: 576px) {
    .text-white\:sm {
        color: #fff;
    }
    ...
}
...
```

To make things easier we've added another way to setup your configuration and create classes. Create a configuration variable with the `separator`, `breakpoints` and `utilities` then pass this to the `windbreakercss` mixin. See example below:

```scss
$config: (

    // Options

    separator: '\\:',

    // Responsive

    breakpoints: (
        sm: 576px,
        md: 768px,
        lg: 992px,
        xl: 1200px,
        2xl: 1600px
    ),

    // Utilities

    utilities: (

        // Color

        color: (
            prefix: 'text-',
            dark-mode: true,
            property: color,
            states: hover focus active,
            responsive: true,
            values: (
                'white': #fff,
                'gray': #666,
                'black': #000
            )
        )

    )

);

@include windbreakercss($config);
```

NOTE: We've added the property `responsive` to the utility above which is necessary to generate the responsive classes.

## Multi Property Utilities

If you have more than one property you would like to create a utility from then you can pass a map to the values instead. In the example below we've added `border-color` and `color` to the utility values.

```scss
$config: (
    prefix: 'text-',
    dark-mode: true,
    group: hover active,
    states: hover focus active,
    values: (
        'white': (
            color: #fff,
            border-color: #fff
        ),
        'black': (
            color: #000,
            border-color: #000
        ),
        'gray': (
            color: #666,
            border-color: #666
        )
    )
);
```

## Configuration

|Option|Description|Value|Example|
|---|---|---|---|
|`prefix`|What the class names should begin with|String|`text-`|
|`dark-mode`|Enable dark mode classes|true or false|`true`|
|`group`|Enable group state classes|Sass List|`group: hover focus`|
|`states`|Enable state classes|Sass List|`states: hover focus`|
|`property`|The CSS property which you would like to generate the classes for|String|`text-align`|
|`responsive`|Enable responsive classes. This will only work when using the `windbreakercss` mixin.|true or false|N/A|
|`values`|The values to generate the classes from|Map|See examples above|

## Classes

When you enable all the options this is the css which is generated. Make note of how the classes are named when enabling group, dark and responsive options.

```css
.active\:text-white:active,
.dark .active\:text-white\:dark:active,
.dark .focus\:text-white\:dark:focus,
.dark .group:active .group\:active\:text-white\:dark,
.dark .group:hover .group\:hover\:text-white\:dark,
.dark .hover\:text-white\:dark:hover,
.dark .text-white\:dark,
.focus\:text-white:focus,
.group:active .group\:active\:text-white,
.group:hover .group\:hover\:text-white,
.hover\:text-white:hover,
.text-white {
    color: #fff
}
...
```

## Purging

As you can see a lot of classes are generated so it's best to purge all the classes that aren't used in your website or app by using something like [PurgeCSS](https://github.com/FullHuman/purgecss).

## Notes

As you can see from some of the examples above we've added quotes around the color names, this is due to sass converting them to hex colors during processing.

## Contributing

1. Fork it!
2. Create your feature/hotfix branch: git checkout -b feature/my-super-feature
3. Commit your changes: git commit -m "Added some feature"
4. Push to the branch: git push origin feature/my-super-feature
5. Submit a pull request
