# Changelog

All notable changes to this project will be documented in this file. See [commit-and-tag-version](https://github.com/absolute-version/commit-and-tag-version) for commit guidelines.

## [1.1.0](https://github.com/tonylea/helm-chart-plex/compare/v1.0.1...v1.1.0) (2026-11-26)

### Features

* tail Plex's log file to stdout from a busybox sidecar ([990dea7](https://github.com/tonylea/helm-chart-plex/commit/990dea7d641b4682d0ec1d281b64a668253a2303))
* turn the log sidecar off by default ([ff754bf](https://github.com/tonylea/helm-chart-plex/commit/ff754bf21d81936e62b74f0626c5af01bd8d85f9))

### Documentation

* drop the README's Kubernetes version requirement ([d27d759](https://github.com/tonylea/helm-chart-plex/commit/d27d759d1a1cc18de3afdadc72436d3b3b0600f2))

## [1.0.1](https://github.com/tonylea/helm-chart-plex/compare/v1.0.0...v1.0.1) (2026-11-26)

### Bug Fixes

* leave resource requests to whoever deploys the chart ([208ce3f](https://github.com/tonylea/helm-chart-plex/commit/208ce3f11219553ef9829bc4b45d75277aa19e99))

### Documentation

* cut the README's resources suggestion to the figures ([cd9902e](https://github.com/tonylea/helm-chart-plex/commit/cd9902e19d6f73d5c9c4b19de1616c53b87dfa05))
* drop the README line saying resources are left to the deployer ([84bc161](https://github.com/tonylea/helm-chart-plex/commit/84bc16126d6382e3ee673b3617d9bc57067626b1))

## 1.0.0 (2026-11-14)

### Features

* deploy Plex Media Server as a Deployment with a resizable config claim, a transcode volume and read-only media folders ([cd43d55](https://github.com/tonylea/helm-chart-plex/commit/cd43d55e562d1254cbbfb9a4222356847c0f72fe))

### Bug Fixes

* replace the transcode emptyDir with the volume given, instead of merging into it ([2e49843](https://github.com/tonylea/helm-chart-plex/commit/2e49843e689c489871d7ad7fc88340436676ea2a))

### Documentation

* describe the chart, its values and how to contribute ([df54089](https://github.com/tonylea/helm-chart-plex/commit/df54089ab420c15a70c1e7d3112b7f9eb829f0e9))
