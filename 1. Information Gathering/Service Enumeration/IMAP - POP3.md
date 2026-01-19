## IMAP
##### Port 143 / 993
#### Commands
| `1 LOGIN username password`     | User's login.                                                                                                 |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `1 LIST "" *`                   | Lists all directories.                                                                                        |
| `1 CREATE "INBOX"`              | Creates a mailbox with a specified name.                                                                      |
| `1 DELETE "INBOX"`              | Deletes a mailbox.                                                                                            |
| `1 RENAME "ToRead" "Important"` | Renames a mailbox.                                                                                            |
| `1 LSUB "" *`                   | Returns a subset of names from the set of names that the User has declared as being `active` or `subscribed`. |
| `1 SELECT INBOX`                | Selects a mailbox so that messages in the mailbox can be accessed.                                            |
| `1 UNSELECT INBOX`              | Exits the selected mailbox.                                                                                   |
| `1 FETCH <ID> all`              | Retrieves data associated with a message in the mailbox.                                                      |
| `1 CLOSE`                       | Removes all messages with the `Deleted` flag set.                                                             |
| `1 LOGOUT`                      | Closes the connection with the IMAP server.                                                                   |

--- 

## POP3

##### Port 110 / 995

#### Commands
| `1 LOGIN username password`     | User's login.                                                                                                 |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `1 LIST "" *`                   | Lists all directories.                                                                                        |
| `1 CREATE "INBOX"`              | Creates a mailbox with a specified name.                                                                      |
| `1 DELETE "INBOX"`              | Deletes a mailbox.                                                                                            |
| `1 RENAME "ToRead" "Important"` | Renames a mailbox.                                                                                            |
| `1 LSUB "" *`                   | Returns a subset of names from the set of names that the User has declared as being `active` or `subscribed`. |
| `1 SELECT INBOX`                | Selects a mailbox so that messages in the mailbox can be accessed.                                            |
| `1 UNSELECT INBOX`              | Exits the selected mailbox.                                                                                   |
| `1 FETCH <ID> all`              | Retrieves data associated with a message in the mailbox.                                                      |
| `1 CLOSE`                       | Removes all messages with the `Deleted` flag set.                                                             |
| `1 LOGOUT`                      | Closes the connection with the IMAP server.                                                                   |

---
#### cURL
```bash
curl -k 'imaps://10.129.14.128' --user user:p4ssw0rd
curl -k 'pop3s://10.129.14.128' --user user:p4ssw0rd
```

---

#### Interact with IMAP-POP3
##### OpenSSL for IMAP
```bash
openssl s_client -connect 10.129.14.128:imaps

? LOGIN robin robin
? LIST "" *
? FETCH 1 ALL
? FETCH 1 BODY[TEXT]
```

##### OpenSSL for POP3
```bash
openssl s_client -connect 10.129.14.128:pop3s
```