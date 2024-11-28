> **Simple Mail Transfer Protocol**: Principal app-layer protocol between mail servers, of which users have a mailbox located in

- Uses reliable data transfer service of TCP
- If mail server A cannot deliver to mail server B, server A holds email in message queue and attempts to transfer the message later

## Properties
- Restricts body of all mail messages to 7-bit ASCII
	- Requires binary multimedia data to be encoded into ASCII 
- Uses persistent connections
- **Push Protocol:** Sending mail server pushes file onto receiving mail server

## Example Operation
![[SMTP Example.png]]
1) Alice composes a message on her email and instructs the user agent to send the message
2) User agent sends her email over to the mail server, where its placed in the **message queue**
3) Client SMTP running on Alice's mail server see the email in the queue and opens up a TCP connection with Bob's SMTP server on port 25
4) After initial SMTP handshake, client sends Alice's email into the TCP connection
5) Bob's SMTP server receives the message and places message in Bob's mailbox
6) Bob invokes his user agent to read message

**Note:** It is assumed that Bob is logging onto the mail server and using a mail reader to retrieve his email (which was common till the 80s). Nowadays, Bob's personal devices have a client allowing them to access the mail stored in the mail server. As SMTP is a **push protocol**, Bob's client has to use another mail access protocol, such as [[Post-Office Protocol (POP3)]], [[Internet Mail Access Protocol (IMAP)]], or [[HTTP]]

### Example Exchange
```bash
220 mail.example.com ESMTP Postfix
HELO client.example.com
250 mail.example.com
MAIL FROM:<alice@example.com>
250 OK
RCPT TO:<bob@example.com>
250 OK
DATA
354 End data with <CRLF>.<CRLF>
Subject: Test Email
Hi Bob,
This is a test email.
.
250 OK: queued as 12345
QUIT
221 Bye
```

## SMTP Commands

| **Command** | **Purpose**                                                                                   | **Syntax**          | **Example**                     |
| ----------- | --------------------------------------------------------------------------------------------- | ------------------- | ------------------------------- |
| `HELO`      | Identifies the client to the server. Used at the beginning of an SMTP session.                | `HELO <hostname>`   | `HELO client.example.com`       |
| `EHLO`      | Extended version of `HELO`, used for enhanced features (ESMTP).                               | `EHLO <hostname>`   | `EHLO client.example.com`       |
| `MAIL FROM` | Specifies the sender's email address (envelope sender).                                       | `MAIL FROM:<email>` | `MAIL FROM:<alice@example.com>` |
| `RCPT TO`   | Specifies the recipient's email address (envelope recipient).                                 | `RCPT TO:<email>`   | `RCPT TO:<bob@example.com>`     |
| `DATA`      | Signals the start of the message content (headers + body). Ends with `<CRLF>.<CRLF>`.         | `DATA`              | `DATA`                          |
| `RSET`      | Resets the session (aborts current mail transaction but keeps the connection open).           | `RSET`              | `RSET`                          |
| `VRFY`      | Verifies if an email address is valid (optional and often disabled for security reasons).     | `VRFY <email>`      | `VRFY bob@example.com`          |
| `EXPN`      | Expands a mailing list to show all members (optional and often disabled for privacy reasons). | `EXPN <list-name>`  | `EXPN marketing@example.com`    |
| `NOOP`      | Does nothing but checks the connection status.                                                | `NOOP`              | `NOOP`                          |
| `QUIT`      | Ends the SMTP session and closes the connection.                                              | `QUIT`              | `QUIT`                          |
| `AUTH`      | Starts the authentication process for the client.                                             | `AUTH <method>`     | `AUTH LOGIN`                    |
| `HELP`      | Requests a list of supported commands or help for a specific command.                         | `HELP [<command>]`  | `HELP MAIL`                     |

---

## SMTP Response Codes

| **Code** | **Category**             | **Meaning**                                                                                       | **Example Usage**                      |
|----------|--------------------------|---------------------------------------------------------------------------------------------------|----------------------------------------|
| **2xx** | Success                  | Indicates that the requested action was successfully completed.                                  | `250 OK`                               |
| `211`    | System Status            | Provides system status or system help response.                                                  | `211 System ready`                     |
| `214`    | Help Message             | Provides help information.                                                                       | `214 This is the SMTP server help`     |
| `220`    | Service Ready            | Server is ready to accept a new connection.                                                     | `220 mail.example.com ESMTP Postfix`   |
| `221`    | Service Closing          | Server is closing the connection.                                                               | `221 Bye`                              |
| `250`    | Requested Action Okay    | The requested action has been completed.                                                        | `250 OK`                               |
| `251`    | User Not Local           | User not local; forward to another address.                                                     | `251 User not local; relayed`          |
| **3xx** | Intermediate Responses   | Further action is required by the client to complete the request.                               | `354 End data with <CRLF>.<CRLF>`      |
| `354`    | Start Mail Input         | Server is ready to receive the message body. Ends with `<CRLF>.<CRLF>`.                         | `354 Start mail input`                 |
| **4xx** | Temporary Failures       | Temporary error; client can retry later.                                                        | `421 Server temporarily unavailable`   |
| `421`    | Service Not Available    | Service unavailable, often due to server overload.                                              | `421 mail.example.com closing connection` |
| `450`    | Mailbox Unavailable      | Requested mailbox is not available at the moment.                                               | `450 Mailbox unavailable`              |
| `451`    | Aborted Action           | Request action aborted due to server error.                                                     | `451 Requested action aborted`         |
| `452`    | Insufficient Storage     | Server cannot process the request due to insufficient storage.                                  | `452 Insufficient system storage`      |
| **5xx** | Permanent Failures       | Permanent error; client should not retry.                                                       | `550 Mailbox not found`                |
| `500`    | Syntax Error             | Syntax error in command.                                                                        | `500 Syntax error, command unrecognized` |
| `501`    | Syntax Error in Arguments| Syntax error in command arguments.                                                              | `501 Syntax error in parameters`       |
| `502`    | Command Not Implemented  | Command is not implemented by the server.                                                       | `502 Command not implemented`          |
| `503`    | Bad Sequence             | Commands are out of order or inappropriate at this time.                                        | `503 Bad sequence of commands`         |
| `504`    | Command Parameter Not Implemented| Command parameter not supported.                                                           | `504 Command parameter not implemented`|
| `550`    | Requested Action Failed  | Mailbox unavailable, or access is denied.                                                      | `550 User unknown`                     |
| `551`    | User Not Local           | User not local; cannot relay.                                                                  | `551 User not local; relaying denied`  |
| `552`    | Storage Limit Exceeded   | Message size exceeds server limits.                                                            | `552 Message size exceeded`            |
| `553`    | Invalid Email Address    | Email address syntax is invalid.                                                              | `553 Invalid address syntax`           |
| `554`    | Transaction Failed       | General transaction failure, such as policy violations or virus detection.                     | `554 Transaction failed`               |
