# Tutorial 7: Handling User Input

## Introduction

User interaction is essential for any mobile app. Let's learn how to capture and handle user input effectively!

## TextInput Component

`TextInput` is the fundamental component for text input from the user.

### Basic TextInput:

```javascript
import { useState } from 'react';
import { View, TextInput, Text, StyleSheet } from 'react-native';

export default function App() {
  const [text, setText] = useState('');

  return (
    <View style={styles.container}>
      <TextInput
        style={styles.input}
        value={text}
        onChangeText={setText}
        placeholder="Type here..."
      />
      <Text>You typed: {text}</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    padding: 20,
  },
  input: {
    borderWidth: 1,
    borderColor: '#ddd',
    padding: 10,
    fontSize: 16,
    borderRadius: 8,
  },
});
```

### TextInput Props:

```javascript
<TextInput
  // Value and handlers
  value={text}
  onChangeText={setText}
  
  // Display
  placeholder="Enter text"
  placeholderTextColor="#999"
  
  // Keyboard
  keyboardType="default"  // 'numeric', 'email-address', 'phone-pad', etc.
  autoCapitalize="sentences"  // 'none', 'sentences', 'words', 'characters'
  autoCorrect={true}
  autoComplete="email"
  
  // Security
  secureTextEntry={false}  // true for passwords
  
  // Behavior
  multiline={false}  // true for textarea
  numberOfLines={4}  // for multiline
  maxLength={100}
  editable={true}
  
  // Events
  onFocus={() => console.log('focused')}
  onBlur={() => console.log('blurred')}
  onSubmitEditing={() => console.log('submitted')}
  
  // Style
  style={styles.input}
/>
```

## Different Keyboard Types:

```javascript
export default function KeyboardTypes() {
  const [default_, setDefault] = useState('');
  const [numeric, setNumeric] = useState('');
  const [email, setEmail] = useState('');
  const [phone, setPhone] = useState('');

  return (
    <View style={styles.container}>
      <TextInput
        style={styles.input}
        placeholder="Default keyboard"
        value={default_}
        onChangeText={setDefault}
        keyboardType="default"
      />
      
      <TextInput
        style={styles.input}
        placeholder="Numeric keyboard"
        value={numeric}
        onChangeText={setNumeric}
        keyboardType="numeric"
      />
      
      <TextInput
        style={styles.input}
        placeholder="Email keyboard"
        value={email}
        onChangeText={setEmail}
        keyboardType="email-address"
        autoCapitalize="none"
      />
      
      <TextInput
        style={styles.input}
        placeholder="Phone keyboard"
        value={phone}
        onChangeText={setPhone}
        keyboardType="phone-pad"
      />
    </View>
  );
}
```

## Button Component

Simple, platform-styled button.

```javascript
import { Button, Alert } from 'react-native';

export default function App() {
  const handlePress = () => {
    Alert.alert('Button Pressed!');
  };

  return (
    <View style={styles.container}>
      <Button 
        title="Click Me" 
        onPress={handlePress}
        color="#3498db"
      />
      
      <Button 
        title="Disabled Button" 
        onPress={handlePress}
        disabled={true}
      />
    </View>
  );
}
```

**Limitations of Button:**
- Limited styling options
- Platform-specific appearance
- Can't add icons

## TouchableOpacity

Touchable component with opacity feedback - better for custom buttons!

```javascript
import { TouchableOpacity, Text, StyleSheet } from 'react-native';

export default function CustomButton() {
  return (
    <TouchableOpacity 
      style={styles.button}
      onPress={() => alert('Pressed!')}
      activeOpacity={0.7}  // Opacity when pressed (0-1)
    >
      <Text style={styles.buttonText}>Custom Button</Text>
    </TouchableOpacity>
  );
}

const styles = StyleSheet.create({
  button: {
    backgroundColor: '#3498db',
    padding: 15,
    borderRadius: 8,
    alignItems: 'center',
  },
  buttonText: {
    color: 'white',
    fontSize: 16,
    fontWeight: 'bold',
  },
});
```

## Touchable Components Comparison:

| Component | Feedback | Use Case |
|-----------|----------|----------|
| `Button` | Platform default | Quick, simple buttons |
| `TouchableOpacity` | Opacity change | Most common, custom buttons |
| `TouchableHighlight` | Highlight color | List items |
| `TouchableWithoutFeedback` | No visual feedback | Special cases |
| `Pressable` | Flexible | Modern, recommended |

## Pressable (Modern Approach):

```javascript
import { Pressable, Text, StyleSheet } from 'react-native';

export default function ModernButton() {
  return (
    <Pressable
      style={({ pressed }) => [
        styles.button,
        pressed && styles.buttonPressed
      ]}
      onPress={() => alert('Pressed!')}
    >
      {({ pressed }) => (
        <Text style={styles.text}>
          {pressed ? 'Pressed!' : 'Press Me'}
        </Text>
      )}
    </Pressable>
  );
}

const styles = StyleSheet.create({
  button: {
    backgroundColor: '#3498db',
    padding: 15,
    borderRadius: 8,
  },
  buttonPressed: {
    backgroundColor: '#2980b9',
  },
  text: {
    color: 'white',
    textAlign: 'center',
    fontSize: 16,
    fontWeight: 'bold',
  },
});
```

## Practical Example - Login Form:

```javascript
import { useState } from 'react';
import { 
  View, 
  TextInput, 
  TouchableOpacity, 
  Text, 
  Alert,
  StyleSheet,
  KeyboardAvoidingView,
  Platform
} from 'react-native';

export default function LoginScreen() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [showPassword, setShowPassword] = useState(false);

  const handleLogin = () => {
    if (!email || !password) {
      Alert.alert('Error', 'Please fill all fields');
      return;
    }
    Alert.alert('Success', `Logging in as ${email}`);
  };

  return (
    <KeyboardAvoidingView 
      behavior={Platform.OS === 'ios' ? 'padding' : 'height'}
      style={styles.container}
    >
      <View style={styles.form}>
        <Text style={styles.title}>Welcome Back</Text>
        
        <TextInput
          style={styles.input}
          placeholder="Email"
          value={email}
          onChangeText={setEmail}
          keyboardType="email-address"
          autoCapitalize="none"
          autoComplete="email"
        />
        
        <View style={styles.passwordContainer}>
          <TextInput
            style={styles.passwordInput}
            placeholder="Password"
            value={password}
            onChangeText={setPassword}
            secureTextEntry={!showPassword}
            autoCapitalize="none"
          />
          <TouchableOpacity 
            onPress={() => setShowPassword(!showPassword)}
            style={styles.eyeButton}
          >
            <Text>{showPassword ? '👁️' : '👁️‍🗨️'}</Text>
          </TouchableOpacity>
        </View>
        
        <TouchableOpacity style={styles.button} onPress={handleLogin}>
          <Text style={styles.buttonText}>Login</Text>
        </TouchableOpacity>
        
        <TouchableOpacity>
          <Text style={styles.forgotPassword}>Forgot Password?</Text>
        </TouchableOpacity>
      </View>
    </KeyboardAvoidingView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f5f5f5',
  },
  form: {
    flex: 1,
    justifyContent: 'center',
    padding: 20,
  },
  title: {
    fontSize: 32,
    fontWeight: 'bold',
    marginBottom: 40,
    textAlign: 'center',
    color: '#2c3e50',
  },
  input: {
    backgroundColor: 'white',
    padding: 15,
    borderRadius: 8,
    fontSize: 16,
    marginBottom: 15,
    borderWidth: 1,
    borderColor: '#ddd',
  },
  passwordContainer: {
    flexDirection: 'row',
    alignItems: 'center',
    backgroundColor: 'white',
    borderRadius: 8,
    borderWidth: 1,
    borderColor: '#ddd',
    marginBottom: 15,
  },
  passwordInput: {
    flex: 1,
    padding: 15,
    fontSize: 16,
  },
  eyeButton: {
    padding: 15,
  },
  button: {
    backgroundColor: '#3498db',
    padding: 15,
    borderRadius: 8,
    marginTop: 10,
  },
  buttonText: {
    color: 'white',
    textAlign: 'center',
    fontSize: 18,
    fontWeight: 'bold',
  },
  forgotPassword: {
    marginTop: 15,
    textAlign: 'center',
    color: '#3498db',
    fontSize: 14,
  },
});
```

## Switch Component

Toggle between two states.

```javascript
import { useState } from 'react';
import { View, Switch, Text, StyleSheet } from 'react-native';

export default function Settings() {
  const [isEnabled, setIsEnabled] = useState(false);

  return (
    <View style={styles.container}>
      <View style={styles.setting}>
        <Text style={styles.label}>Notifications</Text>
        <Switch
          value={isEnabled}
          onValueChange={setIsEnabled}
          trackColor={{ false: '#767577', true: '#81b0ff' }}
          thumbColor={isEnabled ? '#3498db' : '#f4f3f4'}
        />
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    padding: 20,
  },
  setting: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    padding: 15,
    backgroundColor: 'white',
    borderRadius: 8,
  },
  label: {
    fontSize: 16,
  },
});
```

## Slider Component (from community)

For selecting values from a range.

```javascript
// First install: npx expo install @react-native-community/slider
import { useState } from 'react';
import { View, Text } from 'react-native';
import Slider from '@react-native-community/slider';

export default function VolumeControl() {
  const [volume, setVolume] = useState(50);

  return (
    <View style={{ padding: 20 }}>
      <Text>Volume: {volume.toFixed(0)}%</Text>
      <Slider
        value={volume}
        onValueChange={setVolume}
        minimumValue={0}
        maximumValue={100}
        minimumTrackTintColor="#3498db"
        maximumTrackTintColor="#ddd"
        thumbTintColor="#3498db"
      />
    </View>
  );
}
```

## Alert Component

Display native alerts to users.

```javascript
import { Alert, Button } from 'react-native';

// Simple alert
Alert.alert('Title', 'Message');

// Alert with buttons
Alert.alert(
  'Delete Item',
  'Are you sure you want to delete this item?',
  [
    {
      text: 'Cancel',
      onPress: () => console.log('Cancelled'),
      style: 'cancel',
    },
    {
      text: 'Delete',
      onPress: () => console.log('Deleted'),
      style: 'destructive',
    },
  ]
);

// Prompt (iOS only)
Alert.prompt(
  'Enter Name',
  'What is your name?',
  (text) => console.log('Input:', text)
);
```

## Keyboard Handling:

### Dismiss Keyboard on Tap:

```javascript
import { 
  Keyboard, 
  TouchableWithoutFeedback,
  View,
  TextInput
} from 'react-native';

export default function App() {
  return (
    <TouchableWithoutFeedback onPress={Keyboard.dismiss}>
      <View style={styles.container}>
        <TextInput placeholder="Type here..." />
      </View>
    </TouchableWithoutFeedback>
  );
}
```

### KeyboardAvoidingView:

```javascript
import { KeyboardAvoidingView, Platform } from 'react-native';

<KeyboardAvoidingView
  behavior={Platform.OS === 'ios' ? 'padding' : 'height'}
  style={styles.container}
>
  {/* Your inputs */}
</KeyboardAvoidingView>
```

## Form Validation Example:

```javascript
import { useState } from 'react';
import { View, TextInput, Text, Button, StyleSheet } from 'react-native';

export default function ValidatedForm() {
  const [email, setEmail] = useState('');
  const [errors, setErrors] = useState({});

  const validateEmail = () => {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!email) {
      setErrors({ email: 'Email is required' });
      return false;
    }
    if (!emailRegex.test(email)) {
      setErrors({ email: 'Invalid email format' });
      return false;
    }
    setErrors({});
    return true;
  };

  const handleSubmit = () => {
    if (validateEmail()) {
      alert('Form submitted!');
    }
  };

  return (
    <View style={styles.container}>
      <TextInput
        style={[styles.input, errors.email && styles.inputError]}
        placeholder="Email"
        value={email}
        onChangeText={setEmail}
        onBlur={validateEmail}
      />
      {errors.email && (
        <Text style={styles.errorText}>{errors.email}</Text>
      )}
      <Button title="Submit" onPress={handleSubmit} />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    padding: 20,
  },
  input: {
    borderWidth: 1,
    borderColor: '#ddd',
    padding: 10,
    marginBottom: 5,
    borderRadius: 8,
  },
  inputError: {
    borderColor: 'red',
  },
  errorText: {
    color: 'red',
    fontSize: 12,
    marginBottom: 10,
  },
});
```

## Best Practices:

1. ✅ Always control TextInput with state
2. ✅ Provide appropriate keyboard types
3. ✅ Use autoCapitalize and autoComplete
4. ✅ Handle keyboard dismissal
5. ✅ Validate inputs before submission
6. ✅ Provide clear error messages
7. ✅ Use KeyboardAvoidingView for forms
8. ✅ Disable buttons during submission

## Practice Exercise:

Create a registration form with:
1. Name input
2. Email input with validation
3. Password input (hidden)
4. Confirm password (must match)
5. Accept terms switch
6. Submit button (disabled until valid)
7. Show validation errors

## What's Next?

You've mastered user input! Next, let's explore navigation between screens.

**[← Back to Tutorial 6](./06-lists-and-scrollview.md)** | **[Continue to Tutorial 8: Navigation Basics →](./08-navigation-basics.md)**

---

## Key Takeaways:

- ✅ TextInput for text entry with many configuration options
- ✅ TouchableOpacity for custom buttons
- ✅ Pressable is the modern touchable component
- ✅ Switch for toggles, Slider for ranges
- ✅ Alert for native dialogs
- ✅ Always validate user input
- ✅ Handle keyboard properly with KeyboardAvoidingView
- ✅ Provide visual feedback for interactions

Fantastic work! You're almost done! 🎉
