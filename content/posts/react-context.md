+++
date = 2021-06-18
title = "How to use React Context"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++
Learn how to use a React context to pass data to subcomponents.

What do you need to complete this tutorial?
* npm

Create a new React project

```bash
npx create-react-app context --template typescript
```

Replace `src/App.tsx` with the following

```typescript jsx
import * as React from 'react';
import {useContext} from 'react';
import './App.css';

interface User {
name: string;
}

const UserContext = React.createContext<User>({name: 'anonymous'})

const Greeting: React.FC = () => {
const user = useContext(UserContext)
return <div>Hello {user.name}!</div>
}

function App() {

const user: User = {
name: 'Clara',
}

return (
<div className="App">
{/* Uses default value 'anonymous' */}
<Greeting/>

      <UserContext.Provider value={user}>
        {/* Uses provided value 'Clara' */}
        <Greeting/>
      </UserContext.Provider>
    </div>
);
}

export default App;
```


Start it and open <a href="http://localhost:3000/">http://localhost:3000/</a> to see the different greetings.

## Further reading

Uses cases for context include

<https://kentcdodds.com/blog/authentication-in-react-applications>
<https://kentcdodds.com/blog/compound-components-with-react-hooks>
More about React context: <https://reactjs.org/docs/context.html>

