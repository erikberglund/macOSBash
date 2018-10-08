# macOSBash: Users 

The following commands are used to interact with local users on macOS.

### Index

* [Console User](https://github.com/erikberglund/macOSBash/blob/master/macOSBash_users.md#console-user)
* [UID Highest](https://github.com/erikberglund/macOSBash/blob/master/macOSBash_users.md#uid-highest)


## Console User

How to return the currently logged in console user shortname.

> There are multiple methods of getting the currently logged in console user.  
> I have provided four common ways below, where I recommend you use the first method.

Apple has a recommendation here: [Technical Q&A QA1133: Determining console user login status](https://developer.apple.com/library/content/qa/qa1133/_index.html)

I did a blog post about getting this information in bash: [Script Tip: Get the currently logged in user, in Bash](https://erikberglund.github.io/2018/Get-the-currently-logged-in-user,-in-Bash/)

---
###### Use the binary `scutil` to query the SystemConfiguration framework.

```bash
scutil <<< "show State:/Users/ConsoleUser" | awk -F': ' '/[[:space:]]+Name[[:space:]]:/ { if ( $2 != "loginwindow" ) { print $2 }}'
```
---
###### Use the function `SCDynamicStoreCopyConsoleUser` from the SystemConfiguration framework via Python.  

```python
/usr/bin/python -c 'from SystemConfiguration import SCDynamicStoreCopyConsoleUser; import sys; username = (SCDynamicStoreCopyConsoleUser(None, None, None) or [None])[0]; username = [username,""][username in [u"loginwindow", None, u""]]; sys.stdout.write(username + "\n");'
```
---
###### Use the binary `ioreg` to read the IORegistry which holds the same data as the first two examples are querying.
 
```bash
ioreg -n Root -d1 -a | xpath '/plist/dict/key[.="IOConsoleUsers"]/following-sibling::array/dict/key[.="kCGSSessionOnConsoleKey"]/following-sibling::*[1][name()="true"]/../key[.="kCGSSessionUserNameKey"]/following-sibling::string[1]/text()' 2>/dev/null
```
---
###### Read the owner of the special file at `/dev/console`. 
 
```bash
stat -f "%Su" /dev/console
```
---
Example Output:

```console
erikberglund
```

## UID Highest

How to return the highest used UID in the local user database.

---

```bash
dscl /Local/Default -list /Users UniqueID | awk '{ if ( uid < $2 ) uid=$2 } END { print uid }'
```
---
Example Output:

```console
505
```
