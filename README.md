# Requester: HTTP Client for Humans

## 2021-04-17

Building/consuming web APIs is fundamental to web and mobile development. HTTP clients make working with web APIs a lot easier. Roughly speaking, HTTP clients fall into two categories: GUI apps, like Postman, Insomnia and Paw, and CLI apps like cURL and HTTPie.

Requester is an HTTP client for Sublime Text. It combines the "managed UI" features from GUI clients like collections, request history, env vars, syntax highlighting, request chaining, etc, with the speed of CLI clients. Everything is text, so sharing and versioning request collections is easy. It uses [Requests' syntax](http://docs.python-requests.org/en/master/), which is very nice and well documented.

I'd also like to show you how Requester works, how it uses Sublime Text to build a UI that combines the best of GUI and CLI, some of the cool things it does with Python, and how it could be improved by incorporating some "newish" Python features.

## Installation

- Get [Sublime Text 3](https://www.sublimetext.com/)
- Install __Requester__ (not ~~Http Requester~~)

## Basic Features

- Request Body, Query Params, Custom Headers, Cookies (see interactive tutorial)
- "Env" vars
- Syntax (Requests), plus convenience methods

## UI and UX

- Request collections and navigation (fuzzy search!)
- Response tabs
- Request history (`ctrl+h`)

## Portability and teams

-  It's text!
  + `git`, `GitHub`, etc
    * Env vars can go in a `.gitignore`d file, e.g. `requester_env.py`
-  Export to cURL, HTTPie
-  Import from cURL
  + Debugging AJAX/XHR requests sent by your browser

## Bonuses  

- GraphQL support
- Test runner
- Benchmarking tool (like `ab` or `siege`)

## Python stuff

- Parser
  + `parse` takes a string, such as a selection from a view, returns a list of all calls to `requests` in the string
- Eval and Exec
  + `exec` used to build an env dict from a string, which is sort of how `import` populates a variables dict from a Python module
  + the `exec`ed string is a concatenation of env block and env file
  + `eval` used to parse args passed to `requests.<http_method>`
    * e.g. `get` with `filename` argument
  + This lets Requester modify, remove or add to args and kwargs before actually calling `requests` (see `prepare_request`)
- Requests run in parallel in a thread pool
- Syntax is [Requests' syntax](http://docs.python-requests.org/en/master/)

## Python 3.8 stuff

- [Sublime Text 4](https://gist.github.com/jfcherng/7bf4103ea486d1f67b7970e846b3a619) is coming, which means plugins will run on Python 3.8 instead of 3.3
- Biggest new features are type annotations and `asyncio`
- Type annotations would make the code base a lot easier to work with, especially for keeping track of the shape of data
  + Benefits new contributors and maintainers
- `asyncio` would make it easy to improve the benchmarking tool so it can send __lots__ of requests concurrently
  + This would require an async HTTP client library; fortunately there's [HTTPX](https://github.com/encode/httpx), whose syntax mirrors that of `requests`
- `asyncio` means we could build a websocket client using [websockets](https://github.com/aaugustin/websockets)
  + This would require a different design, because wss "full duplex" communication is different from HTTP request/response

## Takeaways

- Text editor plugins can have very powerful UIs
  + There ought to more UIs that mix GUI and text
  + Someone ought to build Requester for VS Code, it could get big
- Build on top of stable, well-documented libraries
- `eval` and `exec` have some cool uses
- `asyncio` and type annotations have made Python a more scalable, versatile language
