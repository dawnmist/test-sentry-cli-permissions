# test-sentry-cli-permissions

A sample test repo to check @sentry/cli-linux-x64/bin/sentry-cli permissions.

## System versions

- Yarn: This is occurring with Yarn version 4.0.2 using the pnpm linker. I have not tested other versions/linkers at this stage.
- Node: v20.10.0

## Issue: sentry-cli script is not executable

To reproduce:

- `yarn install`
- `ls -l node_modules/.store/@sentry-cli-linux-x64-npm-2.22.3-81f995f8ee/package/bin`

Note that the permissions for `sentry-cli` are `rw-r--r--` (644) instead of `rwxr-xr-x` (755), so the sentry-cli script is not executable and hence any use of `SentryCli.execute` will fail with an `EACCES` error.

Example when trying to create a new release with the @sentry/remix package:

```sh
Failed to create a sentry release Error: spawn /home/username/project/node_modules/.store/@sentry-cli-linux-x64-npm-2.22.3-81f995f8ee/package/bin/sentry-cli EACCES
    at __node_internal_captureLargerStackTrace (node:internal/errors:563:5)
    at __node_internal_errnoException (node:internal/errors:690:12)
    at ChildProcess._handle.onexit (node:internal/child_process:286:19)
    at onErrorNT (node:internal/child_process:484:16)
    at process.processTicksAndRejections (node:internal/process/task_queues:82:21) {
  errno: -13,
  code: 'EACCES',
  syscall: 'spawn /home/username/project/node_modules/.store/@sentry-cli-linux-x64-npm-2.22.3-81f995f8ee/package/bin/sentry-cli',
  path: '/home/username/project/node_modules/.store/@sentry-cli-linux-x64-npm-2.22.3-81f995f8ee/package/bin/sentry-cli',
  spawnargs: [ 'releases', 'new', '9de06f6330363c6aae9d0409ef3e9ca6025dec3c' ],
  cmd: '/home/username/project/node_modules/.store/@sentry-cli-linux-x64-npm-2.22.3-81f995f8ee/package/bin/sentry-cli releases new 9de06f6330363c6aae9d0409ef3e9ca6025dec3c'
}
```
