# Buildkit

[Buildkit](https://github.com/civicrm/civicrm-buildkit) is a collection of ~20 tools for developing and testing CiviCRM, the most important of which is [civibuild](/tools/civibuild.md).

Many of these tools are commonly used by web developers, so you may have already installed a few. Even so, it's generally easier to install the full collection &mdash; installing each individually takes a lot of work.

This is the same collection of tools which manages the test/demo/release infrastructure for civicrm.org.


## Installation

### Ubuntu

If you have a new installation of Ubuntu 12.04 or 14.04, then you can download
everything -- buildkit and the system requirements -- with one command. This
command will install buildkit to `~/buildkit`:

```bash
curl -Ls https://civicrm.org/get-buildkit.sh | bash -s -- --full --dir ~/buildkit
```

Note:

 * When executing the above command, you should *not* run as `root`. However, you *should*
have `sudo` permissions.
 * The `--full` option is *very opinionated*; it specifically installs `php`, `apache`, and `mysql` (rather than `hvm`, `nginx`, `lighttpd`, or `percona`). If you try to mix `--full` with alternative systems, then expect conflicts.
 * If you use the Ubuntu feature for "encrypted home directories", then don't put buildkit in `~/buildkit`. Consider `/opt/buildkit`, `/srv/buildkit`, or some other location that remains available during reboot.

### Vagrant

[Full Download: Vagrantbox](https://github.com/civicrm/civicrm-buildkit-vagrant) - Download a prepared virtual-machine with all system dependencies (mysql, etc). This is ideal if you work on Windows or OS X.


### Docker

If you have [Docker](https://www.docker.com/) running, you can [use this Docker image](https://github.com/ErichBSchulz/dcbk) to run buildkit.



### Other platforms

You may install buildkit in other environments. The main pre-requisites are:

* Linux or OS X
* Git
* PHP 5.3+ (Extensions: `bcmath curl gd gettext imap intl imagick json mbstring mcrypt openssl pdo_mysql phar posix soap zip`)
* NodeJS (v5 recommended)
* NPM
* Recommended (_for [amp](https://github.com/totten/amp) and [civibuild](/tools/civibuild.md)_)
    * Apache 2.2 or 2.4 (Modules: `mod_rewrite`. On SUSE, possibly `mod_access_compat`. This list may not be exhaustive.)
    * MySQL 5.1+ (client and server)

All pre-requisites must support command-line access using the standard command
names (`git`, `php`, `node`, `mysql`, `mysqldump`, etc). In some environments,
you may need to enable these commands by configuring `PATH` -- this is especially
true for MAMP, XAMPP, and other downloaded packages.
(See, e.g., [Setup Command-Line PHP](http://wiki.civicrm.org/confluence/display/CRMDOC/Setup+Command-Line+PHP).)

Once the pre-requisites are met, download buildkit to `~/buildkit`:

```bash
git clone https://github.com/civicrm/civicrm-buildkit.git ~/buildkit
cd ~/buildkit
./bin/civi-download-tools
```

### Troubleshooting

* Nodejs version too old or npm update does not work

Download the latest version from nodejs.org and follow to their instructions

* Nodejs problems

It might be handy to run

```bash
npm update
npm install fs-extra
```

## Applying a patch

Using buildkit, you can create a CiviCRM environment with the PR applied.

For Example:

```bash
civibuild create dmaster \
  --url http://localhost:8001 \
  --patch https://github.com/civicrm/civicrm-core/pull/8494 \
  --admin-pass s3cr3t
```

This will create a test environment with the Drupal, CiviCRM master branch
and the patch in PR 8494. More detailed information is in the
[Civibuild documentation](https://github.com/civicrm/civicrm-buildkit/blob/master/doc/civibuild.md)

## Configuring buildkit after installation {:#configuring}

!!! note "Not needed for Vagrant/Docker installations"
    If you set up buildkit using Vagrant or Docker, then you don't need to perform the configuration steps listed here.

Buildkit includes many CLI commands in the `bin/` folder.

You may execute the commands directly (e.g.  `./bin/civix` or `/path/to/buildkit/bin/civix`).  However, this would
become very cumbersome.  Instead, you should configure the shell's `PATH` to recognize these commands automatically.

!!! tip
    Throughout this document, we will provide examples which assume that buildkit was downloaded to `/path/to/buildkit`. Be sure to adjust the examples to match your system.

If you want to ensure that the buildkit CLI tools are always available, then:

1. Determine the location of your shell configuration file. This is usually `~/.bashrc`, `~/.bash_profile`, or
`~/.profile`.
1. At the end of the file, add `export PATH="/path/to/buildkit/bin:$PATH"`
1. Close and reopen the terminal.
1. Enter the command `which civibuild`. This should display a full-path. If nothing appears, then retry the steps.


!!! note

    Buildkit includes specific versions of some fairly popular tools (such as `drush`, `phpunit`, and `wp-cli`), and it's possible that you have already installed other versions of these tools.

    By design, buildkit can coexist with other tools, but you must manually manage the `PATH`.

    Whenever you wish to use buildkit, manually run a command like, e.g.:

    ```bash
    export PATH=/path/to/buildkit/bin:$PATH
    ```

    To restore your normal `PATH`, simply close the terminal and open a new one.

    Each time you open a new terminal while working on Civi development, you would need to re-run the `export` command.



## Upgrading buildkit {:#upgrading}

New versions of buildkit are likely to include new versions of tools. The
new tools will download automatically when you first run `civibuild`.
If you prefer to download explicitly, then re-run `civi-download-tools`.

The configurations and tools in buildkit are periodically updated. To get the latest, simply run:

```bash
cd ~/buildkit
git pull
./bin/civi-download-tools
```

See the [buildkit changelog](https://github.com/civicrm/civicrm-buildkit/blob/master/CHANGELOG.md) for info about specific changes to buildkit.


