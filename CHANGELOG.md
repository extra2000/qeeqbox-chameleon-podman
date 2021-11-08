# Changelog

## [2.0.0](https://github.com/extra2000/qeeqbox-chameleon-podman/compare/v1.0.2...v2.0.0) (2021-11-08)


### ⚠ BREAKING CHANGES

* **deployment:** Pod file `qeeqbox-chameleon-honeypots-pod.yaml` have been renamed to `qeeqbox-chameleon-honeypots-pod.yaml.example`. Refer to documentations on how to create the pod file.
* **deployment:** Pod file `qeeqbox-chameleon-grafana-pod.yaml` have been renamed to `qeeqbox-chameleon-grafana-pod.yaml.example`. Refer to documentations on how to create the pod file.
* **deployment:** Pod file `qeeqbox-chameleon-redis-pod.yaml` have been renamed to `qeeqbox-chameleon-redis-pod.yaml.example`. Refer to documentations on how to create the pod file.
* **deployment:** Pod file `qeeqbox-chameleon-postgres-pod.yaml` have been renamed to `qeeqbox-chameleon-postgres-pod.yaml.example`. Refer to documentations on how to create the pod file.
* **docs:** Podman pod file `docs-qeeqbox-chameleon-pod.yaml` have been renamed to `docs-qeeqbox-chameleon-pod.yaml.example`. Refer to `README.md` how to create pod file.
* **deployment:** Directory `deployment/general/` have been renamed to `deployment/development/`

### Documentations

* rename `Getting Started` to `Development Deployment` ([6ccd2c2](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/6ccd2c264f6382c9421917196b961bb22d815d4c))


### Code Refactoring

* **deployment:** rename `deployment/general/` to `deployment/development/` ([2a91798](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/2a9179815bfe52cd9ec77d2e9e7ef15d0774f97f))
* **deployment:** rename `qeeqbox-chameleon-grafana-pod.yaml` to `qeeqbox-chameleon-grafana-pod.yaml.example` ([1b48ccf](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/1b48ccf4f04f28a991c903fa538a1c2b7a56e30f))
* **deployment:** rename `qeeqbox-chameleon-honeypots-pod.yaml` to `qeeqbox-chameleon-honeypots-pod.yaml.example` ([db3260d](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/db3260dce9c99c133072537d8d08a89182c378ba))
* **deployment:** rename `qeeqbox-chameleon-postgres-pod.yaml` to `qeeqbox-chameleon-postgres-pod.yaml.example` ([6f7a75e](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/6f7a75e3b56b5b1111e4952f582482493402481d))
* **deployment:** rename `qeeqbox-chameleon-redis-pod.yaml` to `qeeqbox-chameleon-redis-pod.yaml.example` ([e89e1e5](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/e89e1e5303c2921c529cb9fe07725c161a178847))
* **docs:** rename `docs-qeeqbox-chameleon-pod.yaml` to `docs-qeeqbox-chameleon-pod.yaml.example` ([6a24ff5](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/6a24ff51d0827f202ba5f6e865b2f20863427f94))

### [1.0.2](https://github.com/extra2000/qeeqbox-chameleon-podman/compare/v1.0.1...v1.0.2) (2021-11-07)


### Fixes

* **docs-pod:** fix typo in naming ([b6fc0c9](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/b6fc0c9ff11fd4f7552932e4404d1976aaec9e77))
* **redis:** add missing SELinux for Redis port ([25a36e0](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/25a36e0fb57ac21a11cedebefc0a99bc799329ca))


### Documentations

* **honeypots:** add `"ip": "0.0.0.0",` into `config.json.example` ([2b5317c](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/2b5317c1cdb0b9354699d5e7016049174ee2bf19))
* **host-preparations:** add `dnsmasq` installations ([bd188c4](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/bd188c49d489ccf2862e410016703aaa4214bf8c))
* **host-preparations:** add `dnsname` installations ([73f4b08](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/73f4b08c4524c0028a7d5ead9a02fdc5fd3e8f84))
* **host-preparations:** add instructions for non-privileged ports bind ([8ae24e6](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/8ae24e692a504b93ad0cd8cdb8f2b0c6a3c36eb9))
* **host-preparations:** add instructions for rootless podman ([0f74da4](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/0f74da49c834706722e739bca5257df71b05794f))
* **host-preparations:** ensures `/etc/containers/containers.conf` readable by groups and others ([7460d6c](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/7460d6c717c87a6637a8d4aa2a3b84bdb14d1db2))
* **README:** add instructions to create `docs/build` ([d2ba04f](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/d2ba04f24c03619708a197ef9dfcada338bd7f4d))

### [1.0.1](https://github.com/extra2000/qeeqbox-chameleon-podman/compare/v1.0.0...v1.0.1) (2021-11-05)


### Documentations

* **grafana:** add simple instructions to test Grafana ([8fd0d11](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/8fd0d1179f3cca7cb2e7e8fc34dcd5dee53fe8b8))
* **honeypots:** add instructions to view Honeypots logs ([e5d5bc5](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/e5d5bc5e63bb0e1421bf094dc0b182e67be2f45f))
* **redis:** remove unused configs for redis deployment ([08b749f](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/08b749f35e3581e312aeafb83d1f7501c36ecf78))

## 1.0.0 (2021-11-05)


### Features

* initial implementations ([72ca924](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/72ca924287faafc4ef67f61883ba5c51a450e01f))
* **submodules:** add [extra2000/qeeqbox-chameleon](https://github.com/extra2000/qeeqbox-chameleon) ([4b925e1](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/4b925e1493427f2dccef62d356de65318a4054ba))


### Documentations

* **README:** update `README.md` ([1f60e63](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/1f60e639c21905ee94b34cf1ef7990b884dc4ad9))


### Continuous Integrations

* add AppVeyor with `semantic-release` ([aaec239](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/aaec239bc669fefde0de5fd026c96247de2807b3))
