# Exercise 06: ExpressJS API (CRUD) with `sequelize` and `sqlite`

## Setup:
1. `exercise06` directory
2. open `exercise06` in VSCode
3. open the terminal and run the following commands: (make sure you are in `exercise06`)
```
cd ..
npx express-generator --view=ejs --git exercise06
cd exercise06
npm i sequelize sqlite3
npm i --save-dev nodemon
```

4. Open up `package.json` and add a `dev` script:

```
...
"scripts": {
  "start": "node ./bin/www",
  "dev": "nodemon ./bin/www"  // <-- add this line
},
...
```

5. Add the following directory names:
* database
* models
* controllers

6. Add a file named `index.js` inside the "models" directory
```
const Sequelize = require('sequelize');
const sequelize = new Sequelize({
  dialect: 'sqlite',
  storage: 'db/school.db'
});

const db = {};
db.Sequelize = Sequelize;
db.sequelize = sequelize;

db.students = require('./student.model.js')(sequelize, Sequelize);
// db.organizations = require('./organization.model.js')(sequelize, Sequelize);

module.exports = db;
```

7. Let's add the first model `Student` in `models/student.model.js`

```
module.exports = (sequelize, Sequelize) => {
  const Student = sequelize.define('student', {
    firstName: {
      type: Sequelize.STRING
    },
    lastName: {
      type: Sequelize.STRING
    },
    idNumber: {
      type: Sequelize.STRING,
      unique: true,
      validate: {
        isInt: true
      }
    },
    emailAddress: {
      type: Sequelize.STRING,
      unique: true,
      validate: {
        isEmail: true
      }
    }
  });

  return Student;
};
```

this adheres to the following schema:

> Student

| column name | data type | notes |
| --- | --- | --- |
| firstName | string | | 
| lastName | string | | 
| idNumber | string | must be unique; must be int-like | 
| emailAddress | string | must be unique; must be a valid email | 

8. Let's add the first controller in `controllers/student.controller.js`

```
const db = require('../models');
const Student = db.students;
const Op = db.Sequelize.Op;
```

> Create

```
exports.create = (req, res) => {
  const student = {
    firstName: req.body.firstName,
    lastName: req.body.lastName,
    idNumber: req.body.idNumber,
    emailAddress: req.body.emailAddress
  };

  Student.create(student)
    .then((data) => {
      res.send(data);
    })
    .catch((err) => {
      res.status(500).send({
        message: err.message || "Some error occured"
      });
    });
};
```

> Find All

```
exports.findAll = (req, res) => {
  const lastName = req.query.lastName;
  var condition = lastName ? { lastName: { [Op.like]: `%${lastName}%` } } : null;

  Student.findAll({ where: condition })
    .then((data) => {
      res.send(data);
    })
    .catch((err) => {
      res.status(500).send({
        message:
          err.message || 'Some error occured while retrieving students'
      });
    });
};
```

> Find One

```
exports.findOne = (req, res) => {
  const id = req.params.id;

  Student.findByPk(id)
    .then((data) => {
      if (data) {
        res.send(data);
      } else {
        res.status(404).send({
          message: `Cannot find Student with id=${id}.`
        });
      }
    })
    .catch((err) => {
      res.status(500).send({
        message: `Error retrieving Stuend with id=${id}`
      })
    })
}
```

> Update

```
exports.update = (req, res) => {
  const id = req.params.id;

  Student.update(req.body, { where: { id: id }})
    .then((numRecords) => {
      if (numRecords == 1) {
        res.send({
          message: 'Student updated successfully!'
        });
      } else {
        res.send({
          message: `Cannot update Student with id=${id}`
        });
      }
    })
    .catch((err) => {
      res.status(500).send({
        message: `Error updating Student with id=${id}`
      })
    })
}
```

> Delete One

```
exports.delete = (req, res) => {
  const id = req.params.id;

  Student.destroy({
    where: { id: id }
  })
    .then((numRecords) => {
      if (numRecords == 1) {
        res.send({
          message: 'Student was deleted successfully!'
        })
      } else {
        res.send({
          message: `Cannot delete Student with id=${id}`
        })
      }
    })
    .catch((err) => {
      res.status(500).send({
        message: `Could not delete Student with id=${id}`
      })
    })
}
```

> Delete All (DANGEROUS!)

```
exports.deleteAll = (req, res) => {
  Student.destroy({
    where: {},
    truncate: false
  })
    .then((numRecords) => {
      res.send({
        message: `${numRecords} Student(s) were deleted successfully!`
      })
    })
    .catch((err) => {
      res.status(500).send({
        message: err.message || 'Some error occured while removing all students.'
      });
    });
}
```

9. Let's add the route for students via `routes/students.js`

```
var express = require('express');
var router = express.Router();
var students = require('../controllers/student.controller.js');

router.post('/', students.create);
router.get('/', students.findAll);
router.get('/:id', students.findOne);
router.put('/:id', students.update);
router.delete('/:id', students.delete);
router.delete('/', students.deleteAll);

module.exports = router;
```

10. Modify `app.js` (routes)
* remove the definitions for `indexRouter` and `usersRouter`
* add the following router definition:
```
var studentsRouter = require('./routes/students');
```
* replace the following:
```
app.use('/', indexRouter);
app.use('/users', usersRouter);
```
with:
```
app.use('/students', studentsRouter);
```

1.  Modify `app.js` (db initialization)
* add the following right before `module.exports = app`:
```
const db = require('./models');
db.sequelize.sync()
  .then(() => {
    console.log("Synced db");
  })
  .catch((err) => {
    console.error(`Failed to sync db: ${err.message}`);
  });
```

1.  Cleanup
* delete the files `index.js` and `users.js` in "`routes`"


13. Exercise 06 Requirements:

> Organization

| column name | data type | notes |
| --- | --- | --- |
| name | string | must be unique | 
| aka | string | |
| description | string | allow nulls | 

* similar to the `students` API, write the code for the `organizations` API:
  * router and routes
  * model
  * controller

** zip the entire `exercise06` folder for submission