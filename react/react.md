# React

## Context API
### 1. Provider-Consumer way (only for Class based components)
#### a. Creating context
* Create context.js at desired directory level, *feel free to choose desired filename
``` js
    import React from 'react'

    // can store any kind of data like string, bool, object, functions as per your need,
    // here i have taken object for this case
    const ExampleContext = React.createContext({
        isLoggedIn: false,
        authHandler: () => {}
    })
    export default ExampleContext
```
#### b. Providing context
* In nearest parent component, from which you want to pass state/props
```js
    import ExampleContext from './path_to_context_file'
    
    {....other code}

    // prop name should be value and it takes object as an arg,
    // here you can change and pass value, like from state, or after loading from an external source
    // everytime prop value changes, rerender will be triggered for all the components enclosed under the Provider
    // for functional component, do not use *this keyword, in case of passing functions in Provider
    <ExampleCode.Provider value={{ isLoggedIn: true, authHandler: this.loginHandler }}>
        <Component_in_which_you_want_to_pass_down_state_or_props />
    </ExampleCode.Provider>

```
#### c.i. Consuming context (1st way)
* In desired children component, in which you wish to consume passed down state/props from Provider
* With this you can only access value in return statement, but not in componentDidMount() or any other lifeCycle method
```js
    import ExampleContext from './path_to_context_file'

    {....other code}

    // you have to use a function and then return your desired jsx code from there
    render() {
        return (
            <ExampleCode.Consumer>
                {(ctx) => {
                    return (
                        <Header>
                            {ctx.isLoggedin ? <h1>LoggedIn!</h1> : <h1>Please authenticate!</h1>}
                        </Header>
                    )
                }}
            </ExampleCode.Consumer>
        )
    }

```

#### c.ii. Consuming context (2st way)
* You can use value wherever you want to use

```js
    import ExampleContext from './path_to_context_file'

    class Home extends Component {
        static contextType = ExampleContext

        componentDidMount() {
            // context keyword is provided by react and should be written like this
            // as always can change variable name
            const ctx = this.context
        }

        render() {
            return (
                ctx.isLoggedin ? <h1>LoggedIn!</h1> : <h1>Please authenticate</h1>
            )
        }
    }
```

### 2. useContext hook (only for functional components)
#### a. Provinding value
**same as above, wrap the parent component with Provider**

#### b. Consuming value
```js
    import { useContext } from 'react'    
    import ExampleContext from './path_to_context_file'

    const Demo = () => {
        const ctx = useContext(ExampleContext)
        
        return(
            <Sidebar>
                {ctx.isLoggedin ? <h1>LoggedIn!</h1> : <h1>Please authenticate!</h1>}
            </Sidebar>
        )
    }
```