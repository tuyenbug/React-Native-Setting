# Comparing to Remote Debug JS
## 1. DevTools
    - Best for web apps 💻
    - Pros:
        + Built-in
        + console.log
        + Network support
        + Extendable (more powerful with extensions)
## 2.Remote Debug JS
    - React-Native build in solution 📱
    - Pros:
        + Built-in
        + console.log support
        + Able to inspect
        + Able to monitor performance
    - Cons:
        + Need to setup redux middleware to see redux actions
        + Need to setup standalone react-devtools for additional information about the tree
        + No network lookup
## 3. Reactotron
    - A tool created by infinitered 💻 📱
    - Pros:
        + Support network lookup
        + Support redux actions lookup (with the help of a plugin)
        + Track AsyncStorage operations
        + You can easily write your own plugin
    - Cons:
        + Have to setup it 😄
        + The configuration needed for console.log behavior like support
# How to setup
## 1.DevTools
    - It’s inside Chrome
## 2.Remote Debug JS
    - It’s already in the box, no need to set up.
## 3.Reactotron
    - Local:
    + Install Reactotron
        MacOS: install by brew
    - In-app:
    + This example will show you how to setup reactotron-react-native and reactotron-redux.
    + Create configuration file f.e. src/config/reactotron.ts
        import { NativeModules } from 'react-native';
        import Reactotron from 'reactotron-react-native';
        import { reactotronRedux } from 'reactotron-redux';
        import url from 'url';
        let reactotron;
        if (__DEV__) { // Check if it's development mode
        const { hostname } =
            url.parse(NativeModules.SourceCode.scriptURL);
            // Geting device hostname
        reactotron = Reactotron.configure({
            name: 'name',
            host: hostname,
            port: 9090,
        }) // Initial configuration 
            .useReactNative({}) // Appling React-Native plugin
            .use(reactotronRedux()) // Appling Redux plugin
            .connect(); // Connect to local client
        console.tron = Reactotron.log;
        // Extend console with tron for being able to debug to Reactotron
        
        export { reactotron };
        // export reactotron for later usage
        }
    + Setup reactotron middleware
        import { createStore, compose } from 'redux';
        import { reactotron } from 'src/config/reactotron';
        import reducers from './reducers';
        const middlewares = [];
        // Initialising a middlewares array, later on you can add a
        // saga middleware for example
        if (__DEV__) { // Check if it's development mode
        const reactotronMiddleware = reactotron.createEnhancer();
        // Creating Reactotron "data capturer"
        middlewares.push(reactotronMiddleware);
        // And pushing it to the middlewares array
        }
        const store = createStore(
        reducers,
        compose(...middlewares),
        ); // Creating a store with given configuration
        export default store;
        // Exporting the store since it will be needed in a Provider
    + That’s it!
## 3.Flipper
    - Local:
        + Install Flipper
            MacOS install by brew
# How to launch
## 1.DevTools
    - Open your browser f.e. Chrome and press OPTION/CTRL + SHIFT + I
## 2.Remote Debug JS
    - Open expo/react-native menu by shaking a device, CMD/CTRL + D
    - Click Debug Remote JS
    - A new tab in a browser will pop up with a console press CMD + SHIFT + I to open dev tools
## 3.Reactotron
    - Open Reactotron
    - Open App on Simulator