# Read and Write with AsyncStorage in React Native

- Account Preferences Settings for users.  
- This allows their users to set their preferences for communication purposes. 
- This screen contains three toggle buttons to enable specific communication channels, like push notifications, marketing emails, and news updates. 
- Use AsyncStorage module to preserve the chosen preferences even when the user quits the application

## Table of contents

- [Overview](#overview)
  - [Screenshot](#screenshot)
  - [Links](#links)
- [My process](#my-process)
  - [Built with](#built-with)
  - [What I learned](#what-i-learned)
  - [Useful resources](#useful-resources)
- [Author](#author)

## Overview

### Screenshot
![image](https://user-images.githubusercontent.com/108392678/215760264-7f9699de-fd9c-46cd-9ed1-ed7e9c65dcdf.png)
![image](https://user-images.githubusercontent.com/108392678/215760296-61548d5e-d6cd-4c44-bfdf-812e6ac97a26.png)


### Links

- Github: [Code](https://github.com/marvedventures/read-and-write-with-asyncstorage)

## My process

### Built with

- [React Native](https://reactnative.dev/docs/environment-setup) - React Native app built with expo
- [AsyncStorage](https://react-native-async-storage.github.io/async-storage/docs/api/) - For storing user preferences.
- [StyleSheet](https://reactnative.dev/docs/stylesheet) - For styles

### What I learned

- Create a React Native App using Expo
- Using AsyncStorage to store user settings.
- Using multiGet and multiSet methods to read and set data to AsyncStorage
- Connecting AsyncStorage to a state
- Handling side-effects using useEffect Hook
- Use View, View, Text Components
- Create a component to display menu items and use FlatList
- Extract all styles to StyleSheet API

Here is a code snippet:

```jsx
 const [preferences, setPreferences] = React.useState({
    pushNotifications: false,
    emailMarketing: false,
    latestNews: false,
  });

  React.useEffect(() => {
    (async () => {
      // Populate preferences from storage using AsyncStorage.multiGet
      try {
        const values = await AsyncStorage.multiGet(Object.keys(preferences));
        const initialState = values.reduce((acc, curr) => {
          // Every item in the values array is itself an array with a string key and a stringified value, i.e ['pushNotifications', 'false']
          acc[curr[0]] = JSON.parse(curr[1]);
          return acc;
        }, {});
        setPreferences(initialState);
      } catch (e) {
        Alert.alert(`An error occurred: ${e.message}`);
      }
    })();
  }, []);

  // This effect only runs when the preferences state updates, excluding initial mount
  useUpdate(() => {
    (async () => {
      // Every time there is an update on the preference state, we persist it on storage
      // use multiSet API to update all values in one API call
      const keyValues = Object.entries(preferences).map((entry) => {
        return [entry[0], String(entry[1])];
      });
      try {
        await AsyncStorage.multiSet(keyValues);
      } catch (e) {
        Alert.alert(`An error occurred: ${e.message}`);
      }
    })();
  }, [preferences]);
```

### Useful resources

- [React Native Docs (StyleSheet) ](https://reactnative.dev/docs/stylesheet) - This helped me for all the neccessary React Native styles. I really liked their documentation and will use it going forward.
- [Async Storage](https://react-native-async-storage.github.io/async-storage/docs/api/) - This helped me for saving user settings.
- [Object.entries()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) - This helped me for converting an object into array then converting a specific element to a string.
- [reduce() method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) - This helped me for extracting the key-value pair from each curr item and updates the acc object with this key-value pair. 

## Author

- Website - [Marvin Morales Pacis](https://marvin-morales-pacis.vercel.app/)
- LinkedIn - [@marvedventures](https://www.linkedin.com/in/marvedventures/)
- Twitter - [@marvedventures](https://www.twitter.com/marvedventures)
