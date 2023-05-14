# Security Policy

## Supported Versions

The Python team applies security fixes according to the table
in [the devguide](
https://devguide.python.org/versions/#supported-versions
).

## Reporting a Vulnerability

Please read the guidelines on reporting security issues [on the
official website](https://www.python.org/dev/security/) for
instructions on how to report a security-related problem to
the Python team responsibly.

To reach the response team, email `security at python dot org`.
#!/usr/bin/env python3
import asyncio

async def cause_problems():
    accepted = asyncio.Event()

    async def _incoming(reader, writer):
        print("entered _incoming")
        print("signaling outer context; we accepted a client")
        accepted.set()
        print("left _incoming")

    print("starting server")
    server = await asyncio.start_unix_server(
        _incoming,
        path="problems.sock",
        backlog=1,
    )
    print("created server; waiting on a client")
    await accepted.wait()
    print("got a client")

    print("closing server")
    server.close()
    print("issued close()")
    await server.wait_closed()
    print("finished waiting on server to close")


if __name__ == '__main__':
    asyncio.run(cause_problems())
