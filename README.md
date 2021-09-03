# flysystem
Flysystem is a file storage library for PHP. It provides one interface to interact with many types of filesystems. When you use Flysystem, you're not only protected from vendor lock-in, you'll also have a consistent experience for which ever storage is right for you.
You can use Flysystem sftp adaptor for php. You can use this in any framework using composer installation. (Core Php,Codeigniter ,Laravel,PHP,Symphony YII ETC).

Flysystem Filesystem Abstraction for PHP (PHP library).
```
https://flysystem.thephpleague.com/v2/docs/
```
install the library using command.
```
composer require league/flysystem-sftp:^2.0
```
use below code to connect with SFTP using Filesystem.
````
<?php
require_once('vendor/autoload.php');
use League\Flysystem\Filesystem;
use League\Flysystem\PhpseclibV2\SftpConnectionProvider;
use League\Flysystem\PhpseclibV2\SftpAdapter;
use League\Flysystem\UnixVisibility\PortableVisibilityConverter;
use League\Flysystem\DirectoryAttributes;
use League\Flysystem\FileAttributes;
$private_key='sftp_keys/new_private.ppk';

$filesystem = new Filesystem(new SftpAdapter(
    new SftpConnectionProvider(
        'host', // host (required)
        'username', // username (required)
        null, // password (optional, default: null) set to null if privateKey is used
        $private_key, // private key (optional, default: null) can be used instead of password, set to null if password is set
        null, // passphrase (optional, default: null), set to null if privateKey is not used or has no passphrase
        22, // port (optional, default: 22)
        false, // use agent (optional, default: false)
        30, // timeout (optional, default: 10)
        10, // max tries (optional, default: 4)
        null, // host fingerprint (optional, default: null),
        null // connectivity checker (must be an implementation of 'League\Flysystem\PhpseclibV2\ConnectivityChecker' to check if a connection can be established (optional, omit if you don't need some special handling for setting reliable connections)
    ),
    '/', // root path (required)
    PortableVisibilityConverter::fromArray([
        'file' => [
            'public' => 0640,
            'private' => 0604,
        ],
        'dir' => [
            'public' => 0740,
            'private' => 7604,
        ],
    ])
));
$allFiles = $filesystem->listContents('Outbound')->toArray();
$response = $filesystem->write('Outbound/info.txt', 'Hello How are you',array());
if($response){
	echo "Success";
}else echo "Error";
print_r($allFiles );
?>
````
Composer.json looks like
```
{
    "name": "league/flysystem-sftp",
    "description": "Flysystem adapter for SFTP",
    "license": "MIT",
    "authors": [
        {
            "name": "Frank de Jonge",
            "email": "info@frenky.net"
        }
    ],
    "require": {
        "php": ">=5.6.0",
        "league/flysystem-sftp": "^2.1",
        "phpseclib/phpseclib": "~2.0"
    },
    "require-dev": {
        "mockery/mockery": "0.9.*",
        "phpunit/phpunit": "^5.7.25"
    },
    "autoload": {
        "psr-4": {
            "League\\Flysystem\\Sftp\\": "src/"
        }
    }
}
```

