# Exercise 05: Forms in ExpressJS

Using the same setup in Activity 14 (`npx express-generator`), create two new pages:
1. registration (`/registration`)
   * first name
   * last name
   * email address
   * ID Number
   * College (must be a dropdown to select SMIT/SDA/etc.)
   * Course (must be a dropdown to select Information Systems/Cybersecurity/etc)
   * Submit button
2. feedback (`/feedback`) must also be restricted to logged-in users
   * message title 
   * message (textarea)
   * Submit button

Registration form should be accessible when visiting e.g. http://localhost:3000/register
Feedback form should be accessible when visiting e.g. http://localhost:3000/feedback