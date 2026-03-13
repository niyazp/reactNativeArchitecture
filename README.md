# React Native Architecture Notes

This repository documents common architectural patterns and implementation techniques used in production React Native applications.
The goal of this repository is to demonstrate practical approaches for building scalable, maintainable, and performant mobile applications using React Native.

This is not a full application but a collection of architecture examples and implementation snippets that are commonly used in real-world projects.

---

# Tech Stack

* React Native
* TypeScript
* Redux Toolkit
* React Navigation
* Axios

---

# Typical Project Folder Structure

A scalable React Native application usually follows a modular folder structure that separates responsibilities clearly.

```
src
 ├── components
 │   ├── Button
 │   └── Loader
 │
 ├── screens
 │   ├── HomeScreen
 │   ├── ProfileScreen
 │   └── LoginScreen
 │
 ├── navigation
 │   ├── AppNavigator.tsx
 │   ├── AuthNavigator.tsx
 │   └── MainNavigator.tsx
 │
 ├── redux
 │   ├── store.ts
 │   └── slices
 │       └── authSlice.ts
 │
 ├── services
 │   └── apiClient.ts
 │
 ├── hooks
 │   └── useDebounce.ts
 │
 ├── utils
 │   └── helpers.ts
 │
 └── constants
     └── apiRoutes.ts
```

Benefits of this structure:

* Better code organization
* Easier scaling of large projects
* Clear separation of UI, logic, and services

---

# Navigation Architecture

Large applications usually separate navigation into two main flows:

1. Authentication flow
2. Main application flow

Example navigation structure:

```tsx
import React from "react";
import { NavigationContainer } from "@react-navigation/native";

export default function AppNavigator() {
  return (
    <NavigationContainer>
      {/* Conditional rendering based on auth state */}
      {/* AuthNavigator or MainNavigator */}
    </NavigationContainer>
  );
}
```

This approach helps maintain a clean separation between authenticated and unauthenticated user flows.

---

# Redux Toolkit Setup

Redux Toolkit simplifies global state management in React Native applications.

Example store configuration:

```ts
import { configureStore } from "@reduxjs/toolkit";
import authReducer from "./slices/authSlice";

export const store = configureStore({
  reducer: {
    auth: authReducer
  }
});
```

Example slice:

```ts
import { createSlice } from "@reduxjs/toolkit";

const authSlice = createSlice({
  name: "auth",
  initialState: {
    token: null,
    user: null
  },
  reducers: {
    loginSuccess(state, action) {
      state.token = action.payload.token;
      state.user = action.payload.user;
    },
    logout(state) {
      state.token = null;
      state.user = null;
    }
  }
});

export const { loginSuccess, logout } = authSlice.actions;
export default authSlice.reducer;
```

Benefits of Redux Toolkit:

* Simplified boilerplate
* Predictable state updates
* Easier debugging

---

# API Client with Axios

Centralizing API configuration helps maintain consistency across the application.

```ts
import axios from "axios";

const apiClient = axios.create({
  baseURL: "https://api.example.com",
  timeout: 5000
});

apiClient.interceptors.request.use(config => {
  const token = "sample_token";

  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }

  return config;
});

export default apiClient;
```

Advantages:

* Centralized API configuration
* Automatic token injection
* Easy error handling

---

# FlatList Performance Optimization

Large lists are common in mobile apps. React Native provides several optimizations.

Example implementation:

```tsx
const Item = React.memo(({ title }) => {
  return <Text>{title}</Text>;
});
```

Optimization techniques:

* React.memo for item components
* keyExtractor for stable keys
* initialNumToRender
* windowSize configuration
* pagination or lazy loading

---

# Pull To Refresh Implementation

Pull-to-refresh is widely used for updating lists.

Example:

```tsx
const [refreshing, setRefreshing] = useState(false);

const onRefresh = async () => {
  setRefreshing(true);

  await fetchData();

  setRefreshing(false);
};
```

Used with:

```
RefreshControl
```

This improves user experience by allowing manual data refresh.

---

# Deep Linking Example

Deep linking allows users to open specific screens from external URLs.

Example configuration:

```ts
const linking = {
  prefixes: ["myapp://"],
  config: {
    screens: {
      Home: "home",
      Offers: "offers",
      Profile: "profile"
    }
  }
};
```

Common use cases:

* Promotional links
* Push notification navigation
* External integrations

---

# Key Concepts Demonstrated

This repository demonstrates common React Native engineering practices:

* Modular project architecture
* Global state management with Redux
* API layer abstraction
* Navigation separation
* List performance optimization
* Deep linking configuration

---

# Purpose of This Repository

This repository is intended as a knowledge reference and architectural guideline for building scalable React Native applications.

It focuses on implementation patterns rather than a complete application.

---
