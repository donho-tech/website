+++
date = 2020-08-19T19:55:18+02:00
title = "Use an HTML Select with React"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++
Learn how to use a HTML select with React and TypeScript.

What do you need to complete this tutorial?
* npm

Create a new React project

```bash
npx create-react-app react-select --template typescript
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
      <select value={pet} onChange={e => setPet(e.target.value)}>
        <option value="dog">Dog</option>
        <option value="cat">Cat</option>
        <option value="mouse">Mouse</option>
      </select>
    </div>
  );
}

export default App;
```

And there you have it, a beautiful React app which handles your select events.

## Further reading

On HTML Select at developer.mozilla.org <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/select>
