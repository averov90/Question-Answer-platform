# Simple Q&A platform
[![License](https://img.shields.io/badge/LICENSE-LGPL%20v2.1-green?style=flat-square)](/LICENSE)  [![Version](https://img.shields.io/badge/VERSION-RELEASE%20--%201.0-green?style=flat-square)](https://github.com/averov90/Question-Answer-platform/releases)
### :small_orange_diamond: [Russian version](/README-ru.md)

The system implements the functionality of the simplest question-answer platform.
It consists of a client and a server that communicate over HTTP using JSON.
The server is under no circumstances the initiator of the request, so it can be called passive.
The client sends a request to the server, after which the server responds with data in JSON format, or only sets the request success code (HTTP response code).
Both GET and POST requests are supported.
The first type is used to request data from the client from the server. If everything goes well, the server sends JSON in response. The second type of request (POST) is used to send data from the client to the server. In response, the server sends a success code for the request.

In general, the system is shown in the figure below (blocks active in GET or POST requests are highlighted in darker color):

![Structure diagram (SVG)](https://github.com/averov90/Question-Answer-platform/raw/master/image-system-struct.svg)

## Server features

The developed server uses HTTP and JSON to communicate with the client, and uses SQLite for data storage, which it interacts with using the Entity Framework Core. Multi-user mode is supported (simultaneous work of several users with the server).
The program has a special class of a data storage (database) access provider, the structure of which will be considered in the UML class diagram. The structure (schema) of the database is presented below.

![DB scheme (SVG)](https://github.com/averov90/Question-Answer-platform/raw/master/image-db.svg)

The server in GET requests accepts the following parameters:
- `question` – a parameter to which you can assign the title of the question, the answers to which the user wants to see;
- `question_pattern` – a parameter to which you can assign a question title template, as a result of which questions matching the template will be displayed;
- `startindex` - an optional parameter, shows from which place in the sample to send data (if the sample is large, it can be divided into several requests);
- `count` – an optional parameter, how many elements of the selection, starting from the specified one, to send to the client.

The server in POST requests accepts the following parameters:
- `question` – a parameter to which you need to assign the title of the question, the answer to which the user wants to send;

The server sorts the selection data by the date and time of addition (in order from newest to oldest), which it takes from the computer on which it is running.
If you do not specify the `question` and `question_pattern` parameters, then depending on the type of the GET or POST request, a list of questions will be requested or a question will be expected to be sent.

### UML diagram

![UML diagram (SVG)](https://github.com/averov90/Question-Answer-platform/raw/master/image-uml.svg)

## Client features

The client has a console interface and has a command parser that supports working with quoted strings, as well as ignoring extra indentation and command case.

In the client, the user can work in several contexts that have the following hierarchy (the context is the list level):

- write a question (ask command)
- get a list of questions (list command)
    - look at the next n questions (next command)
    - view previous n questions (prev command)
    - view the question (view command)
        - answer a question (answer command)
        - watch next n answers (next command)
        - view previous n questions (prev command)
        - view response (view command)
- search for a question (find command)
    - look at the next n questions (next command)
    - view previous n questions (prev command)
    - look for another question (find command)
    - view the question (view command)
        - answer a question (answer command)
        - watch next n answers (next command)
        - view previous n questions (prev command)
        - view response (view command)

The question view menu is universal for both a regular list of questions and a list obtained as a result of a search.
All necessary checks are implemented on the client to minimize error responses from the server.