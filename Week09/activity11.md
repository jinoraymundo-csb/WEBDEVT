# Activity 11: First Steps with ExpressJS

## Setup:
1. `activity11` directory
2. open `activity11` in VSCode
3. open the terminal and run the following commands: (make sure you are in `activity11`)
```
npm init
npm install express
```
4. Add a new file named `app.js` with the following contents:
```
const express = require('express')
const app = express()
const port = 3000

app.use(express.json())

app.get('/', (req, res) => {
  res.send(`Hello, ${req.body.name} ${req.body.surname}`)
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```