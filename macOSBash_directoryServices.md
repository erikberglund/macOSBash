# macOSBash: Directory Services 

The following commands are used to interact with the directory services on macOS.

### Index

* [UID Highest](https://github.com/erikberglund/macOSBash/blob/master/macOSBash_directoryServices.md#uid-highest)

## UID Highest

How to return the highest used UID in the user database.

---

```bash
dscl . -list /Users UniqueID | awk '{ if ( uid < $2 ) uid=$2 } END { print uid }'
```
---
Example Output:

```console
505
```
