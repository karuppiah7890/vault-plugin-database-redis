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

---

https://learn.hashicorp.com/tutorials/vault/getting-started-intro?in=vault/getting-started

https://learn.hashicorp.com/tutorials/vault/getting-started-install?in=vault/getting-started

https://learn.hashicorp.com/tutorials/vault/getting-started-dev-server?in=vault/getting-started

https://learn.hashicorp.com/tutorials/vault/getting-started-first-secret?in=vault/getting-started

---

```bash
vault-plugin-database-redis $ vault server -dev
==> Vault server configuration:

             Api Address: http://127.0.0.1:8200
                     Cgo: disabled
         Cluster Address: https://127.0.0.1:8201
              Go Version: go1.16.7
              Listener 1: tcp (addr: "127.0.0.1:8200", cluster address: "127.0.0.1:8201", max_request_duration: "1m30s", max_request_size: "33554432", tls: "disabled")
               Log Level: info
                   Mlock: supported: false, enabled: false
           Recovery Mode: false
                 Storage: inmem
                 Version: Vault v1.8.2
             Version Sha: aca76f63357041a43b49f3e8c11d67358496959f

==> Vault server started! Log data will stream in below:

2021-09-26T18:13:13.540+0530 [INFO]  proxy environment: http_proxy="" https_proxy="" no_proxy=""
2021-09-26T18:13:13.541+0530 [WARN]  no `api_addr` value specified in config or in VAULT_API_ADDR; falling back to detection if possible, but this value should be manually set
2021-09-26T18:13:13.542+0530 [INFO]  core: security barrier not initialized
2021-09-26T18:13:13.543+0530 [INFO]  core: security barrier initialized: stored=1 shares=1 threshold=1
2021-09-26T18:13:13.544+0530 [INFO]  core: post-unseal setup starting
2021-09-26T18:13:13.554+0530 [INFO]  core: loaded wrapping token key
2021-09-26T18:13:13.554+0530 [INFO]  core: successfully setup plugin catalog: plugin-directory=""
2021-09-26T18:13:13.554+0530 [INFO]  core: no mounts; adding default mount table
2021-09-26T18:13:13.555+0530 [INFO]  core: successfully mounted backend: type=cubbyhole path=cubbyhole/
2021-09-26T18:13:13.556+0530 [INFO]  core: successfully mounted backend: type=system path=sys/
2021-09-26T18:13:13.556+0530 [INFO]  core: successfully mounted backend: type=identity path=identity/
2021-09-26T18:13:13.558+0530 [INFO]  core: successfully enabled credential backend: type=token path=token/
2021-09-26T18:13:13.558+0530 [INFO]  rollback: starting rollback manager
2021-09-26T18:13:13.558+0530 [INFO]  core: restoring leases
2021-09-26T18:13:13.559+0530 [INFO]  identity: entities restored
2021-09-26T18:13:13.559+0530 [INFO]  identity: groups restored
2021-09-26T18:13:13.559+0530 [INFO]  expiration: lease restore complete
2021-09-26T18:13:13.560+0530 [INFO]  core: post-unseal setup complete
2021-09-26T18:13:13.561+0530 [INFO]  core: root token generated
2021-09-26T18:13:13.561+0530 [INFO]  core: pre-seal teardown starting
2021-09-26T18:13:13.561+0530 [INFO]  rollback: stopping rollback manager
2021-09-26T18:13:13.561+0530 [INFO]  core: pre-seal teardown complete
2021-09-26T18:13:13.561+0530 [INFO]  core.cluster-listener.tcp: starting listener: listener_address=127.0.0.1:8201
2021-09-26T18:13:13.561+0530 [INFO]  core.cluster-listener: serving cluster requests: cluster_listen_address=127.0.0.1:8201
2021-09-26T18:13:13.561+0530 [INFO]  core: post-unseal setup starting
2021-09-26T18:13:13.561+0530 [INFO]  core: loaded wrapping token key
2021-09-26T18:13:13.561+0530 [INFO]  core: successfully setup plugin catalog: plugin-directory=""
2021-09-26T18:13:13.561+0530 [INFO]  core: successfully mounted backend: type=system path=sys/
2021-09-26T18:13:13.562+0530 [INFO]  core: successfully mounted backend: type=identity path=identity/
2021-09-26T18:13:13.562+0530 [INFO]  core: successfully mounted backend: type=cubbyhole path=cubbyhole/
2021-09-26T18:13:13.563+0530 [INFO]  core: successfully enabled credential backend: type=token path=token/
2021-09-26T18:13:13.563+0530 [INFO]  rollback: starting rollback manager
2021-09-26T18:13:13.563+0530 [INFO]  core: restoring leases
2021-09-26T18:13:13.564+0530 [INFO]  identity: entities restored
2021-09-26T18:13:13.564+0530 [INFO]  identity: groups restored
2021-09-26T18:13:13.564+0530 [INFO]  expiration: lease restore complete
2021-09-26T18:13:13.564+0530 [INFO]  core: post-unseal setup complete
2021-09-26T18:13:13.564+0530 [INFO]  core: vault is unsealed
2021-09-26T18:13:13.568+0530 [INFO]  core: successful mount: namespace="" path=secret/ type=kv
2021-09-26T18:13:13.579+0530 [INFO]  secrets.kv.kv_4f51ea2f: collecting keys to upgrade
2021-09-26T18:13:13.579+0530 [INFO]  secrets.kv.kv_4f51ea2f: done collecting keys: num_keys=1
2021-09-26T18:13:13.579+0530 [INFO]  secrets.kv.kv_4f51ea2f: upgrading keys finished
WARNING! dev mode is enabled! In this mode, Vault runs entirely in-memory
and starts unsealed with a single unseal key. The root token is already
authenticated to the CLI, so you can immediately begin using Vault.

You may need to set the following environment variable:

    $ export VAULT_ADDR='http://127.0.0.1:8200'

The unseal key and root token are displayed below in case you want to
seal/unseal the Vault or re-authenticate.

Unseal Key: XLHDruvo65AZI/OMPR94BOnrrN++CaAzK2qDf3ZGopI=
Root Token: s.UGoNOBbmEDKGy2OZToN2DQWP

Development mode should NOT be used in production installations!
```

```
Unseal Key: XLHDruvo65AZI/OMPR94BOnrrN++CaAzK2qDf3ZGopI=
Root Token: s.UGoNOBbmEDKGy2OZToN2DQWP
```

```bash
vault-plugin-database-redis $ export VAULT_ADDR='http://127.0.0.1:8200'
vault-plugin-database-redis $ export VAULT_TOKEN="s.UGoNOBbmEDKGy2OZToN2DQWP"
vault-plugin-database-redis $ vault status
Key             Value
---             -----
Seal Type       shamir
Initialized     true
Sealed          false
Total Shares    1
Threshold       1
Version         1.8.2
Storage Type    inmem
Cluster Name    vault-cluster-75b739be
Cluster ID      abaddd77-7241-db31-a606-e3da0e3f518e
HA Enabled      false
vault-plugin-database-redis $
```

```bash
vault-plugin-database-redis $ vault kv get secret/hello
No value found at secret/data/hello
vault-plugin-database-redis $ vault kv put secret/hello foo=world
Key              Value
---              -----
created_time     2021-09-26T12:52:36.918463Z
deletion_time    n/a
destroyed        false
version          1
vault-plugin-database-redis $ vault kv get secret/hello
====== Metadata ======
Key              Value
---              -----
created_time     2021-09-26T12:52:36.918463Z
deletion_time    n/a
destroyed        false
version          1

=== Data ===
Key    Value
---    -----
foo    world
vault-plugin-database-redis $ vault kv get secret/hello/food
No value found at secret/data/hello/food
vault-plugin-database-redis $ vault kv get secret/hello/foo
No value found at secret/data/hello/foo
vault-plugin-database-redis $ vault kv get secret/hello foo
Too many arguments (expected 1, got 2)
vault-plugin-database-redis $ vault kv put secret/hello foo=world excited=yes
Key              Value
---              -----
created_time     2021-09-26T12:53:15.075683Z
deletion_time    n/a
destroyed        false
version          2
vault-plugin-database-redis $ vault kv get secret/hello
====== Metadata ======
Key              Value
---              -----
created_time     2021-09-26T12:53:15.075683Z
deletion_time    n/a
destroyed        false
version          2

===== Data =====
Key        Value
---        -----
excited    yes
foo        world
vault-plugin-database-redis $ vault kv get secret/hello -field=food
Too many arguments (expected 1, got 2)
vault-plugin-database-redis $ vault kv get secret/hello -field=foo
Too many arguments (expected 1, got 2)
vault-plugin-database-redis $ vault kv get -field=foo secret/hello
world
vault-plugin-database-redis $ vault kv get -field=food secret/hello
Field "food" not present in secret
vault-plugin-database-redis $ vault kv get -field=excited secret/hello
yes
vault-plugin-database-redis $ vault kv get -field=excited -format json secret/hello
"yes"
vault-plugin-database-redis $ vault kv get -format json secret/hello
{
  "request_id": "03d5ab09-eab5-f99e-1d62-e8a2aa643a97",
  "lease_id": "",
  "lease_duration": 0,
  "renewable": false,
  "data": {
    "data": {
      "excited": "yes",
      "foo": "world"
    },
    "metadata": {
      "created_time": "2021-09-26T12:53:15.075683Z",
      "deletion_time": "",
      "destroyed": false,
      "version": 2
    }
  },
  "warnings": null
}
vault-plugin-database-redis $ vault kv get -format json secret/hello | jq .data
{
  "data": {
    "excited": "yes",
    "foo": "world"
  },
  "metadata": {
    "created_time": "2021-09-26T12:53:15.075683Z",
    "deletion_time": "",
    "destroyed": false,
    "version": 2
  }
}
vault-plugin-database-redis $ vault kv get -format json secret/hello | jq .data.data
{
  "excited": "yes",
  "foo": "world"
}
vault-plugin-database-redis $ vault kv get -format json secret/hello | jq .data.data.excited
"yes"
vault-plugin-database-redis $ vault kv get -format json secret/hello | jq .data.data.excited -r
yes
vault-plugin-database-redis $ vault kv get -format json secret/hello | jq -r .data.data.excited
yes
vault-plugin-database-redis $ vault kv get -format=json secret/hello | jq -r .data.data.excited
yes
vault-plugin-database-redis $ vault kv delete secret/hello
Success! Data deleted (if it existed) at: secret/hello
vault-plugin-database-redis $ vault kv delete secret/hello
Success! Data deleted (if it existed) at: secret/hello
vault-plugin-database-redis $ vault kv delete secret/hello
Success! Data deleted (if it existed) at: secret/hello
vault-plugin-database-redis $
```

https://learn.hashicorp.com/tutorials/vault/getting-started-secrets-engines?in=vault/getting-started

```bash
vault-plugin-database-redis $ vault secrets
Usage: vault secrets <subcommand> [options] [args]

  This command groups subcommands for interacting with Vault's secrets engines.
  Each secret engine behaves differently. Please see the documentation for
  more information.

  List all enabled secrets engines:

      $ vault secrets list

  Enable a new secrets engine:

      $ vault secrets enable database

  Please see the individual subcommand help for detailed usage information.

Subcommands:
    disable    Disable a secret engine
    enable     Enable a secrets engine
    list       List enabled secrets engines
    move       Move a secrets engine to a new path
    tune       Tune a secrets engine configuration
vault-plugin-database-redis $ vault secrets enable
Not enough arguments (expected 1, got 0)
vault-plugin-database-redis $ vault secrets enable kv
Success! Enabled the kv secrets engine at: kv/
vault-plugin-database-redis $ vault secrets enable kv
Error enabling: Error making API request.

URL: POST http://127.0.0.1:8200/v1/sys/mounts/kv
Code: 400. Errors:

* path is already in use at kv/
vault-plugin-database-redis $ vault secrets enable -path kv kv
Error enabling: Error making API request.

URL: POST http://127.0.0.1:8200/v1/sys/mounts/kv
Code: 400. Errors:

* path is already in use at kv/
vault-plugin-database-redis $ vault secrets enable -path=kv kv
Error enabling: Error making API request.

URL: POST http://127.0.0.1:8200/v1/sys/mounts/kv
Code: 400. Errors:

* path is already in use at kv/
vault-plugin-database-redis $ vault secrets enable -path=keyvalue kv
Success! Enabled the kv secrets engine at: keyvalue/
vault-plugin-database-redis $ vault kv get keyvalue/something
No value found at keyvalue/something
vault-plugin-database-redis $ vault kv put keyvalue/something just=wow
Success! Data written to: keyvalue/something
vault-plugin-database-redis $ vault kv get keyvalue/something
==== Data ====
Key     Value
---     -----
just    wow
vault-plugin-database-redis $ vault kv get -field=blah keyvalue/something
Field "blah" not present in secret
vault-plugin-database-redis $ vault kv get -field=meh keyvalue/something
Field "meh" not present in secret
vault-plugin-database-redis $ vault kv get -field=just keyvalue/something
wow
vault-plugin-database-redis $
```

```bash
vault-plugin-database-redis $ vault secrets
Usage: vault secrets <subcommand> [options] [args]

  This command groups subcommands for interacting with Vault's secrets engines.
  Each secret engine behaves differently. Please see the documentation for
  more information.

  List all enabled secrets engines:

      $ vault secrets list

  Enable a new secrets engine:

      $ vault secrets enable database

  Please see the individual subcommand help for detailed usage information.

Subcommands:
    disable    Disable a secret engine
    enable     Enable a secrets engine
    list       List enabled secrets engines
    move       Move a secrets engine to a new path
    tune       Tune a secrets engine configuration
vault-plugin-database-redis $ vault secrets list
Path          Type         Accessor              Description
----          ----         --------              -----------
cubbyhole/    cubbyhole    cubbyhole_29adf3b2    per-token private secret storage
identity/     identity     identity_cb1660f3     identity store
keyvalue/     kv           kv_a52cf9ae           n/a
kv/           kv           kv_7a97b7e2           n/a
secret/       kv           kv_4f51ea2f           key/value secret storage
sys/          system       system_d3eaf0d9       system endpoints used for control, policy and debugging
vault-plugin-database-redis $
```

```bash
vault-plugin-database-redis $ vault secrets enable -h
Usage: vault secrets enable [options] TYPE

  Enables a secrets engine. By default, secrets engines are enabled at the path
  corresponding to their TYPE, but users can customize the path using the
  -path option.

  Once enabled, Vault will route all requests which begin with the path to the
  secrets engine.

  Enable the AWS secrets engine at aws/:

      $ vault secrets enable aws

  Enable the SSH secrets engine at ssh-prod/:

      $ vault secrets enable -path=ssh-prod ssh

  Enable the database secrets engine with an explicit maximum TTL of 30m:

      $ vault secrets enable -max-lease-ttl=30m database

  Enable a custom plugin (after it is registered in the plugin registry):

      $ vault secrets enable -path=my-secrets -plugin-name=my-plugin plugin

  OR (preferred way):

      $ vault secrets enable -path=my-secrets my-plugin

  For a full list of secrets engines and examples, please see the documentation.

HTTP Options:

  -address=<string>
      Address of the Vault server. The default is https://127.0.0.1:8200. This
      can also be specified via the VAULT_ADDR environment variable.

  -agent-address=<string>
      Address of the Agent. This can also be specified via the
      VAULT_AGENT_ADDR environment variable.

  -ca-cert=<string>
      Path on the local disk to a single PEM-encoded CA certificate to verify
      the Vault server's SSL certificate. This takes precedence over -ca-path.
      This can also be specified via the VAULT_CACERT environment variable.

  -ca-path=<string>
      Path on the local disk to a directory of PEM-encoded CA certificates to
      verify the Vault server's SSL certificate. This can also be specified
      via the VAULT_CAPATH environment variable.

  -client-cert=<string>
      Path on the local disk to a single PEM-encoded CA certificate to use
      for TLS authentication to the Vault server. If this flag is specified,
      -client-key is also required. This can also be specified via the
      VAULT_CLIENT_CERT environment variable.

  -client-key=<string>
      Path on the local disk to a single PEM-encoded private key matching the
      client certificate from -client-cert. This can also be specified via the
      VAULT_CLIENT_KEY environment variable.

  -mfa=<string>
      Supply MFA credentials as part of X-Vault-MFA header. This can also be
      specified via the VAULT_MFA environment variable.

  -namespace=<string>
      The namespace to use for the command. Setting this is not necessary
      but allows using relative paths. -ns can be used as shortcut. The
      default is (not set). This can also be specified via the VAULT_NAMESPACE
      environment variable.

  -output-curl-string
      Instead of executing the request, print an equivalent cURL command
      string and exit. The default is false.

  -policy-override
      Override a Sentinel policy that has a soft-mandatory enforcement_level
      specified The default is false.

  -tls-server-name=<string>
      Name to use as the SNI host when connecting to the Vault server via TLS.
      This can also be specified via the VAULT_TLS_SERVER_NAME environment
      variable.

  -tls-skip-verify
      Disable verification of TLS certificates. Using this option is highly
      discouraged as it decreases the security of data transmissions to and
      from the Vault server. The default is false. This can also be specified
      via the VAULT_SKIP_VERIFY environment variable.

  -wrap-ttl=<duration>
      Wraps the response in a cubbyhole token with the requested TTL. The
      response is available via the "vault unwrap" command. The TTL is
      specified as a numeric string with suffix like "30s" or "5m". This can
      also be specified via the VAULT_WRAP_TTL environment variable.

Command Options:

  -allowed-response-headers=<string>
      Comma-separated string or list of response header values that plugins
      will be allowed to set

  -audit-non-hmac-request-keys=<string>
      Comma-separated string or list of keys that will not be HMAC'd by audit
      devices in the request data object.

  -audit-non-hmac-response-keys=<string>
      Comma-separated string or list of keys that will not be HMAC'd by audit
      devices in the response data object.

  -default-lease-ttl=<duration>
      The default lease TTL for this secrets engine. If unspecified, this
      defaults to the Vault server's globally configured default lease TTL.

  -description=<string>
      Human-friendly description for the purpose of this engine.

  -external-entropy-access
      Enable secrets engine to access Vault's external entropy source. The
      default is false.

  -force-no-cache
      Force the secrets engine to disable caching. If unspecified, this
      defaults to the Vault server's globally configured cache settings. This
      does not affect caching of the underlying encrypted data storage. The
      default is false.

  -listing-visibility=<string>
      Determines the visibility of the mount in the UI-specific listing
      endpoint.

  -local
      Mark the secrets engine as local-only. Local engines are not replicated
      or removed by replication. The default is false.

  -max-lease-ttl=<duration>
      The maximum lease TTL for this secrets engine. If unspecified, this
      defaults to the Vault server's globally configured maximum lease TTL.

  -options=<key=value>
      Key-value pair provided as key=value for the mount options. This can be
      specified multiple times.

  -passthrough-request-headers=<string>
      Comma-separated string or list of request header values that will be
      sent to the plugins

  -path=<string>
      Place where the secrets engine will be accessible. This must be unique
      cross all secrets engines. This defaults to the "type" of the secrets
      engine.

  -plugin-name=<string>
      Name of the secrets engine plugin. This plugin name must already exist
      in Vault's plugin catalog.

  -seal-wrap
      Enable seal wrapping of critical values in the secrets engine. The
      default is false.

  -version=<int>
      Select the version of the engine to run. Not supported by all engines.
vault-plugin-database-redis $ vault secrets enable aws
Success! Enabled the aws secrets engine at: aws/
vault-plugin-database-redis $
vault-plugin-database-redis $
vault-plugin-database-redis $ source .env
vault-plugin-database-redis $ vault secrets
Usage: vault secrets <subcommand> [options] [args]

  This command groups subcommands for interacting with Vault's secrets engines.
  Each secret engine behaves differently. Please see the documentation for
  more information.

  List all enabled secrets engines:

      $ vault secrets list

  Enable a new secrets engine:

      $ vault secrets enable database

  Please see the individual subcommand help for detailed usage information.

Subcommands:
    disable    Disable a secret engine
    enable     Enable a secrets engine
    list       List enabled secrets engines
    move       Move a secrets engine to a new path
    tune       Tune a secrets engine configuration
vault-plugin-database-redis $ vault secrets tune
Not enough arguments (expected 1, got 0)
vault-plugin-database-redis $ vault secrets tune -h
Usage: vault secrets tune [options] PATH

  Tunes the configuration options for the secrets engine at the given PATH.
  The argument corresponds to the PATH where the secrets engine is enabled,
  not the TYPE!

  Tune the default lease for the PKI secrets engine:

      $ vault secrets tune -default-lease-ttl=72h pki/

HTTP Options:

  -address=<string>
      Address of the Vault server. The default is https://127.0.0.1:8200. This
      can also be specified via the VAULT_ADDR environment variable.

  -agent-address=<string>
      Address of the Agent. This can also be specified via the
      VAULT_AGENT_ADDR environment variable.

  -ca-cert=<string>
      Path on the local disk to a single PEM-encoded CA certificate to verify
      the Vault server's SSL certificate. This takes precedence over -ca-path.
      This can also be specified via the VAULT_CACERT environment variable.

  -ca-path=<string>
      Path on the local disk to a directory of PEM-encoded CA certificates to
      verify the Vault server's SSL certificate. This can also be specified
      via the VAULT_CAPATH environment variable.

  -client-cert=<string>
      Path on the local disk to a single PEM-encoded CA certificate to use
      for TLS authentication to the Vault server. If this flag is specified,
      -client-key is also required. This can also be specified via the
      VAULT_CLIENT_CERT environment variable.

  -client-key=<string>
      Path on the local disk to a single PEM-encoded private key matching the
      client certificate from -client-cert. This can also be specified via the
      VAULT_CLIENT_KEY environment variable.

  -mfa=<string>
      Supply MFA credentials as part of X-Vault-MFA header. This can also be
      specified via the VAULT_MFA environment variable.

  -namespace=<string>
      The namespace to use for the command. Setting this is not necessary
      but allows using relative paths. -ns can be used as shortcut. The
      default is (not set). This can also be specified via the VAULT_NAMESPACE
      environment variable.

  -output-curl-string
      Instead of executing the request, print an equivalent cURL command
      string and exit. The default is false.

  -policy-override
      Override a Sentinel policy that has a soft-mandatory enforcement_level
      specified The default is false.

  -tls-server-name=<string>
      Name to use as the SNI host when connecting to the Vault server via TLS.
      This can also be specified via the VAULT_TLS_SERVER_NAME environment
      variable.

  -tls-skip-verify
      Disable verification of TLS certificates. Using this option is highly
      discouraged as it decreases the security of data transmissions to and
      from the Vault server. The default is false. This can also be specified
      via the VAULT_SKIP_VERIFY environment variable.

  -wrap-ttl=<duration>
      Wraps the response in a cubbyhole token with the requested TTL. The
      response is available via the "vault unwrap" command. The TTL is
      specified as a numeric string with suffix like "30s" or "5m". This can
      also be specified via the VAULT_WRAP_TTL environment variable.

Command Options:

  -audit-non-hmac-request-keys=<string>
      Comma-separated string or list of keys that will not be HMAC'd by audit
      devices in the request data object.

  -audit-non-hmac-response-keys=<string>
      Comma-separated string or list of keys that will not be HMAC'd by audit
      devices in the response data object.

  -default-lease-ttl=<duration>
      The default lease TTL for this secrets engine. If unspecified, this
      defaults to the Vault server's globally configured default lease TTL, or
      a previously configured value for the secrets engine.

  -description=<string>
      Human-friendly description of this secret engine. This overrides the
      current stored value, if any.

  -listing-visibility=<string>
      Determines the visibility of the mount in the UI-specific listing
      endpoint.

  -max-lease-ttl=<duration>
      The maximum lease TTL for this secrets engine. If unspecified, this
      defaults to the Vault server's globally configured maximum lease TTL, or
      a previously configured value for the secrets engine.

  -options=<key=value>
      Key-value pair provided as key=value for the mount options. This can be
      specified multiple times.

  -version=<int>
      Select the version of the engine to run. Not supported by all engines.
vault-plugin-database-redis $ vault secrets write aws/config/root \
> access_key=${AWS_ACCESS_KEY_ID} \
> secret_key=${AWS_SECRET_ACCESS_KEY} \
> region=us-west-1
Usage: vault secrets <subcommand> [options] [args]

  This command groups subcommands for interacting with Vault's secrets engines.
  Each secret engine behaves differently. Please see the documentation for
  more information.

  List all enabled secrets engines:

      $ vault secrets list

  Enable a new secrets engine:

      $ vault secrets enable database

  Please see the individual subcommand help for detailed usage information.

Subcommands:
    disable    Disable a secret engine
    enable     Enable a secrets engine
    list       List enabled secrets engines
    move       Move a secrets engine to a new path
    tune       Tune a secrets engine configuration
vault-plugin-database-redis $ vault write aws/config/root access_key=${AWS_ACCESS_KEY_ID} secret_key=${AWS_SECRET_ACCESS_KEY} region=us-west-1
Success! Data written to: aws/config/root
vault-plugin-database-redis $
```

https://learn.hashicorp.com/tutorials/vault/getting-started-dynamic-secrets?in=vault/getting-started

```bash
vault-plugin-database-redis $ vault write aws/roles/my-role \
>         credential_type=iam_user \
>         policy_document=-<<EOF
> {
>   "Version": "2012-10-17",
>   "Statement": [
>     {
>       "Sid": "Stmt1426528957000",
>       "Effect": "Allow",
>       "Action": [
>         "ec2:*"
>       ],
>       "Resource": [
>         "*"
>       ]
>     }
>   ]
> }
> EOF
Success! Data written to: aws/roles/my-role
vault-plugin-database-redis $ vault read aws/creds/my-role
Key                Value
---                -----
lease_id           aws/creds/my-role/cG3x76JxsDa4Qulq2iTZKlAF
lease_duration     768h
lease_renewable    true
access_key         AKIAW2RMPG236W4XZICW
secret_key         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
security_token     <nil>
vault-plugin-database-redis $ vault read aws/creds/my-role
Key                Value
---                -----
lease_id           aws/creds/my-role/ZbDI6TuCyKvMkPauoGI2ciNc
lease_duration     768h
lease_renewable    true
access_key         AKIAW2RMPG234PKYPDBI
secret_key         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
security_token     <nil>
vault-plugin-database-redis $ vault read aws/creds/my-role
Key                Value
---                -----
lease_id           aws/creds/my-role/KFyIpuKffKXZLtDU8r2SlvDl
lease_duration     768h
lease_renewable    true
access_key         AKIAW2RMPG23UF55EZ4Z
secret_key         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
security_token     <nil>
vault-plugin-database-redis $ vault lease revoke aws/creds/my-role/cG3x76JxsDa4Qulq2iTZKlAF
All revocation operations queued successfully!
vault-plugin-database-redis $ vault lease revoke aws/creds/my-role/ZbDI6TuCyKvMkPauoGI2ciNc
All revocation operations queued successfully!
vault-plugin-database-redis $ vault lease revoke aws/creds/my-role/ZbDI6TuCyKvMkPauoGI2ciNc
All revocation operations queued successfully!
vault-plugin-database-redis $ vault lease revoke aws/creds/my-role/ZbDI6TuCyKvMkPauoGI2ciNc
All revocation operations queued successfully!
vault-plugin-database-redis $ vault lease revoke aws/creds/my-role/KFyIpuKffKXZLtDU8r2SlvDl
All revocation operations queued successfully!
vault-plugin-database-redis $ vault lease revoke aws/creds/my-role/KFyIpuKffKXZLtDU8r2SlvDl
All revocation operations queued successfully!
vault-plugin-database-redis $ 
```

```bash
vault-plugin-database-redis $ vault server -dev
==> Vault server configuration:

             Api Address: http://127.0.0.1:8200
                     Cgo: disabled
         Cluster Address: https://127.0.0.1:8201
              Go Version: go1.16.7
              Listener 1: tcp (addr: "127.0.0.1:8200", cluster address: "127.0.0.1:8201", max_request_duration: "1m30s", max_request_size: "33554432", tls: "disabled")
               Log Level: info
                   Mlock: supported: false, enabled: false
           Recovery Mode: false
                 Storage: inmem
                 Version: Vault v1.8.2
             Version Sha: aca76f63357041a43b49f3e8c11d67358496959f

==> Vault server started! Log data will stream in below:

2021-09-26T18:13:13.540+0530 [INFO]  proxy environment: http_proxy="" https_proxy="" no_proxy=""
2021-09-26T18:13:13.541+0530 [WARN]  no `api_addr` value specified in config or in VAULT_API_ADDR; falling back to detection if possible, but this value should be manually set
2021-09-26T18:13:13.542+0530 [INFO]  core: security barrier not initialized
2021-09-26T18:13:13.543+0530 [INFO]  core: security barrier initialized: stored=1 shares=1 threshold=1
2021-09-26T18:13:13.544+0530 [INFO]  core: post-unseal setup starting
2021-09-26T18:13:13.554+0530 [INFO]  core: loaded wrapping token key
2021-09-26T18:13:13.554+0530 [INFO]  core: successfully setup plugin catalog: plugin-directory=""
2021-09-26T18:13:13.554+0530 [INFO]  core: no mounts; adding default mount table
2021-09-26T18:13:13.555+0530 [INFO]  core: successfully mounted backend: type=cubbyhole path=cubbyhole/
2021-09-26T18:13:13.556+0530 [INFO]  core: successfully mounted backend: type=system path=sys/
2021-09-26T18:13:13.556+0530 [INFO]  core: successfully mounted backend: type=identity path=identity/
2021-09-26T18:13:13.558+0530 [INFO]  core: successfully enabled credential backend: type=token path=token/
2021-09-26T18:13:13.558+0530 [INFO]  rollback: starting rollback manager
2021-09-26T18:13:13.558+0530 [INFO]  core: restoring leases
2021-09-26T18:13:13.559+0530 [INFO]  identity: entities restored
2021-09-26T18:13:13.559+0530 [INFO]  identity: groups restored
2021-09-26T18:13:13.559+0530 [INFO]  expiration: lease restore complete
2021-09-26T18:13:13.560+0530 [INFO]  core: post-unseal setup complete
2021-09-26T18:13:13.561+0530 [INFO]  core: root token generated
2021-09-26T18:13:13.561+0530 [INFO]  core: pre-seal teardown starting
2021-09-26T18:13:13.561+0530 [INFO]  rollback: stopping rollback manager
2021-09-26T18:13:13.561+0530 [INFO]  core: pre-seal teardown complete
2021-09-26T18:13:13.561+0530 [INFO]  core.cluster-listener.tcp: starting listener: listener_address=127.0.0.1:8201
2021-09-26T18:13:13.561+0530 [INFO]  core.cluster-listener: serving cluster requests: cluster_listen_address=127.0.0.1:8201
2021-09-26T18:13:13.561+0530 [INFO]  core: post-unseal setup starting
2021-09-26T18:13:13.561+0530 [INFO]  core: loaded wrapping token key
2021-09-26T18:13:13.561+0530 [INFO]  core: successfully setup plugin catalog: plugin-directory=""
2021-09-26T18:13:13.561+0530 [INFO]  core: successfully mounted backend: type=system path=sys/
2021-09-26T18:13:13.562+0530 [INFO]  core: successfully mounted backend: type=identity path=identity/
2021-09-26T18:13:13.562+0530 [INFO]  core: successfully mounted backend: type=cubbyhole path=cubbyhole/
2021-09-26T18:13:13.563+0530 [INFO]  core: successfully enabled credential backend: type=token path=token/
2021-09-26T18:13:13.563+0530 [INFO]  rollback: starting rollback manager
2021-09-26T18:13:13.563+0530 [INFO]  core: restoring leases
2021-09-26T18:13:13.564+0530 [INFO]  identity: entities restored
2021-09-26T18:13:13.564+0530 [INFO]  identity: groups restored
2021-09-26T18:13:13.564+0530 [INFO]  expiration: lease restore complete
2021-09-26T18:13:13.564+0530 [INFO]  core: post-unseal setup complete
2021-09-26T18:13:13.564+0530 [INFO]  core: vault is unsealed
2021-09-26T18:13:13.568+0530 [INFO]  core: successful mount: namespace="" path=secret/ type=kv
2021-09-26T18:13:13.579+0530 [INFO]  secrets.kv.kv_4f51ea2f: collecting keys to upgrade
2021-09-26T18:13:13.579+0530 [INFO]  secrets.kv.kv_4f51ea2f: done collecting keys: num_keys=1
2021-09-26T18:13:13.579+0530 [INFO]  secrets.kv.kv_4f51ea2f: upgrading keys finished
WARNING! dev mode is enabled! In this mode, Vault runs entirely in-memory
and starts unsealed with a single unseal key. The root token is already
authenticated to the CLI, so you can immediately begin using Vault.

You may need to set the following environment variable:

    $ export VAULT_ADDR='http://127.0.0.1:8200'

The unseal key and root token are displayed below in case you want to
seal/unseal the Vault or re-authenticate.

Unseal Key: XLHDruvo65AZI/OMPR94BOnrrN++CaAzK2qDf3ZGopI=
Root Token: s.UGoNOBbmEDKGy2OZToN2DQWP

Development mode should NOT be used in production installations!


2021-09-26T18:30:25.533+0530 [INFO]  core: successful mount: namespace="" path=kv/ type=kv
2021-09-26T18:30:26.758+0530 [ERROR] secrets.system.system_d3eaf0d9: error occurred during enable mount: path=kv/ error="path is already in use at kv/"
2021-09-26T18:30:34.829+0530 [ERROR] secrets.system.system_d3eaf0d9: error occurred during enable mount: path=kv/ error="path is already in use at kv/"
2021-09-26T18:30:37.236+0530 [ERROR] secrets.system.system_d3eaf0d9: error occurred during enable mount: path=kv/ error="path is already in use at kv/"
2021-09-26T18:30:42.305+0530 [INFO]  core: successful mount: namespace="" path=keyvalue/ type=kv
2021-09-26T20:14:45.409+0530 [INFO]  core: successful mount: namespace="" path=aws/ type=aws

2021-09-26T20:31:01.997+0530 [INFO]  expiration: revoked lease: lease_id=aws/creds/my-role/cG3x76JxsDa4Qulq2iTZKlAF
2021-09-26T20:31:07.066+0530 [INFO]  expiration: revoked lease: lease_id=aws/creds/my-role/ZbDI6TuCyKvMkPauoGI2ciNc

2021-09-26T20:31:25.760+0530 [INFO]  expiration: revoked lease: lease_id=aws/creds/my-role/KFyIpuKffKXZLtDU8r2SlvDl

```

https://learn.hashicorp.com/tutorials/vault/getting-started-help

```bash
vault-plugin-database-redis $ vault path-help aws
## DESCRIPTION

The AWS backend dynamically generates AWS access keys for a set of
IAM policies. The AWS access keys have a configurable lease set and
are automatically revoked at the end of the lease.

After mounting this backend, credentials to generate IAM keys must
be configured with the "root" path and policies must be written using
the "roles/" endpoints before any access keys can be generated.

## PATHS

The following paths are supported by this backend. To view help for
any of the paths below, use the help command with any route matching
the path pattern. Note that depending on the policy of your auth token,
you may or may not be able to access certain paths.

    ^(creds|sts)/(?P<name>\w(([\w-.@]+)?\w)?)$
        Generate AWS credentials from a specific Vault role.

    ^config/lease$
        Configure the default lease information for generated credentials.

    ^config/root$
        Configure the root credentials that are used to manage IAM.

    ^config/rotate-root$
        Request to rotate the AWS credentials used by Vault

    ^roles/(?P<name>\w(([\w-.@]+)?\w)?)$
        Read, write and reference IAM policies that access keys can be made for.

    ^roles/?$
        List the existing roles in this backend
vault-plugin-database-redis $ vault read aws/config/lease
No value found at aws/config/lease
vault-plugin-database-redis $ vault read aws/config
No value found at aws/config
vault-plugin-database-redis $ vault read aws
No value found at aws
vault-plugin-database-redis $ vault read 
vault-plugin-database-redis $ vault path-help aws/creds/blah
Request:        creds/blah
Matching Route: ^(creds|sts)/(?P<name>\w(([\w-.@]+)?\w)?)$

Generate AWS credentials from a specific Vault role.

## PARAMETERS

    name (string)

        Name of the role

    role_arn (string)

        ARN of role to assume when credential_type is assumed_role

    role_session_name (string)

        Session name to use when assuming role. Max chars: 64

    ttl (duration (sec))

        Lifetime of the returned credentials in seconds

## DESCRIPTION

This path will generate new, never before used AWS credentials for
accessing AWS. The IAM policy used to back this key pair will be
the "name" parameter. For example, if this backend is mounted at "aws",
then "aws/creds/deploy" would generate access keys for the "deploy" role.

The access keys will have a lease associated with them. The access keys
can be revoked by using the lease ID when using the iam_user credential type.
When using AWS STS credential types (assumed_role or federation_token),
revoking the lease does not revoke the access keys.
vault-plugin-database-redis $ vault path-help aws/st/blah
Error retrieving help: Error making API request.

URL: GET http://127.0.0.1:8200/v1/aws/st/blah?help=1
Code: 404. Errors:

* 1 error occurred:
	* unsupported path


vault-plugin-database-redis $ vault path-help aws/sts/blah
Request:        sts/blah
Matching Route: ^(creds|sts)/(?P<name>\w(([\w-.@]+)?\w)?)$

Generate AWS credentials from a specific Vault role.

## PARAMETERS

    name (string)

        Name of the role

    role_arn (string)

        ARN of role to assume when credential_type is assumed_role

    role_session_name (string)

        Session name to use when assuming role. Max chars: 64

    ttl (duration (sec))

        Lifetime of the returned credentials in seconds

## DESCRIPTION

This path will generate new, never before used AWS credentials for
accessing AWS. The IAM policy used to back this key pair will be
the "name" parameter. For example, if this backend is mounted at "aws",
then "aws/creds/deploy" would generate access keys for the "deploy" role.

The access keys will have a lease associated with them. The access keys
can be revoked by using the lease ID when using the iam_user credential type.
When using AWS STS credential types (assumed_role or federation_token),
revoking the lease does not revoke the access keys.
vault-plugin-database-redis $ 
```

---

```bash
vault-plugin-database-redis $ vault server -dev
==> Vault server configuration:

             Api Address: http://127.0.0.1:8200
                     Cgo: disabled
         Cluster Address: https://127.0.0.1:8201
              Go Version: go1.16.7
              Listener 1: tcp (addr: "127.0.0.1:8200", cluster address: "127.0.0.1:8201", max_request_duration: "1m30s", max_request_size: "33554432", tls: "disabled")
               Log Level: info
                   Mlock: supported: false, enabled: false
           Recovery Mode: false
                 Storage: inmem
                 Version: Vault v1.8.2
             Version Sha: aca76f63357041a43b49f3e8c11d67358496959f

==> Vault server started! Log data will stream in below:

2021-09-26T18:13:13.540+0530 [INFO]  proxy environment: http_proxy="" https_proxy="" no_proxy=""
2021-09-26T18:13:13.541+0530 [WARN]  no `api_addr` value specified in config or in VAULT_API_ADDR; falling back to detection if possible, but this value should be manually set
2021-09-26T18:13:13.542+0530 [INFO]  core: security barrier not initialized
2021-09-26T18:13:13.543+0530 [INFO]  core: security barrier initialized: stored=1 shares=1 threshold=1
2021-09-26T18:13:13.544+0530 [INFO]  core: post-unseal setup starting
2021-09-26T18:13:13.554+0530 [INFO]  core: loaded wrapping token key
2021-09-26T18:13:13.554+0530 [INFO]  core: successfully setup plugin catalog: plugin-directory=""
2021-09-26T18:13:13.554+0530 [INFO]  core: no mounts; adding default mount table
2021-09-26T18:13:13.555+0530 [INFO]  core: successfully mounted backend: type=cubbyhole path=cubbyhole/
2021-09-26T18:13:13.556+0530 [INFO]  core: successfully mounted backend: type=system path=sys/
2021-09-26T18:13:13.556+0530 [INFO]  core: successfully mounted backend: type=identity path=identity/
2021-09-26T18:13:13.558+0530 [INFO]  core: successfully enabled credential backend: type=token path=token/
2021-09-26T18:13:13.558+0530 [INFO]  rollback: starting rollback manager
2021-09-26T18:13:13.558+0530 [INFO]  core: restoring leases
2021-09-26T18:13:13.559+0530 [INFO]  identity: entities restored
2021-09-26T18:13:13.559+0530 [INFO]  identity: groups restored
2021-09-26T18:13:13.559+0530 [INFO]  expiration: lease restore complete
2021-09-26T18:13:13.560+0530 [INFO]  core: post-unseal setup complete
2021-09-26T18:13:13.561+0530 [INFO]  core: root token generated
2021-09-26T18:13:13.561+0530 [INFO]  core: pre-seal teardown starting
2021-09-26T18:13:13.561+0530 [INFO]  rollback: stopping rollback manager
2021-09-26T18:13:13.561+0530 [INFO]  core: pre-seal teardown complete
2021-09-26T18:13:13.561+0530 [INFO]  core.cluster-listener.tcp: starting listener: listener_address=127.0.0.1:8201
2021-09-26T18:13:13.561+0530 [INFO]  core.cluster-listener: serving cluster requests: cluster_listen_address=127.0.0.1:8201
2021-09-26T18:13:13.561+0530 [INFO]  core: post-unseal setup starting
2021-09-26T18:13:13.561+0530 [INFO]  core: loaded wrapping token key
2021-09-26T18:13:13.561+0530 [INFO]  core: successfully setup plugin catalog: plugin-directory=""
2021-09-26T18:13:13.561+0530 [INFO]  core: successfully mounted backend: type=system path=sys/
2021-09-26T18:13:13.562+0530 [INFO]  core: successfully mounted backend: type=identity path=identity/
2021-09-26T18:13:13.562+0530 [INFO]  core: successfully mounted backend: type=cubbyhole path=cubbyhole/
2021-09-26T18:13:13.563+0530 [INFO]  core: successfully enabled credential backend: type=token path=token/
2021-09-26T18:13:13.563+0530 [INFO]  rollback: starting rollback manager
2021-09-26T18:13:13.563+0530 [INFO]  core: restoring leases
2021-09-26T18:13:13.564+0530 [INFO]  identity: entities restored
2021-09-26T18:13:13.564+0530 [INFO]  identity: groups restored
2021-09-26T18:13:13.564+0530 [INFO]  expiration: lease restore complete
2021-09-26T18:13:13.564+0530 [INFO]  core: post-unseal setup complete
2021-09-26T18:13:13.564+0530 [INFO]  core: vault is unsealed
2021-09-26T18:13:13.568+0530 [INFO]  core: successful mount: namespace="" path=secret/ type=kv
2021-09-26T18:13:13.579+0530 [INFO]  secrets.kv.kv_4f51ea2f: collecting keys to upgrade
2021-09-26T18:13:13.579+0530 [INFO]  secrets.kv.kv_4f51ea2f: done collecting keys: num_keys=1
2021-09-26T18:13:13.579+0530 [INFO]  secrets.kv.kv_4f51ea2f: upgrading keys finished
WARNING! dev mode is enabled! In this mode, Vault runs entirely in-memory
and starts unsealed with a single unseal key. The root token is already
authenticated to the CLI, so you can immediately begin using Vault.

You may need to set the following environment variable:

    $ export VAULT_ADDR='http://127.0.0.1:8200'

The unseal key and root token are displayed below in case you want to
seal/unseal the Vault or re-authenticate.

Unseal Key: XLHDruvo65AZI/OMPR94BOnrrN++CaAzK2qDf3ZGopI=
Root Token: s.UGoNOBbmEDKGy2OZToN2DQWP

Development mode should NOT be used in production installations!


2021-09-26T18:30:25.533+0530 [INFO]  core: successful mount: namespace="" path=kv/ type=kv
2021-09-26T18:30:26.758+0530 [ERROR] secrets.system.system_d3eaf0d9: error occurred during enable mount: path=kv/ error="path is already in use at kv/"
2021-09-26T18:30:34.829+0530 [ERROR] secrets.system.system_d3eaf0d9: error occurred during enable mount: path=kv/ error="path is already in use at kv/"
2021-09-26T18:30:37.236+0530 [ERROR] secrets.system.system_d3eaf0d9: error occurred during enable mount: path=kv/ error="path is already in use at kv/"
2021-09-26T18:30:42.305+0530 [INFO]  core: successful mount: namespace="" path=keyvalue/ type=kv
2021-09-26T20:14:45.409+0530 [INFO]  core: successful mount: namespace="" path=aws/ type=aws

2021-09-26T20:31:01.997+0530 [INFO]  expiration: revoked lease: lease_id=aws/creds/my-role/cG3x76JxsDa4Qulq2iTZKlAF
2021-09-26T20:31:07.066+0530 [INFO]  expiration: revoked lease: lease_id=aws/creds/my-role/ZbDI6TuCyKvMkPauoGI2ciNc

2021-09-26T20:31:25.760+0530 [INFO]  expiration: revoked lease: lease_id=aws/creds/my-role/KFyIpuKffKXZLtDU8r2SlvDl
^C==> Vault shutdown triggered
2021-09-26T20:42:44.014+0530 [INFO]  core: marked as sealed
2021-09-26T20:42:44.014+0530 [INFO]  core: pre-seal teardown starting
2021-09-26T20:42:44.014+0530 [INFO]  rollback: stopping rollback manager
2021-09-26T20:42:44.014+0530 [INFO]  core: pre-seal teardown complete
2021-09-26T20:42:44.014+0530 [INFO]  core: stopping cluster listeners
2021-09-26T20:42:44.014+0530 [INFO]  core.cluster-listener: forwarding rpc listeners stopped
2021-09-26T20:42:44.033+0530 [INFO]  core.cluster-listener: rpc listeners successfully shut down
2021-09-26T20:42:44.033+0530 [INFO]  core: cluster listeners successfully shut down
2021-09-26T20:42:44.033+0530 [INFO]  core: vault is sealed
```

