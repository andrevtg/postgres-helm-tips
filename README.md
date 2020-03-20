# postgres-helm-tips

Tips for using Bitnami's Postgres Helm Chart

## The Chart Itself

Bitnami Postgres Chart is pulled from bitnami repo:

```sh
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm upgrade -i my-release bitnami/postgresql
```

Destroying the installed release is also easy:

```sh
helm delete my-release
```

This chart is found and documented in [Helm Hub](https://hub.helm.sh/charts/bitnami/postgresql) too.

## Initializing database and passwords

In the first run you have a chance to set DB admin password and to create a new database (and its owner user). First define a `myvalues.yaml` file:

```yaml
# `my-values.yaml` contents
postgresqlPostgresPassword: newpwd # db admin
postgresqlDatabase: mynewdb    # a new db
postgresqlUsername: mynewuser  # mynewdb owner
postgresqlPassword: mynewpwd   # mynewuser's password
```

Then install a release with it:

```sh
helm upgrade -i my-release -f my-values.yaml bitnami/postgresql
```

## Init Db Scripts

Init DB scripts can be passed by Helm CLI, although it is not quite intuitive. Define a text file containing the SQL scripts:

```sql
CREATE DATABASE myotherdb WITH OWNER mynewuser;
```

Then install a release with it:

```sh
helm upgrade -i my-release bitnami/postgresql \
  -f my-values.yaml \
  --set-file initdbScripts."myinit\.sql"=./my-otherdb.sql
```
