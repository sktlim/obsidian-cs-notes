> Extremely simple mail access protocol

## Properties:
1. **Email Download**:
   - Emails are downloaded to the user's device, making them accessible offline.
   - By default, the emails are deleted from the server after download (though modern configurations may keep them).
   
1. **Simple Protocol**:
   - POP3 is designed to be straightforward and efficient, making it lightweight compared to other protocols like IMAP.

2. **Single Device Use**:
   - POP3 is best for single-device setups, as the email is stored locally and not synchronized across multiple devices.

3. **Ports**:
   - Default port: **110** (non-encrypted).
   - Secure (SSL/TLS): **995**.
## Operation
- Begins when user agent opens a TCP connection on port 110
- POP3 progresses through 3 phases:
	- **Authorization**
		- User agent sends username password to authenticate
	- **Transaction**
		- User agent retrieves messages
		- User agent can also mark emails for deletion, remove deletion marks, and obtain mail statistics
	- **Update**
		- Occurs after user agent sends the `quit` command
		- Mail server deletes emails marked for deletion

### Advantages
1. **Offline Access**
2. **Simple and Lightweight**
3. **Efficient for Limited Server Storage**: Emails are removed from the server after download

### Disadvantages
1. **No Synchronization**: Emails are not synchronized across multiple devices
2. **Limited Features**: Lacks advanced features like folder organization and flagging, which are available in protocols like IMAP.
3. **Risk of Data Loss**: Since emails are downloaded and often removed from the server, losing the local device could result in losing emails.

## Comparison with IMAP:

| **Feature**         | **POP3**                                                  | **IMAP**                                                   |
| ------------------- | --------------------------------------------------------- | ---------------------------------------------------------- |
| **Email Storage**   | Stored locally after download.                            | Stored on the server and accessible from multiple devices. |
| **Synchronization** | No synchronization across devices.                        | Fully synchronized across devices.                         |
| **Access**          | Offline access after download.                            | Requires internet for most operations.                     |
| **Use Case**        | Best for single-device setups and limited server storage. | Ideal for multi-device setups and better server storage.   |

## POP3 Commands:
Here are common commands used in a POP3 session:

| **Command**  | **Description**                                                     | **Example**              |
| ------------ | ------------------------------------------------------------------- | ------------------------ |
| `USER`       | Specifies the username for authentication.                          | `USER alice@example.com` |
| `PASS`       | Specifies the password for authentication.                          | `PASS password123`       |
| `STAT`       | Returns the number of messages and their total size in the mailbox. | `STAT`                   |
| `LIST`       | Lists all messages in the mailbox along with their sizes.           | `LIST`                   |
| `RETR <msg>` | Retrieves the specified message.                                    | `RETR 1`                 |
| `DELE <msg>` | Marks the specified message for deletion from the server.           | `DELE 1`                 |
| `QUIT`       | Ends the session and deletes any messages marked for deletion.      | `QUIT`                   |

## Example POP3 Session:

1. **Client connects**:
   ```
   +OK POP3 server ready
   USER alice@example.com
   +OK
   PASS password123
   +OK Logged in
   ```

2. **Retrieve mailbox status**:
   ```
   STAT
   +OK 3 1200
   ```

3. **List messages**:
   ```
   LIST
   +OK
   1 400
   2 400
   3 400
   ```

4. **Retrieve a message**:
   ```
   RETR 1
   +OK 400 octets
   <email content>
   ```

5. **Mark a message for deletion**:
   ```
   DELE 1
   +OK message 1 deleted
   ```

6. **Quit session**:
   ```
   QUIT
   +OK Bye
   ```
