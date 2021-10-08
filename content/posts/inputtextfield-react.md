+++
date = 2021-06-15
title = "Use an HTML input text field with React"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++
Learn how to use a HTML input text field with React and TypeScript.


What do you need to complete this tutorial?
* npm

Create a new React project

```bash
npx create-react-app react-textfield --template typescript
```

Replace src/App.tsx with the following

```typescript jsx
import * as React from 'react';
import './App.css';

function App() {
const [name, setName] = React.useState('Peter');

return (
<div className="App">
<div>Hello {name}!</div>

      <label htmlFor="name">Name</label>
      <input type="text" name="name" value={name} onChange={e => setName(e.target.value)}/>
    </div>
);
}

export default App;
```

And there you have it, a beautiful React app which handles your input text field.

<h2>Further reading</h2>

On HTML text field at developer.mozilla.org <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/text>