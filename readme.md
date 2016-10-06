AGILE scripts
===

start and stop scripts for various AGILE components

Requirements
---

`docker-compose` at least version 1.8. Check it with `docker-compose -v`

Read more on how to [install or upgrade](https://docs.docker.com/compose/install/)

Usage
---

Use `./agile start` to start the main components, and `./agile stop` to stop them.

Once component started you can visit http://127.0.0.1:8000 to access the AGILE user interface and start building your IoT solution

Troubleshooting
---
You can access the logs with `./compose/agile-compose logs` and view all logs.

To view per component log use `./compose/agile-compose logs <component>`

To get the list of running components use `./compose/agile-compose ps` with the pattern `compose_<component name>_1`
