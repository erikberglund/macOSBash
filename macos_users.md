# macOS Snippets: Users 

The following snippets are used to interact with local users on macOS.

### Index

* [LoggedIn](https://github.com/erikberglund/Scripts/blob/master/snippets/macos_directoryservices.md#uid-highest)

## LoggedIn

Returns the currently logged in user shortname.

`There are multiple methods of getting the currently logged in user. The mos`

This is the 

**BASH**  
```bash
stat -f "%Su" /dev/console
```

--
This snippet is using the SystemConfiguration framework through Python to get the currently logged in user.  

See this link for more information: [Technical Q&A QA1133: Determining console user login status](https://developer.apple.com/library/content/qa/qa1133/_index.html)

**PYTHON**  
```python
/usr/bin/python -c 'from SystemConfiguration import SCDynamicStoreCopyConsoleUser; import sys; username = (SCDynamicStoreCopyConsoleUser(None, None, None) or [None])[0]; username = [username,""][username in [u"loginwindow", None, u""]]; sys.stdout.write(username + "\n");'
```

--
The following snippet is reading the same data as the python version through IOReg (), but native from bash:

**BASH**  
```bash
ioreg -n Root -d1 -a | xpath '/plist/dict/key[.="IOConsoleUsers"]/following-sibling::array/dict/key[.="kCGSSessionOnConsoleKey"]/following-sibling::*[1][name()="true"]/../key[.="kCGSSessionUserNameKey"]/following-sibling::string[1]/text()' 2>/dev/null
```

Example Output:

```console
erikberglund
```
