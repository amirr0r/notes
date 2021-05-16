# SMTP

**SMTP** stands for Simple Mail Transfer Protocol.

It handles the sending of email.

> Usually it uses the port 25. 

In order to retrieve incoming mail, another protocol is required: **POP**/**IMAP**.

**POP** stands for Post Office Protocol and **IMAP** stands for Internet Message Access Protocol. 

![](img/smtp-message-flow.png)

Two commands can be used for enumeration: 
1. `VRFY` to confirm the name of a valid user
2. `EXPN` to reveal the actual address of user's aliases and lists of e-mail (mailing lists)

Tools:
- Metasploit &rarr; `smtp_enum` and `smtp_version`
- `smtp-user-enum`

## Useful links / Resources

- [How E-mail Works](https://computer.howstuffworks.com/e-mail-messaging/email3.htm)

- [SMTP protocol Explained (How Email works?)](https://www.afternerd.com/blog/smtp/)