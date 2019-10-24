# Login Flow

## Account Creation
  1. Get email, password, password confirmation, and optionally a username (if we want a username separate from the email) or any other information needed
  2. Send information to the back-end
  3. Hash password with salt and store as hash + salt, salt + hash, or store salt and hash separately (prefer storing salt and hash together)
  4. Store everything given in the database
  5. Send email confirmation using something like Twilio's SendGrid (send Twilio confirmation for phone if necessary)
  6. If multiple steps needed, add number to database representing which step of the signup process they're on and redirect to that page upon login if < max step
    - Confirmation could be one of these steps, so direct user to confirmation code request page in this case
  7. If no further steps, redirect to home page or account page

## Login

### Admin Login
  - When logging in or calling superclass constructor, check if user is associated with certain roles, and if so, add value to session or field in this page's class
  - Could possibly be done in some helper class/function object?

### User Login
  1. Front-end sends username/email, password, and remember me form info to back-end
  2. Back-end get user info by username and compare saved hashed password to given hashed password and salt
  3. If it matches, then in the user's session save a session token and the username (possibly as JWT???)
  4. Save username and password hash to sessions DB table with timestamp

#### On Every Secure Page
  - During the constructor, call a superclass 'webpage' constructor that does the following
    1. Get username and session token from sessions DB table from username stored in session
    2. Compare session token in DB to session token in user's session
    3. If applicable (when using JWTs), determine if signature matches certificate to see if any edits have been made
      - Must use JWT if session data is not encrypted or signed
    4. See if timestamp for sessions table hasn't exceeded max time yet
      - Determine how long someone should be logged in for
    5. Save current user info (at least username) to some field in this page's class
    6. Redirect to login/home page if something in this process fails

### Forgotten Password
  1. Front-end sends username/email/phone? to back-end
  2. Create unique random reset key
  3. Create password reset table in DB that links keys to username/email with a timestamp
  4. Create link with key appended to it that will redirect to a page where that key will allow the user to update the user's password
  5. If timestamp is expired, redirect to home and display 'invalid key' message
  6. Back-end sends password reset link to email/phone using SendGrid/Twilio
