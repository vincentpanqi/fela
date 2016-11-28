# `<ThemeProvider>`

The `<ThemeProvider>` component passes a single theme object down to its children. It can be used multiple times to achieve different themes for different parts of the component tree. It uses React's [context](https://facebook.github.io/react/docs/context.html) to pass down the theme.
<br>
Nested themes automatically get combined if not explicitly prevented. This helps if you only want to change or add a single value without repeating the whole theming used before.
<br>
<br>
**The theme gets passed to both `createComponent` and `connect` via props.**

## Props
1. `theme` (*Object*): theme object with any information
2. `flat` (*boolean*): pass the theme as flat props to `createComponent` and `connect`
3. `overwrite` (*boolean*): overwrite all previous theme properties

## Example
```javascript
import React from 'react'
import { createComponent, ThemeProvider } from 'react-fela'

// we need to deconstruct the theme and add a default value
// to prevent errors while rendering static subsets
const text = ({ theme = {}, ...props }) => ({
  fontSize: theme.fontSize,
  color: theme.color,
  backgroundColor: theme.bgColor
})

const Text = createComponent(text)

const RenderTreeFragment = (
  <ThemeProvider theme={{ color: 'red', fontSize: 15 }}>
    <Text>I am red and 15px sized</Text>
    <ThemeProvider theme={{ color: 'blue' }}>
      <Text>I am blue and 15px sized</Text>
    </ThemeProvider>
    <ThemeProvider theme={{ bgColor: 'yellow' }}>
      <Text>I am red and 15px sized with a yellow background</Text>
    </ThemeProvider>
  </ThemeProvider>
)
```

#### Flat themes
The `flat` option basically bypasses the nested theme prop. It flattens the whole theme object into the props. Doing this you can use themes to set default props for a given subtree of your application. <br><br>
But **be aware**, that it might be overwritten by additionally passed props.
```javascript
import React from 'react'
import { createComponent, ThemeProvider } from 'react-fela'

const text = props => ({
  fontSize: props.fontSize,
  color: props.color
})

const Text = createComponent(text)

const RenderTreeFragment = (
  <ThemeProvider flat theme={{ color: 'red', fontSize: 15 }}>
    <Text>I am red and 15px sized</Text>
  </ThemeProvider>
)
```

#### Overwriting themes
The `overwrite` options help to prevent theme inheritance for nested `<ThemeProvider>` instances.

```javascript
import React from 'react'
import { createComponent, ThemeProvider } from 'react-fela'

const text = props => ({
  fontSize: props.fontSize,
  color: props.color || 'red'
})

const Text = createComponent(text)

const RenderTreeFragment = (
  <ThemeProvider flat theme={{ color: 'blue', fontSize: 15 }}>
    <Text>I am blue and 15px sized</Text>
    <ThemeProvider flat overwrite theme={{ fontSize: 20 }}>
      <Text>I am red and 20px sized</Text>
    </ThemeProvider>
  </ThemeProvider>
)
```