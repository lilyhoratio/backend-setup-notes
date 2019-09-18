intro-node-js
https://frontendmasters.com/courses/node-js/debugging-testing-exercise/

```javascript
const users = new Array(20).fill(0)
  .map((\_, i) => {
    return {
      id: i,
      createdAt: Date.now() + i,
      email: `readycoder${i}@gmail.com`
    }
})
```
