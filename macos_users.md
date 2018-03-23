# macOSVariables: Users 

The following commands are used to interact with local users on macOS.

### Index

* [LoggedIn](https://github.com/erikberglund/macOSVariables/blob/master/macos_users.md#loggedin)

## LoggedIn

Returns the currently logged in user shortname.

> There are multiple methods of getting the currently logged in user.  
> I have provided three common ways below, where I recommend you use either 1 or 2.

---
###### 1  
This command uses the SystemConfiguration framework through Python to get the currently logged in user.  

See this link for more information: [Technical Q&A QA1133: Determining console user login status](https://developer.apple.com/library/content/qa/qa1133/_index.html)

```python
/usr/bin/python -c 'from SystemConfiguration import SCDynamicStoreCopyConsoleUser; import sys; username = (SCDynamicStoreCopyConsoleUser(None, None, None) or [None])[0]; username = [username,""][username in [u"loginwindow", None, u""]]; sys.stdout.write(username + "\n");'
```
---
###### 2  
This command is reading from the same data as the python example ([1](https://github.com/erikberglund/macOSVariables/blob/master/macos_users.md#loggedin#1)) by asking the IORegistry natively from bash:
 
```bash
ioreg -n Root -d1 -a | xpath '/plist/dict/key[.="IOConsoleUsers"]/following-sibling::array/dict/key[.="kCGSSessionOnConsoleKey"]/following-sibling::*[1][name()="true"]/../key[.="kCGSSessionUserNameKey"]/following-sibling::string[1]/text()' 2>/dev/null
```
---
###### 3  
This command is reading the owner of the special file at /dev/console. 
 
```bash
stat -f "%Su" /dev/console
```
---
Example Output:

```console
erikberglund
```
