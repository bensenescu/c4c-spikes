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

### User Login

### Forgotten Password
