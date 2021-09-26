I'm looking to build a Hashicorp Vault Plugin for Open Source Redis Database. This plugin will help users to interact with Vault and get dynamic credentials and also let Vault take care of the management of credentials including revoking, rotating credentials which are pretty important and also provide unique credentials for each requester, instead of the same shared credentials

I gotta read about the Vault Database Secrets Engine first

https://www.vaultproject.io/docs/secrets/databases

Many examples - https://www.vaultproject.io/docs/secrets/databases/postgresql

And I'll be building a custom database secrets engine using this custom plugin

https://www.vaultproject.io/docs/secrets/databases/custom

There's already a Vault plugin for Redis - it's for the Enterprise Redis -

https://github.com/RedisLabs/vault-plugin-database-redis-enterprise

Here's an issue which also asks about Open Source Redis support -

https://github.com/RedisLabs/vault-plugin-database-redis-enterprise/issues/10

---

https://www.vaultproject.io/api/secret/databases

Vault Plugin for Databases - to generate and manage credentials for databases

https://github.com/search?utf8=%E2%9C%93&q=vault%20database%20plugin

---

TODOs
- Setup a local Vault first to try out the plugin
- Bootstrap a Vault plugin codebase for database secrets engine - with some basic golang code like go.mod , main.go and anything that's basic that's required for the plugin
- Write down user stories based on use cases you had in mind and by checking what's out there in database secrets engine. Start with basic ones and then move on to advanced ones
- Write down features you are looking to build based on the user stories
- Capture features, user stories etc in GitHub issues and GitHub projects and put a version number to the project name

