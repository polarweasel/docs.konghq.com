---
id: page-install-method
title: Install - macOS
header_title: macOS Installation
header_icon: /assets/images/icons/icn-installation.svg
breadcrumbs:
  Installation: /install
---

### Packages

- [Homebrew Formula]({{ site.repos.homebrew }})

----

### Installation

1. **Install Kong**

    Use the [Homebrew](https://brew.sh/) package manager to add Kong as a tap and install it:

    ```bash
    $ brew tap kong/kong
    $ brew install kong
    ```

1. **Configure Kong**

    Kong supports both [PostgreSQL {{site.data.kong_latest.dependencies.postgres}}](http://www.postgresql.org/) and [Cassandra {{site.data.kong_latest.dependencies.cassandra}}](http://cassandra.apache.org/) as its datastore.
    By default, Kong is configured to communicate with a local Postgres instance.
    If you are using Cassandra, or need to modify any other settings, you can configure Kong with either a `kong.conf` file, or by using environment variables.

    **To configure Kong with environment variables**, refer to the [Configuration Reference](/latest/configuration/#environment-variables).

    **To add a `kong.conf` file**, download the [`kong.conf.default`](https://raw.githubusercontent.com/Kong/kong/master/kong.conf.default) file and [adjust][configuration] it as necessary.
    Then, add it to `/usr/local/etc/kong`:

    ```bash
    $ mkdir -p /usr/local/etc/kong
    $ cp kong.conf.default /usr/local/etc/kong/kong.conf
    ```

    **Note**: When you start Kong, you'll need to use the `-c` [command-line option](/latest/cli/#kong-start) to specify the config file location.

1. **Prepare your database**

    If you are using Postgres, provision a database and a user before starting Kong:

    ```sql
    CREATE USER kong; CREATE DATABASE kong OWNER kong;
    ```

    Next, run the Kong migrations:

    ```bash
    $ kong migrations up [-c /path/to/kong.conf]
    ```

    **Note**: Migrations should never be run concurrently; only
    one Kong node should be performing migrations at a time.

1. **Start Kong**

    ```bash
    $ kong start [-c /path/to/kong.conf]
    ```

1. **Use Kong**

    Verify that Kong is running:

    ```bash
    $ curl -i http://localhost:8001/
    ```

    Quickly learn how to use Kong with the [5-minute Quickstart](/latest/getting-started/quickstart).

[configuration]: /{{site.data.kong_latest.release}}/configuration#database
