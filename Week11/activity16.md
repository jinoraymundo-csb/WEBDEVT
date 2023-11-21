# Activity 16: ExpressJS with Bootstrap and ejs

## Setup:
1. `activity16` directory
2. open `activity16` in VSCode
3. open the terminal and run the following commands: (make sure you are in `activity16`)

```
cd ..
npx express-generator --view=ejs --git activity16
cd activity16
npm i bootstrap
```

4. in `app.js`, add the following after the line `var app = express();`

```
app.use(
  "/css",
  express.static(path.join(__dirname, "node_modules/bootstrap/dist/css"))
)

app.use(
  "/js",
  express.static(path.join(__dirname, "node_modules/bootstrap/dist/js"))
)
```

5. in `views/index.ejs`, remove everything and replace with the following html markup:
```
html
  head
    title{Homepage}
  body
    nav.navbar.navbar-expand-md.navbar-dark.fixed-top.bg-dark
      div.container-fluid
        a.navbar-brand{Fixed navbar}
        button:button.navbar-toggler[data-bs-toggle="collapse" data-bs-target="#navbarCollapse" aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation"]
          span.navbar-toggler-icon
        div.collapse.navbar-collapse#navbarCollapse
          ul.navbar-nav.me-auto.mb-2.mb-md-0
            li.nav-item>(a.nav-link{Link$})*3
          form.d-flex
            input:search.form-control.me-2[placeholder="Search" aria-label="Search"]
            button:submit.btn.btn-outline-success{Search}
    main.container
      div.bg-light.p-5.rounded
        h1{WEBDEVT XTIS1 - Activity 16}
        p.lead{Lorem ipsum dolor sit amet...}
        a.btn.btn-lg.btn-primary[role="button"]{Click here}
```

6. create a new file `views/login.ejs`:
```
html
  head
    title{Homepage}
  body
    nav.navbar.navbar-expand-md.navbar-dark.fixed-top.bg-dark
      div.container-fluid
        a.navbar-brand{Fixed navbar}
        button:button.navbar-toggler[data-bs-toggle="collapse" data-bs-target="#navbarCollapse" aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation"]
          span.navbar-toggler-icon
        div.collapse.navbar-collapse#navbarCollapse
          ul.navbar-nav.me-auto.mb-2.mb-md-0
            li.nav-item>(a.nav-link{Link$})*3
          form.d-flex
            input:search.form-control.me-2[placeholder="Search" aria-label="Search"]
            button:submit.btn.btn-outline-success{Search}
    main.form-signin
      form
        h1.h3.mb-3.fw-normal{Sign-in}
        div.form-floating
          input:email.form-control#emailAddress[placeholder="name@benilde.edu.ph"]
          label[for="emailAddress"]{Email Address}
        div.form-floating
          input:password.form-control#password[placeholder="********"]
          label[for="password"]{Password}
        div.checkbox.mb-3
          label
            input:checkbox[value="remember-me"]
            Remember me
        button:submit.w-100.btn.btn-lg.btn-primary{Sign in}
```

7. create a new file `views/dashboard.ejs`:
```
html
  head
    title{Homepage}
  body
    nav.navbar.navbar-expand-md.navbar-dark.fixed-top.bg-dark
      div.container-fluid
        a.navbar-brand{Fixed navbar}
        button:button.navbar-toggler[data-bs-toggle="collapse" data-bs-target="#navbarCollapse" aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation"]
          span.navbar-toggler-icon
        div.collapse.navbar-collapse#navbarCollapse
          ul.navbar-nav.me-auto.mb-2.mb-md-0
            li.nav-item>(a.nav-link{Link$})*3
          form.d-flex
            input:search.form-control.me-2[placeholder="Search" aria-label="Search"]
            button:submit.btn.btn-outline-success{Search}
    main
      section.py-5.text-center.container
        div.row.py-lg-5
          div.col-lg-6.col-md-8.mx-auto
            h1.fw-light{This is the dashboard page}
            p.lead.text-muted{Lorem ipsum dolor sit amet...}
            p
              a.btn.btn-primary.my-2{Get Started}
              a.btn.btn-secondary.my-2{Logout}
```