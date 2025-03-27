# Purpose
This repo is for the [Fly.io](https://fly.io/) deployments. CD to each application folder and leverage
`flyctl`.

## Description
Each application's infrastructure should have its own namespaced folder. In Fly, everything is an "app".
An app runs on one or more machines. Unfortunately, both our database and XWiki application need volumes,
and since there's no shared volume in Fly we're currently 1 to 1 for this demo.

Read Fly's docs to understand how it all works, but basically Fly simplifies our cloud deployment
by abstracting away the runtime hsot. We're only responsible for the container, and it's volume.

### Volumes
Unfortunately, as of this writing Fly does not offer a shared distributed volume. If our volume dies,
our application dies, and Fly does NOT automatically copy data across volumes. We need to set that
up manually for our Postgres, and our XWiki is just screwed. We don't need 100 percent up-time,
but because XWiki writes to its host disk, we don't have a good way to scale the app.

## Important Fly Commands
Execute the FlyCTL commands from within the application directory so it knows which app you are targetting,
or set the app manually via CLI.
```cmd
flyctl scale count <n>
```
This command allows us to disable the apps with 0, or start them with 1. We **CANNOT** scale the apps
because the runtime is currently bound to its volume!
### Rehydrating
For now, rehydrating the image is manual. Simply run
```cmd
flyctl deploy
```

## Deployment Learnings
On the first deployment XWiki performs a lenghty initilization and self-install process. Therefore,
the CPU must be a [performance](https://fly.io/docs/machines/cpu-performance/) and not shared flavor.
After the initialization phase, we can scale down.
