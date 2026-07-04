# Full-Stack Software Development — Complete Exam Summary

> Everything from the theory slides (Front-End 1–5 & Back-End 1–4) in one file,
> organized for a multiple-choice exam with **code-reading questions**
> ("what is the output?", "will this work as intended?", "which change achieves X?",
> "in which lifecycle phase does this run?").
>
> ⭐ = high-probability exam trap. Read those twice.

---

## Table of Contents

1. [JavaScript Fundamentals](#1-javascript-fundamentals)
2. [TypeScript](#2-typescript)
3. [Testing with Jest](#3-testing-with-jest)
4. [React Basics](#4-react-basics)
5. [React Lifecycle, State & Hooks](#5-react-lifecycle-state--hooks)
6. [Forms & Controlled Inputs](#6-forms--controlled-inputs)
7. [Next.js: Pages, Routing, Layouts](#7-nextjs-pages-routing-layouts)
8. [Server Components vs Client Components](#8-server-components-vs-client-components)
9. [Data Fetching: fetch & useSWR](#9-data-fetching-fetch--useswr)
10. [Client-Side Storage](#10-client-side-storage)
11. [React Context](#11-react-context)
12. [Custom Hooks](#12-custom-hooks)
13. [Rendering Strategies (SSR/SSG/CSR, Hydration)](#13-rendering-strategies)
14. [Internationalisation (i18n)](#14-internationalisation-i18n)
15. [Front-End Security (JWT, cookies, logout)](#15-front-end-security)
16. [Back-End Recap: Spring Boot, HTTP, Layers](#16-back-end-recap-spring-boot-http-layered-architecture)
17. [New Java Features: var, records, enums](#17-new-java-features)
18. [DTOs, CORS, PostgreSQL](#18-dtos-cors-postgresql)
19. [Database Access: JDBC (JdbcClient, RowMapper, ResultSetExtractor)](#19-database-access-with-jdbc)
20. [Database Access: JPA & Spring Data JPA](#20-jpa--spring-data-jpa)
21. [JPA Advanced: Transactions, Entity Lifecycle, Cascades, Fetch Types](#21-jpa-advanced)
22. [JPA Relationships](#22-jpa-relationships)
23. [OpenAPI & Swagger](#23-openapi--swagger)
24. [Back-End Security (Spring Security, JWT, bcrypt)](#24-back-end-security)
25. [Back-End Testing](#25-back-end-testing)
26. [The Ultimate Exam-Trap Checklist](#26-the-ultimate-exam-trap-checklist)
27. [Practice Questions (matching your exam style)](#27-practice-questions)

---

# 1. JavaScript Fundamentals

## 1.1 `let` vs `const` ⭐ (your Question 1!)

- `let` — reassignable variable.
- `const` — the **binding** cannot be reassigned. Reassigning throws a **TypeError at runtime**.

⭐ **THE classic trap:** `const` prevents *reassignment*, **not mutation**. Object properties
and array elements of a `const` can be changed freely:

```javascript
const user = { name: "A" };
user.name = "B";           // ✅ perfectly legal — mutating a property
console.log(user.name);    // "B"

user = { name: "C" };      // ❌ TypeError: Assignment to constant variable (RUNTIME error, not compilation)

const arr = [1, 2];
arr.push(3);               // ✅ legal
arr = [];                  // ❌ TypeError
```

> **Exam answer:** `const user = { name: "A" }; user.name = "B"; console.log(user.name);`
> → prints **"B"**. Not an error of any kind.
> Also note: JavaScript is *interpreted* — there is no "compilation error" in plain JS;
> and reassigning a const is a **runtime** error, not a compile error.

## 1.2 Template literals

```javascript
let greeting = `Hello ${username}, ${age} years old`;   // backticks + ${expression}
```

## 1.3 Ternary operator

```javascript
const state = age < 18 ? `Child` : `Adult`;   // condition ? ifTrue : ifFalse
```

## 1.4 Arrow functions ⭐

```javascript
// Block body → needs explicit return
const calculateSum = (a, b) => { return a + b; };

// Expression body → implicit return
const multiply = (a, b) => a * b;
```

⭐ With `{ }` braces you MUST write `return`, otherwise the function returns `undefined`.

## 1.5 First-class & higher-order functions, callbacks

- Functions can be assigned to variables and passed as arguments (**first-class**).
- A **higher-order function** takes a function as parameter; that parameter is a **callback**.

```javascript
const calculate = (a, b, operation) => operation(a, b);
calculate(2, 3, calculateSum);   // 5  — pass the reference, NOT calculateSum(...)
calculate(2, 3, multiply);       // 6
```

> **Callback definition (memorize):** any function passed as an argument to another
> function, which "calls it back" at the appropriate time. Works for sync AND async code.
> React uses this pattern for events (`onClick`, `onChange`).

## 1.6 Objects & destructuring ⭐

```javascript
const identity = { name: `John`, age: 45 };

// Destructuring is BY PROPERTY NAME:
const { name, age } = identity;          // name = "John", age = 45

// ⭐ Wrong names → undefined:
const { myName } = identity;             // myName = undefined (no property "myName"!)

// Renaming syntax:
const { name: myName2 } = identity;      // myName2 = "John"
```

## 1.7 Arrays: spread, destructuring, map/filter/forEach

```javascript
let males = [`John`, `Frank`];
let females = [`Annie`, `Martha`];
let people = [...males, ...females];     // spread: ["John","Frank","Annie","Martha"]

const [first, second] = males;           // array destructuring is BY POSITION

persons.forEach((p) => console.log(p));  // runs callback per element, returns undefined
persons.filter((p) => p.age > 25);       // returns NEW array of matching elements
persons.map((p) => p.name);              // returns NEW array of transformed elements
```

## 1.8 Truthy / falsy ⭐

Falsy values: `false`, `0`, `''`, `null`, `undefined`, `NaN`. **Everything else is truthy**
(including `[]`, `{}`, `"0"`).

## 1.9 `??` nullish coalescing vs `||` ⭐

```javascript
a ?? b   // b only if a is null or undefined
a || b   // b if a is ANY falsy value (also 0, '', false)

items.find(predicate) ?? null;   // convert find()'s undefined to null
```

## 1.10 `==` vs `===`

- `===` strict equality: no type coercion (standard in TS code).
- `==` loose: coerces types (`"5" == 5` is true). Avoid.

## 1.11 Promises & async/await ⭐

- A **Promise** = a value that will be resolved in the future. Three states:
  **pending**, **fulfilled**, **rejected**.
- An **`async` function ALWAYS returns a Promise**.
- **`await`** pauses until the Promise fulfills, or **throws** if it rejects.
- `await` can **only be used inside an `async` function**.

---

# 2. TypeScript

## 2.1 What is TypeScript? (definitions)

- "**JavaScript with syntax for types**" — a **strongly typed** language that builds on JS.
- **Compiled**: the TypeScript Compiler (**TSC/`tsc`**) converts TS to JavaScript.
- Workflow: **Write TS → Compile to JS → Deploy the JS** (browsers can't run TS!).

| | JavaScript | TypeScript |
|---|---|---|
| Typing | Loosely typed, only dynamic | Strongly typed: static **and** dynamic |
| Compilation | None needed (interpreted) | Must be compiled to JS |
| Errors | Only at **runtime** | Caught at **compile time** (early) |
| Features | No interfaces | Interfaces, OOP, classes, static typing |

## 2.2 Typing variables, functions, aliases

```typescript
let firstName: string;                    // explicit annotation
let username = `John`;                    // inferred as string
const multiply = (a: number, b: number): number => a * b;  // params + return type

type Person = { name: string; age: number };   // type alias
const john: Person = { name: `John`, age: 45 };
```

## 2.3 Parameter destructuring + object parameter types ⭐

```typescript
const demo = ({ x }: { x: number }): string => `x is ${x}`;

demo({ x: 2 });               // ✅
demo(2);                      // ❌ number, not an object
demo({ 2 });                  // ❌ invalid syntax
demo({ x: 2, name: `foo` });  // ❌ excess property → compile error
```

## 2.4 Optional (`?`) and union types ⭐

```typescript
class Animal {
  name: string;
  age?: number;    // optional → number | undefined; when absent it's UNDEFINED (not null)
}

// Union type for "not found":
removeProduct(id: number): Product | null   // return null when not found
```

⭐ `?` absent property → **`undefined`**; the course convention for "not found" returns is
**`null`** — hence `find(...) ?? null`.

## 2.5 Classes

```typescript
class Identity {
  name: string;
  age: number;
  constructor(identity: { name: string; age: number }) {
    this.name = identity.name;
    this.age = identity.age;
  }
  equals(other: { name: string; age: number }): boolean {
    return this.name === other.name && this.age === other.age;
  }
}
const emma = new Identity({ name: `Emma`, age: 25 });
```

- Encapsulation pattern from the lab: **`private _name`** fields + **getters/setters**;
  setters contain **validation** (throw `Error(...)` for invalid input);
  the **constructor calls the setters** so validation runs on creation.

## 2.6 Generics

```typescript
const persons: Array<Person> = [john];   // generic class + type argument (≡ Person[])
```

## 2.7 Enums, modules

- **enum** = a set of **named constants**: `enum CartStatus { Active, Checkout, Paid }`
- **`export default`** exports a single value/object/function per module for import elsewhere.

## 2.8 Function types (callback typing)

```typescript
findCartItem(predicate: (item: CartItem) => boolean): CartItem | null {
  return this.items.find(predicate) ?? null;
}
```

## 2.9 Compiling & running TS

- `npx ts-node src/main.ts` — compiles & runs **in memory**, no `.js` files created.
- `npm run build` → runs the `"build": "tsc"` script from **`package.json`** → `tsc` reads
  **`tsconfig.json`** (where sources are, where output goes, e.g. `/dist`) → produces `.js`.
- Browser script tag needs **`type="module"`** (for import/export syntax) and **`defer`**
  (execute only after HTML is parsed).

---

# 3. Testing with Jest

## 3.1 Setup (memorize commands)

```bash
npm i -D jest ts-jest @types/jest   # runner + TS preprocessor + type definitions
npx ts-jest config:init             # creates jest.config.js
npm test                            # runs all *.test.ts files
```

- `-D`/`--save-dev` = dev dependency.
- Test files: **`*.test.ts`**; test folder mirrors source structure;
  test names use **given / when / then**.

## 3.2 Test syntax

```typescript
test('given a cart; when adding a product; then total price is updated', () => {
  expect(actual).toBe(expected);
});
```

## 3.3 Matchers ⭐

| Matcher | Checks |
|---|---|
| `.toBe()` | strict equality (primitives / same reference) |
| `.toEqual()` | **deep** equality — every field of an object |
| `.toBeNull()` | value is `null` |
| `.toBeUndefined()` | value is `undefined` |
| `.toBeTruthy()` / `.toBeFalsy()` | truthiness |
| `.toMatch()` | string vs **regex** |
| `.toContain()` | array contains an item |
| `.toThrow()` | function throws an error |

⭐ `toBe` FAILS for two different objects with identical contents; `toEqual` passes.
Use `toBe` for primitives, `toEqual` for objects/arrays.

## 3.4 Testing that something throws ⭐

```typescript
expect(() => product.setPrice(-1)).toThrow('Price cannot be negative.');
//     ^^^^^ MUST wrap in a lambda!
```

⭐ Calling the method directly inside `expect(...)` throws **before** Jest can catch it →
test errors out. Always wrap in `() => ...`.

## 3.5 Setup & teardown

- Per test: `beforeEach()` / `afterEach()`.
- Once per suite: `beforeAll()` / `afterAll()`.

---

# 4. React Basics

## 4.1 What is React?

- "**A JavaScript library for building user interfaces**" (⭐ library, NOT framework —
  Next.js is the framework).
- **Declarative views** (you describe *what*, React handles rendering/updating),
  **component-based**, built by **Meta**.

**4 building blocks of a web app:** User Interface, Routing, Data Fetching, Rendering.

## 4.2 Components

- Building blocks of the UI ("LEGO bricks") → modularity, maintainability.
- A component is **a JavaScript function**, used **like an HTML tag**:

```tsx
const Header = () => {
  return <h1>Welcome</h1>;
};
// usage: <Header />
```

## 4.3 JSX

- The return value of a component. **JSX = JavaScript XML** — a syntax extension letting
  you write HTML-like code inside JavaScript. It is a "blueprint" for the UI.

## 4.4 Props ⭐

- **The input for a component**: the function receives **one argument — an object** of props.
- Data flows **parent → child** via props. Children talk back via **callback props**.
- Usually **destructured** in the parameter list:

```tsx
type Props = { lecturers: Lecturer[]; selectLecturer: (l: Lecturer) => void };
const LecturerOverviewTable = ({ lecturers, selectLecturer }: Props) => { ... };
```

## 4.5 Rendering rules

React renders (executes) a component:
1. **The first time it appears on screen** (mount), and
2. **every time its props or state change** (update).

## 4.6 Conditional rendering & lists ⭐

```tsx
{lecturers.length > 0 && <table>...</table>}      // inline IF with &&
{!lecturers.length && <p>No lecturers data.</p>}

{lecturers.map((lecturer, index) => (
  <tr key={index} onClick={() => selectLecturer(lecturer)}>   // key required on list items!
    <td>{lecturer.user.firstName}</td>
  </tr>
))}
```

## 4.7 Composition & fragments ⭐

- Return value **must be a single root node**. Multiple siblings → wrap in a **fragment**:

```tsx
return (
  <>
    <div>one</div>
    <div>two</div>
  </>
);
```

## 4.8 The callback pattern (lifting state up) ⭐

Parent passes its **state setter as a callback prop**; child calls it on click:

1. Parent: `const [selectedLecturer, setSelectedLecturer] = useState<Lecturer | null>(null);`
2. Parent renders: `<LecturerOverviewTable lecturers={data} selectLecturer={setSelectedLecturer} />`
3. Child row: `onClick={() => selectLecturer(lecturer)}` → notifies parent.
4. Parent state changes → parent **re-renders** → `{selectedLecturer && <CourseOverviewTable .../>}`
   now **mounts**.

---

# 5. React Lifecycle, State & Hooks

## 5.1 The three lifecycle phases ⭐⭐ (your Question 2!)

In order:

1. **Mounting** — component is created and added to the DOM (initial render).
2. **Updating** — re-renders whenever **state or props change**; repeats many times.
3. **Unmounting** — component is destroyed/removed from the DOM.
   → cleanup happens here: event listeners, timers, cancelling requests.

## 5.2 State ⭐

- State = the component's **memory**: information needed for the next re-render.
- ⭐ Info NOT in state is **lost on the next re-render**.
- **Changing state triggers a re-render** (the Updating phase).

**Why not plain local variables?** (classic question)
1. React doesn't track local variables → **no re-render**.
2. They are function-scoped, not component-scoped.
3. Their values are **reset/lost after every re-render**.

## 5.3 `useState` anatomy ⭐

```tsx
const [username, setUsername] = useState<string | null>(null);
//     ^ state variable  ^ setter          ^ type        ^ initial value (set at MOUNT)
```

- Returns an **array** `[value, setter]`, destructured.
- The setter **updates the value AND triggers a re-render**.
- The value **persists between renders** (React stores it outside the function).

## 5.4 `useEffect` and the dependency array ⭐⭐⭐ (your Question 2!)

For **side effects** = interacting with an **external system** (storage, timers, fetching).
Otherwise use state + event handlers.

```tsx
useEffect(() => {
  /* effect code */
  return () => { /* cleanup */ };
}, deps);
```

| Dependency array | When does the effect run? |
|---|---|
| **none** — `useEffect(cb)` | After **every** render: **Mounting AND every Update** |
| **`[]`** empty | **Only once**, after the first render: **Mounting phase only** |
| **`[userId]`** | **Mounting** AND **Updating — but only when `userId` changed** ⭐ |

> **Your exam question:** `useEffect(() => { console.log(userId) }, [userId]);`
> → runs **during the Mounting phase AND during the Updating phase (whenever userId changes)**. ✅
> It does NOT run on unmounting (only the *cleanup function* would).

- The **cleanup function** (returned from the effect) runs on **unmount** and before the
  effect re-runs on an update.

## 5.5 Rules of Hooks ⭐

- Only call hooks **at the top level** of a component or **from a custom hook**.
- **Never inside loops or conditions.**
- **Why:** React tracks hooks **by call order per render**; a changing order corrupts
  which state belongs to which hook.

## 5.6 Other hooks

- **`useRouter()`** (from **`next/navigation`** in the App Router!) — navigate: `router.push('/')`.
- **`usePathname()`** — current URL path as string; put it in a `useEffect` dependency array
  to run code on every navigation.
- **`useParams`** — dynamic route params.
- **`useInterval`** (3rd-party, `npm install use-interval`) — `setInterval` that respects the
  lifecycle: interval is **cleared on unmount** → no memory leaks. Args: (callback, ms).
- **`useContext`** — read a React context (see §11).

---

# 6. Forms & Controlled Inputs

## 6.1 The controlled input pattern ⭐⭐⭐ (your Question 3!)

Rule: **every input field is bound to a state variable** so the entered value survives
re-renders. Two things are required:

1. **Bind** state to the input: `value={username}`
2. **Update** state on change: `onChange={(e) => setUsername(e.target.value)}`

```tsx
"use client";
const [username, setUsername] = useState("");

<input
  type="text"
  name="username"
  value={username}                              // 1: state → input
  onChange={(e) => setUsername(e.target.value)} // 2: input → state
/>
```

> **Your exam question — which change keeps the input in sync with state?**
> ✅ `value={username}` **AND** `onChange={(e) => setUsername(e.target.value)}`
>
> Why the others are wrong:
> - `onChange={() => setUsername(username)}` — sets state to its own old value; never captures typing.
> - only `value={username}` without `onChange` — input becomes **read-only** (React warns).
> - `defaultValue={username}` — **uncontrolled** input: sets only the initial value, no sync.

## 6.2 Form component structure

A form component contains: **inputs, validation + error handling, event handlers**.

```tsx
const [name, setName] = useState("");
const [nameError, setNameError] = useState<string | null>(null);
const [statusMessages, setStatusMessages] = useState<StatusMessage[]>([]);
// StatusMessage = custom type with fields `type` and `message`

<form onSubmit={handleSubmit}>
  <input value={name} onChange={(e) => setName(e.target.value)} />
  {nameError && <div>{nameError}</div>}     {/* conditional error rendering */}
  <button type="submit">Login</button>
</form>
```

## 6.3 Validation & submit ⭐

- Validate **in the front-end before submitting** → immediate feedback, no round-trip.
- ⭐ Errors persist between clicks → **`clearErrors()` first** in `handleSubmit`.
- **`event.preventDefault()`** — prevents the default form submit behaviour, which is a
  **full-page refresh**. ⭐

```tsx
const handleSubmit = (event) => {
  event.preventDefault();          // stop full-page refresh
  clearErrors();                   // reset previous errors
  if (!validate()) return;
  sessionStorage.setItem("loggedInUser", name);
  setStatusMessages([{ type: "success", message: "Login successful. Redirecting..." }]);
  setTimeout(() => router.push("/"), 2000);   // router from useRouter()
};
```

---

# 7. Next.js: Pages, Routing, Layouts

## 7.1 What is Next.js?

- "A flexible **React framework** that gives you building blocks to create fast web apps."
- Built **on top of React**; handles tooling/config; adds structure, features, optimizations.
- Why: React itself is **unopinionated**; without a framework you reinvent common solutions.

## 7.2 File-based routing (App Router) ⭐

- A **page** = a React component that can be **navigated to** (a route).
- Route = the folder path under `app/`; file must be `page.tsx`:
  - `app/page.tsx` → `/`
  - `app/lecturers/page.tsx` → `/lecturers`

**Reserved filenames per folder:** `page.tsx` (the page), `layout.tsx` (wrapping layout),
`head.tsx` (meta/title), `error.tsx` (error UI), `not-found.tsx` (404), `loading.tsx` (loading UI).

## 7.3 The Root Layout ⭐

- **`app/layout.tsx`** = the main template of the entire application; every page renders
  inside it. Defines `<html>` and `<body>`. Replaces the old `_app.js`.
- Place for: header/footer/nav on every page, global CSS imports, **Context Providers**.
- **`{children}`** = prop passed by Next.js = **the content of the active page** inserted
  into the layout.

```tsx
const RootLayout = ({ children }: { children: React.ReactNode }) => (
  <html lang="en">
    <body>
      <Header />
      <main>{children}</main>
    </body>
  </html>
);
export default RootLayout;
```

## 7.4 `<Link>` ⭐

- From **`next/link`**. Enables **client-side navigation**: the transition happens with
  JavaScript — **faster** than the browser's default full navigation (no full reload).

## 7.5 Dynamic routing ⭐

- Wrap folder names in **square brackets** for a dynamic segment:
  `app/schedule/[lecturerId]/[courseId]/page.tsx` → `/schedule/1/2`
- Navigating to such a URL before the folders exist → **404**.

**Reading params — `params` is a Promise!** ⭐

```tsx
type Props = { params: Promise<{ lecturerId: string; courseId: string }> };

const CreateSchedulePage = async ({ params }: Props) => {   // page is ASYNC
  const { lecturerId, courseId } = await params;            // AWAIT the params
  const { lecturer, course } = await getData(lecturerId, courseId);
  ...
};
```

## 7.6 Static assets & CSS modules

- **Public folder**: fixed-URL, unprocessed assets (favicon); served from root `/`.
- **Importing (recommended)**: import the image, use **`<Image>` from `next/image`** —
  auto-optimized for size/format/responsiveness.
- **CSS Modules** solve global class-name conflicts by **scoping CSS to a component**:
  1. name file `*.module.css` → 2. write CSS → 3. import (treated as a JS object) →
  4. `className={styles.container}`.

## 7.7 Project structure (folder responsibilities)

| Folder | Purpose |
|---|---|
| `components/` | Reusable, composable components |
| `app/` | Pages that need a route |
| `services/` | Interact with the REST API |
| `types/` | Types of the domain objects |
| `tests/` | Tests |
| `styles/` | CSS modules |

- Teamwork: one commits; others **pull + `npm install`** (node_modules isn't committed).
- `npm run dev` starts the dev server; browser hot-reloads on save.

---

# 8. Server Components vs Client Components

## 8.1 Server components (the DEFAULT since Next.js 13) ⭐⭐

- Rendered **on the Next.js server**, sent to the client **as HTML**.
- Benefits: no JS sent to the front-end, **faster data fetching**, **smaller bundles**,
  faster **First Meaningful Paint**, can render client components as children.
- **Limitations** ⭐:
  - **NO hooks, NO state** (no `useState`/`useEffect` — compiler complains!)
  - **NO browser APIs** (no cookies via JS, no localStorage/sessionStorage)
- `console.log` in a server component appears in the **server terminal**
  (mirrored to the browser console marked "server" — dev mode only).
- Restaurant analogy: server-side rendering = traditional restaurant (dish arrives ready);
  client-side rendering = fondue (ingredients arrive, you assemble).

## 8.2 Client components ⭐

- Marked with the directive at the **very top of the file**:

```tsx
'use client';
```

- Needed for: **interactivity** (clicks, forms), **hooks** (`useState`, `useEffect`, `useContext`, …),
  **browser APIs** (sessionStorage, …).

⭐ **Known exam error message:** *"You're importing a component that needs useEffect. This
React Hook only works in a Client Component. To fix, mark the file (or its parent) with
the `"use client"` directive."*

## 8.3 Best practice

Server components render the **base of the UI**; client components add **interactivity
where needed**. If a server page needs state → extract a **client child component** that
holds the state, and pass data down as props.

---

# 9. Data Fetching: fetch & useSWR

## 9.1 In server components

```tsx
const Page = async () => {                       // async component
  const response = await fetch("http://.../lecturers");  // await fetch
  const lecturers = await response.json();               // .json() is ALSO async
  ...
};
```

Best practice: encapsulate fetching in a **service** ("separate data-fetching logic from
presentation logic; components don't need to know how or where the data comes from").

## 9.2 In client components: `useSWR` ⭐

Third-party hook from the **swr library (Vercel)** — `npm install swr`.
**Stale-While-Revalidate**: returns **cached data immediately** (stale), fetches fresh data
**in the background** (while), then **re-renders** with the latest data (revalidate).

**Parameters:** 1. **key** (unique string, usually the URL), 2. **fetcher** (async function),
3. options (optional).

**Returns:**

| Value | Meaning |
|---|---|
| `data` | the fetched data |
| `error` | error thrown by the fetcher |
| `isLoading` | fetch in progress |
| `mutate` | trigger re-evaluation of the cache |
| `isValidating` | cache is being re-evaluated |

```tsx
'use client';
const { data, error, isLoading } = useSWR("http://localhost:3000/lecturers", fetcher);
```

## 9.3 Polling

```tsx
useInterval(() => {
  mutate("lecturers", getLecturers());   // ⭐ SAME key as the useSWR call
}, 1000);
```

- With useSWR, the data is managed **by the swr library**, not your own state → refresh it
  via **`mutate(key, ...)`** with the same key.
- Use `useInterval` (not raw `setInterval`) so the interval is **cleared on unmount**.

---

# 10. Client-Side Storage

## 10.1 Browser storage ⭐

- Two types: **localStorage** (persists indefinitely) and **sessionStorage** (erased when
  the session ends). **Prefer sessionStorage** for temporary data.
- **Never store passwords!**
- Only usable in **client components** (browser API!).

```javascript
sessionStorage.setItem("loggedInUser", name);   // add
sessionStorage.getItem("loggedInUser");         // read
sessionStorage.removeItem("loggedInUser");      // delete
```

- Inspect via DevTools → Application tab.

## 10.2 Cookies vs browser storage ⭐

| | sessionStorage | localStorage | Cookies |
|---|---|---|---|
| Lifetime | until session ends | forever | **custom expiration** |
| Sent to server? | no | no | **yes — with EVERY request** (network cost) |
| Security | JS-readable (XSS risk) | JS-readable (XSS risk) | securable: **HttpOnly, SameSite, Secure** |

- Cookie risks: **XSS, session hijacking, tracking, theft**.

---

# 11. React Context

## 11.1 The problem: prop drilling ⭐

> **Prop drilling** = a prop must be passed through several layers of components to reach
> a nested child that needs it.

Problems: "dirty" middleman components accept props they don't use; boilerplate;
forces `layout.tsx` to become a client component; poor maintainability (rename or
new middle layer → many files change).

## 11.2 The solution — Context in 3 steps ⭐⭐

| Step | What | API |
|---|---|---|
| 1 | Create the "box" | `createContext` |
| 2 | Fill the box + wrap children | `<Context.Provider value={...}>` |
| 3 | Read from the box in any descendant | `useContext` |

```tsx
"use client";                              // Context ONLY works in client components!
type ThemeContextType = { theme: string; toggleTheme: () => void };
const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

export const ThemeProvider = ({ children }: { children: React.ReactNode }) => {
  const [theme, setTheme] = useState("light");
  const toggleTheme = () => setTheme(theme === "light" ? "dark" : "light");
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      <div className={theme}>{children}</div>
    </ThemeContext.Provider>
  );
};
```

Consumer:

```tsx
"use client";
const context = useContext(ThemeContext);
if (!context) throw new Error("must be used within a ThemeProvider");  // guard ⭐
const { theme, toggleTheme } = context;
```

⭐ `useContext` **outside** a Provider returns the context's **default value**
(here `undefined`) — hence the guard/throw pattern.

- Wrap the Provider around children **in `layout.tsx`** — the layout itself can stay a
  server component (the client boundary is the Provider).

## 11.3 Context vs sessionStorage ⭐ (why both?)

- **sessionStorage is NOT reactive** — changing it doesn't notify/re-render components.
- **Context/state = reactivity** (changes trigger re-renders);
  **sessionStorage = persistence** (survives page refresh).
- Use both together (see AuthContext, §15).

## 11.4 Context vs hooks ⭐

- **Context: ONE shared global state.**
- **Custom hooks: every call/instance has its OWN independent state.**

---

# 12. Custom Hooks

## 12.1 Definitions & purpose

- Hooks = special functions that let you use state and other React features in functional
  components by "hooking into" the lifecycle.
- Official docs: "**Use custom hooks to share stateful logic between components.**"
- **UI = f(state)** — the UI is a function of the state.

**Re-render triggers (ONLY these):** ⭐
1. a **state** change, 2. a **props** change, 3. a **parent re-rendering**.
Local variables never trigger re-renders and reset each render.

## 12.2 Why not the alternatives? ⭐

| Alternative | Why it doesn't work for stateful logic |
|---|---|
| Service | state change doesn't trigger re-render; services **can't use hooks** |
| Child component | hooks have no UI; a component's job is rendering, not logic |
| Context | only **1 global state**; each hook instance needs its **own** state |

Best-practice mapping: custom hooks = reusable **stateful** React logic;
services = **stateless**/non-React logic; Context = **global** state;
child components = **UI composition**.

## 12.3 Rules for custom hooks ⭐

1. Name **must start with `use` + capital letter** (`useCounter`, `useAuth`).
2. May return **arbitrary values**.
3. Only functions that **internally call a built-in hook** count as hooks.
4. Consequently: custom hooks are (almost always) **client-component only**.

## 12.4 `useCounter` example

```tsx
const useCounter = () => {
  const [count, setCount] = useState(0);
  const increment = () => setCount((c) => c + 1);
  const reset = () => setCount(0);
  return { count, increment, reset };   // setCount deliberately hidden (encapsulation)
};

// Two INDEPENDENT counters (hooks don't share state!):
const { count: countA, increment: incrementA } = useCounter();
const { count: countB, increment: incrementB } = useCounter();
```

## 12.5 `useAuth` (lab)

```tsx
const useAuth = () => useContext(AuthContext);
// then everywhere: const { user, login, logout } = useAuth();
```

Avoids repeating the `useContext(AuthContext)` boilerplate in every component.

---

# 13. Rendering Strategies

## 13.1 Terminology ⭐

- **Rendering** = turning code into visible UI.
- **Client** = the browser. **"Server" = the Next.js server, NOT the back-end API!** ⭐

## 13.2 RSC Payload ⭐

> **React Server Component (RSC) Payload** = a compact **binary** representation of the
> rendered React Server Components tree.

Contains: 1. the rendered result of server components, 2. **placeholders for client
components** (references to their JS files), 3. props passed from server → client components.

## 13.3 Static vs Dynamic rendering ⭐⭐

| | Static Rendering ("prerendering") | Dynamic Rendering |
|---|---|---|
| When | **build time** (+ cache refresh) | **request time** |
| Use for | rarely-changing pages (about page) | frequently-changing / per-request data |

⭐ **Server Components = WHERE the code runs (server). Dynamic Rendering = WHEN it runs
(request time).** Don't mix these up.

- Next.js **automatically chooses static** unless you use dynamic features:
  **`cookies()`, `headers()`, `searchParams()`**, or **`fetch(url, { cache: 'no-store' })`**.

## 13.4 Hydration ⭐

> **Hydration = React's process of attaching event handlers to the DOM to make the
> static HTML interactive.**

First load: server renders RSC payload → prerendered HTML shown (**not yet interactive**)
→ RSC payload **hydrates** the client components → app interactive.
Subsequent navigations: RSC payload from cache; client components render entirely client-side.

Full render pipeline (with context providers):
1. Server components rendered into the RSC Payload.
2. Client components = placeholders in that payload.
3. Server-side pre-render of static HTML (fast first paint).
4. Browser builds the component tree from the payload.
5. **Hydration** activates client components.
6. Server-rendered children get "plugged into" client components
   (→ that's why a server component can appear as a *child* of a client component).

## 13.5 Prefetching ⭐

> Loading a route in the background before the user navigates to it → navigation feels instant.

- Automatic for routes linked with **`<Link>`** — **NOT for `<a>`**.
- **Dynamic routes are NOT prefetched.**
- Opt out per link: `<Link prefetch={false}>`.

---

# 14. Internationalisation (i18n)

## 14.1 Definitions ⭐

- **i18n** = internationalisation (18 letters between i and n): designing software so it can
  adapt to languages/regions **without code changes**.
- **l10n** = localisation: actually **adapting to a specific market** (date formats, weights,
  language variants). ⭐ i18n ≠ l10n.
- Key aspects: **separate content from code** (text in resource files) and **locale awareness**
  (locale = language + country parameters, e.g. **`en_US`**).

## 14.2 next-intl — the 5 steps ⭐

| Step | Metaphor | What it does |
|---|---|---|
| 1. **Middleware** (`/middleware.ts`) | "traffic controller" | runs on every request; reads URL (`/en`, `/es`) and determines the locale |
| 2. **JSON translation files** | "dictionaries" | `public/locales/en/common.json`, `public/locales/es/common.json` |
| 3. **Config loader** (`/i18n.ts`) | "glue" | server-side; gets the locale from middleware and loads the right JSON |
| 4. **`NextIntlClientProvider`** | "box" (like AuthContext) | in `layout.tsx`; provides messages to **client components** |
| 5. **Hooks/functions** | "tools" | get translations in components |

Details:
- Middleware config: `locales` (supported languages), `defaultLocale` (fallback),
  `matcher` (regex: only run on actual page requests).
- The locale becomes a **dynamic route segment**: all pages move under **`app/[locale]/`**;
  URLs become `/en`, `/es`.
- If locale is `undefined`, `i18n.ts` falls back to the **defaultLocale**.
- Language switcher: implement `handleLanguageChange`, push same path with other locale.
- Adding a language later = add locale to middleware + add a JSON dictionary — **no
  component code changes** (that's the whole point of i18n).

## 14.3 Using translations — server vs client ⭐

| Component type | API | Import |
|---|---|---|
| **Server** (async) | `const t = await getTranslations('scope')` | `next-intl/server` |
| **Client** (`'use client'`) | `const t = useTranslations('scope')` | `next-intl` |

- `t('key')` looks up the key in the messages.
- ⭐ **Server components do NOT read from the Provider** — they fetch translations directly.

## 14.4 Why `next.config.js` and not `.ts`? ⭐

It's the **first file Next.js reads** on `npm run dev` — **before the TypeScript compiler
has run** — so Node expects plain JavaScript (`require` / `module.exports`).
`withNextIntl(nextConfig)` injects next-intl into the dev server.
(And `middleware.ts` must be `.ts`, not `.tsx`.)

---

# 15. Front-End Security

## 15.1 The authentication (login) flow

1. Client: `POST /users/login { username, password }`.
2. Server: authenticates (**compares password hashes**), **generates a JWT with the secret key**.
3. Server returns: user JSON (username, fullname, role) in the body **+ the token in an
   HttpOnly cookie** via the `Set-Cookie` header.

## 15.2 Where is what stored? ⭐⭐

| Data | Where | Why |
|---|---|---|
| **JWT token** | **HttpOnly cookie** (set by server) | JS cannot read it → **protects against XSS** token theft |
| **User display data** (username, role) | **sessionStorage** (+ React state) | needed by the UI; cleared when session ends |

⭐ **Never store raw passwords or tokens in session/localStorage.**

## 15.3 `credentials: "include"` ⭐

Fetch option needed to:
- **store** the cookie at login (accept the Set-Cookie header),
- **send** the cookie on subsequent client-side requests,
- include the cookie on the **logout** call.

```ts
fetch(url, { method: "POST", credentials: "include", ... });
```

CORS side: the back-end must set `allowCredentials(true)` **and an explicit allowed origin**
(wildcard `*` is not allowed with credentials).

## 15.4 Protected fetches: client vs server components ⭐⭐

- **Client component**: `fetch(url, { credentials: "include" })` — the **browser attaches the
  cookie automatically**.
- **Server component**: rendered on the Next.js server → **no access to browser cookie storage**.
  Solution: the Next.js server acts as a **proxy**:

```tsx
import { cookies } from "next/headers";           // server-side Next.js function
const cookieStore = await cookies();               // read cookies the BROWSER sent to Next.js
LecturerService.getAllLecturers(cookieStore);      // pass to service
// service: reads authToken → adds header:  Authorization: `Bearer ${token}`
```

- The back-end accepts **both**: cookie **or** Authorization header.
- ⭐ Server-side fetches are **NOT visible in the browser's Network tab** (proof the fetch
  happened server-side).
- Not logged in → no cookie to forward → back-end returns **401 Unauthorized** → service
  throws → page shows the error.
- Use `cache: "no-store"` to always get fresh data (also opts into dynamic rendering).

## 15.5 AuthContext — 5 responsibilities ⭐

1. **Manages user state** (`useState` holding name/role).
2. **`login(userData)`** — sets React state **AND** writes to sessionStorage.
3. **`logout()`** — calls `UserService.logout` (server clears the HttpOnly cookie) **AND**
   clears React state **AND** clears sessionStorage.
4. **Restores state on refresh** — `useEffect` reads sessionStorage on page load.
5. **Shares globally** — Provider wraps the whole app in RootLayout.

- `UserLoginForm` = **producer** (calls `login` after successful authenticate).
- `Header` = **consumer**: `user` present → "Welcome {fullname}" + Logout; `null` → Login link.

## 15.6 Logout ⭐⭐⭐ (your Question 4!)

A complete, secure logout is a **two-part process**:
1. **Server-side:** call the `/users/logout` endpoint → server replies with a
   **`Set-Cookie` header with `Max-Age=0`** → browser deletes the HttpOnly cookie.
   (Required because **HttpOnly cookies cannot be read or deleted by JavaScript**.)
2. **Client-side:** clear React state (`setUser(null)`) and sessionStorage → UI updates.

> **Your exam question — "What does logging-out via the front-end accomplish when using
> a JWT token?"**
> ✅ **1. It clears the storage of the front-end, so that a new user can log in.**
> ✅ **2. The token is still usable — you can still authenticate with it.**
>
> Why: JWTs are **stateless and self-contained**. The server keeps no session, so it cannot
> revoke an issued token. Front-end logout only removes the client's *copy*. Anyone still
> holding the token can use it **until it expires** — expiry is the only built-in
> invalidation mechanism.

## 15.7 Authorization on the front-end ⭐

- **Authentication = who you are. Authorization = what you may do.**
- The `user` object in AuthContext contains the **role**; conditionally render UI:

```tsx
{user?.role === "admin" && <button>Schedule</button>}
```

- ⭐ Hiding a button is **cosmetic** — the real permission check happens on the **back-end**
  (JWT verified, role checked). Two role-based techniques: back-end returns different data
  per role; front-end enables features per role.

---

# 16. Back-End Recap: Spring Boot, HTTP, Layered Architecture

## 16.1 Spring Boot

- Backend framework: starts/stops an HTTP server, database access, security.
- **Modular** (pick dependencies) and **high-level** (pulls in low-level deps).
- **Spring Initializr** generates the project skeleton.

## 16.2 HTTP methods & idempotency ⭐

| Method | Definition | Idempotent? |
|---|---|---|
| GET | retrieves data | ✅ |
| POST | submits data | ❌ |
| PUT | stores/updates a resource at a location | ✅ |
| DELETE | removes a resource | ✅ |
| PATCH | partial modifications | ❌ |

> **Idempotent** = making the same request multiple times has the same effect as once.
> ⭐ GET/PUT/DELETE are idempotent; POST/PATCH are not.

**Status codes:** 2xx success · 3xx redirect · 4xx client error · 5xx server error.

## 16.3 Layered architecture ⭐

**Rule: layers have no knowledge of the layers above them.**
Controller → Service → Model + Repository (never the other way).

| Layer | Responsibilities |
|---|---|
| **Controller** | listen to HTTP requests; **deserialize input**; call service; **serialize output** (result or errors) |
| **Service** | centralize **business logic**; handle transactions; integrate (repos, queues, other APIs) |
| **Model** | core **business logic & rules**; validation annotations; relationship mappings |
| **Repository** | **abstract & encapsulate data access** |

```java
@PostMapping("/pony")
public Pony addPony(@Valid @RequestBody Pony pony) {   // JSON deserialized here;
    return ponyService.addPony(pony);                  // validation happens AT deserialization
}
```

⭐ `@Valid` = the type has validation annotations in its class definition.
⭐ The method seems to return a `Pony`, but the response is **serialized to JSON** first.

```java
public interface OwnerRepository extends JpaRepository<Owner, Long> { }
// <Entity, IdType>; findAll(), save(), etc. come for free; derived query methods by name
```

---

# 17. New Java Features

## 17.1 `var` ⭐

- Type **inference** — type must be **unambiguously inferable from the right-hand side**.
  (`var x;` and `var x = null;` do NOT compile.)
- Allowed ONLY in: **local variables**, **for loops**, **try-with-resources**.
  (NOT for fields, method parameters, or return types.)

```java
var message = "hello";                       // String
for (var i = 0; i < 10; i++) { }
try (var scanner = new Scanner(System.in)) { }   // auto-closed
```

## 17.2 Records ⭐

- **Immutable data classes** — just declare field types and names.
- Compiler generates: **equals, hashCode, toString, private final fields, public constructor**.
- ⭐ Records **can't extend classes** (they implicitly extend `java.lang.Record`),
  but **CAN implement interfaces** and can have their own methods (which can't mutate state).
- Accessors are `person.firstName()` — not `getFirstName()`.

```java
public record Person(String firstName, String lastName) {
    public Person {          // ⭐ COMPACT constructor: NO parameter list!
        if (firstName == null || firstName.isBlank())
            throw new IllegalArgumentException("firstName required");
    }
}
```

- **Immutability** = state cannot change after creation (e.g. Strings in Java, `final` fields).
- You may override the generated methods if needed.

## 17.3 Enums ⭐

- Special "class" representing a **group of constants**; in Java an enum **can have methods**.
- Why: fixed value list, IDE autocomplete, readability (`WeekDay.Monday` vs `0`).
- Persisting: store as **text** with

```java
@Enumerated(EnumType.STRING)     // ⭐ default is ORDINAL (position number) — fragile!
private WeekDay day;
```

---

# 18. DTOs, CORS, PostgreSQL

## 18.1 DTOs ⭐

Four reasons: **Data hiding** (expose only what's needed), **Decoupling** (internal model vs
API representation), **Performance** (transfer only required data), **Flexibility** (combine
entities, omit fields).

- DTOs = simple objects, **no business logic** — fields/getters/setters/constructors only
  → **perfect fit for Java records**.
- Live in the **controller layer** (`dto` package). Controller deserializes JSON **into the
  DTO**; **validation is on the DTO**; service translates DTO → domain models.
- ⭐ A DTO can nest **other DTOs but NOT model classes**; nested DTOs need **`@Valid` on the
  field** or their validation is silently skipped:

```java
public record PonyInput(@NotBlank String name, @Valid OwnerInput owner) { }
```

## 18.2 CORS ⭐

- **Same-Origin Policy (SOP)**: scripts may interact only when **scheme + domain + port**
  all match. Defence against **CSRF**.
- Frontend `localhost:5173` → backend `localhost:8080` = **different port = different origin**
  → browser blocks without CORS headers.
- Course fix: `@CrossOrigin(origins = "http://...")` on every controller class
  (later: WebConfig with `allowCredentials(true)` — see §15.3).

## 18.3 PostgreSQL

- Real DB instead of in-memory H2; only the **connection settings** change; needs the
  `postgresql` dependency.
- ⭐ **No `auto-increment` in PostgreSQL** — serial types instead:
  `smallserial` (smallint), `serial` (integer), `bigserial` (bigint).

---

# 19. Database Access with JDBC

## 19.1 JDBC vs JPA ⭐

- **JDBC** = **low-level**: you manage the connection and write SQL yourself.
- **JPA** = **high-level**: you operate on objects; the implementation (**Hibernate**)
  generates SQL.
- Use Spring's enhanced versions because of: simplified code, **unified exceptions**,
  **unified transactions**, security (no plain-text SQL → **SQL-injection protection**).

## 19.2 `JdbcClient` basics

```java
@Repository
public class JdbcCourseRepository {
    private final JdbcClient jdbcClient;   // constructor-injected
}

// WRITE: sql(...) → .param(...) → .update()
jdbcClient.sql("truncate table course cascade").update();

// INSERT with named parameters + generated key:
KeyHolder keyHolder = new GeneratedKeyHolder();
jdbcClient.sql("insert into course (name, ...) values (:name, ...) returning id")
    .param("name", course.getName())
    .update(keyHolder);
course.setId(keyHolder.getKeyAs(Long.class));

// READ: .query(...) → .list() (many) or .optional() (0 or 1)
jdbcClient.sql("select ... from course").query(Course.class).list();
jdbcClient.sql("select ... where id = :id").param("id", id)
    .query(new CourseRowMapper()).optional();
```

⭐ With JDBC **you** maintain relationships: insert join-table rows manually, keep FKs
correct, fill the object network when querying.
⭐ Default `.query(Course.class)` mapping fills only **simple fields with matching names**;
relationship fields (like `lecturers`) stay **empty**.

## 19.3 `RowMapper` ⭐

**A RowMapper always maps ONE row to ONE object.**

```java
public class CourseRowMapper implements RowMapper<Course> {
    @Override
    public Course mapRow(ResultSet rs, int rowNum) throws SQLException {
        Course course = new Course(rs.getString("course_name"),
            rs.getString("description"), rs.getInt("phase"), rs.getInt("credits"));
        course.setId(rs.getLong("course_id"));
        return course;
    }
}
```

- Column names must match the **aliases in the SQL** (`name as course_name`).
- Single-valued relationships (Lecturer → User): **inner join** + **compose** mappers
  (`new UserRowMapper().mapRow(rs, rowNum)` inside `LecturerRowMapper`).
- Enum columns: `Role.valueOf(rs.getString("role").toUpperCase(...))`.
- ⭐ A RowMapper can **never** fill a multi-valued (collection) relationship.

## 19.4 `ResultSetExtractor` ⭐⭐⭐ (your Question 5!)

**Problem:** joining lecturer ↔ courses gives **multiple rows per lecturer** → RowMapper
can't handle it. **Solution:** map the **whole result set at once**.

```java
public class LecturerResultSetExtractor implements ResultSetExtractor<List<Lecturer>> {
    @Override
    public List<Lecturer> extractData(ResultSet rs) throws SQLException {
        Lecturer currentLecturer = null;
        List<Long> lecturerIds = new ArrayList<>();
        List<Lecturer> lecturers = new ArrayList<>();
        while (rs.next()) {
            Long currentLecturerId = rs.getLong("lecturer_id");
            if (!lecturerIds.contains(currentLecturerId)) {      // new lecturer?
                Lecturer lecturer = new LecturerRowMapper().mapRow(rs, rs.getRow());
                lecturerIds.add(currentLecturerId);
                lecturers.add(lecturer);
                currentLecturer = lecturer;                       // becomes "current"
            }
            currentLecturer.addCourse(new CourseRowMapper().mapRow(rs, rs.getRow()));
        }
        return lecturers;
    }
}
```

How it works: per row, if the lecturer id is **new**, map + add a new Lecturer and make it
`currentLecturer`; **every** row's course is added to `currentLecturer`.

⭐⭐ **THE trap: this assumes all rows of one lecturer are CONSECUTIVE.** If rows for the
same lecturer are interleaved with other lecturers, `currentLecturer` doesn't switch back
(the id was already seen) → **courses get attached to the WRONG lecturer**.
**Must be enforced with `ORDER BY` in the query!**

> **Your exam question — findAll with LEFT OUTER JOINs and this extractor, will it work?**
> Analyze in two steps:
> 1. **Are all lecturers present?** `left outer join` keeps lecturers without courses → yes,
>    all lecturers appear. But with **inner joins**, lecturers without courses would be
>    **missing**.
> 2. **Are the courses correct?** Only if the result set is **ordered by lecturer** — with
>    no `ORDER BY`, ordering is not guaranteed → courses can be attached to the wrong
>    lecturer. Also: with `left outer join`, a lecturer with NO courses produces a row with
>    NULL course columns, and this code still calls `addCourse(...)` on it (maps a bogus
>    course) — another defect.
>
> So for the variant in your screenshot (no ORDER BY, and the correct answer marked):
> **"No. Not all lecturers will be present. Those that are present will not have their
> correct courses."** applies when the query uses **inner joins** (drops course-less
> lecturers) **and** lacks reliable ordering — learn to check BOTH axes:
> *join type → which lecturers appear* and *row order → whether courses land on the right
> lecturer*.

⭐ Why a good ResultSetExtractor is hard: deeply coupled to result-set details (aliases,
ordering, join shape); complexity grows with relationships (cyclic refs: course ↔ lecturer);
you must know exactly how the result is used → foreshadows **aggregates**.

## 19.5 JDBC conclusions

Any query is possible, but repositories are **very hard** to write correctly and **very
boring** (same structure every time) → JPA fixes both, at the cost of control.

---

# 20. JPA & Spring Data JPA

## 20.1 ORM concepts (definitions) ⭐

- **ORM** = mapping Java objects ↔ database tables; syncs object operations to SQL
  automatically.
- **Entity** = object whose state can be stored/retrieved via JPA.
- **EntityManager** = manages a unit of work + the persistent objects therein
  (= the **persistence context**).
- **EntityManagerFactory** = creates an EntityManager per request.
- **Persistence context** = all entities currently managed by JPA; by default opened and
  closed **per request**.
- **Flush** = syncing the persistence context state to the database (most often at
  **transaction end**).

## 20.2 EntityManager API ⭐

| Method | SQL |
|---|---|
| `persist(o)` | INSERT |
| `remove(o)` | DELETE |
| `find(Class, pk)` | SELECT by primary key |
| `createQuery(jpql)` | JPQL query |
| `flush()` | force sync now |

⭐ **Why is there no `update()` method?** Updates happen **automatically**: retrieve the
managed entity (`find`), mutate it, and the flush at transaction end writes the change
(**dirty checking**). `find` is enough to update.

## 20.3 JPQL ⭐

**Most important rule: you query your ENTITIES, not your database tables!**

```java
entityManager.createQuery("select c from Course c", Course.class).getResultList();
entityManager.createQuery("delete from Course").executeUpdate();

// navigate relationships — join-table is invisible in JPQL:
"select s from Schedule s join s.lecturer l join l.user u where u.username = :username"
```

## 20.4 Spring Data JPA ⭐

Spring generates the repository implementation from the **entity type** and
**method names** (derived queries):

```java
public interface ScheduleRepository extends JpaRepository<Schedule, Long> {
    List<Schedule> findByLecturer_User_Username(String username);  // _ navigates nesting
    boolean existsByCourse_IdAndLecturer_Id(Long courseId, Long lecturerId);
}
```

- **`@Query`**: custom **JPQL** (default); method name is then free-form.
- **`@Query(value = "...", nativeQuery = true)`**: native SQL — use with care.
- Recommendation: **prefer JPA (Spring Data JPA) over JDBC** unless low-level control is
  critical; add `@Query` for complex cases; occasionally mix in custom JDBC.

---

# 21. JPA Advanced

## 21.1 Transactions & ACID ⭐

> **Transaction** = a set of tasks executed as one **atomic, consistent, isolated, durable**
> operation.

- **A**tomic: all-or-nothing. **C**onsistent: constraints never violated.
  **I**solated: transactions isolated from each other. **D**urable: committed = permanent.
- ⭐ **Default: every repository call runs in its OWN transaction** (committed at its end)
  → multi-step use cases are NOT atomic by default.

## 21.2 `@Transactional` ⭐⭐

```java
@Service
@Transactional          // class level = all public methods; method level overrides
public class LecturerService { ... }
```

- Wraps the whole method in one transaction.
- ⭐ **Only works when called from ANOTHER class** (proxy-based; self-invocation bypasses it).
- ⭐ **Rollback on `RuntimeException`** — all changes in the transaction are undone
  (even already-saved rows "disappear"). Tune with `rollbackFor` / `noRollbackFor`.
- Best practice: **services are the unit-of-work boundary**; annotate all services.
- `@PostConstruct` methods (e.g. DbInitializer) can't use `@Transactional` → use a
  **`TransactionTemplate`** (`transactionTemplate.executeWithoutResult(status -> { ... })`).

## 21.3 Isolation levels ⭐

- **Dirty read** = reading another transaction's uncommitted (possibly rolled-back) data.

| Level | Prevents |
|---|---|
| `READ_UNCOMMITTED` | nothing (dirty reads possible) |
| `READ_COMMITTED` | dirty reads — **default for most databases** |
| `REPEATABLE_READ` | + non-repeatable reads |
| `SERIALIZABLE` | + phantom reads (highest) |

⭐ **Higher isolation = lower performance.**

- **Pessimistic locking** — exclusive lock up front, prevents conflicts; best with **many**
  conflicts (no rollbacks ever needed).
- **Optimistic locking** — version number checked at update; conflict → error + rollback;
  best with **few** conflicts.

## 21.4 Propagation ⭐

- **`REQUIRED`** (**default**): join the current transaction; create one if none exists.
- **`REQUIRES_NEW`**: always create a new transaction, **suspending** the current one;
  it commits before returning to the caller.

## 21.5 Entity lifecycle — the 4 states ⭐⭐

| State | Meaning |
|---|---|
| **Transient** | new/unknown object; saved on flush **after `persist`** |
| **Managed** | tracked; changes synced on flush (dirty checking) |
| **Removed** | will be deleted on flush |
| **Detached** | disconnected; ⭐ **after the persistence context closes, ALL entities are detached** |

⭐⭐ **The three update scenarios (classic "will the row be updated?" question):**

| Scenario | `@Transactional`? | explicit `save`? | Row updated? |
|---|---|---|---|
| 1 | no | yes | ✅ (save flushes) |
| 2 | no | no | ❌ — no flush after the mutation! |
| 3 | yes | no | ✅ — entity is managed; flush at tx end (**dirty checking**) |

⭐ **Transient-reference crash:** saving an entity that references an **unsaved transient**
entity → error *"Not-null property references a transient value — transient instance must
be saved before current operation"*. Fix: save the referenced entity first, or use a cascade.

## 21.6 Cascades ⭐

- Propagate an operation from one entity to its associated entity.
- Types: `PERSIST`, `MERGE`, `REMOVE`, `REFRESH`, `DETACH`, `ALL`.

```java
@OneToOne(cascade = CascadeType.PERSIST)   // persisting owner persists the User too
@JoinColumn(name = "user_id")
private User user;
```

⭐ Cascades sit on the **relationship annotation** → apply in **every** use case (helpful in
one, harmful in another).

## 21.7 Fetch types ⭐

- **`EAGER`** — related entity loaded immediately.
- **`LAZY`** — loaded on first access; ⭐ **only works while the entity is MANAGED**
  (detached + lazy access → LazyInitializationException).
- Guideline: single-valued → often EAGER; collections → usually LAZY.
- JPQL: plain **`join`** only **filters** (doesn't load the related entities);
  **`join fetch`** **eagerly loads** them in the same query.

## 21.8 Orphan removal ⭐

- On `@OneToOne` / `@OneToMany` only.
- WITH `orphanRemoval = true`: removing the entity from the relationship **deletes it from
  the DB**. WITHOUT: relationship broken, row **kept**.

## 21.9 Aggregates (outlook)

- Aggregate = cluster of objects treated as a single unit; inside: object references,
  between aggregates: **reference by primary key only**. Counters JPA's lack of control
  as domains grow.

---

# 22. JPA Relationships

## 22.1 Ownership & direction ⭐

- Relationships are declared between `@Entity` classes.
- ⭐ **The owner of the relationship = the table containing the FOREIGN KEY.**
- **Unidirectional** = navigable one way, mapping on one side.
  **Bidirectional** = navigable both ways, mapping on **both** sides.

## 22.2 Bidirectional one-to-many / many-to-one ⭐

```java
// OWNER side (the MANY side — it has the FK column):
@ManyToOne
@JoinColumn(name = "owner_id")     // maps to the FK column
private Owner owner;

// NON-owner side:
@OneToMany(mappedBy = "owner")     // names the FIELD on the owner side
private List<Pony> ponies;
```

## 22.3 Bidirectional many-to-many ⭐

- FK lives in a separate **join table** → neither entity holds the other's PK →
  ⭐ **you choose** which side is the owner.

```java
// chosen owner:
@ManyToMany
@JoinTable(name = "pony_owner",
    joinColumns = @JoinColumn(name = "pony_id"),
    inverseJoinColumns = @JoinColumn(name = "owner_id"))
private Set<Owner> owners;

// other side:
@ManyToMany(mappedBy = "owners")
private Set<Pony> ponies;
```

## 22.4 JSON recursion ⭐

Bidirectional mappings cause **infinite recursion** when serializing:
- **`@JsonManagedReference`** — the **forward** side, serialized normally.
- **`@JsonBackReference`** — the **backward** side, **omitted** from serialization.

---

# 23. OpenAPI & Swagger

## 23.1 The problems it solves

1. **Developer disconnect** (FE/BE out of sync: "what's the endpoint? the JSON structure?").
2. **Manual documentation** is tedious, error-prone, always outdated (time gap between
   deploy and doc update). Generated docs can't drift.

## 23.2 OpenAPI vs Swagger ⭐

- **OpenAPI** = the **specification** (blueprint): language-agnostic description of
  endpoints, parameters, responses, data models.
- **Swagger** = the **set of tools** built around OpenAPI; most famous: **Swagger UI** —
  interactive web docs with a **"Try it out"** button per endpoint (anyone can test in the
  browser, no Postman/code needed).

## 23.3 Setup & URLs ⭐

Dependency: `org.springdoc : springdoc-openapi-starter-webmvc-ui` (v2.8.4). Gives you:

| URL | Content |
|---|---|
| `/v3/api-docs` | the raw OpenAPI **JSON** describing your API |
| `/swagger-ui.html` | the interactive **Swagger UI** page |

## 23.4 Customizing & annotations ⭐

- Config class: **`@Configuration`** = a source of bean definitions — contains
  **`@Bean`** methods producing beans managed by the Spring container.
  Return a customized `OpenAPI` bean (title, description, version, licence).

| Annotation | Describes |
|---|---|
| `@Operation` | what a single endpoint/method does |
| `@ApiResponse` | a possible response (200, 404, …) |
| `@Parameter` | details of a method parameter |

---

# 24. Back-End Security

## 24.1 The four parts

1. **User Signup** — store the password safely.
2. **Authentication** — **"Who are you?"**
3. **Authorization** — **"What can you do?"**
4. Secure traffic (HTTPS).

⭐ Authentication = identity; Authorization = permissions. Don't swap.

## 24.2 Passwords: hashing vs encryption ⭐⭐

> **Never store passwords in plain text.**

| | Hashing | Encryption |
|---|---|---|
| Purpose | integrity / **verification** | **confidentiality** |
| Mechanism | one-way function → fixed-size hash | algorithm + key → ciphertext |
| Reversible? | **NO** | **YES (with the key)** |
| Use case | **password storage**, signatures, checksums | secure communication, file encryption |

- Passwords are **hashed** with **BCrypt** via the **`PasswordEncoder`** interface;
  supply a bean: `new BCryptPasswordEncoder(strength)`.
- ⭐ Bcrypt **strength**: typically **10–14**; **higher = stronger but slower**.
- Login verifies by **hashing the supplied password and comparing hashes** (never
  "decrypting" the stored one).
- ⭐ Seed/DbInitializer users must also get **encoded** passwords or login fails.

## 24.3 Dependencies & OAuth2

- `spring-boot-starter-security` + `spring-boot-starter-oauth2-resource-server`.
- **OAuth 2.0** = industry-standard protocol for **authorization** — secure access to
  resources **without sharing credentials**.
- **Valet-key analogy**: password = master key (total power); token = valet key
  (limited, scoped access).

## 24.4 Default behavior & SecurityFilterChain ⭐⭐

⭐ Adding the security starter → **ALL endpoints require authentication by default**
(even `/login`, `/status`, swagger-ui) and calls get redirected to a default login form.

Fix with a **`SecurityFilterChain`** bean:

```java
http
  .csrf(csrf -> csrf.disable())        // ⭐ ALWAYS disable CSRF in a STATELESS API
  .authorizeHttpRequests(auth -> auth
      .requestMatchers("/users/login", "/users/signup", "/status",
                       "/swagger-ui/**", "/v3/api-docs/**").permitAll()
      .anyRequest().authenticated());  // ⭐ catch-all LAST
```

⭐ **`requestMatchers` order matters — evaluation stops at the FIRST match.**
⭐ CSRF protection targets cookie/session-based *stateful* auth; a stateless token API has
no server session to forge → disable it.

## 24.5 JWT ⭐⭐

- **JWT = JSON Web Token** — transmit data securely as JSON; used for authentication AND
  authorization; stored in browser session storage or **secure HttpOnly cookies**.
- **Stateless security**: client sends the JWT with **every request** (Authorization header
  or cookie); the server keeps **no session state**.
- **Bearer token**: whoever **possesses** ("bears") it gets access — no further proof of
  identity → protect it (HttpOnly, HTTPS, short lifetime).
- **Three parts** ⭐:
  1. **Header** — metadata (token type, algorithm)
  2. **Payload** — **claims** (user ID, expiration, roles)
  3. **Signature** — ensures **integrity** (tamper-proof; only the secret-key holder can sign)

## 24.6 Login plumbing (know the component names) ⭐

| Component | Role |
|---|---|
| `AuthenticationManager` | validates user credentials (bean obtained from `AuthenticationConfiguration`) |
| `JwtService` | generates tokens (from username/password or User) |
| `JwtProperties` | record via `@ConfigurationProperties`: **issuer**, **lifetime/expiry**, **secret key** (from `application.yaml`) |
| `JwtEncoder` | **signs** the token with the SecretKey |
| `JwtDecoder` | parses & **validates** structure + signature, reads contents |
| `BearerTokenResolver` | **extracts** the token from the request; custom one reads the named **cookie** (Spring has no cookie resolver by default) |
| `UserDetails` / `UserDetailsService` / `UserDetailsPasswordService` | interfaces your user/model layer must implement |
| **Principal** | the currently authenticated user (typically a `UserDetails`, cast to your implementation) |

- **`ResponseCookie`** creates cookies "secure by default": **HttpOnly** (no JS access),
  **Secure** (HTTPS only), **SameSite** (Strict/Lax/None), **maxAge** (seconds).
- ⭐ In the login response, the **token is stripped from the body** and returned **only as
  a cookie** (visible in Set-Cookie, NOT in the response body).
- **Roles** = high-level permissions (`ROLE_ADMIN`); **Authorities** = fine-grained
  (`READ_PRIVILEGE`); both are **`GrantedAuthority`** objects.

## 24.7 Authorization ⭐

Flow per API request: client sends JWT (cookie) → server **verifies the JWT** → adds user
info (name, role) to the request → checks the **role** for permission → returns or denies.

- Enable method security: **`@EnableMethodSecurity`** on the SecurityConfig class.

| Annotation | Checks | On failure |
|---|---|---|
| **`@PreAuthorize`** | **before** the method runs — method NOT executed if condition fails | `AccessDeniedException` |
| **`@PostAuthorize`** | **after** the method, **before returning** the result | exception; result withheld (use: filter sensitive data) |

```java
@PreAuthorize("hasRole('ADMIN')")
@PostMapping("/students")
public Student addStudent(...) { ... }   // only admins can add students
```

## 24.8 Security testing

- Dependency `spring-security-test` → **`@WithMockUser`** simulates a logged-in user in
  service tests: `@WithMockUser(username = "admin", roles = {"ADMIN"})`.
- E2E tests: **generate a real token with the `jwtService`** and attach it.

---

# 25. Back-End Testing

## 25.1 Unit tests (JUnit) — domain models

- `@Test` marks a test; `@BeforeEach` runs before **each** test (setup/reset).
- `Assertions.assertEquals/assertTrue/...` verify results.
- Exceptions:

```java
Exception ex = assertThrows(DomainException.class, () -> sut.someMethod(badInput));
assertEquals("expected message", ex.getMessage());
```

## 25.2 Unit tests (Mockito) — services ⭐

Mock the repositories, inject into the service:

```java
@ExtendWith(MockitoExtension.class)
class PonyServiceTest {
    @Mock PonyRepository ponyRepository;
    @InjectMocks PonyService ponyService;

    // 1) stub:    when(ponyRepository.save(pony)).thenReturn(pony);
    // 2) call the service method under test
    // 3) verify:  verify(ponyRepository).save(pony);
    // any-arg:    when(ponyRepository.findByName(anyString())).thenReturn(pony);
}
```

## 25.3 End-to-end tests ⭐

- Use **`WebTestClient`**; **`exchange()` actually performs the HTTP request**;
  inject the **real repository** to check the final DB state.
- ⭐ **Reset data with `@BeforeEach`/`@AfterEach`** or tests pollute each other.

```java
webTestClient.post().uri("/pony").bodyValue(pony)
    .exchange().expectStatus().isOk();
assertEquals(1, ponyRepository.count());
```

---

# 26. The Ultimate Exam-Trap Checklist

**JavaScript / TypeScript**
1. `const` object → **properties CAN be mutated** (prints "B"); reassignment = runtime TypeError.
2. Object destructuring by **name** (wrong name → `undefined`); arrays by **position**.
3. Arrow function with `{ }` needs `return`; without braces returns implicitly.
4. `??` fires only on `null`/`undefined`; `||` on all falsy values.
5. Optional `?` property absent → **`undefined`** (not `null`).
6. `async` functions always return a Promise; `await` only inside `async`.
7. `expect(() => fn()).toThrow(...)` — MUST wrap in a lambda.
8. `toBe` = strict/reference; `toEqual` = deep field-by-field.

**React / Next.js**
9. `useEffect(cb, [x])` → **mount + updates where x changed**; `[]` → mount only;
   no array → every render. Cleanup fn → unmount (and before re-run).
10. Controlled input = `value={state}` **AND** `onChange` setter. `defaultValue` = uncontrolled.
    `value` without `onChange` = read-only.
11. State setter triggers re-render; local variables don't and reset each render.
12. Hooks: top-level only (call-order tracking); names start with `use`+Capital.
13. Server components: default, async, no hooks/state/browser APIs; `'use client'` enables them.
14. `params` in a dynamic route page is a **Promise** → `await params`.
15. Component must return ONE root node (use `<>...</>` fragments).
16. `event.preventDefault()` stops the full-page refresh on form submit.
17. Context: 1 shared state, client components only; outside Provider → default value.
    Custom hook: each instance = own state.
18. sessionStorage = persistence, NOT reactive; state/context = reactivity.
19. Static rendering = build time; dynamic = request time; triggers: `cookies()`, `headers()`,
    `searchParams()`, `cache: 'no-store'`. Hydration = attaching event handlers.
20. Prefetch: `<Link>` only, not `<a>`, not dynamic routes; `prefetch={false}` opts out.
21. Server-side fetches are invisible in the browser Network tab.
22. `next.config.js` must be `.js` (read before TS compiles).

**Security**
23. FE logout with JWT: **clears front-end storage only; the token stays valid until expiry**.
24. HttpOnly cookies can't be read/deleted by JS → logout must call the server
    (`Set-Cookie: Max-Age=0`).
25. `credentials: "include"` to store/send cookies; CORS needs explicit origin +
    `allowCredentials(true)`.
26. Hashing = one-way (passwords); encryption = reversible with key.
27. Authentication = who; Authorization = what. `@PreAuthorize` before, `@PostAuthorize` after.
28. Spring Security default: EVERYTHING requires auth; matcher order = first match wins;
    disable CSRF in stateless APIs.
29. JWT = header + payload (claims) + signature; bearer = possession = access.

**Java / Spring / JPA**
30. GET/PUT/DELETE idempotent; POST/PATCH not.
31. `var`: local vars, for loops, try-with-resources only; type must be inferable.
32. Records: immutable, generated equals/hashCode/toString/ctor; can't extend classes,
    can implement interfaces; compact ctor has NO parameter list.
33. `@Enumerated(EnumType.STRING)` — default ORDINAL is fragile.
34. Relationship owner = table with the **FK**; `mappedBy` on the non-owner;
    many-to-many owner is **your choice**; `@JsonManagedReference`/`@JsonBackReference`
    stop infinite recursion.
35. RowMapper: 1 row → 1 object; can't fill collections.
36. ResultSetExtractor: whole result set; **needs ORDER BY** or children attach to the
    wrong parent; inner joins drop parents without children (left outer join keeps them).
37. EntityManager has **no update()** — dirty checking at flush; `find` + mutate inside a
    transaction is enough.
38. No `@Transactional` + no `save` after mutation → row NOT updated.
39. `@Transactional` only via calls from another class; RuntimeException → rollback;
    default propagation REQUIRED; REQUIRES_NEW suspends + commits separately.
40. Transient referencing transient → crash on save (fix: save first or CascadeType.PERSIST).
41. LAZY only works on MANAGED entities; `join` filters, `join fetch` loads.
42. orphanRemoval: removed from collection ⇒ deleted from DB.
43. DTOs: controller layer, validation on the DTO, nested DTOs need `@Valid`.
44. Same-origin = scheme + domain + **port**; different port = CORS needed.
45. PostgreSQL: no auto-increment → serial/bigserial/smallserial.
46. OpenAPI = spec; Swagger = tools; `/v3/api-docs` = JSON; `/swagger-ui.html` = UI.
47. Bcrypt strength 10–14: higher = stronger + slower.
48. E2E tests: `exchange()` fires the request; reset data between tests.

---

# 27. Practice Questions

*(Modeled on your screenshots — answers at the bottom of each block.)*

**Q1.** What is the output?

```javascript
const user = { name: "A" };
user.name = "B";
console.log(user.name);
```

a) Compilation error b) "A" c) "B" d) Runtime error

> **c) "B"** — const blocks reassignment of the binding, not mutation of the object.

**Q2.** In which lifecycle phases does this run?

```jsx
useEffect(() => { console.log(userId) }, [userId]);
```

> **Mounting AND Updating (whenever userId changes).**
> `[]` → mounting only. No array → mounting + every update. Cleanup fn → unmounting.

**Q3.** Which input keeps `username` state in sync while typing?

> `value={username}` **and** `onChange={(e) => setUsername(e.target.value)}` — both are
> required. `defaultValue` = uncontrolled; `onChange={() => setUsername(username)}` never
> reads the typed value.

**Q4.** What does front-end logout accomplish with a JWT?

> **It does not need to / cannot invalidate the token.** It clears the front-end storage
> (state + sessionStorage; the server deletes the cookie via Max-Age=0) so another user
> can log in — **but the JWT itself remains usable until it expires** (stateless!).

**Q5.** A `findAll()` joins lecturer→user→courses and uses the ResultSetExtractor from §19.4
without an `ORDER BY`. Will it work as intended?

> Check two axes: **join type** (inner join drops lecturers without courses; left outer
> join keeps them but yields NULL course rows this extractor mishandles) and **row order**
> (without ORDER BY, rows of one lecturer may not be consecutive → courses attach to the
> wrong lecturer). Without ORDER BY + inner joins:
> **"No. Not all lecturers will be present. Those that are present will not have their
> correct courses."**

**Q6.** A service method without `@Transactional` calls `findPonyByName`, mutates the pony,
and returns it without `save`. Is the DB updated?

> **No.** Each repo call had its own transaction; no flush happened after the mutation.
> With `@Transactional` on the service (and no save) it **would** update — dirty checking.

**Q7.** `useContext(ThemeContext)` is called in a component that is NOT wrapped in the
Provider. What happens?

> It returns the context's **default value** (e.g. `undefined`); the course pattern then
> throws an explicit Error.

**Q8.** After adding `spring-boot-starter-security`, a GET to `/status` returns a redirect
to `/login`. Why?

> Default behavior: **all endpoints require authentication** until a `SecurityFilterChain`
> with `permitAll()` matchers is configured (matcher order matters — first match wins).

**Q9.** `public record Person(String first, String last) {}` — which is TRUE?

> Generated: equals/hashCode/toString, private final fields, public constructor. Records
> are immutable, can't extend classes, CAN implement interfaces; accessor is `first()`.

**Q10.** Saving a `Lecturer` that references a `User` that was never saved (no cascade)?

> **Crash**: "transient instance must be saved before current operation". Fix: save the
> User first, or `cascade = CascadeType.PERSIST` on the relationship.

---

*Good luck. Read §26 the morning of the exam.*
