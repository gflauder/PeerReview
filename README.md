# PyriteView

Article versioning hub for peer-reviewed publications.

This simple hub allows publication editors, single edition editors, article authors as well as third-party reviewers to come together to centralize the peer-reviewing workflow.

### Why the name "Pyrite"?

"PyriteView" is a play on the words "Peer Review".  The [PyritePHP](https://github.com/vphantom/pyrite-php) framework which it uses was originally created specifically for this application.

### CURRENTLY UNDER ACTIVE DEVELOPMENT!

As of December 2016, PyriteView is under active development towards its initial features list.  You should probably wait until we release version 1.0 before looking too much into this.

## Usage

TODO


## Installation

### Requirements

* PHP 5.x or later
* PHP's `mcrypt` extension module
* PHP's `pdo_sqlite` extension module
* SQLite 3
* Typical Linux command line tools: wget, gzip
* Typical Linux command line tools: make, wget, gzip
* A web server of course

### Web Server Configuration

In order to produce clean, technology-agnostic URLs such as `http://www.yourdomain.com/articles/127`, you need to tell your web server to internally redirect requests for non-existent files to `/index.php`, which will look in `PATH_INFO` for details.  We also want to prevent access to private files.

Here are sample configurations for major server software:

#### Apache

```
RewriteEngine on

RewriteRule ^(bin|lib|modules|node_modules|templates|var|vendor) - [F,L,NC]

RewriteRule ^$ /index.php [L]

RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.+) /index.php/$1 [L]
```

#### Nginx

```
location ~ /(bin|lib|modules|node_modules|templates|var|vendor) {
    deny all;
    return 404;
}

location ~ \.php$ {
    fastcgi_pass 127.0.0.1:9000;
    fastcgi_param SCRIPT_FILENAME $document_root $fastcgi_script_name;
    include fastcgi_params;
}

location / {
    index index.html index.htm index.php;
    try_files $uri $uri/ $uri/index.php /index.php;
}
```

#### Lighttpd

```
# TODO: Deny private directories
url.rewrite-if-not-file (
    "^(.*)$" => "/index.php/$1"
    "^/(.*)$" => "/index.php/$1"
)
```

### First-time initialization

#### Download

Clone or unzip this repository into the document root of the web site this will become.  Alternatively, you can use Composer to create this root directory for you, for example if you want to create `foo/`:

```sh
$ composer create-project vphantom/pyriteview foo
```

#### Configure

Edit `config.ini` to change any defaults as needed.

#### Create directories and empty database

Run `make init`.  This will automatically download and set up PHP's Composer package manager, then use it to download runtime dependencies locally.  Finally, it will create the database tables and the administrative user so you can log into your new installation.  You will be prompted on the command line for an e-mail address and password to use for that unrestricted account.  (**NOTE:** This prompt requires PHP's `readline`, so *it will not work on Windows*.)

You will also need to make sure that your web server or PHP process has read-write access to the `var/` directory where the database, logs and template cache are stored.


## Updating

While the structure of the framework isn't upgradeable since it consisted of a simple starting point for your own application, the bulk of the core components is safely packaged in NPM and Packagist (Composer).  Therefore, `make update` should provide all the bug fixes you'll need down the road.


## Developers

See [Developers](DEVELOPERS.md).  For more information on the framework used to build this application, see related project [PyritePHP](https://github.com/vphantom/pyrite-php).


## GNU Affero GPL v3.0 License

Copyright (c) 2016 Stéphane Lavergne <https://github.com/vphantom>

The GNU Affero GPL license fits this end-user software well because it allows:

* Commercial use
* Modifications and creation of derivatives
* Distribution in original or modified form

...while prohibiting:

* Granting other licenses
* Liability to the license owner

...and requiring:

* Original copyright be retained in derivatives
* License be kept intact in derivatives
* Documenting changes made vs the original
* Keep source code available when distributing *or serving using a modified version* (the "Affero" bit)
* Include/keep installation instructions in derivatives

It protects the original copyright and keeps the project *and its derivatives* available, free and open source.

For full details, see [LICENSE.txt](LICENSE.txt).
