# Tutorial 4: Styling Components with StyleSheet

## Introduction

React Native uses JavaScript objects for styling, similar to CSS but with some key differences. Let's master styling!

## StyleSheet API

The `StyleSheet` API provides an abstraction similar to CSS stylesheets.

### Basic Syntax:

```javascript
import { View, Text, StyleSheet } from 'react-native';

export default function App() {
  return (
    <View style={styles.container}>
      <Text style={styles.text}>Styled Text</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f0f0f0',
  },
  text: {
    fontSize: 20,
    color: '#333',
  },
});
```

### Why Use StyleSheet.create()?

1. **Performance**: Styles are created once and reused
2. **Validation**: Catches errors during development
3. **Organization**: Keeps styles together at the bottom of the file

## Layout with Flexbox

React Native uses Flexbox for layout - it's the primary way to arrange components.

### Flex Container Properties:

```javascript
const styles = StyleSheet.create({
  container: {
    flex: 1,                    // Take up available space
    flexDirection: 'column',    // 'row', 'column', 'row-reverse', 'column-reverse'
    justifyContent: 'center',   // Main axis alignment
    alignItems: 'center',       // Cross axis alignment
    flexWrap: 'wrap',           // Allow wrapping
  },
});
```

### Flex Direction:

```javascript
// Column (default) - vertical
flexDirection: 'column'
// [ Item 1 ]
// [ Item 2 ]
// [ Item 3 ]

// Row - horizontal
flexDirection: 'row'
// [ Item 1 ] [ Item 2 ] [ Item 3 ]
```

### Justify Content (Main Axis):

```javascript
justifyContent: 'flex-start'    // Start of container
justifyContent: 'flex-end'      // End of container
justifyContent: 'center'        // Center
justifyContent: 'space-between' // Space between items
justifyContent: 'space-around'  // Space around items
justifyContent: 'space-evenly'  // Even space
```

### Align Items (Cross Axis):

```javascript
alignItems: 'flex-start'    // Start
alignItems: 'flex-end'      // End
alignItems: 'center'        // Center
alignItems: 'stretch'       // Stretch to fill
alignItems: 'baseline'      // Align baselines
```

## Common Style Properties

### Dimensions:

```javascript
const styles = StyleSheet.create({
  box: {
    width: 100,           // Fixed width
    height: 100,          // Fixed height
    width: '50%',         // Percentage
    minWidth: 50,         // Minimum width
    maxWidth: 300,        // Maximum width
  },
});
```

### Spacing:

```javascript
const styles = StyleSheet.create({
  box: {
    margin: 10,           // All sides
    marginTop: 5,         // Specific side
    marginHorizontal: 10, // Left and right
    marginVertical: 15,   // Top and bottom
    
    padding: 10,          // All sides
    paddingLeft: 5,       // Specific side
    paddingHorizontal: 10,// Left and right
    paddingVertical: 15,  // Top and bottom
  },
});
```

### Colors:

```javascript
const styles = StyleSheet.create({
  box: {
    backgroundColor: '#3498db',   // Hex
    backgroundColor: 'blue',      // Named color
    backgroundColor: 'rgb(52, 152, 219)',
    backgroundColor: 'rgba(52, 152, 219, 0.5)', // With opacity
  },
});
```

### Borders:

```javascript
const styles = StyleSheet.create({
  box: {
    borderWidth: 1,
    borderColor: '#333',
    borderRadius: 10,
    borderStyle: 'solid',    // 'solid', 'dotted', 'dashed'
    
    // Individual sides
    borderTopWidth: 2,
    borderRightColor: 'red',
    borderBottomLeftRadius: 5,
  },
});
```

### Shadows (iOS):

```javascript
const styles = StyleSheet.create({
  box: {
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.25,
    shadowRadius: 3.84,
  },
});
```

### Elevation (Android):

```javascript
const styles = StyleSheet.create({
  box: {
    elevation: 5,  // Shadow on Android
  },
});
```

### Platform-Specific Shadows:

```javascript
import { Platform } from 'react-native';

const styles = StyleSheet.create({
  box: {
    ...Platform.select({
      ios: {
        shadowColor: '#000',
        shadowOffset: { width: 0, height: 2 },
        shadowOpacity: 0.25,
        shadowRadius: 3.84,
      },
      android: {
        elevation: 5,
      },
    }),
  },
});
```

## Text Styling:

```javascript
const styles = StyleSheet.create({
  text: {
    color: '#333',
    fontSize: 16,
    fontWeight: 'bold',        // 'normal', 'bold', '100'-'900'
    fontStyle: 'italic',       // 'normal', 'italic'
    textAlign: 'center',       // 'left', 'center', 'right', 'justify'
    textDecorationLine: 'underline', // 'none', 'underline', 'line-through'
    letterSpacing: 2,
    lineHeight: 24,
    textTransform: 'uppercase', // 'none', 'uppercase', 'lowercase', 'capitalize'
  },
});
```

## Multiple Styles

### Array of Styles:

```javascript
<Text style={[styles.text, styles.bold, styles.large]}>
  Multiple Styles
</Text>

const styles = StyleSheet.create({
  text: {
    color: '#333',
  },
  bold: {
    fontWeight: 'bold',
  },
  large: {
    fontSize: 24,
  },
});
```

**Note:** Later styles override earlier ones if there's a conflict.

### Inline Styles:

```javascript
// Use sparingly - not optimized
<View style={{ backgroundColor: 'red', padding: 10 }}>
  <Text>Inline styled</Text>
</View>

// Combining inline with StyleSheet
<View style={[styles.container, { backgroundColor: 'blue' }]}>
  <Text>Combined styles</Text>
</View>
```

## Conditional Styles:

```javascript
import { useState } from 'react';

export default function App() {
  const [isActive, setIsActive] = useState(false);

  return (
    <View style={[
      styles.box,
      isActive && styles.active,  // Apply if true
    ]}>
      <Text>Conditional Style</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  box: {
    padding: 20,
    backgroundColor: 'gray',
  },
  active: {
    backgroundColor: 'green',
  },
});
```

## Position:

```javascript
const styles = StyleSheet.create({
  absolute: {
    position: 'absolute',  // 'relative' is default
    top: 0,
    left: 0,
    right: 0,
    bottom: 0,
  },
  relative: {
    position: 'relative',
    top: 10,    // Offset from normal position
    left: 10,
  },
});
```

## Practical Example - Card Component:

```javascript
import { View, Text, Image, StyleSheet } from 'react-native';

export default function ProfileCard() {
  return (
    <View style={styles.container}>
      <View style={styles.card}>
        <Image
          source={require('./assets/icon.png')}
          style={styles.avatar}
        />
        <Text style={styles.name}>John Doe</Text>
        <Text style={styles.role}>React Native Developer</Text>
        <View style={styles.stats}>
          <View style={styles.stat}>
            <Text style={styles.statNumber}>128</Text>
            <Text style={styles.statLabel}>Projects</Text>
          </View>
          <View style={styles.stat}>
            <Text style={styles.statNumber}>1.2k</Text>
            <Text style={styles.statLabel}>Followers</Text>
          </View>
          <View style={styles.stat}>
            <Text style={styles.statNumber}>864</Text>
            <Text style={styles.statLabel}>Following</Text>
          </View>
        </View>
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
    padding: 20,
  },
  card: {
    backgroundColor: 'white',
    borderRadius: 15,
    padding: 25,
    width: '100%',
    maxWidth: 400,
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 4 },
    shadowOpacity: 0.1,
    shadowRadius: 8,
    elevation: 5,
  },
  avatar: {
    width: 100,
    height: 100,
    borderRadius: 50,
    marginBottom: 15,
    borderWidth: 3,
    borderColor: '#3498db',
  },
  name: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#2c3e50',
    marginBottom: 5,
  },
  role: {
    fontSize: 16,
    color: '#7f8c8d',
    marginBottom: 20,
  },
  stats: {
    flexDirection: 'row',
    justifyContent: 'space-around',
    width: '100%',
    borderTopWidth: 1,
    borderTopColor: '#ecf0f1',
    paddingTop: 20,
  },
  stat: {
    alignItems: 'center',
  },
  statNumber: {
    fontSize: 20,
    fontWeight: 'bold',
    color: '#2c3e50',
  },
  statLabel: {
    fontSize: 12,
    color: '#95a5a6',
    marginTop: 5,
  },
});
```

## Best Practices:

1. ✅ Use `StyleSheet.create()` instead of inline objects
2. ✅ Keep styles at the bottom of the file
3. ✅ Use meaningful style names
4. ✅ Extract common styles to separate file
5. ✅ Use constants for colors and sizes
6. ✅ Prefer flexbox for layouts
7. ✅ Test on both iOS and Android

## Practice Exercise:

Create a button component with the following features:
- Blue background (#3498db)
- White text
- Rounded corners (radius: 8)
- Padding: 15 vertical, 30 horizontal
- Center aligned text
- Shadow effect
- Hover/press state (different color)

## What's Next?

Now that you can style components beautifully, let's learn about managing data with state and props!

**[← Back to Tutorial 3](./03-basic-components.md)** | **[Continue to Tutorial 5: State and Props →](./05-state-and-props.md)**

---

## Key Takeaways:

- ✅ Use StyleSheet.create() for better performance
- ✅ Flexbox is the primary layout system
- ✅ Styles are JavaScript objects, not CSS strings
- ✅ Platform-specific styling for iOS vs Android
- ✅ Combine multiple styles with arrays
- ✅ All dimensions are unitless (dp on Android, points on iOS)

You're becoming a styling pro! 🎨
