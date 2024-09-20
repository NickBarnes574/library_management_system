# Library Management System Server

## Description

The digital Library Management System will allow authorized users to log in, check available books, borrow books, return books, and view their borrowed books. The server will accept connections from the provided client and process incoming commands in accordance with the command table and line diagrams below. The server will only accept authorized users whose usernames and passwords are in the credentials file. To keep persistent records, the list of available books and borrowed books will be stored in respective files.

## Diagram

```mermaid
flowchart RL
    subgraph Client
        direction LR
        C <--Login Successful--> B{Authenticated}
        C[login <'username'> <'password'> ]
        D[browse]
        E[borrow <'book_id'>]
        F[return <'book_id'>]
        G[query]
    end

    B <--Available Books--> D
    B <--Borrow Success--> E
    B <--Return Success--> F
    B <--Borrowed Books--> G

    subgraph Server
        direction LR
        id1[(Credentials DB)] --> A[Library Management System]
        id2[(Books DB)] --> A[Library Management System]
        id3[(Sessions DB)] --> A[Library Management System]
    end

    Client <--> Server
```

# Commands

## Client Command Table

The client will use a command line interface that handles the following commands in the format below:

| Command         | Description                                   | Client Usage                  |
| --------------- | --------------------------------------------- | ----------------------------- |
| **Log In**      | Authenticates this session to the system.     | `login <username> <password>` |
| **Browse**      | Views the list of available books.            | `check`                       |
| **Borrow Book** | Borrows a book from the library.              | `borrow <book_id>`            |
| **Return Book** | Returns a borrowed book to the library.       | `return <book_id>`            |
| **Query**       | Views the list of books borrowed by the user. | `query`                       |

!!! warning
Requires Authentication

## Client OpCodes

| Command | OpCode |
| ------- | ------ |
| Login   | 0x01   |
| Browse  | 0x02   |
| Borrow  | 0x03   |
| Return  | 0x04   |
| Query   | 0x05   |

## Server RepCodes

| Response Type     | RepCode |
| ----------------- | ------- |
| Login Success     | 0x00    |
| Operation Success | 0x01    |
| Operation Failure | 0xFF    |

# Packet Structures

## Login

<table style="text-align:center;">
    <colgroup>
        <col span="1" style="width:20%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
    </colgroup>
    <thead>
        <tr>
            <td colspan=9>REQUEST</td>
        </tr>
        <tr>
            <th>Byte Location</th>
            <th>0</th>
            <th>1</th>
            <th>2</th>
            <th>3</th>
            <th>4</th>
            <th>5</th>
            <th>6</th>
            <th>7</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>+0</td>
            <td>OPCODE</td>
            <td colspan=3>RESERVED</td>
            <td colspan=2>USERNAME LENGTH</td>
            <td colspan=2>PASSWORD LENGTH</td>
        </tr>
        <tr>
            <td>+8</td>
            <td colspan=4>USERNAME...</td>
            <td colspan=4>PASSWORD...</td>
        </tr>
    </tbody>
</table>

<table style="text-align:center;">
    <colgroup>
        <col span="1" style="width:20%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
    </colgroup>
    <thead>
        <tr>
            <td colspan=9>RESPONSE</td>
        </tr>
        <tr>
            <th>Byte </th>
            <th>0</th>
            <th>1</th>
            <th>2</th>
            <th>3</th>
            <th>4</th>
            <th>5</th>
            <th>6</th>
            <th>7</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>+0</td>
            <td>REPCODE</td>
            <td colspan=3>RESERVED</td>
            <td colspan=4>SESSION ID</td>
        </tr>
        <tr>
            <td>+8</td>
            <td colspan=8></td>
        </tr>
    </tbody>
</table>

## Browse

<table style="text-align:center;">
    <colgroup>
        <col span="1" style="width:20%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
    </colgroup>
    <thead>
        <tr>
            <td colspan=9>REQUEST</td>
        </tr>
        <tr>
            <th>Byte Location</th>
            <th>0</th>
            <th>1</th>
            <th>2</th>
            <th>3</th>
            <th>4</th>
            <th>5</th>
            <th>6</th>
            <th>7</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>+0</td>
            <td>OPCODE</td>
            <td colspan=3>RESERVED</td>
            <td colspan=4>SESSION ID</td>
        </tr>
        <tr>
            <td>+8</td>
            <td colspan=8>EMPTY</td>
        </tr>
    </tbody>
</table>

<table style="text-align:center;">
    <colgroup>
        <col span="1" style="width:20%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
    </colgroup>
    <thead>
        <tr>
            <td colspan=9>RESPONSE</td>
        </tr>
        <tr>
            <th>Byte </th>
            <th>0</th>
            <th>1</th>
            <th>2</th>
            <th>3</th>
            <th>4</th>
            <th>5</th>
            <th>6</th>
            <th>7</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>+0</td>
            <td>REPCODE</td>
            <td colspan=3>RESERVED</td>
            <td colspan=4>BUFFER LENGTH</td>
        </tr>
        <tr>
            <td>+8</td>
            <td colspan=8>BUFFER DATA...</td>
        </tr>
    </tbody>
</table>

## Borrow

<table style="text-align:center;">
    <colgroup>
        <col span="1" style="width:20%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
    </colgroup>
    <thead>
        <tr>
            <td colspan=9>REQUEST</td>
        </tr>
        <tr>
            <th>Byte Location</th>
            <th>0</th>
            <th>1</th>
            <th>2</th>
            <th>3</th>
            <th>4</th>
            <th>5</th>
            <th>6</th>
            <th>7</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>+0</td>
            <td>OPCODE</td>
            <td colspan=3>RESERVED</td>
            <td colspan=4>SESSION ID</td>
        </tr>
        <tr>
            <td>+8</td>
            <td colspan=8>BOOK ID</td>
        </tr>
    </tbody>
</table>

<table style="text-align:center;">
    <colgroup>
        <col span="1" style="width:20%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
    </colgroup>
    <thead>
        <tr>
            <td colspan=9>RESPONSE</td>
        </tr>
        <tr>
            <th>Byte </th>
            <th>0</th>
            <th>1</th>
            <th>2</th>
            <th>3</th>
            <th>4</th>
            <th>5</th>
            <th>6</th>
            <th>7</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>+0</td>
            <td>REPCODE</td>
            <td colspan=7></td>
        </tr>
    </tbody>
</table>

## Return

<table style="text-align:center;">
    <colgroup>
        <col span="1" style="width:20%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
    </colgroup>
    <thead>
        <tr>
            <td colspan=9>REQUEST</td>
        </tr>
        <tr>
            <th>Byte Location</th>
            <th>0</th>
            <th>1</th>
            <th>2</th>
            <th>3</th>
            <th>4</th>
            <th>5</th>
            <th>6</th>
            <th>7</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>+0</td>
            <td>OPCODE</td>
            <td colspan=3>RESERVED</td>
            <td colspan=4>SESSION ID</td>
        </tr>
        <tr>
            <td>+8</td>
            <td colspan=8>BOOK ID</td>
        </tr>
    </tbody>
</table>

<table style="text-align:center;">
    <colgroup>
        <col span="1" style="width:20%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
    </colgroup>
    <thead>
        <tr>
            <td colspan=9>RESPONSE</td>
        </tr>
        <tr>
            <th>Byte </th>
            <th>0</th>
            <th>1</th>
            <th>2</th>
            <th>3</th>
            <th>4</th>
            <th>5</th>
            <th>6</th>
            <th>7</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>+0</td>
            <td>REPCODE</td>
            <td colspan=7></td>
        </tr>
    </tbody>
</table>

## Query

<table style="text-align:center;">
    <colgroup>
        <col span="1" style="width:20%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
    </colgroup>
    <thead>
        <tr>
            <td colspan=9>REQUEST</td>
        </tr>
        <tr>
            <th>Byte Location</th>
            <th>0</th>
            <th>1</th>
            <th>2</th>
            <th>3</th>
            <th>4</th>
            <th>5</th>
            <th>6</th>
            <th>7</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>+0</td>
            <td>OPCODE</td>
            <td colspan=3>RESERVED</td>
            <td colspan=4>SESSION ID</td>
        </tr>
        <tr>
            <td>+8</td>
            <td colspan=8>EMPTY</td>
        </tr>
    </tbody>
</table>

<table style="text-align:center;">
    <colgroup>
        <col span="1" style="width:20%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
        <col span="1" style="width:10%;">
    </colgroup>
    <thead>
        <tr>
            <td colspan=9>RESPONSE</td>
        </tr>
        <tr>
            <th>Byte </th>
            <th>0</th>
            <th>1</th>
            <th>2</th>
            <th>3</th>
            <th>4</th>
            <th>5</th>
            <th>6</th>
            <th>7</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>+0</td>
            <td>REPCODE</td>
            <td colspan=3>RESERVED</td>
            <td colspan=4>BUFFER LENGTH</td>
        </tr>
        <tr>
            <td>+8</td>
            <td colspan=8>BUFFER DATA...</td>
        </tr>
    </tbody>
</table>
