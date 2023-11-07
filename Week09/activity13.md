# Activity 13: Express Routing

## Setup:
1. `activity13` directory
2. open `activity13` in VSCode
3. open the terminal and run the following commands: (make sure you are in `activity13`)
```
npm init
npm install express
```
4. Add a new file named `app.js` with the following contents:
```
const express = require('express')
const app = express()
const port = 3000

const users = {
  "1": "Saul Goodman",
  "2": "Kim Wexler",
  "3": "Gustavo Fring",
  "4": "Ignacio Varga",
  "5": "Michael Ehrmentraut"
}

app.get('/user/:id?', (req, res) => {
  res.send(users[req.params.id])
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```