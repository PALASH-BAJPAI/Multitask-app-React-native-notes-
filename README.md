
# REACT NATIVE




## Initiate react native project

Install my-project with npm

```bash
    npm install -g expo-cli
    expo init myproject
    cd myproject
```
Run project 

```bash
    expo start / npm start 
```
Types of templates available:

1. blank-> for basic template(preferred)
2. tabs -> use for navigation
3. minimal-> for routing

Notes:-
* If LAN not works, change to tunnel
* Can even run on web browser
* React native works by building component tree
* App.js is page shown at screen



## Our first component

1.BasicComponentScreen.jsx
```bash
    import React from 'react';
    import {Text,StyleSheet,View} from 'react-native';

    const BasicComponent=()=>{
        // Use of variables 
        const name="User";
        return(
            <View>
            <Text>
                This is first Component
            </Text>
            <Text>
                Hello {name}
            </Text>
            </View>
        )
    };

    const styles=StyleSheet.create({
        textStyle: {
            fontSize:30,
        }
    })

    export default BasicComponent;
```

* React library manages how different components will work together
* React-native contains the component
* To use any component call it in App.js

### Styles
* Here styles are not inherited by children
* StyleSheet.create() validates our styling and shows error messages when required, while inline styling not shows errors.

### Rules of JSX
* JSX is converted to basic HTML and javascript using babel compiler
* We can configure different JSX elements using props
* We can refer JS variables inside of JSX block by using curly braces
* We can assign JSX elements to a variable , then show that variable inside of JSX block
* Return can return only one component, therefore we wrap all components within <View> element






## List

* FlatList element turns an array into list of elements
* Two props 1) prop of data : array to send.   2)renderItem : function returning item to show
* Using Flatlist instead of map function brings optimization
* See docs for: hroizontal scroll , indicator bar etc
* Can give key using keyExtractor function.
* If list extends page, scroll is activated itself

2.ListScreen.jsx
```bash
    const ListScreen=()=>{
        const friends=[
            {name:'Aman',key:'1'},
            {name:'Binod',key:'2'},
            {name:'Chintu',key:'3'},
            {name:'Deepak',key:'4'}
        ];
        return  <View>
            <FlatList
            keyExtractor={friend=>friend.key}
            data={friends}
            renderItem={({item})=>{
                return <Text>{item.name}</Text>
            }}
            />
        </View>
    }
```
## Button
Two types of clickable components
1. Button :- very basic component for showing a button and detecting a press
2. TouchableOpacity :- highly customizable component that can detect press on just any kind of element

```bash
    <Button
    onPress={ () => {console.log("Pressed")} }
    title="Submit"
    >

    <TouchableOpacity
    onPress={()=> console.log("Pressed")}
    >
    <Text>Go to next page</Text>
    </TouchableOpacity>

```
## Navigation

3 types:-
1. Stack
2. Drawer
3. Tab

* See its documentation (react native navigation, a third party lib) and use latest version.
* Included in app.js

```bash
    import * as React from 'react';
    import { View, Text } from 'react-native';
    import { NavigationContainer } from '@react-navigation/native';
    import { createNativeStackNavigator } from '@react-navigation/native-stack';

    function HomeScreen() {
    return (
        <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>Home Screen</Text>
        </View>
    );
    }

    const Stack = createNativeStackNavigator();

    function App() {
    return (
        <NavigationContainer>
        <Stack.Navigator>
            <Stack.Screen name="Home" component={HomeScreen} />
        </Stack.Navigator>
        </NavigationContainer>
    );
    }

    export default App;

```

### Nsting Navigation
When we want to include more than one type of navigation. 
Remember to install all required dependencies first.

```bash
    function Root() {
    return (
        <Drawer.Navigator>
        <Drawer.Screen name="Home" component={Home} />
        <Drawer.Screen name="Profile" component={Profile} />
        <Stack.Screen name="Settings" component={Settings} />
        </Drawer.Navigator>
    );
    }

    function App() {
    return (
        <NavigationContainer>
        <Stack.Navigator>
            <Stack.Screen
            name="Root"
            component={Root}
            options={{ headerShown: false }}
            />
            <Stack.Screen name="Feed" component={Settings} />
        </Stack.Navigator>
        </NavigationContainer>
    );
    }
```


## Images

```bash
import {Image} from 'react-native';
<Image source={ require('../assets/Images/a.jpg') } />
```

## Reusable Components


Make components in Components folder, data is send from parent to components via props

Components/Post.jsx
```bash
    import React from 'react';
    import {Text, View,Image} from 'react-native';

    const Post = (props) =>{
        console.log(props.imagePath)
        return(
            <View>

                <Image 
                    source={props.imagePath}
                    style={{width:100, height:100}}
                />
            <Text>{props.title}</Text>
            </View>
        )
    }

    export default Post;
```

PropsScreen.jsx
```bash
    import Post from '../Components/Post';
    <Post imagePath={item.link} title={item.title}/>
```


## States

* Props:- System to pass data from parent to child
* State:- System to track a piece of data that willl change over time, if data changes our  app will rerender

4.StateScreen.jsx
```bash
    import React,{useState} from 'react';
    import {View, Text,Button} from 'react-native';

    const StateScreen=()=>{
        const [counter,setCounter]= useState(0);
        return(
            <View style={{flex:1,backgroundColor: 'white'}}>
                <Button 
                onPress={()=>{setCounter(counter+1)}}
                title="Increase"
                />
                <Button 
                onPress={()=>{setCounter(counter-1)}}
                title="Decrease"
                />
                <Text style={{fontSize:30,textAlign: 'center'}}>{counter}</Text>
            </View>
        )

    }
    export default StateScreen;
```
1. setCounter basically rerender StateScreen with updated value of counter
2. State variable can be passed to child component,, now here state variable will be treated as props.

### State with reusable component

* Generally we create state variables in the most parent component  that need to read or change a state value.
* If child  needs to read a state value, parent can pass it as props.
* If a child needs to change a state value, the parent can pass down a callback function to change state value as prop.

```bash
Parent:
    const [red,setRed] = useState(0);
    return(
    <Counter color="red
    onIncrease={()=>setRed(red+1)}
    onDecrease={()=>setRed(red-1)}
    >
    )
    

Child:
    const Counter=({color,onIncrease,onDecrease})=>{
    return (
    <View>
        <Button
        title={`Increase ${color}`}
        onPress={()=>onIncrease()}
        />
        <Button
        title={`Decrease ${color}`}
        onPress={()=>onDecrease()}
        />
    </View>
    )
    }




```

## Text Input
We have to use states to save value that has been entered in TextInput.

5.InputScreen.jsx
```bash
    import {TextInput} from 'react-native'

    const [name, setName]=useState('');
    return(
        <View>
            <Text>Enter Name:</Text>
            <TextInput 
            autoCorrect={false}
            value={name}
            onChangeText={newValue=>{setName(newValue)}} />
            <Text>My name is {name}</Text>

        </View>
    )

```


### Conditional rendering
We can use {} to render a component on specific conditions.

```bash
<TextInput value={password}
onChangeText={(newValue)=>setPassword(newValue)}
>

{password.length < 5 ? <Text> password should be longer than 5 digits </Text> : null}
```







## Reducers

We use {useReducer}, it is a hook. Hooks add functionality to functional components.

When to use:- 

1.We have set of states which are extremely related.

2.There is precise set of well known ways in which we update this values.



* It is a function that gets called with 2 objects.
* Argument 1: Object that has all of our states in it.
* Argument 2: Object that describes updates we want to make.
* We never change argument 1 directly, we must always return a value to be used as argument 1.
* Define reducer outside functional component.
* Dispatch is usd to call reducer.


6.ReducerScreen.jsx
```bash
    const reducer=(state,action)=>{
        switch(action.type){
            case 'firstValue':
                return {...state, first:state.first+action.amount}
            case 'secondValue':
                return {...state, second:state.second+action.amount}
            default:
                return state;
        }
    }

    const ReducerScreen=()=>{
        const [state,dispatch]= useReducer(reducer,{first:1,second:1});
        const {first,second}=state;
        const sum = first+second;
        const diff = first-second;

        return (
            <View>
                <Text>X+Y / X-Y</Text>
                <Text>{first} + {second} = {sum}</Text>
                <Text>{first} - {second} = {diff}</Text>
                <Button
                title="Increase X"
                onPress={()=>dispatch({type:"firstValue",amount:1})}
                />
                <Button
                title="Decrease X"
                onPress={()=>dispatch({type:"firstValue",amount:-1})}
                />
                <Button
                title="Increase Y"
                onPress={()=>dispatch({type:"secondValue",amount:1})}
                />
                <Button
                title="Decrease Y"
                onPress={()=>dispatch({type:"secondValue",amount:-1})}
                />
            </View>
        )
    }
```





