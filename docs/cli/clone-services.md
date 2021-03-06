# Clone services

The `clone services` command creates a copy of one nio service configuration. This is useful when you would like to replicate the functionality of a service. Simply clone the service and make any desired changes without needing to recreate the blocks and connections.

This command must be run from the root level of your nio project directory.

---

## Example:

```bash
nio clone service simulate-and-log simulate-and-log2
```
creates a second service **simulate-and-log2** which will have the exact same blocks and configuration as **simulate-and-log**. This can be checked by viewing the services in the nio System Designer or by running `nio list services`.
