# MongoDB Support Scripts

These scripts are for supporting dev on MongoDB.

Main scripts:

* `setup-mongo-versions` - used for downloading various MongoDB versions.
* `mongopath` - used for generating the MONGOPATH environment variable from your downloaded versions
* `start-mongodb` - start a specific mongodb
* `stop-mongodb` - stop a running mongodb

Call these scripts from the root folder (the parent folder of this repo).

# Assumptions

These scripts make a couple of assumptions about the folder structure:

```
 Root Folder
 |
 +- bin
 |    (this repo - can actually be called anything)
 +- mongodb
 |    (storage for downloaded mongodb binaries)
 +- var
      (storage for the databases)
```

# Setup

Copy `script-config.example` to `script-config` and uncomment the correct
`DISTRO_PREFIX` for your os (or add the correct one as needed from
[here](https://www.mongodb.com/download-center?jmp=nav#community) - you want
the part before the MongoDB version number.

After that, run the setup-mongo-versions script to download the latest versions
of the currently support set.

To start a specific mongo version, first create a config for it using the
`create-config` script, then start it using the `start-mongodb` script:

```
LOCAL_MONGO_VER=4.1.1 ./bin/create-config
LOCAL_MONGO_VER=4.1.1 ./bin/start-mongodb
```

If the start script complains of issues in the config, or you want a custom
configuration, look in the `var/$version/mongodb.conf` file for the config.

# MONGOPATH

The MongoDB perl driver has a harness that uses `MONGOPATH` for finding the
various mongodb installs for custom setups. After downloading the required
versions using `setup-mongo-versions` then source the mongopath script, and
then you can use the harness to run tests:

```
source ./bin/mongopath
cd mongo-perl-driver
./devel/bin/harness.pl devel/config/mongod-3.6.yml -- prove -lr t
```
