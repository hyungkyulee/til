# Major Packages Setup and How-to for Webapp project

## React Core
### Use State

Create Context and set a provider
```
import React, {
  useState, 
  createContext, 
  useContext
} from 'react'

const JwtContext = createContext();
const JwtProvider = props => {
    const [jwt, setJwt] = useState('')
    return <JwtContext.Provider value={[jwt, setJwt]} {...props} ></JwtContext.Provider>
};

const GetJwt = () => useContext(JwtContext)[0];

export { 
  JwtContext,
  GetJwt,
  JwtProvider
}
```

```
...
import { JwtContext } from '../Infrastructure/JwtStore'
...

const Main = () => {
  const history = useHistory()
  const [username, setUsername] = useState('')
  const [password, setPassword] = useState('')
  const [jwt, setJwt] = useContext(JwtContext)
  const { addToast } = useToasts()

  useEffect(() => {
    (async () => {
      console.log('[login] >> jwt: ', jwt)
      
    })()
  }, [])
```

---
## Community Package
---
### Fontawesome
```
npm i --save @fortawesome/fontawesome-svg-core
npm install --save @fortawesome/free-solid-svg-icons
npm install --save @fortawesome/react-fontawesome

[additional]
npm install --save @fortawesome/free-brands-svg-icons
npm install --save @fortawesome/free-regular-svg-icons
```
> brand icons: https://fontawesome.com/v5.15/icons?d=gallery&p=2&s=brands&m=free

> ref : https://fontawesome.com/v5.15/how-to-use/on-the-web/using-with/react


### React Toast
```
npm i react-toast-notification --save
```

provider setup on App.js
```
...
import { JwtProvider } from './Infrastructure/store'
import { ToastProvider } from 'react-toast-notifications'

export default function App() {
  return (
    <Router>
      <JwtProvider>
        <ToastProvider>
        ...
        # page components
...
```
        
use in each page
```
import { useToasts } from 'react-toast-notifications'
...

const Main = () => {
  ...
  const { addToast } = useToasts()
```
> ref: https://github.com/jossmac/react-toast-notifications

### 
