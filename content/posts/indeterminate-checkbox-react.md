+++
date = 2021-06-19
title = "Indeterminate input checkbox with React"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++
Learn how to render an indeterminate checkbox with React.


What do you need to complete this tutorial?
* npm

Create a new React project

```
npx create-react-app indeterminate-checkbox --template typescript
```

Replace src/App.tsx with the following

```typescript jsx
import * as React from 'react';
import './App.css';
import {useState} from "react";

function App() {

const [selectAll, setSelectAll] = useState<boolean | null>(null)

return (
<div className="App">
Indeterminate checkbox:
<input type="checkbox"
checked={selectAll !== null ? selectAll : false}
ref={el => el &amp;&amp; (el.indeterminate = selectAll === null)}
onChange={() => setSelectAll(!selectAll)}
/>
</div>
);
}

export default App;
```


Start it and open <a href="http://localhost:3000/">http://localhost:3000/</a> to see your indeterminate checkbox.

<h2>Further reading</h2>

As you have seen, we set the indeterminate attribute programmatically via JavaScript. At the moment, setting it via HTML is not supported yet. Read more about it here: <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Input/checkbox#indeterminate">https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Input/checkbox#indeterminate</a>