# Full-Stack Practice Exam — 150 Questions

> Multiple-choice questions in the exact style of the real exam: "What is the output?",
> "Will this code work as intended?", "Which change achieves X?", "In which lifecycle
> phase does this run?".
>
> Every question has a hidden answer — click **"Show answer"** to reveal it.
> Based ONLY on the theory PDFs in this repo (Front-End 1–5, Back-End 1–4).

**Sections**

1. [JavaScript fundamentals (Q1–Q20)](#section-1--javascript-fundamentals)
2. [TypeScript (Q21–Q35)](#section-2--typescript)
3. [Jest (Q36–Q45)](#section-3--testing-with-jest)
4. [React basics, state & lifecycle (Q46–Q70)](#section-4--react-basics-state--lifecycle)
5. [Forms & controlled inputs (Q71–Q78)](#section-5--forms--controlled-inputs)
6. [Next.js, server/client components, rendering (Q79–Q100)](#section-6--nextjs-servwith-client-components--rendering)
7. [Context, custom hooks, storage, i18n (Q101–Q115)](#section-7--context-custom-hooks-storage-i18n)
8. [Front-end & back-end security (Q116–Q130)](#section-8--security)
9. [Java, Spring, HTTP, DTOs (Q131–Q140)](#section-9--java-spring-http-dtos)
10. [JDBC, JPA, transactions, relationships (Q141–Q150)](#section-10--jdbc-jpa-transactions-relationships)

---

# Section 1 — JavaScript fundamentals

### Q1. What is the output of the following code?

```javascript
const user = { name: "A" };
user.name = "B";
console.log(user.name);
```

- A. A compilation error: you are not allowed to change the value of a const variable.
- B. `"A"`
- C. `"B"`
- D. A runtime error: you are not allowed to change the value of a const variable.

<details><summary>Show answer</summary>

**C. `"B"`** — `const` prevents *reassignment of the binding*, not *mutation of the object*. Changing a property of a `const` object is perfectly legal. (Also note: plain JavaScript has no compilation step, so "compilation error" is never the answer for JS.)
</details>

### Q2. What is the output of the following code?

```javascript
const user = { name: "A" };
user = { name: "B" };
console.log(user.name);
```

- A. `"B"`
- B. `"A"`
- C. A runtime error: assignment to constant variable.
- D. `undefined`

<details><summary>Show answer</summary>

**C.** Reassigning the *binding itself* of a `const` throws a **TypeError at runtime** ("Assignment to constant variable"). Contrast with Q1: mutating a property is fine, reassigning the variable is not.
</details>

### Q3. What is the output of the following code?

```javascript
const arr = [1, 2];
arr.push(3);
console.log(arr.length);
```

- A. A runtime error: `arr` is a const.
- B. `2`
- C. `3`
- D. `undefined`

<details><summary>Show answer</summary>

**C. `3`** — same rule as Q1: a `const` array can be mutated (`push`, `pop`, index assignment); only `arr = ...` would fail.
</details>

### Q4. What is the output of the following code?

```javascript
const identity = { name: "John", age: 45 };
const { myName, myAge } = identity;
console.log(myName);
```

- A. `"John"`
- B. `undefined`
- C. A runtime error.
- D. `null`

<details><summary>Show answer</summary>

**B. `undefined`** — object destructuring matches **by property name**. `identity` has no property `myName`, so the new variable is `undefined`. To rename you must write `const { name: myName } = identity;`.
</details>

### Q5. What is the output of the following code?

```javascript
const identity = { name: "John", age: 45 };
const { name: myName } = identity;
console.log(myName);
```

- A. `undefined`
- B. `"John"`
- C. `"name"`
- D. A syntax error.

<details><summary>Show answer</summary>

**B. `"John"`** — this is the correct renaming syntax: take property `name`, put it in variable `myName`.
</details>

### Q6. What is the output of the following code?

```javascript
const males = ["John", "Frank"];
const [a, b] = males;
console.log(b);
```

- A. `"John"`
- B. `"Frank"`
- C. `undefined`
- D. `["John", "Frank"]`

<details><summary>Show answer</summary>

**B. `"Frank"`** — array destructuring is **by position** (unlike object destructuring, which is by name). `a = "John"`, `b = "Frank"`.
</details>

### Q7. What is the output of the following code?

```javascript
const multiply = (a, b) => { a * b; };
console.log(multiply(2, 3));
```

- A. `6`
- B. `undefined`
- C. `NaN`
- D. A syntax error.

<details><summary>Show answer</summary>

**B. `undefined`** — the arrow function has a **block body** (`{ }`) but no `return` statement, so it returns `undefined`. `(a, b) => a * b` (no braces) would return `6` implicitly.
</details>

### Q8. What is the output of the following code?

```javascript
const calculate = (a, b, operation) => operation(a, b);
const sum = (a, b) => a + b;
console.log(calculate(2, 3, sum));
```

- A. `5`
- B. `undefined`
- C. `"sum"`
- D. A runtime error: `operation` is not a function.

<details><summary>Show answer</summary>

**A. `5`** — `calculate` is a **higher-order function**; `sum` is passed as a **callback** (a reference, not a call) and invoked inside.
</details>

### Q9. What happens when you run this?

```javascript
const calculate = (a, b, operation) => operation(a, b);
const sum = (a, b) => a + b;
console.log(calculate(2, 3, sum(2, 3)));
```

- A. `5`
- B. `10`
- C. A runtime error: `operation` is not a function.
- D. `undefined`

<details><summary>Show answer</summary>

**C.** `sum(2, 3)` is **called immediately** and evaluates to `5`, so `calculate` receives the number `5` as `operation` and then tries `5(2, 3)` → TypeError. Pass the function *reference* (`sum`), not a call.
</details>

### Q10. What is the output of the following code?

```javascript
let males = ["John", "Frank"];
let females = ["Annie"];
let people = [...males, ...females];
console.log(people.length);
```

- A. `2`
- B. `3`
- C. `1`
- D. `[["John","Frank"],["Annie"]]` has length 2.

<details><summary>Show answer</summary>

**B. `3`** — the **spread operator** flattens both arrays into a new one: `["John", "Frank", "Annie"]`.
</details>

### Q11. What is the output of the following code?

```javascript
const persons = [{ name: "John", age: 45 }, { name: "Annie", age: 23 }];
const result = persons.filter((p) => p.age > 25);
console.log(result.length);
```

- A. `0`
- B. `1`
- C. `2`
- D. `45`

<details><summary>Show answer</summary>

**B. `1`** — `filter` returns a **new array** with the elements for which the predicate is true; only John (45) matches.
</details>

### Q12. What is the output of the following code?

```javascript
const result = [1, 2, 3].map((n) => n * 2);
console.log(result);
```

- A. `[2, 4, 6]`
- B. `[1, 2, 3]`
- C. `6`
- D. `undefined`

<details><summary>Show answer</summary>

**A. `[2, 4, 6]`** — `map` returns a **new array** of the results of the callback applied to every element of the original array.
</details>

### Q13. What is the output of the following code?

```javascript
const result = [1, 2, 3].forEach((n) => n * 2);
console.log(result);
```

- A. `[2, 4, 6]`
- B. `[1, 2, 3]`
- C. `undefined`
- D. A syntax error.

<details><summary>Show answer</summary>

**C. `undefined`** — unlike `map`, `forEach` executes the callback per element but **returns nothing**. Classic `map` vs `forEach` trap.
</details>

### Q14. What is the output of the following code?

```javascript
const age = 17;
const state = age < 18 ? "Child" : "Adult";
console.log(`You are a ${state}`);
```

- A. `You are a Child`
- B. `You are a Adult`
- C. `You are a ${state}`
- D. A syntax error.

<details><summary>Show answer</summary>

**A.** The ternary evaluates to `"Child"` (17 < 18) and the **template literal** (backticks + `${}`) interpolates it.
</details>

### Q15. What is the output of the following code?

```javascript
const value = 0;
console.log(value ?? 42);
console.log(value || 42);
```

- A. `42` then `42`
- B. `0` then `0`
- C. `0` then `42`
- D. `42` then `0`

<details><summary>Show answer</summary>

**C. `0` then `42`** — `??` (nullish coalescing) only falls back on `null`/`undefined`, so `0` survives. `||` falls back on **any falsy value**, and `0` is falsy.
</details>

### Q16. What is the output of the following code?

```javascript
const items = [{ id: 1 }];
const found = items.find((i) => i.id === 2);
console.log(found);
```

- A. `null`
- B. `undefined`
- C. `{}`
- D. A runtime error.

<details><summary>Show answer</summary>

**B. `undefined`** — `find()` returns **`undefined`** when nothing matches (not `null`!). That's why the course pattern is `items.find(predicate) ?? null` when a method must return `null`.
</details>

### Q17. Which of the following values is truthy?

- A. `""`
- B. `0`
- C. `[]`
- D. `NaN`

<details><summary>Show answer</summary>

**C. `[]`** — an empty array (and an empty object `{}`) is **truthy**. Falsy values are exactly: `false`, `0`, `''`, `null`, `undefined`, `NaN`.
</details>

### Q18. What is true about this function?

```javascript
const getData = async () => {
  return 42;
};
```

- A. It returns the number 42.
- B. It returns a Promise that resolves to 42.
- C. It returns undefined.
- D. It throws, because there is no await inside.

<details><summary>Show answer</summary>

**B.** An **`async` function always returns a Promise** — the return value gets wrapped. To read the 42 you must `await getData()` (inside another async function).
</details>

### Q19. What are the three states of a Promise?

- A. created, running, done
- B. pending, fulfilled, rejected
- C. open, resolved, closed
- D. mounted, updated, unmounted

<details><summary>Show answer</summary>

**B. pending, fulfilled, rejected.** (`await` pauses until fulfilled, and **throws** when the Promise rejects. D is the React component lifecycle — different list!)
</details>

### Q20. Where may `await` be used?

- A. Anywhere.
- B. Only inside an `async` function.
- C. Only inside a `try` block.
- D. Only at the top of a file.

<details><summary>Show answer</summary>

**B.** `await` can only be used **inside an `async` function** (per the course slides).
</details>

---

# Section 2 — TypeScript

### Q21. Which statement about TypeScript vs JavaScript is correct?

- A. TypeScript is interpreted, JavaScript is compiled.
- B. TypeScript errors are detected only at runtime.
- C. TypeScript is strongly typed and must be compiled to JavaScript.
- D. Browsers execute TypeScript directly.

<details><summary>Show answer</summary>

**C.** TypeScript = strongly typed, supports static typing, and is **compiled** by `tsc` to JavaScript. JavaScript = loosely/dynamically typed, interpreted, errors only at runtime. Browsers **cannot** run TS directly.
</details>

### Q22. Given this function:

```typescript
const demo = ({ x }: { x: number }): string => `x is ${x}`;
```

Which call compiles?

- A. `demo(2)`
- B. `demo({ 2 })`
- C. `demo({ x: 2 })`
- D. `demo({ x: 2, name: "foo" })`

<details><summary>Show answer</summary>

**C.** The parameter is an **object with a field `x: number`**. A: a raw number. B: invalid object-literal syntax. D: **excess property check** — object literals may not contain properties that aren't in the type.
</details>

### Q23. In `const demo = ({ x }: { x: number }): string => ...`, what does the final `: string` mean?

- A. The type of `x`.
- B. The type of the parameter object.
- C. The return type of the function.
- D. The name of the function.

<details><summary>Show answer</summary>

**C.** Annotation after the parameter list = **return type**. `{ x: number }` is the parameter type; `{ x }` is destructuring.
</details>

### Q24. What is the output?

```typescript
class Animal {
  name: string;
  age?: number;
  constructor(a: { name: string; age?: number }) {
    this.name = a.name;
    this.age = a.age;
  }
}
const cat = new Animal({ name: "Minou" });
console.log(cat.age);
```

- A. `null`
- B. `undefined`
- C. `0`
- D. A compile error: age is required.

<details><summary>Show answer</summary>

**B. `undefined`** — `?` makes the property optional; when omitted its value is **`undefined`** (not `null`). This exact check is one of the lab's required tests.
</details>

### Q25. What is the purpose of the return type `Product | null` on `removeProduct(id: number)`?

- A. It means the product can have a null id.
- B. It is a union type: the method returns the removed product, or null when not found.
- C. It makes the parameter optional.
- D. It is invalid TypeScript.

<details><summary>Show answer</summary>

**B.** A **union type** handles the "not found" case: found → the `Product`; not found → `null`.
</details>

### Q26. Which is the correct TypeScript type for a predicate callback over cart items?

- A. `predicate: boolean`
- B. `predicate: Function`
- C. `predicate: (item: CartItem) => boolean`
- D. `predicate: callback<CartItem>`

<details><summary>Show answer</summary>

**C.** A function type is written as `(params) => returnType`. This is the exact signature used by `findCartItem` in the lab.
</details>

### Q27. `type Person = { name: string; age: number };` — what is this?

- A. A class.
- B. An interface.
- C. A type alias.
- D. An enum.

<details><summary>Show answer</summary>

**C.** The `type` keyword defines a **type alias** (your own custom type).
</details>

### Q28. What is an enum in TypeScript?

- A. A function that enumerates arrays.
- B. A feature to define a set of named constants.
- C. A special kind of string.
- D. A generic class.

<details><summary>Show answer</summary>

**B.** An enum (enumeration) = a **set of named constants**, e.g. `enum CartStatus { Active, Checkout, Paid }`.
</details>

### Q29. What does `export default` do?

- A. Exports all functions in a file.
- B. Exports a single value, object or function from a module so it can be imported elsewhere.
- C. Marks a function as asynchronous.
- D. Makes a variable global.

<details><summary>Show answer</summary>

**B.** `export default` = one main export per module, importable from another module.
</details>

### Q30. `const persons: Array<Person> = [john];` — what is `Array<Person>`?

- A. A tuple.
- B. A generic class with `Person` as type argument (equivalent to `Person[]`).
- C. An enum of persons.
- D. Invalid syntax.

<details><summary>Show answer</summary>

**B.** `Array` is a **generic** class; the angle brackets supply the type argument.
</details>

### Q31. In the lab's `Product` class, why should the constructor call the set methods instead of assigning fields directly?

- A. It is faster.
- B. So the validation logic in the setters also runs when the object is created.
- C. Because TypeScript requires it.
- D. To avoid getters.

<details><summary>Show answer</summary>

**B.** Setters contain validation (empty name → throw, negative price → throw); calling them in the constructor means an invalid Product **cannot even be created**.
</details>

### Q32. What happens?

```typescript
const p = new Product(1, "Laptop", -5);
// set price(value: number) { if (value < 0) throw new Error("Price cannot be negative."); ... }
// constructor calls this.price = price;
```

- A. A product with price -5 is created.
- B. The price silently becomes 0.
- C. An Error "Price cannot be negative." is thrown during construction.
- D. A compile error.

<details><summary>Show answer</summary>

**C.** The constructor routes through the setter, whose validation throws immediately.
</details>

### Q33. What does `npx ts-node src/main.ts` do?

- A. Compiles TS to a `/dist` folder.
- B. Compiles and runs the TypeScript in memory, without creating visible .js files.
- C. Starts the Next.js dev server.
- D. Runs the Jest tests.

<details><summary>Show answer</summary>

**B.** `ts-node` compiles + runs **in memory** — no `.js` files appear. (Producing `.js` files is `npm run build` → `tsc` → `tsconfig.json` → `/dist`.)
</details>

### Q34. Which two attributes must the `<script>` tag have to run the compiled app in a browser?

- A. `async` and `src`
- B. `type="module"` and `defer`
- C. `type="typescript"` and `lazy`
- D. `defer` and `strict`

<details><summary>Show answer</summary>

**B.** `type="module"` — so the browser understands `import`/`export`; `defer` — so the script executes only after the HTML document is parsed.
</details>

### Q35. What does `tsc` read to know where the source files are and where to put the output?

- A. `package.json`
- B. `jest.config.js`
- C. `tsconfig.json`
- D. `next.config.js`

<details><summary>Show answer</summary>

**C. `tsconfig.json`.** (`package.json` only holds the `"build": "tsc"` script that *invokes* tsc.)
</details>

---

# Section 3 — Testing with Jest

### Q36. Which command installs everything needed to test a TypeScript project with Jest?

- A. `npm i -D jest ts-jest @types/jest`
- B. `npm i jest`
- C. `npm i -g typescript`
- D. `npx create-jest-app`

<details><summary>Show answer</summary>

**A.** `jest` = test runner, `ts-jest` = TypeScript preprocessor, `@types/jest` = type definitions; `-D` installs them as dev dependencies. Then `npx ts-jest config:init` creates `jest.config.js`.
</details>

### Q37. Will this test work as intended?

```typescript
test("negative price throws", () => {
  expect(product.setPrice(-1)).toThrow("Price cannot be negative.");
});
```

- A. Yes.
- B. No — the error is thrown before expect can catch it; the call must be wrapped in a lambda: `expect(() => product.setPrice(-1)).toThrow(...)`.
- C. No — toThrow does not accept a message.
- D. No — setPrice never throws.

<details><summary>Show answer</summary>

**B.** Calling the method directly executes it (and throws) **before** `expect` runs. You must pass a function: `expect(() => product.setPrice(-1)).toThrow('Price cannot be negative.')`.
</details>

### Q38. Two different objects with identical contents. Which is true?

```typescript
const a = { x: 1 };
const b = { x: 1 };
```

- A. `expect(a).toBe(b)` passes.
- B. `expect(a).toEqual(b)` passes.
- C. Both pass.
- D. Neither passes.

<details><summary>Show answer</summary>

**B.** `toBe` = strict/reference equality → fails for two distinct objects. `toEqual` = **deep** equality, checks every field → passes. Use `toBe` for primitives, `toEqual` for objects/arrays.
</details>

### Q39. Which matcher checks that an array contains a particular item?

- A. `.toMatch()`
- B. `.toContain()`
- C. `.toInclude()`
- D. `.toHave()`

<details><summary>Show answer</summary>

**B. `.toContain()`.** (`.toMatch()` checks a **string against a regex**.)
</details>

### Q40. `removeProduct` on a non-existent id must return null. Which matcher tests this?

- A. `.toBeUndefined()`
- B. `.toBeFalsy()`
- C. `.toBeNull()`
- D. `.toBe(0)`

<details><summary>Show answer</summary>

**C. `.toBeNull()`** — the lab explicitly uses this for the unhappy path. (`toBeFalsy` would also pass but is less precise and not what the course teaches for this case.)
</details>

### Q41. Which functions run once per test file rather than around every test?

- A. `beforeEach` / `afterEach`
- B. `beforeAll` / `afterAll`
- C. `setup` / `teardown`
- D. `beforeTest` / `afterTest`

<details><summary>Show answer</summary>

**B.** `beforeAll`/`afterAll` = one-time setup; `beforeEach`/`afterEach` = repeated around every test.
</details>

### Q42. Which files does `npm test` pick up by default in this course's setup?

- A. All `.ts` files.
- B. Files ending in `.test.ts`.
- C. Files in a folder named `jest`.
- D. Only `main.test.ts`.

<details><summary>Show answer</summary>

**B.** Jest automatically finds and runs all files ending in **`.test.ts`**; the test folder should mirror the source structure.
</details>

### Q43. What naming structure does the course recommend for test names?

- A. should / must / can
- B. given / when / then
- C. arrange / act / assert
- D. test1 / test2 / test3

<details><summary>Show answer</summary>

**B. given / when / then**, e.g. `test('given an empty cart; when adding a product; then the cart contains 1 item', ...)`.
</details>

### Q44. `expect("hello world").toMatch(/world/)` — what does toMatch check?

- A. Deep object equality.
- B. A string against a regular expression.
- C. Array membership.
- D. That a function throws.

<details><summary>Show answer</summary>

**B.** `toMatch` = string vs **regex**.
</details>

### Q45. Which passes?

```typescript
expect(0).toBeFalsy();      // (1)
expect([]).toBeFalsy();     // (2)
```

- A. Both.
- B. Only (1).
- C. Only (2).
- D. Neither.

<details><summary>Show answer</summary>

**B.** `0` is falsy; `[]` (empty array) is **truthy**. Falsy = `false, 0, '', null, undefined, NaN` — nothing else.
</details>

---

# Section 4 — React basics, state & lifecycle

### Q46. According to the course, React is…

- A. a JavaScript framework for building back-ends.
- B. a JavaScript library for building user interfaces.
- C. a compiler for TypeScript.
- D. a database ORM.

<details><summary>Show answer</summary>

**B.** React = **library** for building UIs (declarative, component-based, by Meta). **Next.js** is the *framework* built on top of it — the library/framework distinction is a favorite MC question.
</details>

### Q47. When does React render (execute) a component?

- A. Only once, when the app starts.
- B. The first time it appears on screen, and every time its props or state change.
- C. Every second.
- D. Only when the user refreshes the page.

<details><summary>Show answer</summary>

**B.** Initial render (mount) + every props/state change (update).
</details>

### Q48. You have written the following useEffect hook to sync user data:

```jsx
useEffect(() => {
  console.log(userId)
}, [userId]);
```

In which React lifecycle phases will the code inside this function be executed?

- A. During the Mounting phase AND during the Updating phase (whenever userId changes).
- B. Only during the Updating phase (only when userId changes).
- C. Only during the Mounting phase.
- D. During the Mounting, Updating, and Unmounting phases.

<details><summary>Show answer</summary>

**A.** With a dependency array containing `userId`: runs at **mount**, and during **updates only when `userId` changed**. It never runs on unmount (only a *cleanup function* returned by the effect would).
</details>

### Q49. In which lifecycle phases does this run?

```jsx
useEffect(() => {
  console.log("hello");
}, []);
```

- A. Mounting and every update.
- B. Only mounting (once, after the first render).
- C. Only unmounting.
- D. Never.

<details><summary>Show answer</summary>

**B.** An **empty dependency array** = run once, after the first render (mount phase only).
</details>

### Q50. In which lifecycle phases does this run?

```jsx
useEffect(() => {
  console.log("hello");
});
```

- A. Only mounting.
- B. Only updating.
- C. Mounting AND after every update (every render).
- D. Only unmounting.

<details><summary>Show answer</summary>

**C.** **No dependency array** = after **every** render: mount + every update.
</details>

### Q51. When does the function returned by a useEffect callback run?

```jsx
useEffect(() => {
  const id = setInterval(tick, 1000);
  return () => clearInterval(id);
}, []);
```

- A. After every render.
- B. On unmount (and before the effect re-runs on updates).
- C. On mount.
- D. Never — it is dead code.

<details><summary>Show answer</summary>

**B.** The returned function is the **cleanup**: it runs when the component **unmounts** (and before re-running the effect on an update). Cleanup of timers/listeners/requests belongs in the unmount phase.
</details>

### Q52. Put the lifecycle phases in the correct order.

- A. Updating → Mounting → Unmounting
- B. Mounting → Updating → Unmounting
- C. Mounting → Unmounting → Updating
- D. Rendering → Painting → Committing

<details><summary>Show answer</summary>

**B.** Mount (created + added to DOM) → Update (re-renders on state/props changes, repeats) → Unmount (destroyed/removed; cleanup here).
</details>

### Q53. Which phase can occur many times during a component's life?

- A. Mounting
- B. Updating
- C. Unmounting
- D. All three occur exactly once.

<details><summary>Show answer</summary>

**B.** Updating **repeats** until unmount; mounting and unmounting happen once each.
</details>

### Q54. Will this component work as intended?

```jsx
const Counter = () => {
  let count = 0;
  return <button onClick={() => count++}>{count}</button>;
};
```

- A. Yes, the button shows an increasing number.
- B. No — count is a local variable: changing it doesn't trigger a re-render, and its value is reset on every render.
- C. No — you can't use onClick on a button.
- D. Yes, but only in client components.

<details><summary>Show answer</summary>

**B.** Local variables (1) are not tracked by React → no re-render, and (2) are **reset after each re-render**. You need `useState`.
</details>

### Q55. What does `useState` return?

- A. Just the state value.
- B. An object with `value` and `set` fields.
- C. An array: [stateVariable, setterFunction].
- D. A Promise of the state.

<details><summary>Show answer</summary>

**C.** An array of two things — the **state variable** (persists between renders) and the **setter** (updates the value AND triggers a re-render) — normally destructured: `const [count, setCount] = useState(0);`.
</details>

### Q56. When is the initial value passed to `useState` applied?

- A. On every render.
- B. During the mount phase only.
- C. During the update phase.
- D. On unmount.

<details><summary>Show answer</summary>

**B.** The initial value is set **during mounting**; later renders keep the current state.
</details>

### Q57. Which of these does NOT trigger a re-render of a component?

- A. A change of its state.
- B. A change of its props.
- C. Its parent component re-rendering.
- D. A change of a local variable inside the component.

<details><summary>Show answer</summary>

**D.** The only re-render triggers are: **state change, props change, parent re-render**. Local variables are invisible to React.
</details>

### Q58. Will this compile/run as intended?

```jsx
const App = () => {
  return (
    <div>one</div>
    <div>two</div>
  );
};
```

- A. Yes.
- B. No — a component must return a single root node; wrap the siblings in a fragment `<>...</>`.
- C. No — divs are not allowed in JSX.
- D. Yes, but only in server components.

<details><summary>Show answer</summary>

**B.** The return value must be **one root node**. Use a fragment (`<>…</>`) or a wrapping element.
</details>

### Q59. What is JSX?

- A. A templating language that replaces JavaScript.
- B. A syntax extension that lets you write HTML-like code directly in JavaScript.
- C. JSON with extra features.
- D. The Java Syntax eXtension.

<details><summary>Show answer</summary>

**B.** JSX = JavaScript XML — the component's return value, a "blueprint" for the UI.
</details>

### Q60. How does data flow between parent and child components?

- A. Data goes down via props; children notify parents via callback functions passed as props.
- B. Children can directly modify parent variables.
- C. Data flows only upward.
- D. Components can only communicate through the database.

<details><summary>Show answer</summary>

**A.** Props go **parent → child**; the child calls a **callback prop** (often the parent's state setter) to communicate back.
</details>

### Q61. Which JSX renders the table only when there are lecturers?

- A. `{lecturers.length > 0 && <table>...</table>}`
- B. `{<table>...</table> && lecturers.length > 0}`
- C. `if (lecturers.length > 0) <table>...</table>`
- D. `{lecturers.length > 0 ? null : <table>...</table>}`

<details><summary>Show answer</summary>

**A.** The **inline IF with `&&`**: the JSX after `&&` renders only when the condition is truthy. (D is inverted; C is a statement, not an expression.)
</details>

### Q62. What is missing?

```jsx
{lecturers.map((lecturer) => (
  <tr onClick={() => selectLecturer(lecturer)}>
    <td>{lecturer.expertise}</td>
  </tr>
))}
```

- A. Nothing.
- B. A `key` prop on the repeated `<tr>` element.
- C. A return statement.
- D. A dependency array.

<details><summary>Show answer</summary>

**B.** React requires a **`key`** on list items: `<tr key={index}>` (or a stable id).
</details>

### Q63. A parent passes `selectLecturer={setSelectedLecturer}` to a table. A row's onClick calls `selectLecturer(lecturer)`. What happens next?

- A. Nothing until the page refreshes.
- B. The parent's state changes → the parent re-renders → components conditioned on `selectedLecturer` now mount.
- C. Only the table re-renders.
- D. A runtime error: children can't call parent functions.

<details><summary>Show answer</summary>

**B.** The callback IS the parent's state setter → state update → **updating phase** of the parent → `{selectedLecturer && <CourseOverviewTable/>}` now renders (mounts).
</details>

### Q64. Why must hooks never be called inside loops or conditions?

- A. It's slow.
- B. React tracks hooks by their call order per render; a changing order corrupts which state belongs to which hook.
- C. Hooks only work in for-loops.
- D. It causes CSS conflicts.

<details><summary>Show answer</summary>

**B.** The **Rules of Hooks**: top level only, because React relies on a **consistent call order** every render to match state/effects to hooks.
</details>

### Q65. Which pair of hooks must be imported from `next/navigation` in the App Router?

- A. `useState` and `useEffect`
- B. `useRouter` and `usePathname`
- C. `useContext` and `useSWR`
- D. `useParams` and `useState`

<details><summary>Show answer</summary>

**B.** In the App Router, **`useRouter`** (navigation: `router.push('/')`) and **`usePathname`** (current URL as string) come from **`next/navigation`** — not the old `next/router`.
</details>

### Q66. You want a useEffect to re-run on every URL change. Which dependency belongs in the array?

- A. `useRouter()`
- B. the result of `usePathname()`
- C. `window.location`
- D. `[]`

<details><summary>Show answer</summary>

**B.** Put the **pathname** from `usePathname()` in the dependency array — the effect re-runs whenever the URL path changes (the course uses this to re-read sessionStorage in the Header).
</details>

### Q67. Why use `useInterval` instead of plain `setInterval` in a component?

- A. It's faster.
- B. It considers the component lifecycle: the interval is cleared on unmount, preventing memory leaks and stray callbacks.
- C. setInterval doesn't exist in the browser.
- D. useInterval runs on the server.

<details><summary>Show answer</summary>

**B.** `useInterval` (3rd-party hook, `npm install use-interval`) clears its interval **when the component unmounts** — raw `setInterval` would keep firing.
</details>

### Q68. What is state, in one sentence from the course?

- A. Data stored in the database.
- B. The component's memory: information needed for the next re-render; anything not in state is lost at the next re-render.
- C. The CSS of a component.
- D. A global variable.

<details><summary>Show answer</summary>

**B.** State stores information **between renders**; setting it triggers a re-render.
</details>

### Q69. `useEffect` should be used…

- A. for all logic in a component.
- B. only to interact with an external system (storage, timers, fetching); otherwise use state and event handlers.
- C. instead of useState.
- D. only in server components.

<details><summary>Show answer</summary>

**B.** Side effects = interactions with **external systems**. Everything else: state + event handlers. (And hooks work only in **client** components.)
</details>

### Q70. What happens on this click, given `const [count, setCount] = useState(0);`?

```jsx
<button onClick={() => setCount(count + 1)}>{count}</button>
```

- A. Nothing.
- B. `count` becomes 1, the setter triggers a re-render, the button now shows 1.
- C. The whole page reloads.
- D. `count` becomes 1 but the display still shows 0.

<details><summary>Show answer</summary>

**B.** The setter updates the persisted value **and** signals React to re-render → the UI shows the new state.
</details>

---

# Section 5 — Forms & controlled inputs

### Q71. Consider this component:

```jsx
"use client";
import { useState } from "react";

export default function SignupForm() {
  const [username, setUsername] = useState("");
  return (
    <form>
      <input type="text" name="username" />
    </form>
  );
}
```

The goal is that the typed value is stored in the `username` state variable and kept in sync while typing. Which change to the input-tag achieves this?

- A. `<input type="text" name="username" onChange={() => setUsername(username)} />`
- B. `<input type="text" name="username" value={username} />`
- C. `<input type="text" name="username" defaultValue={username} />`
- D. `<input type="text" name="username" value={username} onChange={(e) => setUsername(e.target.value)} />`

<details><summary>Show answer</summary>

**D.** A controlled input needs BOTH: `value={username}` (state → input) and `onChange={(e) => setUsername(e.target.value)}` (input → state).
A sets state to its own old value — the typed text is never read. B without onChange makes the input **read-only** (React warns). C `defaultValue` = uncontrolled: initial value only, no sync.
</details>

### Q72. Why must every form input be bound to a state variable?

- A. HTML requires it.
- B. The component can re-render multiple times, and you want to retain what the user already typed.
- C. To prevent XSS.
- D. So the server can render it.

<details><summary>Show answer</summary>

**B.** Values not held in state are **lost on re-render**; binding retains the user's input.
</details>

### Q73. What does `event.preventDefault()` do in a form's submit handler?

- A. Prevents the user from typing.
- B. Prevents the default browser behavior on submit — a full-page refresh.
- C. Stops React from re-rendering.
- D. Validates the form.

<details><summary>Show answer</summary>

**B.** Without it, submitting reloads the whole page and all state is lost.
</details>

### Q74. Validation errors keep showing even after the user fixes the input and resubmits. What did the developer forget?

- A. `event.preventDefault()`
- B. Calling a `clearErrors()` function at the start of `handleSubmit` to reset previous error state.
- C. Making the form a server component.
- D. Adding a key prop.

<details><summary>Show answer</summary>

**B.** Errors are state and **persist between clicks** — the lab explicitly requires clearing them first.
</details>

### Q75. Which state type does the course use for a field-specific validation error?

- A. `useState<boolean>(false)`
- B. `useState<string | null>(null)`
- C. `useState<Error>()`
- D. `useState<number>(0)`

<details><summary>Show answer</summary>

**B.** e.g. `const [nameError, setNameError] = useState<string | null>(null);` — `null` = no error; a string = the message, rendered conditionally: `{nameError && <div>{nameError}</div>}`.
</details>

### Q76. Why validate in the front-end before submitting?

- A. Back-end validation is impossible.
- B. To give the user immediate feedback without a round-trip to the back-end server.
- C. Because HTML forms can't reach a server.
- D. It's required by React.

<details><summary>Show answer</summary>

**B.** Immediate feedback, no round-trip. (The back-end still validates too — front-end checks are UX, not security.)
</details>

### Q77. What is `StatusMessage` in the course's form pattern?

- A. A built-in React type.
- B. A custom type with fields `type` and `message`, kept in state as `StatusMessage[]` and rendered above the form with `.map`.
- C. An HTTP status code.
- D. A Jest matcher.

<details><summary>Show answer</summary>

**B.** General success/error messages use this custom type; multiple can be shown at once (array state), cleared in `clearErrors`.
</details>

### Q78. After a successful login, the course's handler does what?

- A. Reloads the page.
- B. Saves the user in sessionStorage, shows a success StatusMessage, and redirects with `router.push("/")` after a delay (setTimeout).
- C. Stores the password in localStorage.
- D. Sends an email.

<details><summary>Show answer</summary>

**B.** sessionStorage (`"loggedInUser"`), success message ("Login successful. Redirecting..."), then `setTimeout(() => router.push("/"), ...)` with `useRouter` from `next/navigation`.
</details>

---

# Section 6 — Next.js, server/client components & rendering

### Q79. In the App Router, which file creates the route `/lecturers`?

- A. `app/lecturers.tsx`
- B. `pages/lecturers.tsx`
- C. `app/lecturers/page.tsx`
- D. `app/routes/lecturers.tsx`

<details><summary>Show answer</summary>

**C.** Routes = folder path under `app/`, with the component in a file named **`page.tsx`**.
</details>

### Q80. Which route does `app/schedule/[lecturerId]/[courseId]/page.tsx` serve?

- A. `/schedule?lecturerId=1&courseId=2`
- B. `/schedule/1/2`
- C. `/schedule/lecturerId/courseId`
- D. `/[lecturerId]/[courseId]/schedule`

<details><summary>Show answer</summary>

**B.** Square-bracket folders are **dynamic segments**; `/schedule/1/2` fills `lecturerId=1`, `courseId=2`. Navigating there before those folders exist → **404**.
</details>

### Q81. Will this page work as intended?

```tsx
type Props = { params: { lecturerId: string } };
const Page = ({ params }: Props) => {
  const { lecturerId } = params;
  return <div>{lecturerId}</div>;
};
```

- A. Yes.
- B. No — in this course's App Router version, `params` is a Promise: the page must be async and `await params`.
- C. No — pages can't take props.
- D. No — lecturerId must be a number.

<details><summary>Show answer</summary>

**B.** Pattern from the slides: `type Props = { params: Promise<{ lecturerId: string }> };` and `const Page = async ({ params }) => { const { lecturerId } = await params; ... }`.
</details>

### Q82. What is `{children}` in `app/layout.tsx`?

- A. The list of all components in the app.
- B. A prop passed by Next.js representing the content of the active page, inserted into the layout.
- C. The CSS children selector.
- D. The route parameters.

<details><summary>Show answer</summary>

**B.** The RootLayout is the template for the **entire application** (defines `<html>`/`<body>`, holds header/nav, global CSS, Context Providers); `{children}` is where the current `page.tsx` renders.
</details>

### Q83. Why use `<Link>` from `next/link` instead of `<a>`?

- A. `<a>` is deprecated.
- B. `<Link>` does client-side navigation with JavaScript — faster than the browser's full navigation — and gets automatic prefetching.
- C. `<Link>` works in server components, `<a>` doesn't.
- D. No difference.

<details><summary>Show answer</summary>

**B.** Client-side navigation (no full reload) + Next.js **prefetches `<Link>` routes automatically** (not `<a>`, and not dynamic routes; opt out with `prefetch={false}`).
</details>

### Q84. What are components in the Next.js App Router by default?

- A. Client components.
- B. Server components.
- C. Static HTML files.
- D. Web components.

<details><summary>Show answer</summary>

**B.** **Server components are the default since Next.js 13**; you opt into client components with `'use client'`.
</details>

### Q85. You add `useState` to a page component and the compiler complains. Why?

- A. useState is deprecated.
- B. The page is a server component; hooks and state are not allowed there. Fix: add 'use client' (or extract a client child component).
- C. You forgot to import React.
- D. useState only works in layouts.

<details><summary>Show answer</summary>

**B.** Server components: **no hooks, no state, no browser APIs**. The exact error the lab shows: *"This React Hook only works in a Client Component. To fix, mark the file (or its parent) with the `"use client"` directive."*
</details>

### Q86. A server component contains `console.log("hi")`. Where does the output appear?

- A. Only in the browser console.
- B. In the server's terminal output (and mirrored in the browser console marked "server" — in development only).
- C. Nowhere.
- D. In a log file.

<details><summary>Show answer</summary>

**B.** Server components run on the **Next.js server**. The browser mirror (tagged "server") happens only in dev, not production.
</details>

### Q87. Which is NOT a valid reason to make a component a client component?

- A. It needs onClick handlers.
- B. It uses useState/useEffect.
- C. It reads sessionStorage.
- D. It fetches data with `await fetch(...)` at render time.

<details><summary>Show answer</summary>

**D.** Async data fetching at render time is exactly what **server components** are good at (`const Page = async () => { const res = await fetch(...); ... }`). A–C (interactivity, hooks, browser APIs) require a client component.
</details>

### Q88. In the rendering slides, what does "the server" refer to?

- A. The Java/Spring back-end API.
- B. The Next.js server.
- C. The database server.
- D. Any cloud machine.

<details><summary>Show answer</summary>

**B.** ⭐ Gotcha: "server" = **the Next.js server**, NOT our back-end API.
</details>

### Q89. What is the RSC Payload?

- A. The JSON body of a REST response.
- B. A compact binary representation of the rendered React Server Components tree.
- C. The JavaScript bundle.
- D. A cookie.

<details><summary>Show answer</summary>

**B.** It contains: the rendered result of server components, **placeholders for client components** (references to their JS files), and props passed from server to client components.
</details>

### Q90. What is hydration?

- A. Fetching data from the server.
- B. React's process of attaching event handlers to the DOM to make the static HTML interactive.
- C. Caching translated strings.
- D. Compiling TypeScript.

<details><summary>Show answer</summary>

**B.** First load: prerendered HTML is shown but **not yet interactive**; then the RSC payload **hydrates** the client components.
</details>

### Q91. Static Rendering vs Dynamic Rendering — when do they happen?

- A. Static = request time; Dynamic = build time.
- B. Static = build time (cached); Dynamic = request time.
- C. Both at build time.
- D. Both at request time.

<details><summary>Show answer</summary>

**B.** Static ("prerendering") happens at **build time** and is cached — for rarely-changing pages. Dynamic happens at **request time**. Also: *Server Components* = WHERE code runs; *Dynamic Rendering* = WHEN it runs.
</details>

### Q92. Which of these does NOT opt a page into Dynamic Rendering?

- A. Using `cookies()`.
- B. Using `headers()`.
- C. `fetch(url, { cache: 'no-store' })`.
- D. Importing a CSS module.

<details><summary>Show answer</summary>

**D.** Dynamic-rendering triggers: `cookies()`, `headers()`, `searchParams()`, and explicit `cache: 'no-store'`. CSS modules are irrelevant to it.
</details>

### Q93. Which links are prefetched automatically by Next.js?

- A. All `<a>` tags.
- B. Routes linked with `<Link>` — but not dynamic routes.
- C. Every URL in the app.
- D. None; prefetching must be enabled manually.

<details><summary>Show answer</summary>

**B.** Automatic for `<Link>` (NOT `<a>`), **dynamic routes are not prefetched**, and you can opt out per link with `prefetch={false}`.
</details>

### Q94. Why do CSS Modules exist?

- A. To make CSS faster.
- B. To scope CSS locally to a component, preventing class-name conflicts across a large app.
- C. To replace JavaScript.
- D. To inline all styles.

<details><summary>Show answer</summary>

**B.** Name the file `*.module.css`, import it (Next.js treats it as a JS object), then `className={styles.container}`.
</details>

### Q95. Which folder holds code that interacts with the REST API, per the course's project structure?

- A. `components/`
- B. `services/`
- C. `types/`
- D. `styles/`

<details><summary>Show answer</summary>

**B. `services/`.** Principle: **separate data-fetching logic from presentation logic** — components call services without knowing how or where the data comes from. (`components/` = reusable components, `types/` = domain types, `styles/` = CSS modules, `app/` = routed pages.)
</details>

### Q96. A teammate clones the repo and the app won't start: modules are missing. What must they run?

- A. `npm run build`
- B. `npm install`
- C. `npx tsc`
- D. `git pull` again

<details><summary>Show answer</summary>

**B.** `node_modules` is not committed; every member runs **`npm install`** after pulling.
</details>

### Q97. In a client component, which is the course's preferred data-fetching tool?

- A. XMLHttpRequest.
- B. The useSWR hook from the swr library (Vercel).
- C. jQuery.ajax.
- D. Direct database access.

<details><summary>Show answer</summary>

**B.** `useSWR` — install with `npm install swr`. (The `use` hook and `useEffect` are the alternatives mentioned.)
</details>

### Q98. What does Stale-While-Revalidate mean?

- A. The data is never cached.
- B. Cached data is returned immediately (stale), fresh data is fetched in the background (while), and the component re-renders when it arrives (revalidate).
- C. The cache is validated before every render.
- D. Data expires after one minute.

<details><summary>Show answer</summary>

**B.** That's the exact three-part semantics of useSWR.
</details>

### Q99. What does `useSWR` return?

- A. `data` only.
- B. `data, error, isLoading, mutate, isValidating`.
- C. A Promise.
- D. `response, status`.

<details><summary>Show answer</summary>

**B.** `data` (fetched data), `error` (thrown by the fetcher), `isLoading`, `mutate` (trigger cache re-evaluation), `isValidating`. Parameters: **key** (unique string, usually the URL), **fetcher** (async fn), options.
</details>

### Q100. Will this polling setup refresh the SWR data as intended?

```tsx
const { data } = useSWR("lecturers", getLecturers);
useInterval(() => {
  mutate("schedule", getLecturers());
}, 1000);
```

- A. Yes.
- B. No — mutate must be called with the SAME key used in useSWR ("lecturers"), otherwise the wrong cache entry is refreshed.
- C. No — useInterval can't call mutate.
- D. Yes, but only in server components.

<details><summary>Show answer</summary>

**B.** The swr library manages the data by **key**; `mutate("lecturers", getLecturers())` is required here. Mismatched keys = the visible data never updates.
</details>

---

# Section 7 — Context, custom hooks, storage, i18n

### Q101. What is prop drilling?

- A. Passing props from child to parent.
- B. A prop being passed through several layers of components to reach a nested child that needs it.
- C. Using too many useState hooks.
- D. A CSS technique.

<details><summary>Show answer</summary>

**B.** Problems: "dirty" middleman components accept props they don't use, boilerplate, forces the root layout to become a client component, poor maintainability.
</details>

### Q102. Put the three steps of React Context in order.

- A. useContext → createContext → Provider
- B. createContext → wrap children with Context.Provider (value = state + functions) → read with useContext
- C. Provider → Consumer → Reducer
- D. createStore → dispatch → subscribe

<details><summary>Show answer</summary>

**B.** 1. Create the "box" (`createContext`) — 2. Fill it & wrap children (`<Context.Provider value={...}>`) — 3. Read it in any descendant (`useContext`). (D is Redux, not covered.)
</details>

### Q103. `useContext(ThemeContext)` is called in a component that is NOT inside the Provider. What is returned?

- A. The latest state anyway.
- B. The context's default value (e.g. `undefined`) — which is why the course pattern throws an explicit Error as a guard.
- C. A compile error.
- D. An empty object.

<details><summary>Show answer</summary>

**B.** `createContext<T | undefined>(undefined)` → outside a Provider you get `undefined`; guard: `if (!context) throw new Error("... must be used within a ThemeProvider");`.
</details>

### Q104. Can React Context be used in server components?

- A. Yes, always.
- B. No — it relies on hooks and state, which server components don't support; context files need 'use client'.
- C. Only with `async`.
- D. Only in production.

<details><summary>Show answer</summary>

**B.** Context = client-component territory. Trick: put the Provider in its own client component and wrap it around `{children}` in the layout — the **layout itself stays a server component**.
</details>

### Q105. Two components each call `useCounter()` (a custom hook wrapping useState). What is true?

- A. They share one counter.
- B. Each call has its own independent state — hooks don't share state; Context is what has one shared state.
- C. The second call throws.
- D. Only the first component re-renders.

<details><summary>Show answer</summary>

**B.** ⭐ **Hooks: each instance = own state. Context: one shared global state.** Repeated twice in the slides — prime exam material.
</details>

### Q106. Which is a valid custom hook name that React will treat as a hook?

- A. `counterHook`
- B. `UseCounter`
- C. `useCounter`
- D. `use_counter`

<details><summary>Show answer</summary>

**C.** Rule: **`use` + capital letter** (`useCounter`, `useAuth`). Also: it only *counts* as a hook if it internally calls a built-in hook — and therefore usually works only in client components.
</details>

### Q107. Why is reusable stateful logic NOT put in a service (plain TS function)?

- A. Services are too slow.
- B. A state change in a service doesn't trigger a re-render, and services can't use hooks internally.
- C. Services can't be imported.
- D. Services only run on the server.

<details><summary>Show answer</summary>

**B.** Also from the same slide: not a child component (hooks have no UI; components are for rendering) and not Context (1 global state vs per-instance state). Best-practice mapping: hooks = reusable **stateful** logic; services = **stateless**/non-React logic; Context = **global** state; child components = **UI composition**.
</details>

### Q108. The `useCounter` hook returns `{ count, increment, reset }` but not `setCount`. Why?

- A. setCount doesn't exist.
- B. Encapsulation: consumers can only use the exposed operations; the raw setter is deliberately hidden.
- C. Returning setters is a syntax error.
- D. To save memory.

<details><summary>Show answer</summary>

**B.** The hook controls *how* state may change; components can't call `setCount` directly because it was never returned.
</details>

### Q109. Which statement about browser storage is correct?

- A. sessionStorage survives forever; localStorage is erased when the session ends.
- B. localStorage persists indefinitely; sessionStorage is erased when the session ends — and the course prefers sessionStorage.
- C. Both are sent to the server with every request.
- D. Both are accessible from server components.

<details><summary>Show answer</summary>

**B.** And neither is sent to the server (that's **cookies**), and both are browser APIs → **client components only**. Never store passwords!
</details>

### Q110. Which API call deletes the logged-in user from session storage?

- A. `sessionStorage.delete("loggedInUser")`
- B. `sessionStorage.removeItem("loggedInUser")`
- C. `sessionStorage.clearItem("loggedInUser")`
- D. `sessionStorage.setItem("loggedInUser", null)`

<details><summary>Show answer</summary>

**B.** The trio: `setItem(key, value)`, `getItem(key)`, `removeItem(key)`.
</details>

### Q111. What is the key difference between cookies and browser storage?

- A. Cookies can store more data.
- B. Cookies are sent back and forth to the server with every request (network cost) and can have a custom expiration time.
- C. Cookies are not affected by security concerns.
- D. Browser storage is sent with every request.

<details><summary>Show answer</summary>

**B.** Plus: cookies carry security/privacy risks (XSS, session hijacking, tracking) and can be secured with attributes like **HttpOnly** and **SameSite**.
</details>

### Q112. i18n vs l10n?

- A. They are synonyms.
- B. i18n = designing software to be adaptable to languages/regions without code changes; l10n = actually adapting a product to a specific market (date formats, weights, language variants).
- C. l10n is the abbreviation of internationalisation.
- D. i18n only concerns translations of error messages.

<details><summary>Show answer</summary>

**B.** i18n = 18 letters between i and n in "internationalisation". Key aspects: **separate content from code** (resource files) and **locale awareness** (locale = language + country, e.g. `en_US`).
</details>

### Q113. Put the five next-intl steps in order.

- A. Provider → Middleware → Hooks → JSON files → Config loader
- B. Middleware ("traffic controller") → JSON translation files ("dictionaries") → Config loader i18n.ts ("glue") → NextIntlClientProvider ("box") → Hooks ("tools")
- C. JSON files → Hooks → Provider → Middleware → Config
- D. Config → Provider → Middleware → Hooks → JSON files

<details><summary>Show answer</summary>

**B.** Middleware reads the URL (`/en`, `/es`) and sets the locale; dictionaries live in `public/locales/<lang>/common.json`; `i18n.ts` loads the right one; the Provider (in layout.tsx) exposes messages to client components; hooks/functions read them. The locale becomes a **dynamic route segment**: `app/[locale]/...`.
</details>

### Q114. How do you get translations in a server component vs a client component?

- A. Both use useTranslations from next-intl.
- B. Server: `await getTranslations()` from `next-intl/server`; Client: `useTranslations()` hook from `next-intl`. Server components do NOT read from the Provider.
- C. Server components can't be translated.
- D. Both use getTranslations from next-intl/server.

<details><summary>Show answer</summary>

**B.** Then `t('key')` looks up the key in the messages, for both.
</details>

### Q115. Why must the next-intl plugin config be `next.config.js` and not `next.config.ts`?

- A. TypeScript is forbidden in Next.js.
- B. It is the very first file Next.js reads on `npm run dev` — before the TypeScript compiler has run — so Node expects plain JavaScript (require / module.exports).
- C. .ts config files are slower.
- D. It's just a convention with no reason.

<details><summary>Show answer</summary>

**B.** Related gotcha from the lab: `middleware.ts` must be **`.ts`, not `.tsx`**.
</details>

---

# Section 8 — Security

### Q116. What does logging-out via the front-end accomplish when using a JWT token?

- A. 1. It clears the storage of the front-end, so that a new user can log in. 2. The token is still usable, you can still authenticate with it.
- B. 1. It does not clear the storage of the front-end. 2. The token is still usable.
- C. 1. It clears the storage of the front-end. 2. It invalidates the token, you can no longer use it to authenticate.
- D. 1. It does not clear the storage of the front-end. 2. It invalidates the token.

<details><summary>Show answer</summary>

**A.** JWT security is **stateless** — the server keeps no session and cannot revoke an issued token. Front-end logout clears the client's copy (state + sessionStorage; the server deletes the cookie via `Set-Cookie: Max-Age=0`), but **the token itself stays valid until it expires**.
</details>

### Q117. Why must the logout flow call the back-end at all?

- A. To write a log entry.
- B. HttpOnly cookies cannot be read or deleted by JavaScript, so the server must send a Set-Cookie header with Max-Age=0 to make the browser delete the cookie.
- C. To invalidate the JWT cryptographically.
- D. It doesn't need to.

<details><summary>Show answer</summary>

**B.** Complete logout = **two parts**: (1) server-side: delete the HttpOnly cookie via `Max-Age=0`; (2) client-side: clear React state and sessionStorage so the UI updates.
</details>

### Q118. What are the three parts of a JWT?

- A. Username, password, role.
- B. Header (metadata: type, algorithm), Payload (claims: user ID, expiry, roles), Signature (integrity).
- C. Cookie, token, session.
- D. Public key, private key, certificate.

<details><summary>Show answer</summary>

**B.** The **signature** is created with the server's secret key (JwtEncoder) — tampering breaks it; the **payload claims** include the expiration, which is the only built-in invalidation mechanism.
</details>

### Q119. What is a bearer token?

- A. A token that requires a fingerprint.
- B. An access token where whoever possesses ("bears") it is granted access — no additional proof of identity required.
- C. A token that only works once.
- D. A database key.

<details><summary>Show answer</summary>

**B.** Implication: a stolen token works for the thief → protect it (HttpOnly cookie, HTTPS, short lifetime). A JWT is a common bearer-token format.
</details>

### Q120. Where does the course store the JWT vs the user's display data?

- A. Both in localStorage.
- B. JWT in an HttpOnly cookie (set by the server, unreadable by JS → protects against XSS); display data (username, role) in sessionStorage + React state.
- C. JWT in sessionStorage; user data in the cookie.
- D. Both in the URL.

<details><summary>Show answer</summary>

**B.** And the login response **strips the token from the body** — it exists only in the cookie.
</details>

### Q121. Which fetch option is required to store and send the auth cookie?

- A. `mode: "no-cors"`
- B. `credentials: "include"`
- C. `cache: "no-store"`
- D. `redirect: "follow"`

<details><summary>Show answer</summary>

**B.** Needed to **accept** the Set-Cookie at login, to **send** the cookie on later client-side requests, and on the logout call. CORS side: back-end must set `allowCredentials(true)` + an **explicit** allowed origin (no `*` with credentials).
</details>

### Q122. A server component must fetch protected data. Which is correct?

- A. It sends the browser cookie automatically.
- B. It has no access to browser cookie storage; use Next.js `cookies()` to read the incoming request cookies, pass them to the service, which adds `Authorization: Bearer ${token}` — the Next.js server acts as a proxy.
- C. Protected data can't be fetched server-side.
- D. It should read localStorage.

<details><summary>Show answer</summary>

**B.** The back-end accepts **both** cookie and Authorization header. Bonus fact: this server-side fetch is **not visible in the browser's Network tab**.
</details>

### Q123. Not logged in, you visit the lecturers page (server-fetched). What happens?

- A. Empty table.
- B. No authToken cookie to forward → back-end returns 401 Unauthorized → the service throws → the page renders the error.
- C. Automatic redirect to Google.
- D. The data loads anyway.

<details><summary>Show answer</summary>

**B.** And after logout, revisiting reproduces the 401 — proof the server-side cookie was destroyed.
</details>

### Q124. Why is React Context needed on top of sessionStorage for auth state?

- A. sessionStorage is too small.
- B. sessionStorage is not reactive — changing it doesn't notify or re-render components. Context/state = reactivity; sessionStorage = persistence (survive refresh). Use both.
- C. Context is faster.
- D. sessionStorage is deprecated.

<details><summary>Show answer</summary>

**B.** AuthContext responsibilities: hold user state, `login` (state + sessionStorage), `logout` (server call + clear both), **restore on refresh** via useEffect reading sessionStorage, share globally via the Provider in RootLayout.
</details>

### Q125. `{user?.role === "admin" && <button>Schedule</button>}` — what protection does this give?

- A. Full security: non-admins can never schedule.
- B. Cosmetic UI adaptation only — the real permission check happens on the back-end (JWT verified, role checked).
- C. It encrypts the button.
- D. It blocks the API call.

<details><summary>Show answer</summary>

**B.** Front-end role checks adapt the UI; **authorization is enforced server-side**. Authentication = who you are; Authorization = what you may do.
</details>

### Q126. Hashing vs encryption — which is correct?

- A. Hashing is reversible with a key; encryption is one-way.
- B. Hashing is one-way (verification, password storage); encryption is reversible with the correct key (confidentiality).
- C. Both are reversible.
- D. Passwords should be encrypted so they can be recovered.

<details><summary>Show answer</summary>

**B.** "Hashing is for verification, encryption is for confidentiality." **Never store passwords in plain text** — hash them with **bcrypt** via the `PasswordEncoder` bean. Login = hash the supplied password and **compare hashes**.
</details>

### Q127. Bcrypt strength parameter — which is true?

- A. Typically 10–14; higher = stronger security but slower hashing.
- B. Typically 1–5; higher = faster.
- C. It sets the password's max length.
- D. It is the number of allowed login attempts.

<details><summary>Show answer</summary>

**A.** e.g. `new BCryptPasswordEncoder(12)`. Gotcha: **seed/DbInitializer users must also get encoded passwords** or their logins fail.
</details>

### Q128. Right after adding `spring-boot-starter-security`, a GET to `/status` (a public endpoint) returns a redirect to a login page. Why?

- A. The endpoint is broken.
- B. By default ALL endpoints require authentication — even /status, /login and swagger-ui — until you configure a SecurityFilterChain with permitAll matchers.
- C. Spring Security blocks GET requests.
- D. The port changed.

<details><summary>Show answer</summary>

**B.** In the SecurityFilterChain: **matcher order matters — evaluation stops at the first match** (catch-all `anyRequest().authenticated()` goes LAST), and **CSRF should be disabled in a stateless API** (no server session to forge against).
</details>

### Q129. `@PreAuthorize` vs `@PostAuthorize`?

- A. Both run before the method.
- B. @PreAuthorize checks before the method runs (method not executed on failure, AccessDeniedException); @PostAuthorize checks after the method but before returning (result withheld; used to filter sensitive data). Both need @EnableMethodSecurity.
- C. @PostAuthorize runs the method twice.
- D. They are identical.

<details><summary>Show answer</summary>

**B.** Course use case: `@PreAuthorize("hasRole('ADMIN')")` on the add-student endpoint.
</details>

### Q130. Which annotation lets a service test simulate a logged-in user?

- A. `@MockUser`
- B. `@WithMockUser(username = "admin", roles = {"ADMIN"})` from spring-security-test
- C. `@Authenticated`
- D. `@TestUser`

<details><summary>Show answer</summary>

**B.** For **E2E tests** a mock user isn't enough — generate a real token with the `jwtService` and attach it.
</details>

---

# Section 9 — Java, Spring, HTTP, DTOs

### Q131. Which HTTP methods are idempotent?

- A. GET, POST, PUT
- B. GET, PUT, DELETE
- C. POST, PATCH
- D. All of them

<details><summary>Show answer</summary>

**B.** Idempotent = repeating the request has the same effect as doing it once: **GET, PUT, DELETE**. POST and PATCH are NOT idempotent (though they're *allowed* to have side effects).
</details>

### Q132. In the layered architecture, which is correct?

- A. Repositories may call services.
- B. Layers have no knowledge of the layers above them: Controller → Service → Model/Repository, never the other way.
- C. Controllers contain the business logic.
- D. Models handle HTTP requests.

<details><summary>Show answer</summary>

**B.** Controller: listen/deserialize/serialize; Service: business-logic orchestration + integration; Model: business rules + validation; Repository: abstract & encapsulate data access.
</details>

### Q133. In `public Pony addPony(@Valid @RequestBody Pony pony)`, when does validation happen?

- A. In the database.
- B. At deserialization of the JSON request body (and @Valid indicates the type has validation annotations in its class definition).
- C. After the service call.
- D. Never automatically.

<details><summary>Show answer</summary>

**B.** Also: the method *seems* to return a Pony, but the response is **serialized to JSON** before being sent.
</details>

### Q134. Which `var` declaration compiles?

- A. `var x;`
- B. `var x = null;`
- C. `var list = new ArrayList<String>();`
- D. `private var name = "a";` (a field)

<details><summary>Show answer</summary>

**C.** `var` needs a type **unambiguously inferable from the right-hand side** and only works for **local variables, for loops, and try-with-resources** — not fields, parameters, or return types.
</details>

### Q135. Which is TRUE about Java records?

- A. Records can extend other classes.
- B. Records are mutable.
- C. The compiler generates equals, hashCode, toString, private final fields and a public constructor; records can implement interfaces but cannot extend classes.
- D. Record accessors are named getX().

<details><summary>Show answer</summary>

**C.** They implicitly extend `java.lang.Record`. Accessors are `person.firstName()` (no `get`). A **compact constructor** adds validation logic and has **NO parameter list**: `public Person { if (...) throw ...; }`.
</details>

### Q136. How should an enum be persisted, per the course?

- A. `@Enumerated(EnumType.ORDINAL)`
- B. `@Enumerated(EnumType.STRING)` — store as text; the ORDINAL default stores the position number and breaks if the enum order changes.
- C. As a BLOB.
- D. Enums can't be persisted.

<details><summary>Show answer</summary>

**B.** Easiest and safest: a text column + `EnumType.STRING`.
</details>

### Q137. Which are the four reasons to use DTOs?

- A. Speed, style, size, safety.
- B. Data hiding, Decoupling, Performance, Flexibility.
- C. Caching, logging, testing, versioning.
- D. Encryption, hashing, signing, encoding.

<details><summary>Show answer</summary>

**B.** DTOs are simple objects with **no business logic** (fields/getters/setters/ctors → perfect fit for **records**), live in the controller layer, and carry the **validation**.
</details>

### Q138. Will validation of the nested owner run?

```java
public record PonyInput(@NotBlank String name, OwnerInput owner) { }
```

- A. Yes, automatically.
- B. No — the nested DTO field must be annotated with @Valid, otherwise its validation rules are silently skipped.
- C. No — records can't be validated.
- D. Yes, but only at the service layer.

<details><summary>Show answer</summary>

**B.** Must be `@Valid OwnerInput owner`. Also: DTOs may nest **other DTOs, never model classes**.
</details>

### Q139. Frontend on `http://localhost:5173`, backend on `http://localhost:8080`. Same origin?

- A. Yes — same domain.
- B. No — same-origin requires same scheme + domain + PORT; different ports = different origins, so the browser blocks the response without CORS headers (e.g. @CrossOrigin).
- C. Yes — localhost is always exempt.
- D. Only for GET requests.

<details><summary>Show answer</summary>

**B.** The Same-Origin Policy (a defence against **CSRF**) checks all three: scheme, domain, port.
</details>

### Q140. Which is TRUE about auto-increment in PostgreSQL?

- A. Use the `AUTO_INCREMENT` keyword.
- B. `auto-increment` doesn't exist in PostgreSQL's dialect; use serial types: smallserial (smallint), serial (integer), bigserial (bigint).
- C. PostgreSQL can't generate keys.
- D. Use `IDENTITY(1,1)`.

<details><summary>Show answer</summary>

**B.** A dialect gotcha called out explicitly in the slides.
</details>

---

# Section 10 — JDBC, JPA, transactions, relationships

### Q141. Take a look at this JDBC method and ResultSetExtractor. Will it work as intended?

```java
public List<Lecturer> findAll() {
    return jdbcClient.sql("""
        select lecturer.id as lecturer_id, expertise, public.user.id as user_id,
               username, first_name, last_name, email, password, role,
               course.id as course_id, course.name as course_name, description, phase, credits
        from lecturer
        left outer join public.user on lecturer.user_id = public.user.id
        left outer join course_lecturers on lecturer.id = course_lecturers.lecturer_id
        left outer join course on course_lecturers.course_id = course.id """)
        .query(lecturersWithUserAndCourses);
}

private static final ResultSetExtractor<List<Lecturer>> lecturersWithUserAndCourses = (rs) -> {
    Lecturer currentLecturer = null;
    List<Long> lecturerIds = new ArrayList<>();
    List<Lecturer> lecturers = new ArrayList<>();
    while (rs.next()) {
        Long currentLecturerId = rs.getLong("lecturer_id");
        if (!lecturerIds.contains(currentLecturerId)) {
            Lecturer lecturer = new LecturerRowMapper().mapRow(rs, rs.getRow());
            lecturerIds.add(currentLecturerId);
            lecturers.add(lecturer);
            currentLecturer = lecturer;
        }
        Course course = new CourseRowMapper().mapRow(rs, rs.getRow());
        currentLecturer.addCourse(course);
    }
    return lecturers;
};
```

- A. No. Not all lecturers will be present. Those that are present will have their correct courses.
- B. Yes. All lecturers will be present with their correct courses.
- C. No. All lecturers will be present. Those that are present will not have their correct courses.
- D. No. Not all lecturers will be present. Those that are present will not have their correct courses.

<details><summary>Show answer</summary>

Analyze on **two axes**:

1. **Which lecturers appear?** — `left outer join` keeps lecturers even without courses, so all lecturers produce rows. (With **inner joins**, lecturers without courses would be missing entirely.)
2. **Do courses land on the right lecturer?** — The extractor **assumes all rows of one lecturer are consecutive**: `currentLecturer` only changes when an *unseen* id appears. There is **no ORDER BY**, so row order is not guaranteed → interleaved rows attach courses to the **wrong** lecturer. Additionally, for a lecturer with no courses, the left join yields NULL course columns and the code still calls `addCourse(...)` on a bogus mapped course.

So for THIS exact code: lecturers all present, courses unreliable → **C** is the best-matching option. If the query had used **inner joins** (as in the exam screenshot variant), course-less lecturers would also vanish → answer **D**. Learn to check both axes: **join type → who appears; ordering → whether children attach correctly.**
</details>

### Q142. A RowMapper maps…

- A. the whole result set to a list.
- B. one row to one object — it can NEVER fill a multi-valued (collection) relationship.
- C. one object to one SQL insert.
- D. entities to JSON.

<details><summary>Show answer</summary>

**B.** That's exactly why multi-valued relationships need a **ResultSetExtractor** (the join produces multiple rows per parent). RowMappers can be **composed** for single-valued relations (LecturerRowMapper delegating to UserRowMapper).
</details>

### Q143. `jdbcClient.sql("select ...").query(Course.class).list()` — what happens to the `lecturers` field of Course?

- A. Filled automatically via the foreign keys.
- B. It stays empty — the default mapping only fills simple fields with matching names.
- C. A runtime error.
- D. Lazily loaded on first access.

<details><summary>Show answer</summary>

**B.** With JDBC, **you** are responsible for filling the object network (and for keeping FKs correct on updates). Also memorize: `.list()` = many results, `.optional()` = 0 or 1, `.update()` = state-changing queries, `GeneratedKeyHolder` + `returning id` for generated keys.
</details>

### Q144. Why does the EntityManager have no `update()` method?

- A. Updates are forbidden in JPA.
- B. Saving changes happens automatically when the persistence context is flushed (dirty checking): find the managed entity, mutate it, and the flush at transaction end writes the change.
- C. You must always delete and re-insert.
- D. update() exists but is deprecated.

<details><summary>Show answer</summary>

**B.** `find` is enough to update — *inside a transaction*. API recap: `persist`→INSERT, `remove`→DELETE, `find`→SELECT by PK, `createQuery`→JPQL, `flush`→force sync.
</details>

### Q145. Service WITHOUT @Transactional; method does `findPonyByName`, mutates the pony, returns it (no save). Is the database updated?

- A. Yes — dirty checking always works.
- B. No — every repository call ran its own transaction; there is no flush after the mutation.
- C. Yes, but only for small changes.
- D. It throws an exception.

<details><summary>Show answer</summary>

**B.** The three variants: (1) no @Transactional + explicit `save` → updated; (2) no @Transactional + no save → **NOT updated**; (3) @Transactional + no save → updated (entity is managed; flush at transaction end = **dirty checking**).
</details>

### Q146. What happens here?

```java
@Transactional
public Lecturer addLecturer(LecturerInput in) {
    var user = userService.signup(Role.LECTURER, in.user()); // saves the user
    throw new RuntimeException();
}
```

- A. The user stays in the database.
- B. Nothing is committed — the RuntimeException rolls back the whole transaction, and the already-saved user disappears again.
- C. Only the lecturer is rolled back.
- D. The exception is swallowed.

<details><summary>Show answer</summary>

**B.** A **RuntimeException** in `@Transactional` code → full rollback (tune with `rollbackFor`/`noRollbackFor`). Other @Transactional gotchas: only works when called **from another class** (proxy-based); `@PostConstruct` needs a **TransactionTemplate**; default propagation **REQUIRED** (join or create); **REQUIRES_NEW** suspends the current transaction and commits its own.
</details>

### Q147. Saving a Lecturer that references a User that was never saved (no cascade). What happens?

- A. Both are saved.
- B. Crash: "Not-null property references a transient value — transient instance must be saved before current operation". Fix: save the User first, or put CascadeType.PERSIST on the relationship.
- C. The lecturer is saved with user = null.
- D. The user is saved automatically.

<details><summary>Show answer</summary>

**B.** Entity states: **transient** (new, unsaved), **managed** (tracked, synced on flush), **removed**, **detached** (after the persistence context closes, ALL entities are detached). A transient entity may not reference another transient one on save.
</details>

### Q148. LAZY fetching — which is TRUE?

- A. LAZY loads the related entity immediately.
- B. LAZY loads on first access and only works while the entity is MANAGED; in JPQL, a plain `join` only filters, while `join fetch` eagerly loads the related entities.
- C. LAZY works on detached entities too.
- D. Collections should always be EAGER.

<details><summary>Show answer</summary>

**B.** Guideline: single-valued → often EAGER; collections → usually LAZY. Accessing a lazy relation on a **detached** entity fails.
</details>

### Q149. What does `orphanRemoval = true` on a @OneToMany do?

- A. Deletes the parent when a child is removed.
- B. When an entity is removed from the relationship in Java code, that entity is automatically deleted from the database (without it, the link is broken but the row is kept).
- C. Prevents deletion entirely.
- D. Works on @ManyToMany too.

<details><summary>Show answer</summary>

**B.** Only valid on **@OneToOne** and **@OneToMany**. Whether you want delete-or-keep depends on the domain model.
</details>

### Q150. Bidirectional relationships — which is correct?

- A. `mappedBy` goes on the owner side.
- B. The owner is the side whose table contains the FOREIGN KEY; it gets @JoinColumn. The non-owner side uses mappedBy (naming the owner's field). For many-to-many the FK is in a join table, so you choose the owner (@JoinTable + joinColumns + inverseJoinColumns).
- C. Both sides get @JoinColumn.
- D. The owner is always the "One" side.

<details><summary>Show answer</summary>

**B.** And to avoid infinite JSON recursion when serializing bidirectional mappings: **@JsonManagedReference** (forward side, serialized) + **@JsonBackReference** (backward side, omitted).
</details>

---

*Score yourself: 135+/150 = ready. Anything you missed → look up the matching section in the theory PDFs and redo those questions the next day.*
