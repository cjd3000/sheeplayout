Sheepdog file layout test
=========================

This is a vagrant setup that will pull and build a particular revision of
sheepdog, start a local cluster, populate some data, and create listings of
the storage directory.

Requirements
------------

Install [vagrant](https://www.vagrantup.com/) and [virtualbox](https://www.virtualbox.org/wiki/Downloads).

Specifying the branch/tag
-------------------------

Use SHEEPDOG_TAG environment variable and pass it to vagrant:

    SHEEPDOG_TAG=v0.9.1 vagrant up

Listings
--------

The provisioning script will produce listings in the vagrant directory of
the form `tree_$SHEEPDOG_TAG` and `listing_$SHEEPDOG_TAG` (where the tag is
`master` if it wasn't specified). You can also just `vagrant ssh` and poke
around in the storage directory `/sheepstore`.