# Changelog

All notable changes to this project will be documented in this file. See [commit-and-tag-version](https://github.com/absolute-version/commit-and-tag-version) for commit guidelines.

## 1.0.0 (2026-11-14)

### Features

* deploy Plex Media Server as a Deployment with a resizable config claim, a transcode volume and read-only media folders ([cd43d55](https://github.com/tonylea/helm-chart-plex/commit/cd43d55e562d1254cbbfb9a4222356847c0f72fe))

### Bug Fixes

* replace the transcode emptyDir with the volume given, instead of merging into it ([2e49843](https://github.com/tonylea/helm-chart-plex/commit/2e49843e689c489871d7ad7fc88340436676ea2a))

### Documentation

* describe the chart, its values and how to contribute ([df54089](https://github.com/tonylea/helm-chart-plex/commit/df54089ab420c15a70c1e7d3112b7f9eb829f0e9))
