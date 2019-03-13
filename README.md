# RPM Spec for Consul

Tries to follow the [packaging guidelines](https://fedoraproject.org/wiki/Packaging:Guidelines) from Fedora.

* Binary: `/usr/bin/consul`
* Config: `/etc/consul.d/`
* Shared state: `/var/lib/consul/`
* Sysconfig: `/etc/sysconfig/consul`
* WebUI: `/usr/share/consul/`

# Versioning

The version number is hardcoded into the SPEC, however should you so choose, it can be set explicitly by passing an argument to `rpmbuild` directly:

```bash
$ rpmbuild --define "_version 0.8.10"
```

# Build

* Build the Docker image. Note that you must amend the `Dockerfile` header if you want a specific OS build (default is `centos7`).
    ```
    docker build --rm -t consul:build .
    ```

* Run the build.
    ```
    docker run --rm -v $PWD:/build consul:build
    ```

* Retrieve the built RPMs from `$PWD/rpmbuild/RPMS`.

# Result

Three RPMs:
- consul server
- consul web UI
- consul-template

# Run

* Install the RPM.
* Put config files in `/etc/consul.d/`.
* Change command line arguments to consul in `/etc/sysconfig/consul`.
  * Add `-bootstrap` **only** if this is the first server and instance.
* Start the service and tail the logs `systemctl start consul.service` and `journalctl -f`.
  * To enable at reboot `systemctl enable consul.service`.
* Consul may complain about the `GOMAXPROCS` setting. This is safe to ignore;
  however, the warning can be supressed by uncommenting the appropriate line in
  `/etc/sysconfig/consul`.

## Config

Config files are loaded in lexical order from the `config-dir`. Some
sample configs are provided.

# More info

See the [consul.io](http://www.consul.io) website.
