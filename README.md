# p3mail - Send terminal outputs by mail
p3mail (Pathetic Python Pipe Mail) allows you to send terminal outputs by mail

Basically something like this

``` bash
./long_running_command_printing_logs | p3mail "My mail's subject"
```

Since it is highly probable that you are both the sender and receiver, these latter are
pre-defined in a config file (see below).

**Important note** Remember that only stdout is captured when using pipes. You may want
to redirect stderr to stdout to also catch errors. For example
``` bash
./my_command_printing_errors 2>&1 | p3mail "My mail's subject"
```
Note: the order of stdout and stderr outputs is not preserved :( See: https://hisham.hm/2016/11/24/fun-hack-to-redirect-stdout-and-stderr-in-order/

## Install

```
pip install p3mail
```

## Usage

### Using `config` file

Copy the `config_example` file to `$HOME/.config/p3mail/config` and start using p3mail !
See the examples below.

### Using command line
```
usage: p3mail [-h] [--to TO] [--from DEST] [--config CONFIG] subject

positional arguments:
  subject          The subject of the mail

optional arguments:
  -h, --help       show this help message and exit
  --to TO          The receiver's address. If not provided, it will look at
                   the config file
  --from DEST      The sender's address. If not provided, it will look at the
                   config file
  --config CONFIG  Specify a custom location for the config file
```

## Examples

### Send backup logs by mail
``` bash
my_backup_program | p3mail "Backup for `hostname` on `date`"
```

### Cronjob notification
If your cron job is ran by `root` p3mail will look at `/home/root/.config/p3mail/config`
which is not something you might want. You can specify the `config` file location with:
``` bash
echo "Hello msg body" | p3mail --config /home/alice/.config/p3mail/config "My subject"
```

Tip: you can put an alias in your `.bashrc` to avoid specifying this file everytime
like this:
```
p3mail="p3mail --config /home/alice/.config/p3mail/config"
```

## Motivation

I coded this application because it is always painful (at least for me) to make
the built-in mail service work (or whatever it is called on Linux distributions).
Here are some other reasons:

  1. You don't always have the rights to configure this mail service (e.g. on
  a remote server)
  2. `sendmail`, `mail` or other mail sender command line tools are not
  always installed or configured on the machines you are using
  3. p3mail only requires that Python 3 is installed
  4. p3mail does not requires sudo rights
  5. p3mail needs a reason to live

## Security concerns

You may not want to hardcode your smtp user or your smtp password in a config file. I understand that so p3mail allows you to read your credentials from environment variables.

1. In your `config` set as an empty value for either/both `SmtpUser`/`SmtpPass` e.g. `SmtpPass = `
2. Then call `p3mail` like this:
```bash
export SMTP_USER=alice
export SMTP_PASS=super_secr3t_passw0rd

./my_verbose_script | p3mail "Verbose script report"
```

# TODO

* Add Gmail oauth support

