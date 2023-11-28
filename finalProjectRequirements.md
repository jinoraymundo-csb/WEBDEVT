# Final Project Requirements

## Pages
1. Landing Page (Home)
2. Registration
3. Sign-In

## Schemas

> Student

| column name | data type | notes |
| --- | --- | --- |
| idNumber | string | must be unique; must be int-like | 
| password | string | |
| firstName | string | | 
| lastName | string | | 
| emailAddress | string | must be unique; must be a valid email | 
| course | string | must be one of: BIA, CA, IS, IEMC, BM, EGBM, HRM, MM, CSEC, REM, SIE | 

> Organization

| column name | data type | notes |
| --- | --- | --- |
| name | string | must be unique | 
| aka | string | |
| description | string | allow nulls | 

--- 

## Functions

1. Landing Page (Home)
  a. must be able to view a list of organizations
  b. must be able to view a list of registered students (minus the password)

2. Registration
  a. must be able to view registration page
  b. students can fill-up and submit the registration form
  c. a new student must be inserted unto the db

3. Sign-In
  a. must be able to view the sign-in page
  b. students can fill-up and submit the sign-in form
  c. must show in the console if the login is successful or not