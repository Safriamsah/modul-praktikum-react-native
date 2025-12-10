# Tutorial 6: Working with Lists and ScrollView

## Introduction

Most mobile apps display lists of data. React Native provides powerful components for rendering scrollable lists efficiently.

## ScrollView Component

`ScrollView` is a scrollable container that can hold multiple components.

### Basic ScrollView:

```javascript
import { ScrollView, Text, View, StyleSheet } from 'react-native';

export default function App() {
  return (
    <ScrollView style={styles.container}>
      <Text>Item 1</Text>
      <Text>Item 2</Text>
      <Text>Item 3</Text>
      {/* ... many more items ... */}
    </ScrollView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
});
```

### ScrollView Props:

```javascript
<ScrollView
  horizontal={false}           // false = vertical, true = horizontal
  showsVerticalScrollIndicator={true}
  showsHorizontalScrollIndicator={false}
  contentContainerStyle={styles.content}
  refreshControl={/* pull to refresh */}
  onScroll={(event) => {/* scroll event */}}
>
  {/* Content */}
</ScrollView>
```

### Horizontal ScrollView:

```javascript
export default function HorizontalScroll() {
  return (
    <ScrollView 
      horizontal={true}
      showsHorizontalScrollIndicator={false}
      style={styles.container}
    >
      <View style={styles.box} />
      <View style={styles.box} />
      <View style={styles.box} />
      <View style={styles.box} />
    </ScrollView>
  );
}

const styles = StyleSheet.create({
  container: {
    flexGrow: 0,  // Don't take full height
  },
  box: {
    width: 150,
    height: 150,
    backgroundColor: '#3498db',
    margin: 10,
    borderRadius: 10,
  },
});
```

### When to Use ScrollView:

✅ Small number of items (< 50)
✅ All items rendered at once
✅ Different types of content mixed together
❌ Large lists (use FlatList instead)

## FlatList Component

`FlatList` is optimized for large lists - it only renders visible items!

### Basic FlatList:

```javascript
import { FlatList, Text, View, StyleSheet } from 'react-native';

export default function App() {
  const data = [
    { id: '1', title: 'Item 1' },
    { id: '2', title: 'Item 2' },
    { id: '3', title: 'Item 3' },
  ];

  const renderItem = ({ item }) => (
    <View style={styles.item}>
      <Text>{item.title}</Text>
    </View>
  );

  return (
    <FlatList
      data={data}
      renderItem={renderItem}
      keyExtractor={item => item.id}
    />
  );
}

const styles = StyleSheet.create({
  item: {
    padding: 20,
    borderBottomWidth: 1,
    borderBottomColor: '#ccc',
  },
});
```

### Key FlatList Props:

```javascript
<FlatList
  data={dataArray}                    // Array of data
  renderItem={renderItem}             // How to render each item
  keyExtractor={(item) => item.id}    // Unique key for each item
  
  // Optional props
  horizontal={false}                  // Horizontal list
  numColumns={1}                      // Grid layout
  ItemSeparatorComponent={Separator}  // Between items
  ListHeaderComponent={Header}        // At the top
  ListFooterComponent={Footer}        // At the bottom
  ListEmptyComponent={Empty}          // When list is empty
  
  onEndReached={loadMore}             // Load more at bottom
  onEndReachedThreshold={0.5}         // When to trigger loadMore
  refreshing={isLoading}              // Pull to refresh
  onRefresh={handleRefresh}
/>
```

## Practical Example - Contact List:

```javascript
import { useState } from 'react';
import { FlatList, View, Text, StyleSheet, TouchableOpacity } from 'react-native';

export default function ContactList() {
  const [contacts] = useState([
    { id: '1', name: 'Alice Johnson', phone: '555-0101' },
    { id: '2', name: 'Bob Smith', phone: '555-0102' },
    { id: '3', name: 'Charlie Brown', phone: '555-0103' },
    { id: '4', name: 'Diana Prince', phone: '555-0104' },
    { id: '5', name: 'Ethan Hunt', phone: '555-0105' },
  ]);

  const renderContact = ({ item }) => (
    <TouchableOpacity 
      style={styles.contactItem}
      onPress={() => alert(`Calling ${item.name}`)}
    >
      <View style={styles.avatar}>
        <Text style={styles.avatarText}>
          {item.name.charAt(0)}
        </Text>
      </View>
      <View style={styles.contactInfo}>
        <Text style={styles.name}>{item.name}</Text>
        <Text style={styles.phone}>{item.phone}</Text>
      </View>
    </TouchableOpacity>
  );

  return (
    <View style={styles.container}>
      <Text style={styles.header}>Contacts</Text>
      <FlatList
        data={contacts}
        renderItem={renderContact}
        keyExtractor={item => item.id}
        ItemSeparatorComponent={() => <View style={styles.separator} />}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f5f5f5',
  },
  header: {
    fontSize: 24,
    fontWeight: 'bold',
    padding: 20,
    backgroundColor: '#fff',
  },
  contactItem: {
    flexDirection: 'row',
    padding: 15,
    backgroundColor: '#fff',
    alignItems: 'center',
  },
  avatar: {
    width: 50,
    height: 50,
    borderRadius: 25,
    backgroundColor: '#3498db',
    alignItems: 'center',
    justifyContent: 'center',
    marginRight: 15,
  },
  avatarText: {
    color: '#fff',
    fontSize: 20,
    fontWeight: 'bold',
  },
  contactInfo: {
    flex: 1,
  },
  name: {
    fontSize: 16,
    fontWeight: '600',
    marginBottom: 4,
  },
  phone: {
    fontSize: 14,
    color: '#666',
  },
  separator: {
    height: 1,
    backgroundColor: '#e0e0e0',
  },
});
```

## Grid Layout with FlatList:

```javascript
export default function ImageGrid() {
  const images = [
    { id: '1', url: 'https://via.placeholder.com/150' },
    { id: '2', url: 'https://via.placeholder.com/150' },
    { id: '3', url: 'https://via.placeholder.com/150' },
    { id: '4', url: 'https://via.placeholder.com/150' },
    { id: '5', url: 'https://via.placeholder.com/150' },
    { id: '6', url: 'https://via.placeholder.com/150' },
  ];

  const renderImage = ({ item }) => (
    <View style={styles.gridItem}>
      <Image source={{ uri: item.url }} style={styles.image} />
    </View>
  );

  return (
    <FlatList
      data={images}
      renderItem={renderImage}
      keyExtractor={item => item.id}
      numColumns={3}
      contentContainerStyle={styles.grid}
    />
  );
}

const styles = StyleSheet.create({
  grid: {
    padding: 5,
  },
  gridItem: {
    flex: 1,
    margin: 5,
    aspectRatio: 1,
  },
  image: {
    width: '100%',
    height: '100%',
    borderRadius: 8,
  },
});
```

## SectionList Component

`SectionList` is for lists with section headers (like grouped contacts).

```javascript
import { SectionList, Text, View, StyleSheet } from 'react-native';

export default function GroupedList() {
  const sections = [
    {
      title: 'Fruits',
      data: ['Apple', 'Banana', 'Orange'],
    },
    {
      title: 'Vegetables',
      data: ['Carrot', 'Broccoli', 'Spinach'],
    },
  ];

  return (
    <SectionList
      sections={sections}
      keyExtractor={(item, index) => item + index}
      renderItem={({ item }) => (
        <View style={styles.item}>
          <Text>{item}</Text>
        </View>
      )}
      renderSectionHeader={({ section: { title } }) => (
        <View style={styles.header}>
          <Text style={styles.headerText}>{title}</Text>
        </View>
      )}
    />
  );
}

const styles = StyleSheet.create({
  header: {
    backgroundColor: '#f0f0f0',
    padding: 10,
  },
  headerText: {
    fontWeight: 'bold',
    fontSize: 16,
  },
  item: {
    padding: 15,
    borderBottomWidth: 1,
    borderBottomColor: '#ddd',
  },
});
```

## Pull to Refresh:

```javascript
import { useState } from 'react';
import { FlatList, RefreshControl } from 'react-native';

export default function RefreshableList() {
  const [data, setData] = useState(['Item 1', 'Item 2', 'Item 3']);
  const [refreshing, setRefreshing] = useState(false);

  const onRefresh = () => {
    setRefreshing(true);
    // Simulate API call
    setTimeout(() => {
      setData([...data, `Item ${data.length + 1}`]);
      setRefreshing(false);
    }, 2000);
  };

  return (
    <FlatList
      data={data}
      renderItem={({ item }) => <Text>{item}</Text>}
      keyExtractor={(item, index) => index.toString()}
      refreshControl={
        <RefreshControl
          refreshing={refreshing}
          onRefresh={onRefresh}
          colors={['#3498db']}  // Android
          tintColor="#3498db"   // iOS
        />
      }
    />
  );
}
```

## Infinite Scroll / Load More:

```javascript
import { useState } from 'react';
import { FlatList, Text, ActivityIndicator, View } from 'react-native';

export default function InfiniteList() {
  const [data, setData] = useState(Array.from({ length: 20 }, (_, i) => `Item ${i + 1}`));
  const [loading, setLoading] = useState(false);

  const loadMore = () => {
    if (loading) return;
    
    setLoading(true);
    // Simulate API call
    setTimeout(() => {
      const newData = Array.from(
        { length: 10 }, 
        (_, i) => `Item ${data.length + i + 1}`
      );
      setData([...data, ...newData]);
      setLoading(false);
    }, 1500);
  };

  const renderFooter = () => {
    if (!loading) return null;
    return (
      <View style={{ padding: 20 }}>
        <ActivityIndicator size="large" color="#3498db" />
      </View>
    );
  };

  return (
    <FlatList
      data={data}
      renderItem={({ item }) => (
        <View style={{ padding: 20 }}>
          <Text>{item}</Text>
        </View>
      )}
      keyExtractor={(item, index) => index.toString()}
      onEndReached={loadMore}
      onEndReachedThreshold={0.5}
      ListFooterComponent={renderFooter}
    />
  );
}
```

## Empty State:

```javascript
const EmptyComponent = () => (
  <View style={styles.emptyContainer}>
    <Text style={styles.emptyText}>No items found</Text>
  </View>
);

<FlatList
  data={data}
  renderItem={renderItem}
  ListEmptyComponent={EmptyComponent}
/>
```

## Performance Tips:

### 1. Use keyExtractor:
```javascript
keyExtractor={item => item.id}  // Better than index
```

### 2. Optimize renderItem:
```javascript
// ✅ Good - define outside or use useCallback
const renderItem = ({ item }) => <Item data={item} />;

// ❌ Bad - creates new function every render
renderItem={({ item }) => <Item data={item} />}
```

### 3. Use getItemLayout for fixed-size items:
```javascript
getItemLayout={(data, index) => ({
  length: ITEM_HEIGHT,
  offset: ITEM_HEIGHT * index,
  index,
})}
```

### 4. Set initialNumToRender:
```javascript
initialNumToRender={10}  // Render only 10 items initially
```

## ScrollView vs FlatList:

| Feature | ScrollView | FlatList |
|---------|-----------|----------|
| Item count | Small (<50) | Large (any) |
| Performance | All rendered | Lazy loading |
| Memory | High | Low |
| Flexibility | Very flexible | Structured |
| Use case | Mixed content | Uniform lists |

## Practice Exercise:

Create a todo list app with:
1. FlatList to display todos
2. Ability to mark todos as complete
3. Different styling for completed items
4. Empty state when no todos
5. Pull to refresh
6. Add new todo functionality

## What's Next?

Now you can handle lists efficiently! Let's learn about user input and interactions.

**[← Back to Tutorial 5](./05-state-and-props.md)** | **[Continue to Tutorial 7: User Input →](./07-user-input.md)**

---

## Key Takeaways:

- ✅ Use ScrollView for small, mixed content
- ✅ Use FlatList for large lists (better performance)
- ✅ Always provide unique keys with keyExtractor
- ✅ SectionList for grouped data
- ✅ Pull to refresh with RefreshControl
- ✅ Infinite scroll with onEndReached
- ✅ Optimize with getItemLayout and initialNumToRender

You're doing amazing! 📱
