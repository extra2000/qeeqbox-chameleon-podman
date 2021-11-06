# qeeqbox-chameleon-podman

| License | Versioning | Build |
| ------- | ---------- | ----- |
| [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) | [![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release) | [![Build status](https://ci.appveyor.com/api/projects/status/vb5rrjxup2yjq0dc/branch/master?svg=true)](https://ci.appveyor.com/project/nikAizuddin/qeeqbox-chameleon-podman/branch/master) |

Podman deployment for [qeeqbox/chameleon](https://github.com/qeeqbox/chameleon).


## Prerequisites

Build Sphinx image:
```
podman build -t extra2000/sphinx .
```


## How to build docs into HTML

**NOTE:** The `chcon` commands are for SELinux platforms only.

Allow `./docs` to be mounted into Podman container:
```
chcon -R -v -t container_file_t ./docs
```

Create `./output` and `docs/build` directories to store rendered HTML output and allow the directory to be mounted into Podman container:
```
mkdir -pv output docs/build
chcon -R -v -t container_file_t ./output docs/build
```

Build docs:
```
podman run -it --rm --network none -v ./docs:/srv/docs:ro -v ./output:/srv/docs/build:rw extra2000/sphinx make clean html
```


## How to deploy docs

Import SELinux Security Policy for `httpd` container:
```
sudo semodule -i docs_qeeqbox_chameleon_pod.cil /usr/share/udica/templates/{base_container.cil,net_container.cil}
```

Create `no-internet` network:
```
podman network create no-internet --internal
```

Deploy docs:
```
podman run --rm --network no-internet -p 18080:80 -v ./output/html:/usr/local/apache2/htdocs:ro --security-opt label=type:docs_qeeqbox_chameleon_pod.process docker.io/library/httpd:2.4
```

Deploy docs using pod:
```
podman play kube --network no-internet docs-qeeqbox-chameleon-pod.yaml
```
