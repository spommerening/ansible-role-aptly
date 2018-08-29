# Ansible Role Aptly

Aptly is a swiss army knife for Debian repository management:
it allows you to mirror remote repositories, manage local package repositories,
take snapshots, pull new versions of packages along with dependencies,
publish as Debian repository.

## Configuration

Most important value is `aptly_version`. You may want to update
this to a newer version if available. See https://www.aptly.info/download/
for information regarding the current Aptly release status.

Default values for the Aptly configuration file are listed in [defaults/main.yml].

Usual installations do not require any change of the default values.
If you plan to use a separate web server for your repository you may want
to update the `aptly_rootdir` path to a suitable value. Please note that
the web server's document root will be `{{ aptly_rootdir }}/public`.

## Tips and Tricks

### Creation of a PGP key pair for signing your repository

Create a new PGP key pair using `gpg --gen-key` on your aptly server after
installation. The public part of the generated key should be exported from
your keyring using `gpg --export --armor` and imported using `apt-key` on
all machines that would be using published repositories. Paste the exported
public part of the key into a file e.g. `Release.key` and put it in the root
directory of your webserver (e.g. `{{ aptly_rootdir }}/public`).

### Duration of publish commands

For large repository snapshots the publish command may take several hours and
therefore it's a good idea to run this command in a virtual terminal (`screen`).

### AWS instance sizing

My experience is that you require a (large) instance with about 8 GB of memory (e.g. t2.large)
for running `aptly publish snapshot` for large repository snapshots like the
complete Ubuntu repository.

## References

* [Aptly Homepage](https://www.aptly.info/)
* [Ansible Role Aptly from Tom Paine](https://github.com/aioue/ansible-role-aptly)

## Author

Stefan Pommerening <step@dmsp.de>
