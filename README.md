Adapted from https://github.com/farinap5/FTPHoney by farinap5 (MIT licence, kept in LICENSE).

## Changes in this copy

The FTP dialogue is unchanged: same greeting, same replies, same behaviour on the wire. Only these changes were made:

- `go.mod` and `go.sum`, so the program builds with current Go (module mode). `go build` needs cgo and a C compiler for go-sqlite3.
- With `-log`, a client that disconnected before sending USER and PASS (for example a port scanner reading the greeting), or sent a command without an argument, made the whole program stop with `index out of range`. Such sessions are now logged with the fields that were received (empty otherwise), and the line ending is no longer stored with the user name and password.
- Under load (many clients at once) the SQLite insert could fail with "database is locked" and the program then stopped with a nil pointer error (the original README warns that too many threads may corrupt SQLite). Writes now go through one database connection and a failed write is reported instead of stopping the program.
- New option `-db` for the SQLite log file (default `./logs.db`, as before).

Build: `go build -o ftphoney .`  Run: `./ftphoney -l 0.0.0.0:2121 -v -log -db /var/lib/ftphoney/logs.db`

---

<h1 align="center">FTPHoney</h1>
<h3 align="center">Fake FTP - Honeypot</h3>

---
Project of a **simple** fake FTP service made in Golang.

A sqlite file is generated for the insertion of the logs, however !too many threads may cause memory corruption in SQLite!

#### Help
```
COMMAND  DESCRIPTION                        REQUIRED
-------  -----------                        --------
-l       Local host and port. ip:port       No
-a       All tested password are correct.   No
-v       Show connections in verbose mode.  No
-log     Write log.
-db      SQLite log file (default ./logs.db).
-h       Help menu.
```
### Download
> git clone https://github.com/referefref/FTPHoney-nx && cd FTPHoney-nx
### Run
> go run .
### Compile and run
> go build -o ftphoney .
> 
> ./ftphoney

