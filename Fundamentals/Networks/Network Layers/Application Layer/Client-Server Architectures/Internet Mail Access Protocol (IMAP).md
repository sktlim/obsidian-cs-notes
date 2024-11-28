> **Interactive Mail Access Protocol (IMAP)**

## Overview
- IMAP is a mail access protocol defined in **RFC 3501**, offering more features than POP3.
- Emails are organized into folders, with new messages initially placed in the **INBOX**.
- Users can create folders, move messages, and delete or manage emails directly on the server.
- IMAP supports searching for messages across folders using specific criteria.
- Unlike POP3, IMAP maintains **user state across sessions**, including folder structures and message associations.
- IMAP allows fetching specific components of a message (e.g., headers or parts of a MIME message).
## Properties:
1. **Server-Side Storage**:
   - Emails are stored on the server, allowing access from multiple devices.
   - Any changes (read/unread status, folders, etc.) are reflected across all devices.

2. **Synchronization**:
   - IMAP synchronizes email status and folders in real time between the client and the server.

3. **Partial Downloading**:
   - Allows downloading only headers or specific parts of the email, which is useful for slow connections.

4. **Advanced Features**:
   - Supports folder organization, searching, and selective fetching of emails.

5. **Ports**:
   - Default port: **143** (non-encrypted).
   - Secure (SSL/TLS): **993**.

---

## Operation:
- Begins when the client opens a TCP connection on port 143 or 993 (for SSL).
- IMAP progresses through **4 phases**:
  - **Connection**:
    - Client establishes a connection with the server and initiates communication.
  - **Authentication**:
    - Client logs in using a username and password for authentication.
  - **Transaction**:
    - The client retrieves messages, modifies flags (e.g., read/unread), manages folders, or fetches message headers.
  - **Logout**:
    - Client sends the `LOGOUT` command, terminating the session.

---

### Advantages:
1. **Multi-Device Synchronization**:
   - Keeps emails consistent across all devices (e.g., phone, laptop, webmail).
2. **Server-Side Storage**:
   - Saves space on the client device and ensures emails remain accessible even if the device is lost.
3. **Advanced Organization**:
   - Supports folders, searching, and message flags like "important" or "read."

### Disadvantages:
1. **Requires Constant Internet**:
   - Accessing emails in real time often requires a stable internet connection.
2. **Higher Server Storage Needs**:
   - Emails are retained on the server, which can lead to storage costs or limitations.
3. **More Complex**:
   - IMAP is more resource-intensive and harder to implement compared to POP3.

---

## IMAP Commands:
Here are common commands used in an IMAP session:

| **Command**     | **Description**                                                           | **Example**              |
| ---------------- | ------------------------------------------------------------------------- | ------------------------ |
| `LOGIN`         | Logs in the user with their username and password.                        | `LOGIN alice password123`|
| `LIST`          | Lists all mailboxes (folders) on the server.                              | `LIST "" *`              |
| `SELECT <mailbox>`| Opens a specific mailbox (e.g., Inbox) for operations.                  | `SELECT INBOX`           |
| `FETCH <msg>`   | Retrieves a specific message or its parts (e.g., headers only).           | `FETCH 1 BODY[HEADER]`   |
| `STORE <msg>`   | Modifies message flags (e.g., mark as read or delete).                    | `STORE 1 +FLAGS (\Seen)` |
| `SEARCH`        | Searches for emails meeting specified criteria (e.g., by sender or date). | `SEARCH FROM "bob@example.com"` |
| `EXPUNGE`       | Permanently removes messages marked for deletion from the mailbox.        | `EXPUNGE`                |
| `LOGOUT`        | Ends the session and closes the connection.                               | `LOGOUT`                 |

---

## Example IMAP Session:

1. **Client connects**:
   ```
   * OK IMAP server ready
   A001 LOGIN alice@example.com password123
   A001 OK Logged in
   ```

2. **List mailboxes**:
   ```
   A002 LIST "" *
   * LIST (\HasNoChildren) "." INBOX
   A002 OK LIST completed
   ```

3. **Select mailbox**:
   ```
   A003 SELECT INBOX
   * 3 EXISTS
   * 0 RECENT
   A003 OK [READ-WRITE] SELECT completed
   ```

4. **Fetch email headers**:
   ```
   A004 FETCH 1 BODY[HEADER]
   * 1 FETCH (BODY[HEADER] {120}
   From: Bob <bob@example.com>
   Subject: Meeting Update
   )
   A004 OK FETCH completed
   ```

5. **Mark email as read**:
   ```
   A005 STORE 1 +FLAGS (\Seen)
   * 1 FETCH (FLAGS (\Seen))
   A005 OK STORE completed
   ```

6. **Log out**:
   ```
   A006 LOGOUT
   * BYE IMAP server logging out
   A006 OK LOGOUT completed
   ```
