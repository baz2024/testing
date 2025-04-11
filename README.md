# 🚀 React Native Starter Template

This is a solid foundation for a React Native mobile app using:

- React Navigation (stack navigation)
- Axios for API calls
- AsyncStorage for local persistence
- Functional components with Hooks

---

## 📦 1. Create the App

```bash
npx react-native init RNStarterApp
cd RNStarterApp
```

### Optional: Use TypeScript template

```bash
npx react-native init RNStarterApp --template react-native-template-typescript
```

---

## 📚 2. Install Dependencies

```bash
npm install @react-navigation/native
npm install @react-navigation/native-stack
npm install react-native-screens react-native-safe-area-context react-native-gesture-handler react-native-reanimated
npm install axios
npm install @react-native-async-storage/async-storage
```

Then link dependencies:

```bash
npx pod-install ios
```

---

## 🧭 3. Setup Navigation (`App.tsx` or `App.js`)

```tsx
import React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import HomeScreen from './src/screens/HomeScreen';
import DetailsScreen from './src/screens/DetailsScreen';

const Stack = createNativeStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Home">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

---

## 🏠 4. Home Screen (API + AsyncStorage Example)

`src/screens/HomeScreen.tsx`:

```tsx
import React, { useEffect, useState } from 'react';
import { View, Text, Button, FlatList } from 'react-native';
import axios from 'axios';
import AsyncStorage from '@react-native-async-storage/async-storage';

export default function HomeScreen({ navigation }) {
  const [posts, setPosts] = useState([]);

  const fetchPosts = async () => {
    const res = await axios.get('https://jsonplaceholder.typicode.com/posts');
    setPosts(res.data.slice(0, 10));
    await AsyncStorage.setItem('cachedPosts', JSON.stringify(res.data));
  };

  useEffect(() => {
    fetchPosts();
  }, []);

  return (
    <View style={{ flex: 1, padding: 20 }}>
      <Text style={{ fontSize: 20 }}>Posts</Text>
      <FlatList
        data={posts}
        keyExtractor={(item) => item.id.toString()}
        renderItem={({ item }) => <Text>{item.title}</Text>}
      />
      <Button title="Go to Details" onPress={() => navigation.navigate('Details')} />
    </View>
  );
}
```

---

## 📄 5. Details Screen

`src/screens/DetailsScreen.tsx`:

```tsx
import React, { useEffect, useState } from 'react';
import { View, Text } from 'react-native';
import AsyncStorage from '@react-native-async-storage/async-storage';

export default function DetailsScreen() {
  const [cachedData, setCachedData] = useState([]);

  useEffect(() => {
    const loadData = async () => {
      const json = await AsyncStorage.getItem('cachedPosts');
      if (json) {
        setCachedData(JSON.parse(json));
      }
    };
    loadData();
  }, []);

  return (
    <View style={{ flex: 1, padding: 20 }}>
      <Text style={{ fontSize: 20 }}>Cached Posts</Text>
      {cachedData.map((item, i) => (
        <Text key={i}>{item.title}</Text>
      ))}
    </View>
  );
}
```

---

## 📱 Run the App

```bash
npx react-native run-android
# or
npx react-native run-ios
```

---

## ✅ Summary

| Feature        | Tool / Library |
|----------------|----------------|
| Navigation     | `@react-navigation/native` |
| API Calls      | `axios`        |
| Local Storage  | `AsyncStorage` |
| Stack UI       | `react-native` |
