# Tutorial 1: Introduction to Expo and React Native

## Welcome to Expo JS Template! 🎉

This is the first tutorial in a series of 8 lessons designed to help you learn React Native development with Expo.

## What is Expo?

Expo is a framework and platform for universal React applications. It provides a set of tools and services built around React Native, making it easier to build, deploy, and quickly iterate on iOS, Android, and web apps from the same JavaScript/TypeScript codebase.

### Key Benefits of Expo:

1. **No Native Code Required**: Start building without Xcode or Android Studio
2. **Fast Development**: Hot reloading and instant previews
3. **Cross-Platform**: One codebase for iOS, Android, and Web
4. **Rich Ecosystem**: Pre-built components and APIs
5. **Easy Testing**: Use Expo Go app to test on real devices

## What is React Native?

React Native is a JavaScript framework for building native mobile applications. It uses React, a popular JavaScript library for building user interfaces, and allows you to build mobile apps using familiar web development concepts.

### Core Concepts:

- **Components**: Building blocks of your app (similar to React on web)
- **JSX**: JavaScript XML syntax for describing UI
- **Props**: Data passed to components
- **State**: Data that changes over time
- **Native Rendering**: UI renders as native platform components

## Project Structure Overview

```
expoJS-template/
├── App.js              # Main application component
├── app.json            # Expo configuration
├── package.json        # Project dependencies
├── assets/             # Images, fonts, and other static files
├── tutorials/          # These tutorial files
└── node_modules/       # Installed packages
```

## Getting Started

### Prerequisites:
- Node.js (v14 or newer)
- npm or yarn
- Expo Go app on your mobile device (optional)

### Installation:

```bash
# Install dependencies
npm install

# Start the development server
npm start

# Or run on specific platforms
npm run android  # Android
npm run ios      # iOS (requires macOS)
npm run web      # Web browser
```

## Your First Look at App.js

Open `App.js` in the root directory. This is the main entry point of your application:

```javascript
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View } from 'react-native';

export default function App() {
  return (
    <View style={styles.container}>
      <Text>Open up App.js to start working on your app!</Text>
      <StatusBar style="auto" />
    </View>
  );
}
```

This simple component:
1. Imports necessary components from React Native
2. Defines a function component called `App`
3. Returns JSX describing what should be rendered
4. Uses inline styles defined in `styles.container`

## What's Next?

In the next tutorial, we'll dive deeper into the project structure and understand how different files work together.

**[Continue to Tutorial 2: Project Structure →](./02-project-structure.md)**

---

## Quick Tips:

- 💡 Press `r` to reload the app during development
- 💡 Press `m` to toggle the menu
- 💡 Check the terminal for errors and warnings
- 💡 Use `console.log()` for debugging

## Common Issues:

**Port already in use?**
```bash
# Kill the process on port 8081
npx expo start --port 8082
```

**Module not found?**
```bash
# Clear cache and reinstall
rm -rf node_modules
npm install
```

Happy coding! 🚀
