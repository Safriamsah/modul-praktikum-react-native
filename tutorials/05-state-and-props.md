# Tutorial 5: Understanding State and Props

## Introduction

State and Props are fundamental concepts in React Native. They control how your app handles and displays data.

## Props (Properties)

Props are arguments passed to components - like function parameters. They're **read-only** and flow down from parent to child.

### Basic Props:

```javascript
import { View, Text } from 'react-native';

// Child component receiving props
function Greeting(props) {
  return (
    <Text>Hello, {props.name}!</Text>
  );
}

// Parent component passing props
export default function App() {
  return (
    <View>
      <Greeting name="Alice" />
      <Greeting name="Bob" />
    </View>
  );
}
```

### Destructuring Props:

```javascript
// Instead of using props.name, props.age, etc.
function UserCard({ name, age, email }) {
  return (
    <View>
      <Text>Name: {name}</Text>
      <Text>Age: {age}</Text>
      <Text>Email: {email}</Text>
    </View>
  );
}

// Usage
<UserCard name="John" age={30} email="john@example.com" />
```

### Default Props:

```javascript
function Button({ title = "Click Me", color = "blue" }) {
  return (
    <View style={{ backgroundColor: color }}>
      <Text>{title}</Text>
    </View>
  );
}

// All these are valid:
<Button title="Submit" color="green" />
<Button title="Cancel" />  // Uses default color
<Button />  // Uses both defaults
```

### Props with Different Types:

```javascript
function ProfileCard({ 
  name,           // string
  age,            // number
  isActive,       // boolean
  tags,           // array
  onPress,        // function
  style           // object
}) {
  return (
    <View style={style}>
      <Text>{name} ({age})</Text>
      <Text>Status: {isActive ? 'Active' : 'Inactive'}</Text>
      <Text>Tags: {tags.join(', ')}</Text>
      <Button title="View" onPress={onPress} />
    </View>
  );
}

// Usage
<ProfileCard
  name="Alice"
  age={25}
  isActive={true}
  tags={['developer', 'designer']}
  onPress={() => alert('Pressed!')}
  style={{ padding: 20 }}
/>
```

### Props.children:

```javascript
function Card({ children, title }) {
  return (
    <View style={styles.card}>
      <Text style={styles.title}>{title}</Text>
      {children}
    </View>
  );
}

// Usage - everything between tags is "children"
<Card title="My Card">
  <Text>Line 1</Text>
  <Text>Line 2</Text>
  <Image source={require('./icon.png')} />
</Card>
```

## State

State is data that **changes over time** in your component. When state changes, the component re-renders.

### useState Hook:

```javascript
import { useState } from 'react';
import { View, Text, Button } from 'react-native';

export default function Counter() {
  // Declare state variable
  const [count, setCount] = useState(0);
  //     ^       ^            ^
  //   value  updater    initial value

  return (
    <View>
      <Text>Count: {count}</Text>
      <Button 
        title="Increment" 
        onPress={() => setCount(count + 1)}
      />
    </View>
  );
}
```

### Multiple State Variables:

```javascript
export default function Form() {
  const [name, setName] = useState('');
  const [age, setAge] = useState(0);
  const [email, setEmail] = useState('');
  const [isSubscribed, setIsSubscribed] = useState(false);

  return (
    <View>
      <TextInput 
        value={name}
        onChangeText={setName}
        placeholder="Name"
      />
      <TextInput 
        value={email}
        onChangeText={setEmail}
        placeholder="Email"
      />
    </View>
  );
}
```

### State with Objects:

```javascript
export default function UserProfile() {
  const [user, setUser] = useState({
    name: 'John',
    age: 30,
    email: 'john@example.com'
  });

  const updateName = (newName) => {
    setUser({
      ...user,        // Keep other properties
      name: newName   // Update only name
    });
  };

  return (
    <View>
      <Text>{user.name}</Text>
      <Button title="Change Name" onPress={() => updateName('Jane')} />
    </View>
  );
}
```

### State with Arrays:

```javascript
export default function TodoList() {
  const [todos, setTodos] = useState(['Learn React', 'Build App']);

  const addTodo = () => {
    setTodos([...todos, 'New Todo']);
  };

  const removeTodo = (index) => {
    setTodos(todos.filter((_, i) => i !== index));
  };

  return (
    <View>
      {todos.map((todo, index) => (
        <View key={index}>
          <Text>{todo}</Text>
          <Button title="Remove" onPress={() => removeTodo(index)} />
        </View>
      ))}
      <Button title="Add Todo" onPress={addTodo} />
    </View>
  );
}
```

## Props vs State

| Props | State |
|-------|-------|
| Passed from parent | Defined in component |
| Read-only | Can be changed |
| Used to configure | Used to track changes |
| Like function parameters | Like local variables |

## Lifting State Up

When multiple components need to share state, move it to their common parent.

```javascript
// Parent component holds shared state
export default function App() {
  const [count, setCount] = useState(0);

  return (
    <View>
      <Display count={count} />
      <Controls onIncrement={() => setCount(count + 1)} />
    </View>
  );
}

// Child displays the value
function Display({ count }) {
  return <Text>Count: {count}</Text>;
}

// Child modifies via callback
function Controls({ onIncrement }) {
  return <Button title="+" onPress={onIncrement} />;
}
```

## Practical Example - Toggle Button:

```javascript
import { useState } from 'react';
import { View, Text, TouchableOpacity, StyleSheet } from 'react-native';

export default function App() {
  const [isOn, setIsOn] = useState(false);

  return (
    <View style={styles.container}>
      <Text style={styles.status}>
        The switch is {isOn ? 'ON' : 'OFF'}
      </Text>
      <TouchableOpacity
        style={[styles.button, isOn && styles.buttonOn]}
        onPress={() => setIsOn(!isOn)}
      >
        <Text style={styles.buttonText}>
          {isOn ? 'Turn OFF' : 'Turn ON'}
        </Text>
      </TouchableOpacity>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
    backgroundColor: '#f5f5f5',
  },
  status: {
    fontSize: 24,
    marginBottom: 20,
    fontWeight: 'bold',
  },
  button: {
    backgroundColor: '#e74c3c',
    paddingVertical: 15,
    paddingHorizontal: 40,
    borderRadius: 25,
  },
  buttonOn: {
    backgroundColor: '#2ecc71',
  },
  buttonText: {
    color: 'white',
    fontSize: 18,
    fontWeight: 'bold',
  },
});
```

## Practical Example - Form Input:

```javascript
import { useState } from 'react';
import { View, TextInput, Text, Button, StyleSheet } from 'react-native';

export default function LoginForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [message, setMessage] = useState('');

  const handleLogin = () => {
    if (email && password) {
      setMessage(`Logging in as ${email}`);
    } else {
      setMessage('Please fill all fields');
    }
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Login</Text>
      
      <TextInput
        style={styles.input}
        placeholder="Email"
        value={email}
        onChangeText={setEmail}
        keyboardType="email-address"
        autoCapitalize="none"
      />
      
      <TextInput
        style={styles.input}
        placeholder="Password"
        value={password}
        onChangeText={setPassword}
        secureTextEntry
      />
      
      <Button title="Login" onPress={handleLogin} />
      
      {message ? <Text style={styles.message}>{message}</Text> : null}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    justifyContent: 'center',
    backgroundColor: '#fff',
  },
  title: {
    fontSize: 32,
    fontWeight: 'bold',
    marginBottom: 30,
    textAlign: 'center',
  },
  input: {
    borderWidth: 1,
    borderColor: '#ddd',
    padding: 15,
    marginBottom: 15,
    borderRadius: 8,
    fontSize: 16,
  },
  message: {
    marginTop: 20,
    textAlign: 'center',
    fontSize: 16,
    color: '#666',
  },
});
```

## State Update Patterns:

### Based on Previous State:

```javascript
// ❌ Wrong - may be unreliable
setCount(count + 1);

// ✅ Correct - uses functional update
setCount(prevCount => prevCount + 1);
```

### Multiple Updates:

```javascript
// These happen together
const handleMultipleUpdates = () => {
  setCount(c => c + 1);
  setName('New Name');
  setIsActive(true);
};
```

## Common Mistakes:

### ❌ Mutating State Directly:

```javascript
// Wrong
user.name = 'New Name';
setUser(user);

// Correct
setUser({ ...user, name: 'New Name' });
```

### ❌ Using State Immediately After Setting:

```javascript
// Wrong - count is not updated yet
setCount(count + 1);
console.log(count);  // Still old value!

// Use useEffect for side effects
useEffect(() => {
  console.log(count);  // Updated value
}, [count]);
```

## Practice Exercise:

Create a simple counter app with:
1. Display current count
2. Increment button (+1)
3. Decrement button (-1)
4. Reset button (back to 0)
5. Show "Even" or "Odd" based on count
6. Disable decrement when count is 0

## What's Next?

Now that you understand state and props, let's learn how to work with lists and scrollable content!

**[← Back to Tutorial 4](./04-styling.md)** | **[Continue to Tutorial 6: Lists and ScrollView →](./06-lists-and-scrollview.md)**

---

## Key Takeaways:

- ✅ Props pass data from parent to child (read-only)
- ✅ State manages data that changes over time
- ✅ Use useState hook to create state variables
- ✅ State updates trigger re-renders
- ✅ Never mutate state directly, always use setter
- ✅ Lift state up to share between components
- ✅ Use functional updates for state based on previous state

Excellent progress! You're mastering React Native! 🚀
