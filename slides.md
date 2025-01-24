---
theme: seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: 'text-center'
highlighter: shiki
lineNumbers: true
info: |
  ## Asynchronous JavaScript
  Learn about Callbacks, Promises, and Async/Await
drawings:
  persist: false
css: unocss
---

# Asynchronous JavaScript

Understanding Callbacks, Promises, and Async/Await

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

---

# What is a Synchronous System?

<div class="grid grid-cols-2 gap-4">
<div>

Well, JavaScript is by default Synchronous [single threaded]. Think about it like this – one thread means one hand with which to do stuff.

In a synchronous system, tasks are completed one after another.

- Think of it like having just one hand to accomplish 10 tasks
- Each task must wait for the previous one to complete
- Tasks are executed in a sequential order
- Blocking: If one task takes a long time, it blocks all other tasks



</div>
<div>

<div class="flex items-center justify-center">
  <img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExbnRqZDh4anlxNHNqc3gyM3p1bzhtdmI4cDUyOGxzNm95dWIxcGZsYyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/ICIS16DkE9qB9HVxtq/giphy.gif" class="rounded-lg shadow-xl" alt="One task at a time"/>


</div>

<div class="col-span-2">

  <v-click>

### Example:
```javascript
console.log('First');
const result = someTimeConsumingOperation(); // Blocks here!
console.log('Second'); // Has to wait
```

</v-click>
</div>
</div>


</div>

---

# What is an Asynchronous System?

<div class="grid grid-cols-2 gap-4">
<div>

In this system, tasks are completed independently.

Here, imagine that for 10 tasks, you have 10 hands. So, each hand can do each task independently and at the same time.

Again, all the images are loading at their own pace. None of them is waiting for any of the others.

<v-click>

### Example:
```javascript
console.log("I"); // first run

// This will be shown after 2 seconds

setTimeout(()=>{
  console.log("play"); // third run
},2000)

console.log("Cricket") // second run
```

</v-click>
</div>


<div class="flex items-center justify-center">
  <img src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExeWR0a3lvMWttMnJ1bjhpOGZkYzY5MGk5YjBiYzZxNzNrcjI2ZG51eiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/MMDnmJnE7uhX6KtcKc/giphy.gif" class="rounded-lg shadow-xl" alt="Multiple tasks at once"/>
</div>

</div>

---

# Synchronous vs Asynchronous JS

To Summarize:
When three images are on a marathon, in a:

<div class="grid grid-cols-2 gap-4 mt-4">
<div>

### Synchronous System
- Three images are in the same lane
- One can't overtake the other
- Race is finished one by one
- If image number 2 stops, the following image stops

<v-click>

<img src="/images/sync.png" class="rounded-lg shadow-xl mt-4" alt="Synchronous system diagram"/>
</v-click>

</div>

<div>

### Asynchronous System
- Three images are in different lanes
- They'll finish the race at their own pace
- Nobody stops for anybody

<v-click>

<img src="/images/async.png" class="rounded-lg shadow-xl mt-4" alt="Asynchronous system diagram"/>
</v-click>


</div>
</div>

---

# Callbacks

When you nest a function inside another function as an argument, that's called a callback

The traditional way to handle asynchronous operations.





```javascript {all|1|2-4|6-8|all}
function fetchData(callback) {
  setTimeout(() => {
    callback('Data fetched!');
  }, 2000);
}

fetchData((result) => {
  console.log(result); // 'Data fetched!'
});

```

<!-- <arrow v-click="3" x1="90" y1="420" x2="230" y2="300" color="#564" width="3" arrowSize="1" />
<span v-click="3">Code is executed!</span> -->


### SetTimeOut Syntax


```javascript
setTimeout ( ()=>{}, 1000 )
```


<v-click v-click="5"> <arrow v-click="5"  x1="120" y1="450" x2="200" y2="410" color="red" width="3" arrowSize="1" /> <div style="color: red;margin-top:10px;margin-top:30px;">Calling Function [callback]</div> </v-click>

<v-click v-click="6"> <arrow v-click="6" x1="290" y1="475" x2="280" y2="410" color="green" width="3" arrowSize="1" /> <div style="color: green;padding-left:200px;margin-top:0px;">Setting time <br>(1000 milliseconds = 1 second)</div> </v-click>
---

### Callback Hell Example 😱
<v-click>



```javascript
fetchUserData((user) => {
  fetchUserPosts(user.id, (posts) => {
    fetchComments(posts[0].id, (comments) => {
      console.log(comments);
    });
  });
});
```

</v-click>

---

# Promises

The Promise object represents the eventual completion (or failure) of an asynchronous operation and its resulting value.

<v-click>
<img src="/images/promise.png" width="300">
</v-click>


---

# Promises

A cleaner way to handle asynchronous operations

```javascript {all|1-7|9-12|all}
function fetchUser() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({ id: 1, name: 'John' });
    }, 1000);
  });
}

fetchUser()
  .then(user => console.log(user))
  .catch(error => console.error(error))
  .finally(() => console.log('Done!'));
```

---

# Promises

A cleaner way to handle asynchronous operations

```javascript {all|1-7|9-12|all}
function fetchUser() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({ id: 1, name: 'John' });
    }, 1000);
  });
}

fetchUser()
  .then(user => console.log(user))
  .catch(error => console.error(error))
  .finally(() => console.log('Done!'));
```

<v-click>

### Promise Chaining

```javascript
fetchUser()
  .then(user => fetchPosts(user.id))
  .then(posts => fetchComments(posts[0].id))
  .then(comments => console.log(comments))
  .catch(error => console.error(error));
```

</v-click>

---

# Promise Methods

Useful Promise static methods

```javascript {all|1-6|8-13|15-20|all}
// Promise.all - Wait for all promises to resolve
Promise.all([
  fetchUser(),
  fetchPosts(),
  fetchComments()
]).then(([user, posts, comments]) => console.log(user, posts, comments));

// Promise.race - Get the first promise to resolve
Promise.race([
  fetch('endpoint-1'),
  fetch('endpoint-2')
]).then(result => console.log('Fastest response:', result));

// Promise.allSettled - Wait for all promises to settle
Promise.allSettled([
  Promise.resolve(1),
  Promise.reject('error'),
  Promise.resolve(3)
]).then(results => console.log(results));
```

---

# Async/Await

The modern way to handle asynchronous operations

```javascript {all|1-7|9-19|all}
async function fetchUser() {
  const response = await fetch('https://api.example.com/user');
  if (!response.ok) {
    throw new Error('Failed to fetch user');
  }
  return response.json();
}

async function getUserData() {
  try {
    const user = await fetchUser();
    const posts = await fetchPosts(user.id);
    const comments = await fetchComments(posts[0].id);

    console.log({ user, posts, comments });
  } catch (error) {
    console.error('Error:', error);
  } finally {
    console.log('Done!');
  }
}
```

---

# Real-World Example

Building a simple image loader with all three approaches

```javascript
// 1. Callback Approach
function loadImageCallback(url, callback) {
  const img = new Image();
  img.onload = () => callback(null, img);
  img.onerror = () => callback(new Error('Failed to load image'));
  img.src = url;
}

// 2. Promise Approach
function loadImagePromise(url) {
  return new Promise((resolve, reject) => {
    const img = new Image();
    img.onload = () => resolve(img);
    img.onerror = () => reject(new Error('Failed to load image'));
    img.src = url;
  });
}

// 3. Async/Await Approach
async function loadImages(urls) {
  try {
    const images = await Promise.all(
      urls.map(url => loadImagePromise(url))
    );
    return images;
  } catch (error) {
    console.error('Failed to load images:', error);
  }
}
```

---

# Questions 1

<div class="p-6">
  <p class="text-lg font-semibold mb-4">1. What is the default nature of JavaScript?</p>
  <ul class="list-disc pl-6 mb-4">
    <li v-click="1">A) Asynchronous</li>
    <li v-click="2">B) Multi-threaded</li>
    <li v-click="3">C) Synchronous</li>
    <li v-click="4">D) Event-driven</li>
  </ul>
</div>

---

# Answer of this 1st question is

<v-click>
<div class="p-6">

  <p class="text-lg font-semibold mb-4"> C) Synchronous </p>

  <img width="300"  src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExeHQxM21meHc0NmpvY2E1c3Y3d3NjMmNrdDNsaTdycTI3YTJueGQ2ZyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/6nuiJjOOQBBn2/giphy.gif">
</div>
</v-click>
---

# Questions 2

<div class="p-6">
  <p class="text-lg font-semibold mb-4">2. Which of the following represents "Callback Hell"?</p>
  <ul class="list-disc pl-6 mb-4">
    <li v-click="1">A) Functions being reused in multiple modules</li>
    <li v-click="2">B) Nesting of multiple callback functions</li>
    <li v-click="3">C) A function throwing an unhandled exception</li>
    <li v-click="4">D) Writing functions that are too large</li>
  </ul>
</div>

---

# Answer of this 2nd question is

<v-click>
<div class="p-6">

  <p class="text-lg font-semibold mb-4"> B) Nesting of multiple callback functions </p>

  <img width="300"  src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExeHQxM21meHc0NmpvY2E1c3Y3d3NjMmNrdDNsaTdycTI3YTJueGQ2ZyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/6nuiJjOOQBBn2/giphy.gif">
</div>
</v-click>

---

# Questions 3

<div class="p-6">
  <p class="text-lg font-semibold mb-4">3. Which method is used to attach a handler for a fulfilled Promise?</p>
  <ul class="list-disc pl-6 mb-4">
    <li v-click="1">A) .then()</li>
    <li v-click="2">B) .catch()</li>
    <li v-click="3">C) .all()</li>
    <li v-click="4">D) .finally()</li>
  </ul>
</div>

---

# Answer of this 3rd question is

<v-click>
<div class="p-6">

  <p class="text-lg font-semibold mb-4"> B) .then()</p>

  <img width="300"  src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExeHQxM21meHc0NmpvY2E1c3Y3d3NjMmNrdDNsaTdycTI3YTJueGQ2ZyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/6nuiJjOOQBBn2/giphy.gif">
</div>
</v-click>

---

# Questions 4

<div class="p-6">
  <p class="text-lg font-semibold mb-4">4. How do you handle errors when using async/await?</p>
  <ul class="list-disc pl-6 mb-4">
  <li v-click="1">A) Using .then() </li>
  <li v-click="2">B) Wrapping in a try...catch block</li>
  <li v-click="3">C) Using finally()</li>
  <li v-click="4">D) By writing a synchronous fallback</li>
  </ul>
</div>

---

# Answer of this 4rth question is

<v-click>
<div class="p-6">

  <p class="text-lg font-semibold mb-4"> B) Wrapping in a `try...catch` block </p>

  <img width="300"  src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExeHQxM21meHc0NmpvY2E1c3Y3d3NjMmNrdDNsaTdycTI3YTJueGQ2ZyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/6nuiJjOOQBBn2/giphy.gif">
</div>
</v-click>

---

# Questions 5

<div class="p-6">
  <p class="text-lg font-semibold mb-4">5. Which JavaScript method allows you to execute multiple API calls concurrently and ensures that all responses are resolved before processing the results?</p>
  <ul class="list-disc pl-6 mb-4">
  <li v-click="1">A) Promise.all() </li>
  <li v-click="2">B) Promise.allSettled()</li>
  <li v-click="3">C) Promise.race()</li>
  <li v-click="4">D) Promise.any()</li>
  </ul>
</div>

---

# Answer of this 5th question is

<v-click>
<div class="p-6">

  <p class="text-lg font-semibold mb-4"> A) Promis.all() </p>

  <img width="300"  src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExeHQxM21meHc0NmpvY2E1c3Y3d3NjMmNrdDNsaTdycTI3YTJueGQ2ZyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/6nuiJjOOQBBn2/giphy.gif">
</div>
</v-click>


---
layout: center
class: text-center
---

# Questions?

[MDN Async JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous) · [JavaScript.info Promises](https://javascript.info/promise-basics)

