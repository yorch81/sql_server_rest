# SQL Server REST #

## Description ##
Consuming REST Web Services with SQL Server 2019 in Windows Server 2019.

## Step by step ##
In first step install Server Server 2019.

#### Configure Ole Automation Procedures ####
~~~
sp_configure 'show advanced options', 1;
GO
RECONFIGURE;
GO
sp_configure 'Ole Automation Procedures', 1;
GO
RECONFIGURE;
GO
~~~

#### Test script ####
~~~
DECLARE @Object AS INT;
DECLARE @ResponseText AS VARCHAR(8000);

-- Send JSON data
DECLARE @Body AS VARCHAR(8000) = '{"param":"value"}';  

-- Create object
EXEC sp_OACreate 'MSXML2.XMLHTTP', @Object OUT;

-- Send POST
EXEC sp_OAMethod @Object, 'open', NULL, 'post','https://httpbin.org/post', 'false'

-- Add header
EXEC sp_OAMethod @Object, 'setRequestHeader', null, 'Content-Type', 'application/json'

-- Send body
EXEC sp_OAMethod @Object, 'send', null, @body

-- Gets response
EXEC sp_OAMethod @Object, 'responseText', @ResponseText OUTPUT

-- Release object
EXEC sp_OADestroy @Object

-- Print or Select response text
PRINT @ResponseText
SELECT @ResponseText AS RESPONSE
~~~

#### Response example ####
~~~
{
  "args": {}, 
  "data": "{\"param\":\"value\"}", 
  "files": {}, 
  "form": {}, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Cache-Control": "no-cache", 
    "Content-Length": "17", 
    "Content-Type": "application/json", 
    "Host": "httpbin.org", 
    "Ua-Cpu": "AMD64", 
    "User-Agent": "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.2; Win64; x64; Trident/7.0; .NET4.0C; .NET4.0E)", 
    "X-Amzn-Trace-Id": "Root=1-6494aeef-7669ab9612016e4257779492"
  }, 
  "json": {
    "param": "value"
  }, 
  "origin": "20.84.70.103", 
  "url": "https://httpbin.org/post"
}
~~~

## Notes ##
Tested on Windows Server 2019 with SQL Server 2019 Standard.

P.D. Let's go play !!!
