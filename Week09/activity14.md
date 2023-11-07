# Activity 14: Simple Login

## Setup:
1. Create a `activity14` directory
2. Open this directory in VSCode
3. open up a terminal (powershell) then execute the following commands:
```
cd ..
npx express-generator --view=pug --git activity14
cd activity14
npm i express-session
npm i pbkdf2-password
npm i
```
4. add a new pug layout file named `login.pug` inside the `views` directory
```
extends layout

block content
  form(method="post")
    legend Login
    p
      label(for="username") Username:
        input(type="text", name="username")
    p
      label(for="password") Password:
        input(type="password", name="password")
    p.buttons
      input(type="submit", value="Save")
```
5. additional config for sessions in `app.js` (after `app.use(express.static...)`):
```
app.use(session({
  resave: false,
  saveUninitialized: false,
  secret: 'supersecret'
}));

app.use((req, res, next) => {
  var err = req.session.error;
  var msg = req.session.success;
  delete req.session.error;
  delete req.session.success;
  res.locals.message = '';
  if (err) res.locals.message = '<p class="msg error">' + err + '</p>';
  if (msg) res.locals.message = '<p class="msg success">' + msg + '</p>';
  next();
});
```
6. additional module import `routes/index.js`:
```
var hash = require('pbkdf2-password')();
```

7. dummy "users" database in `routes/index.js`:
```
var users = {
  juandelacruz: { name: 'juan de la cruz' }
};

hash({ password: 'kalayaan' }, (err, pass, salt, hash) => {
  if (err) throw err;
  users.juandelacruz.salt = salt;
  users.juandelacruz.hash = hash;
});
```

8. utility methods in `routes/index.js`:
```
function restrict(req, res, next) {
  if (req.session.user) {
    next();
  } else {
    req.session.error = 'Access denied!';
    res.redirect('/login');
  }
}

function authenticate(username, password, fn) {
  if (!module.parent) console.log(`authenticating ${username}:${password}`);
  var user = users[username];

  if (!user) return fn(null, null);

  hash({ password: password, salt: user.salt }, (err, pass, salt, hash) => {
    if (err) return fn(err);
    if (hash === user.hash) return fn(null, user);
    fn(null, null);
  });
}
```

9. additional router methods in `routes/index.js`:
```
router.get('/restricted', restrict, (req, res) => {
  res.send('Accessing a restricted page, click to <a href="/logout">logout</a>');
})

router.get('/login', (req, res, next) => {
  res.render('login');
});

router.get('/logout', (req, res, next) => {
  req.session.destroy(() => {
    res.redirect('/');
  });
});

router.post('/login', (req, res, next) => {
  authenticate(req.body.username, req.body.password, (err, user) => {
    if (err) return next(err);

    if (user) {
      req.session.regenerate(() => {
        req.session.user = user;
        req.session.success = `Authenticated as ${user.username}`;
        res.redirect('/restricted);
      });
    } else {
      req.session.error = 'Authentication failed';
      res.redirect('/login');
    }
  });
});
```
