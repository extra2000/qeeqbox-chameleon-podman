# Changelog

### [4.0.1](https://github.com/extra2000/qeeqbox-chameleon-podman/compare/v4.0.0...v4.0.1) (2021-12-22)


### Fixes

* **logstash-pipeline:** ignore HTTP/HTTPS events with empty request ([fa8c2f8](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/fa8c2f89295c4a494159eb0940865df794340e72))

## [4.0.0](https://github.com/extra2000/qeeqbox-chameleon-podman/compare/v3.1.3...v4.0.0) (2021-12-20)


### ⚠ BREAKING CHANGES

* **filebeat:** `logname: qeeqbox-chameleon-honeypots` has been renamed to `logname: qeeqbox-honeypots`

### Features

* **filebeat:** add `timezone`, `organization_name`, `project_name`, `sensor_name`, and `sensor_ip` option into YML config file ([6f9f4b4](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/6f9f4b4a898dc566ecafb99092c3d5bf008f96ee))
* **filebeat:** add Logstash pipelines, templates, and ILM policies ([c80f609](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/c80f60996b3b50cdbda2b5eb48059106afedbaad))


### Maintenance

* **src/qeeqbox-chameleon:** update to merged PR commit ([29df6b3](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/29df6b385a73735ffff78a54d62a12e015de0244))


### Code Refactoring

* **filebeat:** rename `logname: qeeqbox-chameleon-honeypots` to `logname: qeeqbox-honeypots` ([50da3c2](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/50da3c29f742fdc4dbb08102e39f8ab5542e6ce8))
* **sphinx:** change to full width page ([a4dccc3](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/a4dccc3fe6671ed79a04786770b3794b8df4a937))


### Documentations

* **deployment-production:** add instructions to configure Logstash ([49a3457](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/49a3457aa3761acbbdbaf1bc5f9ef33a87927c50))

### [3.1.3](https://github.com/extra2000/qeeqbox-chameleon-podman/compare/v3.1.2...v3.1.3) (2021-12-17)


### Fixes

* **src/qeeqbox-chameleon:** update commit ([2304970](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/230497038d57129cc20f13ddda3ccacf501187d0))


### Documentations

* **deployments:** add `systemd` instructions for auto startup ([f527353](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/f5273537a20086c0e55165cd028caab8a4537fae))
* **host-preparations:** add instructions to enable linger user ([0ff1b44](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/0ff1b44d53f1e0c0fa4a7667d3066f33705e9196))

### [3.1.2](https://github.com/extra2000/qeeqbox-chameleon-podman/compare/v3.1.1...v3.1.2) (2021-11-22)


### Documentations

* **host-preparations:** add `dnsname` into whitelisted application ([dea9776](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/dea97760fc07cdba31b4c674561f0f99c6e342e4))

### [3.1.1](https://github.com/extra2000/qeeqbox-chameleon-podman/compare/v3.1.0...v3.1.1) (2021-11-21)


### Documentations

* **deployments:** add instruction to cd into filebeat ([8ebbf0d](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/8ebbf0d5e14314f57af03f6cbafbd87dce20ddbe))
* **deployments:** add instruction to clone repository ([4b21c86](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/4b21c8626b44ddaedf88231fba1f5e29b13ba7db))
* **host-preparations:** improve `grubby` commands to prevent updating all kernels ([a4b13c7](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/a4b13c79c840626bb5e83b3349eee9c1ccced57c))
* **host-preparations:** improve solution for cgroup controller ([6161092](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/6161092a4385dd28de5276f5164d17ce1ac28743))
* **host-preparations:** reboot after `dnf update` ([3764de0](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/3764de006adb9ca0b6e3fb4f827092266bd434a4))


### Fixes

* **filebeat:** exclude `- "'ip': '0.0.0.0'"` into production filebeat config ([7831e3e](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/7831e3e7607f0f88c41ebe1a1c0d81225ab9c35e))

## [3.1.0](https://github.com/extra2000/qeeqbox-chameleon-podman/compare/v3.0.0...v3.1.0) (2021-11-18)


### Features

* **deployment:** add Filebeat for development deployment ([d930d62](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/d930d62ee7affbc049887c596463878e4daa3f28))
* **deployment:** add Filebeat for production deployment ([a931ff8](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/a931ff85ee237b31f5de9d14e6842816e86e74c8))


### Styles

* **deployment-production:** fix code syntax ([cdc94ab](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/cdc94ab9bad97eed9c2201fbf029444d379d440e))


### Documentations

* **deployment-development:** restrict `qeeqboxnet` with no Internet access ([35ed228](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/35ed228b43c32772116ecb492ec53546dd8df84f))
* **deployment-production:** add instructions to deploy Filebeat ([d6534be](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/d6534be66a218ba16ebf87a9a2178e9daa4ed062))
* **deployment-production:** improve firewall instruction ([940b8b8](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/940b8b8e9597d74a0392c4d78cf0a89e356a9399))
* **host-preparations:** add solution to systemd cgroupv2 delegation bugs ([c889f57](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/c889f57f62e702812f30b887a2480a34d62d3036))
* **host-preparations:** improve cgroupv2 instructions ([d097c01](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/d097c011362b1c2d712ae2d7601817efdb9bc7a9))
* **host-preparations:** remove unnecessary `systemd-pam` package ([c4df5b9](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/c4df5b909276e7cb74e186c93eb2910bf678be19))
* **host-preparations:** specify AlmaLinux 8 and make sure all packages are up to date ([9490b59](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/9490b59aec76a041df15959aa9a84a4103fb9e3a))
* **qeeqbox-chameleon:** minor update repo ([3b66386](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/3b663862b8dbf94c4c4d20b014ed6cc180864443))

## [3.0.0](https://github.com/extra2000/qeeqbox-chameleon-podman/compare/v2.0.0...v3.0.0) (2021-11-08)


### ⚠ BREAKING CHANGES

* **postgres:** Deployment for Postgres development environment have changed. Refer to documentations on how to create configs and deploy.

### Features

* **deployment:** add example for Production Deployment ([4b9169a](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/4b9169a533616d114500fe02344ad423d2d98e05))


### Documentations

* **host-preparations:** add instruction to change runtime from `runc` to `crun` ([547579a](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/547579a821e2be53b2ac9d3691ba97553482f3bd))


### Fixes

* **postgres:** add configs for development deployment to fix SELinux AVC constraint issue ([3e8ebcb](https://github.com/extra2000/qeeqbox-chameleon-podman/commit/3e8ebcbacd1ec9757025f5a02087d83836632ff6))

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
