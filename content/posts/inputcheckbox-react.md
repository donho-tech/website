+++
date = 2020-08-19T19:55:18+02:00
title = "Use an HTML Checkbox with React"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++
Learn how to use a HTML input checkbox with React and TypeScript.

What do you need to complete this tutorial?
* npm

Create a new React project

```bash
npx create-react-app react-checkbox --template typescript
```

Replace src/App.tsx with the following

```typescript jsx
import * as React from 'react';
import './App.css';

function App() {

const [selected, setSelected] = React.useState(false);

return (
<div className="App">
<span> Checkbox clicked: {selected ? 'yes' : 'no'}      </span>
<input type="checkbox" onChange={e => setSelected(e.target.checked)}/>
</div>
);
}

export default App;
```


And there you have it, a beautiful React app which handles your checkbox events.

## Further reading

On HTML Checkbox at developer.mozilla.org <https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/checkbox>