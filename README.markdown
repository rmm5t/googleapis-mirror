# Google Library APIs Mirror

This is a subset mirror of the [Google Libraries
API](http://code.google.com/apis/libraries/devguide.html) CDN meant to be served
from your local development machine when internet access is lacking.

## Instructions

**Step 0:** Clone this repository

    git clone git://github.com/rmm5t/googleapis-mirror.git

**Step 1:** Run `rake sync` to download a copy of all the libraries listed in
  `libraries.txt`.

  _You'll probably want to run this step before you lose internet access._

**Step 2:** Run `sudo rake serve` or just `sudo rake` (serve is the default
  task). This binds a new virtual IP address (172.16.88.88) to the loopback
  interface, and maps `ajax.googleapis.com` to it using the OS X Directory
  Service.  It also starts a web server bound to the new virtual IP address such
  that http://ajax.googleapis.com/ behaves like a local mirror for the Google
  Libraries.

**NOTE: You must run this as sudo.**  To stop the local web server mirror, just `Ctrl-C` the rake process.

## Alternatives

If you aren't on OS X, you can alternatively map ajax.googleapis.com to
127.0.0.1 using `/etc/hosts` or any equivalent.  You will also need to create a
virtual host on your local web server to serve ajax.googleapis.com.  Here's an
example for Apache:

    <VirtualHost *:80>
      ServerName ajax.googleapis.com
      DocumentRoot "/path/to/googleapis-mirror"
      <Directory "/path/to/googleapis-mirror">
         Options Indexes
         Order allow,deny
        Allow from all
      </Directory>
    </VirtualHost>

## Author

[Ryan McGeary](http://ryan.mcgeary.org) ([@rmm5t](http://twitter.com/rmm5t))

## License

[MIT License](https://rmm5t.mit-license.org/)
