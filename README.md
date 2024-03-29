# React Native Todo App

## Button Style

### TouchableOpacity

- A wrapper for making views respond properly to touches. On press down, the opacity of the wrapped view is decreased, dimming it.

- Props

  - style

  - activeOpacity: Determines what the opacity of the wrapped view should be when touch is active. Defaults to 0.2.

- ```jsx
  <TouchableOpacity>
    <Text style={styles.btnText}>Work</Text>
  </TouchableOpacity>
  ```

### TouchableHighlight

- A wrapper for making views respond properly to touches. On press down, the opacity of the wrapped view is decreased, which allows the underlay color to show through, darkening or tinting the view.

- ```jsx
  <TouchableHighlight
    onPress={() => console.log('pressed')}
    underlayColor='red'
    activeOpacity={0.5}
  >
    <Text style={styles.btnText}>Travel</Text>
  </TouchableHighlight>
  ```

### TouchableWithoutFeedback

- Do not use unless you have a very good reason. All elements that respond to press should have a visual feedback when touched.

- TouchableWithoutFeedback supports only one child. If you wish to have several child components, wrap them in a View. Importantly, TouchableWithoutFeedback works by cloning its child and applying responder props to it. It is therefore required that any intermediary components pass through those props to the underlying React Native component.

- ```jsx
  <TouchableWithoutFeedback>
    <Text style={styles.btnText}>Work</Text>
  </TouchableWithoutFeedback>
  ```

### Pressable

- If you're looking for a more extensive and future-proof way to handle touch-based input, check out the Pressable API.

- Pressable is a Core Component wrapper that can detect various stages of press interactions on any of its defined children.

- Props

  - hitSlop: Sets additional distance outside of element in which a press can be detected.

- ```jsx
  <Pressable onPress={onPressFunction}>
    <Text>I'm pressable!</Text>
  </Pressable>
  ```

### Active and Inactive Style

- Create `colors.js`

  - ```js
    export const theme = {
      bg: 'black',
      grey: '#3a3d40',
    };
    ```

- On `App.js`

  - ```jsx
    import { StatusBar } from 'expo-status-bar';
    import React, { useState } from 'react';
    import { StyleSheet, Text, View, TouchableOpacity } from 'react-native';
    import { theme } from './colors';

    export default function App() {
      const [working, setWorking] = useState(true);
      const travel = () => setWorking(false);
      const work = () => setWorking(true);
      return (
        <View style={styles.container}>
          <StatusBar style='auto' />
          <View style={styles.header}>
            <TouchableOpacity onPress={work}>
              <Text
                style={{
                  ...styles.btnText,
                  color: working ? 'white' : theme.grey,
                }}
              >
                Work
              </Text>
            </TouchableOpacity>
            <TouchableOpacity onPress={travel}>
              <Text
                style={{
                  ...styles.btnText,
                  color: working ? theme.grey : 'white',
                }}
              >
                Travel
              </Text>
            </TouchableOpacity>
          </View>
        </View>
      );
    }

    const styles = StyleSheet.create({
      container: {
        flex: 1,
        backgroundColor: theme.bg,
        paddingHorizontal: 20,
      },
      header: {
        flexDirection: 'row',
        justifyContent: 'space-between',
        marginTop: 100,
      },
      btnText: {
        fontSize: 40,
        fontWeight: '600',
      },
    });
    ```

## TextInput

- A foundational component for inputting text into the app via a keyboard. Props provide configurability for several features, such as auto-correction, auto-capitalization, placeholder text, and different keyboard types, such as a numeric keypad.

- The most basic use case is to plop down a TextInput and subscribe to the onChangeText events to read the user input. There are also other events, such as onSubmitEditing and onFocus that can be subscribed to.

- `placeholder`: The string that will be rendered before text input has been entered.

- `placeholderTextColor`: The text color of the placeholder string.

- `keyboardType`: The following values work across platforms: `default`, `number-pad`, `decimal-pad`, `numeric`, `email-address`, `phone-pad`

- `returnKeyType`: Determines how the return key should look. On Android you can also use `returnKeyLabel`.

  - The following values work across platforms: `done`, `go`, `next`, `search`, `send`

- `onSubmitEditing`: Callback that is called when the text input's submit button is pressed.

- `secureTextEntry`: If `true`, the text input obscures the text entered so that sensitive text like passwords stay secure. The default value is `false`. Does not work with `multiline={true}`.

- `multiline`: If `true`, the text input can be multiple lines. The default value is `false`. It is important to note that this aligns the text to the top on iOS, and centers it on Android. Use with `textAlignVertical` set to `top` for the same behavior in both platforms.

- `onChangeText`: Callback that is called when the text input's text changes. Changed text is passed as a single string argument to the callback handler.

- `autoCorrect`: If `false`, disables auto-correct. The default value is `true`.

- `autoCapitalize`: Tells TextInput to automatically capitalize certain characters. This property is not supported by some keyboard types such as name-phone-pad.

  - `characters`: all characters.

  - `words`: first letter of each word.

  - `sentences`: first letter of each sentence (default).

  - `none`: don't auto capitalize anything.

- ```jsx
  import { ..., TextInput } from 'react-native';
  ...

  export default function App() {
    ...
    const [text, setText] = useState('');
    const onChangeText = (payload) => setText(payload);
    return (
      <View style={styles.container}>
        ...
        <View>
          <TextInput
            placeholder={working ? 'Add a To Do' : 'Where do you want to go?'}
            onChangeText={onChangeText}
            value={text}
            returnKeyType='done'
            style={styles.input}
          />
        </View>
      </View>
    );
  }

  const styles = StyleSheet.create({
    ...
    input: {
      backgroundColor: 'white',
      paddingVertical: 8,
      paddingHorizontal: 15,
      borderRadius: 25,
      marginTop: 20,
      fontSize: 18,
    },
  });
  ```

## Add ToDos

- `onSubmitEditing`: Callback that is called when the text input's submit button is pressed.

- Add object to object

  - ```js
    Object.assign(
      {},
      obj,
      {newObj}
    );
    ```

- ```jsx
  export default function App() {
    ...
    const [toDos, setToDos] = useState({});
    ...
    const addTodo = () => {
      if (text === '') {
        return;
      }
      // save toDos
      // const newToDos = Object.assign({}, toDos, {
      //   [Date.now()]: { text, work: working },
      // });
      // ES6
      const newToDos = {
        ...toDos,
        [Date.now()]: { text, work: working },
      };
      setToDos(newToDos);
      // initialize input
      setText('');
    };
    return (
      ...
          <TextInput
            ...
            onSubmitEditing={addTodo}
          />
        ...
  ```

## ToDos List

- ```JSX
  ...
  import { ..., ScrollView } from 'react-native';

  export default function App() {
    ...
        <ScrollView style={styles.scrollView}>
          {Object.keys(toDos).map((key) => (
            <View key={key} style={styles.toDo}>
              <Text style={styles.toDoText}>{toDos[key].text}</Text>
            </View>
          ))}
        </ScrollView>
      </View>
    );
  }

  const styles = StyleSheet.create({
    ...,
    scrollView: {
      marginVertical: 15,
    },
    toDo: {
      backgroundColor: theme.toDoBg,
      marginBottom: 15,
      paddingVertical: 8,
      paddingHorizontal: 15,
      borderRadius: 8,
    },
    toDoText: {
      color: 'white',
      fontSize: 16,
      fontWeight: '500',
    },
  });
  ```

## Show ToDos depending on the category

- ```jsx
  <ScrollView style={styles.scrollView}>
    {Object.keys(toDos).map((key) =>
      toDos[key].working === working ? (
        <View key={key} style={styles.toDo}>
          <Text style={styles.toDoText}>{toDos[key].text}</Text>
        </View>
      ) : null
    )}
  </ScrollView>
  ```

## Save ToDos on a phone

- `AsyncStorage` - Expo API : An asynchronous, unencrypted, persistent, key-value storage API.

  - `Async Storage` can only store `string` data, so in order to store object data you need to serialize it first. For data that can be serialized to JSON you can use `JSON.stringify()` when saving the data and `JSON.parse()` when loading the data.

  - `expo install @react-native-async-storage/async-storage`

  - `import AsyncStorage from '@react-native-async-storage/async-storage';`

  - `AsyncStorage.setItem(key: string, value: string, [callback]: ?(error: ?Error, result: ?string) => void)`

  - `AsyncStorage.getItem(key: string, [callback]: ?(error: ?Error, result: ?string) => void)`

- ```jsx
  ...
  import AsyncStorage from '@react-native-async-storage/async-storage';

  const STORAGE_KEY = '@toDos';

  export default function App() {
    ...
    const saveToDos = async (toSave) => {
      try {
        await AsyncStorage.setItem(STORAGE_KEY, JSON.stringify(toSave));
      } catch (err) {
        console.log(err);
      }
    };
    const getToDos = async () => {
      try {
        const getToDos = await AsyncStorage.getItem(STORAGE_KEY);
        setToDos(getToDos != null ? JSON.parse(getToDos) : {});
      } catch (err) {
        console.log(err);
      }
    };
    useEffect(async () => {
      await getToDos();
    }, []);
    const addToDo = async () => {
      ...
      await saveToDos(newToDos);
      setText('');
    };
  ```

## Remove ToDo on a phone

- `Alert`: Launches an alert dialog with the specified title and message.

  - `AlertButtonStyle` (iOS): `default`, `cancel`, `destructive`
  - ```jsx
    Alert.alert(
      'Alert Title',
      'Alert Message',
      [
        {
          text:'Ask me later',
          onPress: () => console.log('Ask me later pressed'),
          style: 'ask'
        },
        {
          text: '',
          onPress: () => {}
        }
      ]
    );
    ```

- ```jsx
  ...
  import { ..., Alert } from 'react-native';
  import { FontAwesome5 } from '@expo/vector-icons';
  ...

  export default function App() {
    ...
    const removeToDo = async (key) => {
      Alert.alert(
        'Remove ToDo',
        'Are you sure to delete "' + toDos[key].text + '?"',
        [
          {
            text: 'Cancel',
          },
          {
            text: 'OK',
            style: 'destructive',
            onPress: async () => {
              const newToDos = { ...toDos };
              delete newToDos[key];
              setToDos(newToDos);
              await saveToDos(newToDos);
            },
          },
        ]
      );
    };
    return (
      ...
        <ScrollView style={styles.scrollView}>
          {Object.keys(toDos).map((key) =>
            ...
                <TouchableOpacity
                  onPress={() => removeToDo(key)}
                  style={styles.delBtn}
                >
                  <FontAwesome5 name='trash-alt' size={16} color={theme.gray} />
                </TouchableOpacity>
              ...
    );
  }

  const styles = StyleSheet.create({
    ...
    toDo: {
      ...
      flexDirection: 'row',
      alignItems: 'center',
      justifyContent: 'space-between',
    },
    ...
    delBtn: {
      paddingVertical: 8,
      paddingHorizontal: 16,
    },
  });

## Track the last active menu

- ```jsx
  ...
  const STORAGE_TRACK_KEY = '@work';

  export default function App() {
    ...
    const tracking = async (trackWorking) => {
      try {
        await AsyncStorage.setItem(
          STORAGE_TRACK_KEY,
          JSON.stringify(trackWorking)
        );
      } catch (err) {
        console.warn(err);
      }
    };
    const travel = () => {
      setWorking(false);
      tracking(false);
    };
    const work = () => {
      setWorking(true);
      tracking(true);
    };
  ```

## Complete ToDo

- `Checkbox`: `expo-checkbox` provides a basic boolean input element for all platforms.

  - `expo install expo-checkbox`

  - Props

    - `color` (ColorValue): The tint or color of the checkbox. This overrides the disabled opaque style.

    - `disabled` (boolean): If the checkbox is disabled, it becomes opaque and uncheckable.

    - `value` (boolean): Value indicating if the checkbox should be rendered as checked or not. Default value is false.

    - `onChange` (callback): Callback that is invoked when the user presses the checkbox.

    - `onValueChange` (callback): Callback that is invoked when the user presses the checkbox.

- ```jsx
  ...
  import { ..., TouchableWithoutFeedback } from 'react-native';
  import Checkbox from 'expo-checkbox';
  ...
  export default function App() {
    ...
    const completeToDo = async (toDoKey) => {
      const editToDos = { ...toDos };
      Object.keys(editToDos).forEach((key) => {
        if (key === toDoKey) {
          editToDos[toDoKey].isComplete = !editToDos[toDoKey].isComplete;
        }
      });
      setToDos(editToDos);
      await saveToDos(editToDos);
    };
    return (
      ...
                <Checkbox
                  style={styles.checkbox}
                  value={toDos[key].isComplete}
                  onValueChange={() => completeToDo(key)}
                  color={theme.grey}
                />
                <TouchableWithoutFeedback onPress={() => completeToDo(key)}>
                  <Text
                    style={{
                      ...styles.toDoText,
                      textDecorationLine: toDos[key].isComplete
                        ? 'line-through'
                        : 'none',
                      textDecorationStyle: 'solid',
                      color: toDos[key].isComplete ? theme.grey : 'white',
                    }}
                  >
                    {toDos[key].text}
                  </Text>
                </TouchableWithoutFeedback>
              ...
  const styles = StyleSheet.create({
    ...
    checkbox: {
      marginRight: 8,
    },
    ...
  ```

## challenge 3 - edit todo

- ```jsx
  ...
  export default function App() {
    ...
    const [editText, setEditText] = useState('');
    ...
    const onEditText = (payload) => setEditText(payload);
    ...
    const addToDo = async () => {
      ...
      const newToDos = {
        ...toDos,
        [Date.now()]: { ..., edit: false },
      };
      ...
    };
    ...
    const onEdit = (toDoKey) => {
      const editToDos = { ...toDos };
      Object.keys(editToDos).forEach((key) => {
        if (key === toDoKey) {
          if (!editToDos[toDoKey].edit) {
            setEditText('');
          }
          editToDos[toDoKey].edit = !editToDos[toDoKey].edit;
        }
      });
      setToDos(editToDos);
    };
    const editToDos = async (toDoKey) => {
      const editToDos = { ...toDos };
      Object.keys(editToDos).forEach((key) => {
        if (key === toDoKey) {
          editToDos[toDoKey].text = editText;
          editToDos[toDoKey].edit = false;
        }
      });
      setToDos(editToDos);
      await saveToDos(editToDos);
      setEditText('');
    };
    return (
      ...
                <Checkbox
                  ...
                />
                {toDos[key].edit ? (
                  <TextInput
                    placeholder={toDos[key].text}
                    onChangeText={onEditText}
                    value={editText}
                    onSubmitEditing={() => editToDos(key)}
                    returnKeyType='done'
                    style={styles.editInput}
                  />
                ) : (
                  <TouchableWithoutFeedback onPress={() => completeToDo(key)}>
                    ...
                  </TouchableWithoutFeedback>
                )}
                <TouchableOpacity
                  onPress={() => onEdit(key)}
                  style={styles.btn}
                  disabled={toDos[key].isComplete}
                >
                  <FontAwesome5
                    name='pencil-alt'
                    size={16}
                    color={toDos[key].isComplete ? 'transparent' : theme.grey}
                  />
                </TouchableOpacity>
              ...
  const styles = StyleSheet.create({
    ...
    editInput: {
      backgroundColor: 'white',
      borderRadius: 5,
      fontSize: 16,
      fontWeight: '500',
      width: '58%',
      paddingVertical: 8,
      paddingHorizontal: 8,
    },
    toDoText: {
      ...
      width: '58%',
      paddingVertical: 8,
      paddingHorizontal: 8,
    },
    btn: {
      paddingVertical: 8,
      paddingHorizontal: 16,
    },
    ...
  ```

## Publish App

- on Expo Developer Tool

  - Click `Publish or republish project`

  - Copy the url: https://expo.io/@jinpark1504/projects/react-work-travel

## Fix bugs related to React Native Web

- Style

  - use inline style instead of StyleSheet for Text

  - ```jsx
    
    <Text
      style={{
        ...styles.toDoText,
        fontSize: 16,
        fontWeight: '500',
        width: 'calc(100% - 24px - 48px - 46px)',
        paddingVertical: 8,
        paddingHorizontal: 8,
        textDecorationLine: toDos[key].isComplete
          ? 'line-through'
          : 'none',
        textDecorationStyle: 'solid',
        color: toDos[key].isComplete ? theme.grey : 'white',
      }}
    >
    ```

- Alert is not working on Web

  - ```jsx
    import { Platform } from 'react-native';

    const removeToDo = async (key) => {
      if (Platform.OS === 'web') {
        const ok = confirm('Are you sure to delete "' + toDos[key].text + '?"');
        if (ok) {
          const newToDos = { ...toDos };
          delete newToDos[key];
          setToDos(newToDos);
          saveToDos(newToDos);
        }
      } else {
        Alert.alert(...);
      }
    ```

## Splash page

- change it on `app.json`

  - Properties: https://docs.expo.dev/versions/v44.0.0/config/app/

## Build for App Store

- expo build a standalone app: https://docs.expo.dev/distribution/uploading-apps/

  - the building will happen on Expo server

## Publish Website

- `yarn add -D gh-pages`

- On `package.json`

  - ```json
    "homepage": "https://canadaprogrammer.github.io/react-native-work-travel",
    "scripts": {
      "deploy": "gh-pages -d web-build",
      "predeploy": "expo build:web"
    }
    ```

- `yarn deploy`

## Expo

- Using Expo, we can't access the infrastructure.

- Expo app is heavy because it has all API.

- You can use create react native app instead of expo.
