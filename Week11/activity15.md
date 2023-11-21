# Activity 15: ExpressJS CRUD with SQLite

## Setup:
1. `activity15` directory
2. open `activity15` in VSCode
3. open the terminal and run the following commands: (make sure you are in `activity15`)

```
npm init -y
npm i express
npm i sqlite3
npm i --save-dev nodemon
```

> if `npm i sqlite3` does not work:
```
npm i sqlite3@4.2.0
```

4. Add a new file named `app.js` with the following contents:

```
const express = require('express');
const sqlite3 = require('sqlite3').verbose();
const http = require('http');

const app = express();
const server = http.createServer(app);
const db = new sqlite3.Database('./database/student.db');

db.run('CREATE TABLE IF NOT EXISTS student(id INT, first_name TEXT, last_name TEXT)');

app.use(express.json());
```

5. Add a new directory named `database` under `activity15`
6. At the bottom of `app.js`, add the following handlers:
```
app.route('/students/:id')
  .get((req, res) => {

  })
  .post((req, res) => {

  })
  .put((req, res) => {

  })
  .delete((req, res) => {

  });
```

7. The CREATE (post) handler:
```
db.serialize(() => {
  db.run(
    'INSERT INTO student(id, first_name, last_name) VALUES(?, ?, ?)',
    [req.params.id, req.body.first_name, req.body.last_name],
    (err) => {
      if (err) return console.error(err.message);
      console.log('New student has been added.');
      res.send(`New student has been added: ${req.params.id} - ${req.body.first_name} ${req.body.last_name}`);
    }
  )
});
```

8. The READ (get) handler:
```
db.serialize(() => {
  db.get(
    'SELECT id, first_name, last_name FROM student WHERE id = ?',
    [req.params.id],
    (err, row) => {
      if (err) return console.error(`Error encountered while querying db: ${err.message}`);

      if (row) {
        res.json({
          id: row.id,
          first_name: row.first_name,
          last_name: row.last_name
        });  
      } else {
        res.send('Student not found', 404);
      }
    }
  )
});
```

9. The UPDATE (put) handler:
```
db.serialize(() => {
  db.run(
    'UPDATE student SET first_name = ?, last_name = ? WHERE id = ? ',
    [req.body.first_name, req.body.last_name, req.params.id],
    (err) => {
      if (err) return console.error(err.message);
      console.log('Student updated successfully!');
      res.send(`Student ${req.params.id} updated!`);
    }
  );
});
```

10. The DELETE (delete) handler:
```
db.serialize(() => {
  db.run(
    'DELETE FROM student WHERE id = ?',
    [req.params.id],
    (err) => {
      if (err) return console.error(err.message);
      console.log('Student deleted successfully!');
      res.status(204);
    }
  );
});
```

11. The Get All handler:
```
app.get('/students', (req, res) => {
  db.serialize(() => {
    db.all(
      'SELECT id, first_name, last_name FROM student',
      [],
      (err, rows) => {
        if (err) return console.error(err.message);
        if (rows) {
          res.send(rows);
        } else {
          res.status(404).send('No students found!');
        }
      }
    )
  });
});
```

12. at the bottom of `app.js`:
```
server.listen(3000, () => {
  console.log('app server running on port 3000');
})
```

13. add inside `scripts` on `package.json`:
```
"start:dev": "nodemon ./bin/www"
```