+++
date = 2021-06-19
title = "Render a comma separated array with React and TypeScript"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++
Learn how to render a comma separated array of JSX elements with React and TypeScript

What do you need to complete this tutorial?
* npm

Create a new React project

```bash
npx create-react-app comma-separated-array --template typescript
```

Replace src/App.tsx with the following

```typescript jsx
import * as React from 'react';
import './App.css';

interface Person {
firstName: string;
lastName: string;
}

const PersonName: React.FC<Person> = ({firstName, lastName}) => {
return <span>{firstName} {lastName}</span>
}

function App() {
const persons = [
{firstName: 'John', lastName: 'Snow'},
{firstName: 'Daenerys', lastName: 'Targaryen'},
{firstName: 'Hodor ', lastName: 'Hodor '},
]

return (
<div className="App">
GoT characters:
{persons
.map(p => <PersonName firstName={p.firstName} lastName={p.lastName}/>)
.reduce<JSX.Element | null>((prev, cur) => prev === null ? cur : <>{prev}, {cur}</>, null)
}
</div>
);
}

export default App;
```


Start it and open <http://localhost:3000/> to see your list of objects rendered, separated with commas.

## Further reading

More about the Array.reduce function: <a href="https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce">https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce</a>