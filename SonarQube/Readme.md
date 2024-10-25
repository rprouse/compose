# SonarQube

This creates a [SonarQube](https://www.sonarsource.com/products/sonarqube/) instance with database.

## Important Prerequisites[^1]:

There are certain requirements for ElasticSearch to run. According to SonarQube's documents:

- `vm.max_map_count` is greater than or equal to 524288 (it worked 262144)
- `fs.file-max` is greater than or equal to 131072
- the user running SonarQube can open at least 131072 file descriptors
- the user running SonarQube can open at least 8192 threads

Check the hosts running values to check which values need to be changed.

```sh
sysctl vm.max_map_count
sysctl fs.file-max
ulimit -n
ulimit -u
```

**If values by default are higher, do not change them.**

To change them on the fly:

```sh
sysctl -w vm.max_map_count=524288
sysctl -w fs.file-max=131072
ulimit -n 131072
ulimit -u 8192
```

To make the change persistent:
On the container host create a file `/etc/sysctl.d/99-sonarqube.conf` with the following:

```sh
vm.max_map_count=262144
fs.file-max=131072
```

On the container host create a file `/etc/security/limits.d/99-sonarqube.conf` with the following:

```sh
root   -   nofile   131072
root   -   nproc    8192
```

[^1]: https://docs.sonarsource.com/sonarqube/latest/requirements/prerequisites-and-overview/
