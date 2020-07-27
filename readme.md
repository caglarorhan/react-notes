**React Notes**

- jsx file
- scss file


**Most Needed Libraries**
- react
- react-dom
- react-redux
- react-router-dom
- redux
- redux-logger

**React Rulez**


**Naming Conventions**
 
 - Components are named like `component-name.component.jsx`
 - Style files are named like `component-name.style.scss`
 

**Special Syntax For SVG**

 - `import {ReactComponent as Logo} from "react-scripts"`;
 
 
**Init & Setup**

`npx create-react-app APPNAME` this will install react, react-dom and react-scripts


**The Job of React Developer**

- Decide on Components
- Decide the State and where it lives
- What changes when state changes

**File Structure is Important**

- build
- node_modules
- src
    - assets
    - components
        - component-name
            - componentname.component.jsx
            - componentname.styles.scss
    - pages
       - page_name
        - page-name.component.jsx
        - page-name.style.scss
    - redux (if you need a component to interact with main state, put the component's redux files here)
        - xyz
            - xyz.reducer.js
            - xyz.action.js
            - xyz.types.js   
            - xyz.utils.js 
       
**State**

- State is converted into  props while stepping down in to a component.       
- Whenever state changed all sub positioning (under the states position in component tree) rerendered (render method called again). This is useful when some content effects the state from inside related component.     

**Life Cycle Methods**

- componentDidMount
- componentDidUpdate
- componentWillUnmount
- shouldComponentUpdate (render oncesidir, render ve sonrasini tercihe bagli hle getirir (return true dondururse render devam eder, return false olursa render olmaz))


**SyntheticEvent**

React intercept the real event and make wanted changes about this event. 

- onChange is triggering synthetic event.


**Components**
 
 -  

**Routers**

    import {Switch, Route, Link} from 'react-router-dom'

 - Routers are part of react-router-dom
 - 'Switch', 'Route', 'Link' are most useful imports from react-router-dom
 - 'Switch' made only one route component match with the url
 - 'Route' is a routing to components and at least must have 3 properties
    - 'exact' (boolean to  match with  exact url pattern), default value is false
    - 'path' is the url pattern
    - 'component' is the target component route to 

Sample routing is like:

          <Switch>
              <Route exact path='/' component={HomePage}/>
              <Route exact path='/topics' component={TopicList}/>
              <Route exact path='/topics/:topicId' component={TopicDetail}/>
          </Switch>  

**NPM notes**
 - npm update (package.json daki bagimliliklari update eder (onlerinde ^ isareti olanlari))
 - npm audit fix (bagimliliklar dahil vulnerabilities olanlarin son surumlerine guncellenmesini saglar)
 

**GIT Notes**

- to clone an existing repo

    - go to the folder location you want to work on the terminal you use

         `git clone git@github.com:USERNAME/REPONAME.git LOCALFOLDERNAME`
- when you clone a repo to your local, your local 'remote' (target) value is the copied location, and it is called 'origin'.

    `git remote` returns `origin`    
    
    to change this first remove the origin
    `git remote remove origin`
   
   now `git remote` returns nothing ` `
    
   If you already have a repo or create new one, take it's SSH key address 
     `git@github.com:USERNAME/REPONAME.git`
   
   and to add new origin value
   `git remote add origin git@github.com:USERNAME/REPONAME.git`
   
   - remote is just a pointer to some repository that is not in your device
   
   now you can `push` or `pull` to `origin` and branch `master`
   
   `git push origin master`
   
   - after pulling a repository    run `npm install` to install dependencies, etc.
   
    
**Firebase Notes**
 - install firabase with npm/yarn
 - create a **firebase** folder under **src** folder
 - create **firebase.utils.js** in this firebase folder
 - import firebase libraries in this file
 
        import firebase from "firebase/app";
        import 'firebase/firestore';
        import 'firebase/auth';
        
        // add config data (apikeys etc) from firebase project to here
        
        const config = { ... }
        
        //initialize firebase with this config
        
        firebase.initializeApp(config);
        
        export const auth = firebase.auth();
        export const firestore = firebase.firestore();
        
        const provider = new firebase.auth.GoogleAuthProvider(); // For signing in with google
        provider.setCustomParameters({prompt: 'select account'});
        
        export sonct signInWithGoogle = ()=> auth.singInWithPopup(provider);
        
        export default firebase;
 
 - Now we can import  `auth` from `firebase.utils`
 
       import {auth} from '../../firebase/firebase.utils';
       
 So we can use all `auth` methods there. Like `auth.signOut()`, `auth.onAuthStateChanged()`, `signInWithPopup()`, `auth.googleAuthProvider()`;
 - Every time we query firebase, firebase return one of two objects even if nothing exist at from that query. **QueryReference** and **QuerySnapShot** . These objects can be either Document or Collection versions.
 
 **A typical firebase scenario is like:** 
 
 
  - **Reach data source** 
    
    ` const ref = firestore.doc(DOCUMENT_PATH)`
  
  - get target data with `.get()` method. Beware of that this method is async. So use it with await in a async function.
    
     `const snapShot = await ref.get()`
     
  - check if returning object has `.exist` true.
  
     `if(!snapShot.exist){}`
  - Put all ref, snapShot in a function to call later/anytime.
  
  - Initialize firebase with config parameter
  
    **SAMPLE FUNCTION**
    
        export const createUserProfileDocument = async (userAuth, additionalData)=>{
            if(!userAuth) return;
        const userRef = firestore.doc(`user/${userAuth.uid}`);
        const snapShot = await userRef.get();
        if(!snapShot.exists){
            const {displayName, email} = userAuth;
            const createdAt = new Date();
            try{
                await userRef.set({
                    displayName,
                    email,
                    createdAt,
                    ...additionalData
                })
            }catch(error){
                console.log(`Error creating user: ${error.message}`)
            }
            }
        return userRef;
        } 
    
    Above returned userRef has `onSnapShot()` method which has a callback with default snapShot argument. `snapShot` object has id, and `data()` method.
    
    Sample usage on componentDidMount lifecycle:
    
                componentDidMount() {
                     this.unsubscribeFromAuth = auth.onAuthStateChanged(async userAuth => {
                         if(userAuth){
                             const userRef = await createUserProfileDocument(userAuth);
                             userRef.onSnapshot(snapShot => {
                                 this.setState({
                                     currentUser: {
                                         id: snapShot.id,
                                         ...snapShot.data()
                                     }
                                 },()=>{
                                     console.log(this.state)
                                 });
             
                             });
             
                         }
                         this.setState({currentUser:userAuth})
             
                     })
                 }
         
        
  **Authentication**
    
  - firebase auth method return auth object
  
    `const auth = firebase.auth()`      
    auth has some methods which can be used for login (authentication). These are provide login UI also.
    
     ` const provider = new firebase.auth.GoogleAuthProvider();`
     
     ` provider.setCustomParameters({prompt: 'select_account'});`
     
     `export const signInWithGoogle = ()=> auth.signInWithPopup(provider);` // you can call this function from any buttons click event after importing it.
     
     Using auth method to sign out: 
     
     `auth.signOut() `
     
     other auth methods
        ` signUpWithEmailAndPassword({email, password})`
        ` signInWithEmailAndPassword({email, password})`
     
     
 
       
**Redux Notes**

Redux is a library to manage large states. Redux can work with other UI libraries. Redux best fit with React. Redux is useful for sharing data between components. Redux uses the Flux Pattern (Action->Dispatcher->Store->View). Redux Flow is coming from this pattern.
Redux allows react state to be more scalable.

Predictable state management using the 3 principles.

- Single source of truth (one single big object which describes entire state of the app)
- State is ready only 
- Changes using pure functions

**Redux Flow** 
 
 Action -> MIDDLEWARE -> Root Reducer -> Store ->REACT-> Dom Changes
 
- Before root reducer, there are other reducers listening their component's actions.
- Reducers are functions. Accept 2 parameters 1) currentState 2) action
- action.type and action.payload will used to recreate new state and reducer returns this new state.


**Root Reducer**

- Combine all reducers and export them as one single rootReducer object

**Store**

-Inside `store.js` import createStore and applyMiddleware from redux 
- Creates a store object and put (with applyMiddleware() method) all of them into that store. `store` came from `redux/store`
- We are going to give this store to teh Provider (from react-redux library) in `index.js` file `<Provider store={store}>`, by doing this, provider can include all components and take actions from these components to give them to reducers.
- In `store.js` file we can `import logger from 'redux-logger'` as a middleware to log and debug our code. 
- Setting middlewares that store expecting from redux is an array.



**Turkish Quick Note**
- redux redux-logger yukle
- logger middlewareleri dizi olarak biriktirsin
- store olustur, bunu yaparken createStore iki arguman alir birisi rootReducer digeri applyMiddleware fonksiyonu
- applyMiddleware diger middlewareleri toplayip rootReducer icin hazilar. Root reducer da tum bu reducerlari(middleware) toplayip tek bir obje yapar.
- redux bildigimiz gibi state isini daha dolaysiz sekilde kotariyor -ama daha kompleks bir yapisi var-
- State gereken componen icinde {connect} i react-redux icinden import ediyoruz
- Sonra bu komponenti `export default connect()(komponenetAdi)` ile connect higher functionu icinden gecirip sunuyoruz. connect bizi root-reducer'a ulastiriyor. Boylece state root-reducer oluyor. 
- logger log ekranina prev state, action, next state objelerinin degerlerini yazdiriyor.
- her reducer olusturuldugunda root reducer'a import edilmelidir.
- Bir komponentte state icinden gerekli bir parcayi kullanmak istersek react-redux icinden import edilen connect higher function componentini kullanip state icindeki gerekli kismi bizim calistigimiz komponente arguman olarak dondurecek sekilde kullaniyoruz.
- Genelde bu `mapStateToProps` degisken ismiyle `state`'ten gerekli kismi temsilen nesne olusturup,
 
    `const mapStateToProps = ({cart: {cartItems}}) =>({
         cartItems
     })`
    daha sonra bu nesneyi bizim komponent ile birlikte connect higher fonksiyonuna veriyoruz ve komponentimizi bu sekilde export ediyoruz (connect bize komponenti state icerigi girdisi ile geri veriyor)
    
    `const mapDispatchToProps = dispatch =>({
         toggleCartHidden: ()=> dispatch(toggleCartHidden())
     })`
    
    `export default connect(mapStateToProps)(CartDropdown);`
    Bu arada komponentimiz eski girdileri yerine `mapStateToProps` ile gonderilen `{cartItems}` nesnesini arguman olarak alabiliyor. Bu sadece bunu alacagi anlamina gelmiyor.

- **React Redux'ta component'ler store'a asla dogrudan ulasamazlar. `connect` bu iletisime aracilik eder.**

- `mapStateToProps` ile state'i alip props olarak komponente sunuyoruz. connect'in ilk argumani
- `mapDispatchToProps` ile state change tetikleniyor. connect'in ikinci argumani.

- Ornegin CartIcon komponentinde 
    `export default connect(mapStateToProps, mapDispatchToProps)(CartIcon);
`
- `connect` bizi komponente baglar.

- mapDispatchToProps dispatch adinda argumani alir ve geriye duz bir obje dondurmek zorundadir.
    - Bu objedeki her entry bizim komponent icin bir prop sayilir.
    - Her entry'nin key adi (kabul goren yaklasim olarak) aksiyon yaratici ile ayni isimde olmalidir. Isim verilmis bu key'ler bizim komponenete prop olarak gider ve komponent icinde dogrudan isimleriyle cagrilabilirler. (Event trgigger icinde vs.)
    - Entry nin value degeri ise genelde bir fonksiyon olur. Bu fonksiyon cagirildiginda bir aksiyon gonderir.
    - Bu fonksiyon bir obje donderir, type degeri aksiyonun adini tasir.
    - Asagidaki kodlar net bir sekilde gosteriyor
    
            const increment = () => ({ type: 'INCREMENT' })
            const decrement = () => ({ type: 'DECREMENT' })
            const reset = () => ({ type: 'RESET' })
            
            const mapDispatchToProps = dispatch => {
              return {
                // dispatching actions returned by action creators
                increment: () => dispatch(increment()),
                decrement: () => dispatch(decrement()),
                reset: () => dispatch(reset())
              }
            }

    - Boylece komponent props olarak bunlari alir ve dogrudan iceride kullanir.

        function Counter({ count, increment, decrement, reset }) {
          return (
            <div>
              <button onClick={decrement}>-</button>
              <span>{count}</span>
              <button onClick={increment}>+</button>
              <button onClick={reset}>reset</button>
            </div>
          )
        }
