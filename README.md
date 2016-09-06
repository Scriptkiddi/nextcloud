# nextcloud
Create a nextcloud server in CLC cloud

## Parameters
* datacenter # ex: GB3
* nextcloud_root # ex: data_nc
* nextcloud_path # ex: www
* nextcloud_dest # ex: nextcloud
* nextcloud_version # ex: 10.0.0
* ht_user # ex: www-data
* ht_group # ex: www-data
* mysqlpass # ex: "abc123!@#"

### DESCRIPTION:
This playbook is designed to be called by Runner and will provision an Ubuntu 14.04 server in CenturyLink Cloud and deploys a working instance of nextcloud to it.
The high level tasks are as follows:
* provision a group and server
* install and harden MySQL and create the nextcloud database
* install apache, postfix and some php packages
* make some directory structures, set some permissions & ownership and copy some config files to the server
* configure apache and postfix

### NOTES:
Because nextcloud is a sort of private dropbox it's probable that a disk (partition) will need to be added to the instance to serve as storage, to do this I would move /data_nc to a temporary location, add a 'partition' mounted to /data_nc then move the contents of the original /data_nc into your new partition.
Once the play is successful, hit the private IP at http://aa.bb.cc.dd/nextcloud

### PREREQUISITES
You must have a CenturyLink Cloud account to be able to use Runner

### ADDITIONAL STEPS
Once the Server has been configured there are some steps that need to be completed.  When you log into the http address previously mentioned it will ask for an initial login and going through that login process generates the hashed passwords and such like in the config.php file below.

Once that is done, change the http in that config.php file to become https and it should redirect you.
You can also add the public IP here to the 'array' section and I've also added some sample email config that works in CenturyLink Cloud.

### EXAMPLE config.php
```
<?php
$CONFIG = array (
  'instanceid' => 'xxxxxxxxxxxx',
  'passwordsalt' => 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxx',
  'secret' => 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx',
  'trusted_domains' =>
  array (
    0 => 'PRIVATEIPADDRESS',
    1 => 'PUBLICIPADDRESS',
  ),
  'datadirectory' => '/data_oc/www/nextcloud/data',
  'overwrite.cli.url' => 'https://IPADDRESS/nextcloud',
  'dbtype' => 'mysql',
  'version' => '9.0.2.2',
  'dbname' => 'nextcloud',
  'dbhost' => 'localhost',
  'dbtableprefix' => 'oc_',
  'dbuser' => 'oc_admin',
  'dbpassword' => 'BPBPUKXrJ51+AUvT4fUrMPW+FTvBPh',
  'logtimezone' => 'UTC',
  'installed' => true,
  'mail_smtpmode' => 'sendmail',
  'mail_from_address' => 'admin',
  'mail_smtphost' => 'relay@t3mx.com',
  'mail_smtpport' => '25',
  'mail_smtpauthtype' => 'PLAIN',
  'mail_smtpauth' => 1,
  'mail_domain' => 'clctest.com',
  'memcache.local' => '\OC\Memcache\APCu',
);
```
