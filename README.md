# Purpose
This repo is for the [Fly.io](https://fly.io/) deployments. CD to each application folder and leverage
`flyctl`.

## Description
Each application's infrastructure should have its own namespaced folder. In Fly, everything is an "app".
An app runs on one or more machines. Unfortunately, both our database and XWiki application need volumes,
and since there's no shared volume in Fly we're currently 1 to 1 for this demo.

I haven't figured out how to get this deployment working, but we will soon!