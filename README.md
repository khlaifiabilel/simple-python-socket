# Simple Python Socket

A small educational TCP chat experiment with a threaded server and terminal
client. The server accepts multiple connections and broadcasts each received
message to every other connected client.

## Current status

The server is a runnable Python 3 example. The checked-in client is **not
currently runnable**: two indented Git configuration lines at the end of
`client.py` cause an `IndentationError`. The client also imports PyCryptodome
but does not encrypt network messages. This README documents the repository as
it exists; it does not claim secure messaging or a passing end-to-end demo.

## Protocol and behavior

- IPv4 TCP sockets with a host and port entered interactively
- One server thread per accepted client
- Messages read in chunks of at most 32 bytes
- Broadcast to peers, excluding the sender
- Plain UTF-8 terminal input with no framing, authentication, or encryption

TCP is a byte stream, so a single `recv(32)` call is not guaranteed to equal one
complete user message. Longer input can be split across reads.

## Local usage

No dependency is needed to start the server:

```bash
git clone https://github.com/khlaifiabilel/simple-python-socket.git
cd simple-python-socket
python3 server.py
```

For a same-machine test, enter `127.0.0.1` and an unused unprivileged port such
as `5000`. The client cannot be started until its syntax error is corrected in a
separate code change. If that is done, its unused `Crypto` imports require the
third-party `pycryptodome` package, which is not declared in this repository.

Do not expose this server to the public internet. Bind to `127.0.0.1` for local
experiments; binding to `0.0.0.0` accepts traffic on every network interface.

## Syntax checks

```bash
python3 -m py_compile server.py
python3 -m py_compile client.py
```

The first command should pass. The second intentionally reports the current
`IndentationError`, making the limitation reproducible.

## Configuration and security

There are no configuration files, credentials, or persistent data. Host and
port are prompted at runtime. Messages are plaintext, clients are unauthenticated,
input is unbounded at the protocol level, and broad exception handlers hide
specific network failures. The SHA-256 object in `client.py` does not protect
messages, and AES is imported but never used.

## Provenance

The repository history begins with a commit by `kalifiabillal` on 2021-01-29
and contains a later peer-to-peer experiment branch merged on 2021-02-05. No
upstream source or third-party code notice is recorded in the files or Git
history.

## License

The project is available under the MIT License; see [`LICENSE`](LICENSE).
PyCryptodome is a third-party dependency with its own license and is not covered
by this repository's license.
