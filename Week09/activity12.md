# Activity 12: URL Encoded form submissions

## Setup:
1. `activity12` directory
2. open `activity12` in VSCode
3. open the terminal and run the following commands: (make sure you are in `activity12`)
```
npm init
npm install express
```
4. Add a new file named `app.js` with the following contents:
```
const express = require('express')
const app = express()
const port = 3000

app.use(express.urlencoded({ extended: true }))

app.get('/', (req, res) => {
  res.json(req.body)
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```