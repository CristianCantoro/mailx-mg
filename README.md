# mailx-mg

mailx-mg — send mail like mailx via Mailgun

## Description

mailx-mg is a script that emulates `mail`/`mailx` using
[Mailgun](https://mailgun.com/) to send emails.

## Usage
```
Usage:
     mailx-mg [-dEv] [-C <config-file>] [-s <subject>] [-a <attachment>]
              [-c <cc-addrs>] [-b <bcc-addrs>] [-r <from-addr>] <to-addr>...
     mailx-mg ( -h | --help )
     mailx-mg ( -V | --version )
```

## Options:
    -a attachment
        Attach the given file to the message.

    -b bcc-addrs
        Send blind carbon copies to list. List should be a comma-separated
        list of names.

    -c cc-addrs
        Send carbon copies to list of users.

    -C config-file
        Specify config file [default: /etc/mailx-mg/config]

    -d
        Enables debugging messages and disables the actual delivery of messages.
        Unlike -v, this option is intended for mailx-mg development only.

    -E
        If an outgoing message does not contain any text in its first or only
        message part, do not send it but discard it silently, effectively
        setting the skipemptybody variable at program startup. This is useful
        for sending messages from scripts started by cron(8).

    -H
        Print header summaries for all messages and exit.

    -r from-addr
        Sets the From address. Overrides any from variable specified in
        environment or startup files. Tilde escapes are disabled. The -r
        address options are passed to the mail transfer agent unless SMTP is
        used. This option exists for compatibility only; it is recommended to
        set the from variable directly instead.

    -s subject
        Specify subject on command line (only the first argument after the -s
        flag is used as a subject; be careful to quote subjects containing
        spaces).

    -v
        Verbose mode. The details of delivery are displayed on the user's
        terminal.

    -V
        Print mailx's version and exit. 

## Installation & dependencies

1. You need to have [docopts](https://github.com/docopt/docopts) installed to
run mailx-mg

2. Clone the repo
```
git clone https://github.com/CristianCantoro/mailx-mg.git
```

3. Copy the sample configuration to `/etc/mailx-mg/config`
```
sudo mkdir -p /etc/mailx-mg/
sudo cp ./mailx-mg/config.example /etc/mailx-mg/config
```

4. Edit the config with your favorite editor.
```
MAILGUN_API_VERSION=v3
MAILGUN_API_KEY=<your_api_key>          # start with 'key-aaaaaa...'
MAILGUN_DOMAIN=<your_mailgun_domain>
```
You can obtain you `MAILGUN_API_KEY` for your `MAILGUN_DOMAIN` from your
dashboard on Mailgun's website.

5. Copy the executable 

### Example usage

The content of the e-mail can be read from standard input.

In the following example:

* your `MAILGUN_DOMAIN` is `mg.example.org` and you want to send an e-mail from
  `postmaster@mg.example.org`;
* you want to send an e-mail to `foo@mydomain.com`;
* the text of your e-mail is saved in a file named `text.txt`;

```bash
$ mailx-mg -r 'Mailgun <postmaster@mg.example.org>' \
           -s "Testing mail-mg" \
            foo@mydomain.com \
            < test.txt
```