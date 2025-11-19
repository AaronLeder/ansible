# Role: Patching
One-stop-shop for anything related to patching.

Supported package managers:
* apt
* dnf
* yum

Provides a couple minutes after reboot (if required) for the host to crash. 

## Notes:
* `any_errors_fatal: true` in the play is required for the logic to work correctly.

## Available tags
* 