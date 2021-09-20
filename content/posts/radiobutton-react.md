+++
date = 2020-08-19T19:55:18+02:00
title = "Use an HTML Radio Button with React"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++
Learn how to use a HTML radio button with React and TypeScript.


What do you need to complete this tutorial?
* npm

Create a new React project

```
npx create-react-app react-radio --template typescript
```

Replace src/App.tsx with the following

```typescript jsx
import * as React from 'react';
import './App.css';

function App() {
    const [pet, setPet] = React.useState('dog');

    return (
        <div className="App">
            <div>My pet is a {pet}</div>

            <input type="radio" name="pet" value="dog" onChange={e => setPet(e.target.value)}/>
            <label htmlFor="dog">Dog</label><br/>
            <input type="radio" name="pet" value="cat" onChange={e => setPet(e.target.value)}/>
            <label htmlFor="cat">Cat</label><br/>
            <input type="radio" name="pet" value="mouse" onChange={e => setPet(e.target.value)}/>
            <label htmlFor="mouse">Mouse</label>
        </div>
    );
}

export default App;
```


And there you have it, a beautiful React app which handles your radio button events.

## Further reading

On HTML Select at developer.mozilla.org <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/radio">https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/radio</a>