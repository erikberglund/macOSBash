# macOSBash: Users 

The following commands are used to interact with local users on macOS.

### Index

* [LoggedIn](https://github.com/erikberglund/macOSVariables/blob/master/macos_users.md#loggedin)

## LoggedIn

Returns the currently logged in console user shortname.

> There are multiple methods of getting the currently logged in user.  
> I have provided three common ways below, where I recommend you use either 1 or 2.

Apple put up a &&A article about getting the currently logged in user here: [Technical Q&A QA1133: Determining console user login status](https://developer.apple.com/library/content/qa/qa1133/_index.html)

I did a blog post about getting this information in bash: [Script Tip: Get the currently logged in user, in Bash](https://erikberglund.github.io/2018/Get-the-currently-logged-in-user,-in-Bash/)

---
##### 1
This command uses the binary `scutil` to query the SystemConfiguration framework to get the currently logged in user.

```bash
loggedInUser=$( scutil <<< "show State:/Users/ConsoleUser" | awk -F': ' '/[[:space:]]+Name[[:space:]]:/ { if ( $2 != "loginwindow" ) { print $2 }}' )
```

###### 2
This command uses the SystemConfiguration framework through Python to get the currently logged in user.  

```python
/usr/bin/python -c 'from SystemConfiguration import SCDynamicStoreCopyConsoleUser; import sys; username = (SCDynamicStoreCopyConsoleUser(None, None, None) or [None])[0]; username = [username,""][username in [u"loginwindow", None, u""]]; sys.stdout.write(username + "\n");'
```
---
###### 3  
This command is reading from the same data as the first two examples by asking the IORegistry natively from bash:
 
```bash
ioreg -n Root -d1 -a | xpath '/plist/dict/key[.="IOConsoleUsers"]/following-sibling::array/dict/key[.="kCGSSessionOnConsoleKey"]/following-sibling::*[1][name()="true"]/../key[.="kCGSSessionUserNameKey"]/following-sibling::string[1]/text()' 2>/dev/null
```
---
###### 4  
This command is reading the owner of the special file at /dev/console. 
 
```bash
stat -f "%Su" /dev/console
```
---
Example Output:

```console
erikberglund
```
