# Tutorial 8: Basic Navigation Concepts

## Introduction

Most mobile apps have multiple screens. Navigation allows users to move between these screens. Let's learn the basics!

## Why Navigation?

Mobile apps typically have:
- Home screen
- Detail screens
- Settings screen
- Profile screen
- etc.

Users need to navigate between these screens smoothly.

## React Navigation

The most popular navigation library for React Native is **React Navigation**.

### Installation:

```bash
# Install React Navigation
npm install @react-navigation/native

# Install dependencies
npx expo install react-native-screens react-native-safe-area-context

# Install stack navigator
npm install @react-navigation/native-stack
```

## Basic Navigation Setup

### 1. Create Navigation Container:

```javascript
// App.js
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

const Stack = createNativeStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

### 2. Create Screen Components:

```javascript
// screens/HomeScreen.js
import { View, Text, Button, StyleSheet } from 'react-native';

export default function HomeScreen({ navigation }) {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Home Screen</Text>
      <Button
        title="Go to Details"
        onPress={() => navigation.navigate('Details')}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 20,
  },
});
```

```javascript
// screens/DetailsScreen.js
export default function DetailsScreen({ navigation }) {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Details Screen</Text>
      <Button
        title="Go Back"
        onPress={() => navigation.goBack()}
      />
    </View>
  );
}
```

## Navigation Methods

### navigate():
```javascript
// Navigate to a screen
navigation.navigate('Details');

// Navigate with parameters
navigation.navigate('Details', { 
  itemId: 42, 
  itemName: 'Product A' 
});
```

### goBack():
```javascript
// Go back to previous screen
navigation.goBack();
```

### push():
```javascript
// Push a new screen (allows multiple instances)
navigation.push('Details');
```

### popToTop():
```javascript
// Go back to the first screen in the stack
navigation.popToTop();
```

## Passing Data Between Screens

### Send Data:
```javascript
// From HomeScreen
navigation.navigate('Details', {
  itemId: 42,
  itemName: 'Product A',
  price: 99.99
});
```

### Receive Data:
```javascript
// In DetailsScreen
export default function DetailsScreen({ route, navigation }) {
  const { itemId, itemName, price } = route.params;

  return (
    <View>
      <Text>Item ID: {itemId}</Text>
      <Text>Name: {itemName}</Text>
      <Text>Price: ${price}</Text>
    </View>
  );
}
```

## Customizing Headers

### Set Screen Options:

```javascript
<Stack.Navigator>
  <Stack.Screen 
    name="Home" 
    component={HomeScreen}
    options={{
      title: 'Welcome',
      headerStyle: {
        backgroundColor: '#3498db',
      },
      headerTintColor: '#fff',
      headerTitleStyle: {
        fontWeight: 'bold',
      },
    }}
  />
</Stack.Navigator>
```

### Dynamic Header:

```javascript
export default function DetailsScreen({ route, navigation }) {
  const { itemName } = route.params;

  // Set header title dynamically
  navigation.setOptions({
    title: itemName,
  });

  return <View>{/* Content */}</View>;
}
```

### Header Buttons:

```javascript
<Stack.Screen
  name="Home"
  component={HomeScreen}
  options={{
    headerRight: () => (
      <Button
        onPress={() => alert('Info pressed')}
        title="Info"
        color="#fff"
      />
    ),
  }}
/>
```

## Practical Example - Product App:

```javascript
// App.js
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import ProductListScreen from './screens/ProductListScreen';
import ProductDetailScreen from './screens/ProductDetailScreen';

const Stack = createNativeStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator
        screenOptions={{
          headerStyle: {
            backgroundColor: '#3498db',
          },
          headerTintColor: '#fff',
          headerTitleStyle: {
            fontWeight: 'bold',
          },
        }}
      >
        <Stack.Screen 
          name="ProductList" 
          component={ProductListScreen}
          options={{ title: 'Products' }}
        />
        <Stack.Screen 
          name="ProductDetail" 
          component={ProductDetailScreen}
        />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

```javascript
// screens/ProductListScreen.js
import { FlatList, TouchableOpacity, Text, View, StyleSheet } from 'react-native';

export default function ProductListScreen({ navigation }) {
  const products = [
    { id: '1', name: 'Laptop', price: 999 },
    { id: '2', name: 'Phone', price: 699 },
    { id: '3', name: 'Tablet', price: 499 },
  ];

  const renderProduct = ({ item }) => (
    <TouchableOpacity
      style={styles.item}
      onPress={() => navigation.navigate('ProductDetail', { product: item })}
    >
      <Text style={styles.name}>{item.name}</Text>
      <Text style={styles.price}>${item.price}</Text>
    </TouchableOpacity>
  );

  return (
    <FlatList
      data={products}
      renderItem={renderProduct}
      keyExtractor={item => item.id}
      contentContainerStyle={styles.list}
    />
  );
}

const styles = StyleSheet.create({
  list: {
    padding: 10,
  },
  item: {
    backgroundColor: 'white',
    padding: 20,
    marginBottom: 10,
    borderRadius: 8,
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 4,
    elevation: 3,
  },
  name: {
    fontSize: 18,
    fontWeight: '600',
  },
  price: {
    fontSize: 16,
    color: '#3498db',
    fontWeight: 'bold',
  },
});
```

```javascript
// screens/ProductDetailScreen.js
import { View, Text, Button, StyleSheet } from 'react-native';
import { useEffect } from 'react';

export default function ProductDetailScreen({ route, navigation }) {
  const { product } = route.params;

  useEffect(() => {
    navigation.setOptions({
      title: product.name,
    });
  }, [navigation, product.name]);

  return (
    <View style={styles.container}>
      <Text style={styles.label}>Product Name:</Text>
      <Text style={styles.value}>{product.name}</Text>
      
      <Text style={styles.label}>Price:</Text>
      <Text style={styles.value}>${product.price}</Text>
      
      <View style={styles.buttonContainer}>
        <Button 
          title="Add to Cart" 
          onPress={() => alert('Added to cart!')}
        />
      </View>
      
      <Button 
        title="Go Back" 
        onPress={() => navigation.goBack()}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#f5f5f5',
  },
  label: {
    fontSize: 14,
    color: '#666',
    marginTop: 20,
    marginBottom: 5,
  },
  value: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#2c3e50',
  },
  buttonContainer: {
    marginTop: 30,
    marginBottom: 10,
  },
});
```

## Types of Navigators

### 1. Stack Navigator (Most Common):
- Screens stack on top of each other
- Back button to previous screen
- Like browsing history

### 2. Tab Navigator:
```bash
npm install @react-navigation/bottom-tabs
```

```javascript
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';

const Tab = createBottomTabNavigator();

<Tab.Navigator>
  <Tab.Screen name="Home" component={HomeScreen} />
  <Tab.Screen name="Settings" component={SettingsScreen} />
</Tab.Navigator>
```

### 3. Drawer Navigator:
```bash
npm install @react-navigation/drawer
```

```javascript
import { createDrawerNavigator } from '@react-navigation/drawer';

const Drawer = createDrawerNavigator();

<Drawer.Navigator>
  <Drawer.Screen name="Home" component={HomeScreen} />
  <Drawer.Screen name="Profile" component={ProfileScreen} />
</Drawer.Navigator>
```

## Navigation Lifecycle

### Focus Events:

```javascript
import { useEffect } from 'react';
import { useFocusEffect } from '@react-navigation/native';

export default function MyScreen({ navigation }) {
  // Runs when screen is focused
  useFocusEffect(
    useCallback(() => {
      console.log('Screen focused');
      
      // Cleanup when leaving
      return () => {
        console.log('Screen unfocused');
      };
    }, [])
  );

  return <View>{/* Content */}</View>;
}
```

## Best Practices

1. ✅ Keep navigation logic in screen components
2. ✅ Use meaningful screen names
3. ✅ Pass minimal data through params
4. ✅ Use TypeScript for param type safety (advanced)
5. ✅ Don't nest navigators unnecessarily
6. ✅ Test navigation flows thoroughly
7. ✅ Handle deep linking for better UX

## Common Patterns

### Conditional Navigation:

```javascript
export default function HomeScreen({ navigation }) {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  useEffect(() => {
    if (!isLoggedIn) {
      navigation.navigate('Login');
    }
  }, [isLoggedIn, navigation]);

  return <View>{/* Content */}</View>;
}
```

### Reset Navigation Stack:

```javascript
import { CommonActions } from '@react-navigation/native';

navigation.dispatch(
  CommonActions.reset({
    index: 0,
    routes: [{ name: 'Home' }],
  })
);
```

## Beyond Basics

There's much more to learn about navigation:
- Tab Navigators with icons
- Drawer Navigation
- Modal Screens
- Deep Linking
- Authentication Flows
- Nested Navigators
- Custom Transitions

Check the [React Navigation docs](https://reactnavigation.org/) for more!

## Practice Exercise:

Create a simple app with:
1. Home screen with list of items
2. Detail screen showing item details
3. Settings screen
4. Tab navigation between Home and Settings
5. Pass data from Home to Detail screen
6. Custom header with a button

## Congratulations! 🎉

You've completed all 8 tutorials! You now have a solid foundation in:
- ✅ Expo and React Native basics
- ✅ Project structure
- ✅ Core components (View, Text, Image)
- ✅ Styling with StyleSheet and Flexbox
- ✅ State and Props
- ✅ Lists and ScrollView
- ✅ User Input handling
- ✅ Basic Navigation

## What's Next?

Continue your learning journey:
1. **Build Projects**: Best way to learn is by building
2. **Explore Expo SDK**: Camera, Location, Sensors, etc.
3. **API Integration**: Fetch data from APIs
4. **State Management**: Redux, Context API, Zustand
5. **Advanced Navigation**: Complex navigation patterns
6. **Animations**: React Native Reanimated
7. **Native Modules**: When you need platform-specific code
8. **Publishing**: Deploy to App Store and Play Store

## Resources:

- [Expo Documentation](https://docs.expo.dev/)
- [React Native Docs](https://reactnative.dev/)
- [React Navigation](https://reactnavigation.org/)
- [React Native Directory](https://reactnative.directory/)

**[← Back to Tutorial 7](./07-user-input.md)**

---

## Key Takeaways:

- ✅ React Navigation is the standard for navigation
- ✅ Stack Navigator for screen-to-screen navigation
- ✅ Pass data using route.params
- ✅ Customize headers with options
- ✅ Tab and Drawer navigators for main app structure
- ✅ Use navigation.navigate(), goBack(), push()
- ✅ Handle navigation lifecycle with hooks

**Thank you for completing this tutorial series!** Keep building amazing apps! 🚀📱

Happy Coding! 💻✨
