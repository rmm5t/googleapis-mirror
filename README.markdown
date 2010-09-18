This is a subset mirror of the [Google Libraries
API](http://code.google.com/apis/libraries/devguide.html) CDN meant to be served
from your local development machine when internet access is lacking.

**Step 1:** Run `rake sync` to download a copy of all the libraries listed in
  `libraries.txt`.

**Step 2:** Create a virtual host on your development machine that points to a
  local copy of this repository.

Here's an example for Apache:

    <VirtualHost *:80>
      ServerName ajax.googleapis.com
      DocumentRoot "/path/to/googleapis-mirror"
      <Directory "/path/to/googleapis-mirror">
         Options Indexes
         Order allow,deny
        Allow from all
      </Directory>
    </VirtualHost>

**Step 3:** Run `rake serve` to map ajax.googleapis.com to 127.0.0.1 on OS X.
  Alternatively add a record to your `/etc/hosts` file:

    127.0.0.1 ajax.googleapis.com

To cancel the local hostname mapping, just `Ctrl-C` the `rake serve` or comment
the record in your `/etc/hosts` file.
