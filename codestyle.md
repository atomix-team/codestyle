# CodeStyle

## Main rule

There is no goal to overcontrol you and your code solutions.

Our main purpose to decrease code-review costs and reach consistent code 🤙

## 1. React

### 1.1 Access to internal resources

Its recommended not to use React namespace to access inner hooks and etc.:

> **Why?**
>
> Namespace is redundant here, because React has consistent and not-frequently-changing API ~~:D~~

```diff
# Bad
- import * as React from "react";
- ...
- const [...] = React.useState(...);
- ...
- React.useEffect(() => {
- ...
# Good
+ import React, { useState, useEffect } from "react";
+ ...
+ const [...] = useState(...);
+ ...
+ useEffect(() => {
+ ...
```

### 1.2 Hooks usage

Its recommended to [use convinient way](https://reactjs.org/docs/hooks-rules.html#only-call-hooks-from-react-functions) of hooks calling:

- Not in conditions or cycles
- Only at start of component
- Only in function or other hooks
- Not in JSX (!!!)

> **Why?**
>
> Its good example of **"small, but not explicit code"**
>
> If you use, for example, in JSX - it won't has predictable behaviour in future logic (specially - after adding some conditions)
>
> Also - its more difficult to maintain

```diff
# Bad
- const Foo = () => (
-   <div>
-      {useSome("/bar") && <Bar>}
-      <button onClick={useEvent(() => model.some())}>
-   </div>
- )
# Good
+ const Foo = () => {
+   const bar = useSome("/bar");
+   const someEvent = useEvent(model.some);
+   return (
+     <div>
+        {bar && <Bar>}
+        <button onClick={() => someEvent()}>
+     </div>
+   )
+ }
# Also Good
+ const useFoo = (baz: numer) => {
+    return useBar(baz);
+ }
```

### 1.3 Implicit return

Use implicit return only for simple components

When you note, that component has started to increase - please add explicit return expression

```diff
# Bad
- const Foo = ({ one, two, three, four, five, six, seven, array }: FooProps) => (
-      <article>
-        <ul>
-          {array.map((item) => {
-               const handleClick = () => model.some(item.id);
- 
-               return (
-                 <li key={item.id} onClick={handleClick}>{item.name}</li>
-               )
-          })}
-        </ul>
-        ...
-        ...
-        ...
-        ...
-        ...
-        ...
-        ...
-      </article>
-    )
# OK
+ const Bar = ({ bar }: BarProps) => <span>{bar}</span>
# Good
+ const Baz = () => {
+    return (
+        <article>
+            ...
+        </article>
+    )
+ }
```

## 2. TypeScript

## 2.1 Types vs Interfaces

Please,

- Use `interface` for all general cases
- Use `type` for specific cases, only if there no way to use interfaces
  - (&, |, some types issues, ...)

## 3. API

### 3.1 API responses

API responses should be normalized and usable

If you have some troulbes with response format, then please discuss this with your backender

There should not be any hardcoding normalization processing on client-side!

*(do not confuse with `entities normalizing`)*

```diff
# Bad
- some_field: number;
- AnotherField: string;
- "yet-one-field": string[];
# Good
+ someField: number;
+ anotherField: string;
+ yetOneField": string[];
```
