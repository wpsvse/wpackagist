WordPress Packagist by WordPress Sweden
===

> This is a fork of [WPackagist.org](https://github.com/outlandishideas/wpackagist) that aims to provide an up-to-date and highly available alternative and to contribute patches upstream. Feel free to use our repository by changing your repository URL in `composer.json` to https://wpackagist.wpsv.se/

This is the repository for [wpackagist.wpsv.se](https://wpackagist.wpsv.se/) which allows WordPress plugins and themes to be
managed along with other dependencies using [Composer](https://getcomposer.org).

More info and usage instructions at [wpackagist.wpsv.se](https://wpackagist.wpsv.se/) or follow us on
Twitter [@WPSverige](https://twitter.com/wpackagist).

For support and discussion, please use the issue tracker above.

## Usage

Example composer.json:

```json
{
    "name": "acme/brilliant-wordpress-site",
    "description": "My brilliant WordPress site",
    "repositories":[
        {
            "type":"composer",
            "url":"https://wpackagist.wpsv.se/"
        }
    ],
    "require": {
        "aws/aws-sdk-php":"*",
        "wpackagist-plugin/akismet":"dev-trunk",
        "wpackagist-plugin/captcha":">=3.9",
        "wpackagist-theme/hueman":"*"
    },
    "autoload": {
        "psr-0": {
            "Acme": "src/"
        }
    }
}
```

## WordPress core

This does not provide WordPress itself.

See https://github.com/fancyguy/webroot-installer or https://github.com/johnpbloch/wordpress.

## Running Wpackagist

### Installing

1. Make sure you have PDO with sqlite support enabled.
2. Make sure [`data`](data/) is writable. Do NOT create `data/packages.sqlite`, it will be created automatically.
3. Run `composer install`.
4. Point your Web server to [`web`](web/). A [`.htaccess`](web/.htaccess) is provided for Apache.

### Updating the database

The first database fetch may easily take 30-60 minutes, be patient.

1. `bin/cmd refresh`: Refresh the list of plugins and themes to find new and updated packages.
2. `bin/cmd update`: Update the versions for packages identified in `1.`.
3. `bin/cmd build`: Rebuild all `.json` files in `web/`.

### CRON job

The CRON job runs the three commands above. We recommend fetching new packages every 30 minutes. Add this to your crontab to set that up:

```
0,30 * * * * cd /var/www/your-wpackagist-location && ./run-cron.sh >> /var/log/wpackagist-update.log
```
