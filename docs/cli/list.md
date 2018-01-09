# List

The `list` or `ls` command lists all of the blocks or services within a running nio instance.

This command must be run from the root level of your project directory.

Example (blocks):
```bash
nio list blocks
```
returns a list of configured blocks in your running instance
```
Simulate
Log
```

Example (services):
```bash
nio list services
```
returns a list of services in your running instance
```
simulate-and-log
```