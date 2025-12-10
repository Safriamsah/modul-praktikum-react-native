# Tutorial 3: Basic Components - View, Text, and More

## Introduction

React Native provides a set of core components that are the building blocks of any mobile app. Let's learn about the most essential ones!

## Core Components Overview

| Component | Description | Similar to (Web) |
|-----------|-------------|------------------|
| `View` | Container for other components | `<div>` |
| `Text` | Display text | `<p>`, `<span>` |
| `Image` | Display images | `<img>` |
| `ScrollView` | Scrollable container | `<div>` with scroll |
| `TextInput` | Input field | `<input>` |
| `Button` | Clickable button | `<button>` |

## 1. View Component 📦

`View` is the most fundamental component - it's a container that supports layout, styling, and touch handling.

### Basic Usage:

```javascript
import { View } from 'react-native';

export default function App() {
  return (
    <View>
      {/* Other components go here */}
    </View>
  );
}
```

### View with Styling:

```javascript
import { View, StyleSheet } from 'react-native';

export default function App() {
  return (
    <View style={styles.container}>
      <View style={styles.box}>
        {/* Nested views */}
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
  },
  box: {
    width: 100,
    height: 100,
    backgroundColor: 'blue',
  },
});
```

### Key Properties:
- `style`: Apply styles
- `onLayout`: Callback when layout changes
- Touch handlers: `onTouchStart`, `onTouchEnd`

## 2. Text Component 📝

`Text` is used to display text. ALL text must be wrapped in a `<Text>` component.

### Basic Usage:

```javascript
import { Text } from 'react-native';

export default function App() {
  return (
    <Text>Hello, World!</Text>
  );
}
```

### Text Styling:

```javascript
import { Text, StyleSheet } from 'react-native';

export default function App() {
  return (
    <View>
      <Text style={styles.title}>Title Text</Text>
      <Text style={styles.body}>Body text goes here</Text>
      <Text style={[styles.body, styles.bold]}>Bold body text</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#333',
  },
  body: {
    fontSize: 16,
    color: '#666',
  },
  bold: {
    fontWeight: 'bold',
  },
});
```

### Text Features:

```javascript
<Text numberOfLines={2}>
  This text will be truncated after 2 lines...
</Text>

<Text onPress={() => alert('Clicked!')}>
  Clickable Text
</Text>

<Text>
  You can <Text style={{fontWeight: 'bold'}}>nest</Text> text components
</Text>
```

### Key Properties:
- `numberOfLines`: Limit lines displayed
- `onPress`: Make text clickable
- `style`: Text styling (color, size, weight, etc.)

## 3. Image Component 🖼️

`Image` displays images from various sources.

### Local Images:

```javascript
import { Image } from 'react-native';

export default function App() {
  return (
    <Image
      source={require('./assets/icon.png')}
      style={{ width: 100, height: 100 }}
    />
  );
}
```

### Network Images:

```javascript
<Image
  source={{ uri: 'https://example.com/image.png' }}
  style={{ width: 200, height: 200 }}
/>
```

### Image with Resize Mode:

```javascript
<Image
  source={require('./assets/splash-icon.png')}
  style={{ width: 300, height: 200 }}
  resizeMode="cover"  // 'cover', 'contain', 'stretch', 'center'
/>
```

### Key Properties:
- `source`: Image source (local or network)
- `style`: Size and styling
- `resizeMode`: How image should be resized
- `onLoad`: Callback when image loads

## 4. Combining Components

Let's create a simple card component:

```javascript
import { View, Text, Image, StyleSheet } from 'react-native';

export default function App() {
  return (
    <View style={styles.container}>
      <View style={styles.card}>
        <Image
          source={require('./assets/icon.png')}
          style={styles.image}
        />
        <Text style={styles.title}>Expo JS Template</Text>
        <Text style={styles.description}>
          Learn React Native with this comprehensive tutorial series!
        </Text>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f5f5f5',
    alignItems: 'center',
    justifyContent: 'center',
  },
  card: {
    backgroundColor: 'white',
    borderRadius: 10,
    padding: 20,
    width: '90%',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.25,
    shadowRadius: 3.84,
    elevation: 5,
  },
  image: {
    width: 100,
    height: 100,
    alignSelf: 'center',
    marginBottom: 15,
  },
  title: {
    fontSize: 20,
    fontWeight: 'bold',
    marginBottom: 10,
    textAlign: 'center',
  },
  description: {
    fontSize: 14,
    color: '#666',
    textAlign: 'center',
  },
});
```

## 5. SafeAreaView Component 📱

`SafeAreaView` renders content within the safe area boundaries of a device (avoiding notches, status bars, etc.).

```javascript
import { SafeAreaView, Text, StyleSheet } from 'react-native';

export default function App() {
  return (
    <SafeAreaView style={styles.container}>
      <Text>This text respects safe areas!</Text>
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
  },
});
```

**Note:** Use `SafeAreaView` as your root component on iOS to avoid content being hidden by the notch or home indicator.

## Component Hierarchy Rules

1. **Text must be in Text**: All text content must be wrapped in `<Text>`
2. **Views can contain anything**: `<View>` can contain any component
3. **Images are self-closing**: `<Image />` not `<Image></Image>`
4. **Proper nesting**: Components must be properly nested (no overlapping tags)

## Practice Exercise

Try creating this layout:

```
+---------------------------+
|        [Logo Image]       |
|                           |
|      Welcome to App       |
|                           |
|  This is a description    |
|    of your awesome app    |
|                           |
+---------------------------+
```

### Starter Code:

```javascript
import { View, Text, Image, StyleSheet, SafeAreaView } from 'react-native';

export default function App() {
  return (
    <SafeAreaView style={styles.container}>
      <View style={styles.content}>
        {/* Add your Image here */}
        {/* Add your title Text here */}
        {/* Add your description Text here */}
      </View>
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
  },
  content: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
    padding: 20,
  },
  // Add more styles here
});
```

## Common Pitfalls to Avoid

❌ **Wrong:**
```javascript
<View>
  Some text here
</View>
```

✅ **Correct:**
```javascript
<View>
  <Text>Some text here</Text>
</View>
```

❌ **Wrong:**
```javascript
<Image source="./assets/icon.png" />
```

✅ **Correct:**
```javascript
<Image source={require('./assets/icon.png')} />
```

## What's Next?

Now that you know the basic components, let's learn how to style them properly!

**[← Back to Tutorial 2](./02-project-structure.md)** | **[Continue to Tutorial 4: Styling →](./04-styling.md)**

---

## Key Takeaways:

- ✅ `View` is your container component
- ✅ `Text` is required for all text content
- ✅ `Image` displays images with various sources
- ✅ Components can be nested and combined
- ✅ Always wrap text in `<Text>` components
- ✅ Use `SafeAreaView` for proper device boundaries

Great work! Keep going! 💪
