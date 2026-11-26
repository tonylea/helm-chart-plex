# helm-chart-plex

Deploys [Plex Media Server](https://www.plex.tv/) from its official image, [`plexinc/pms-docker`](https://github.com/plexinc/pms-docker).

The chart runs one replica:

- A Deployment that stops the old pod before starting the new one, since Plex's database is SQLite and its claim is `ReadWriteOnce`.
- A PersistentVolumeClaim at `/config`, for Plex's database, metadata and thumbnails.
  It's an ordinary claim the chart owns, so its size can be raised in the values; most storage classes allow claims to grow but not shrink.
- A volume at `/transcode` for Plex's transcoder, an empty directory unless another volume is given.
- Each folder in `media` mounted at its own path, read-only unless set otherwise.
- A Service on Plex's port, 32400, and an optional Gateway API `HTTPRoute` to it.
- A start-up, liveness and readiness probe on Plex's `/identity`.
  Start-up gets 5 minutes, since the image chowns the whole config folder before Plex listens.
- An optional busybox sidecar, `plex-logs`, that tails `Plex Media Server.log` to stdout, since Plex logs only to files.
  It's a native sidecar, so it starts before Plex and stops after it.

The pod's hostname is the release name, which Plex names a new server after.
Plex exposes no Prometheus metrics, so the chart has no `ServiceMonitor` or dashboards.

Plex's own chart, `plex-media-server`, runs Plex as a StatefulSet that makes its config claim from `volumeClaimTemplates`.
A StatefulSet's `volumeClaimTemplates` can't change once it exists, so that claim can't be resized from the chart's values.

## Values

| Value | Default |
| --- | --- |
| `image.repository` | `plexinc/pms-docker` |
| `image.tag` | the chart's `appVersion` |
| `timezone` | `Etc/UTC` |
| `advertiseUrls` | none |
| `claimToken.existingSecret` | none |
| `claimToken.key` | `token` |
| `resources` | none |
| `logs.enabled` | `false` |
| `logs.image.repository` | `busybox` |
| `logs.image.tag` | `1.38.0` |
| `logs.resources` | none |
| `persistence.config.claimName` | `<release>-config` |
| `persistence.config.size` | `10Gi` |
| `persistence.config.storageClassName` | the cluster's default class |
| `media` | none |
| `transcode.volume` | an `emptyDir` |
| `affinity` | none |
| `service.type` | `ClusterIP` |
| `service.annotations` | none |
| `service.externalTrafficPolicy` | the cluster's default |
| `httpRoute.enabled` | `false` |
| `httpRoute.parentRefs` | none; required when enabled |
| `httpRoute.hostnames` | none |

`advertiseUrls` are passed to the image as `ADVERTISE_IP`, the addresses Plex tells clients it can be reached on besides its own.
The image applies them only on a first start, when it creates `Preferences.xml`; after that they're changed in Plex's settings.

`claimToken.existingSecret` names a Secret holding a claim token from <https://plex.tv/claim>, passed to the image as `PLEX_CLAIM`.
The image uses it on a first start to link the server to a Plex account, and ignores it once the server is claimed.
A claim token expires 4 minutes after it's issued, so the Secret has to be created just before the first start.

Each entry in `media` has a `name`, a `mountPath`, a `volume`, any Kubernetes volume source, and an optional `readOnly`.
Plex only reads media unless "Allow media deletion" is on in its settings, so a folder is mounted read-only unless its `readOnly` is `false`.

Requests of `cpu: 300m` and `memory: 320Mi` are a suggested starting point.

Set `service.type` to `LoadBalancer` to give Plex an address of its own on the network, which clients on it connect to directly.

## Requirements

- A storage class for the config claim, the cluster's default unless named.
- The claim token's Secret, when `claimToken.existingSecret` is set.
- The Gateway API's `HTTPRoute` CRD, when `httpRoute.enabled` is set.

## Usage

```sh
helm repo add plex https://tonylea.github.io/helm-chart-plex
helm install plex plex/plex --values my-values.yaml
```

```yaml
timezone: Europe/London
media:
  - name: movies
    mountPath: /data/movies
    volume:
      hostPath:
        path: /srv/movies
        type: Directory
```

## Development

Pull requests run:

- `helm lint --strict`, and the rendered chart checked by kubeconform against Kubernetes 1.37.
- helm-unittest's tests in `tests/`.
- An install and upgrade with chart-testing in a kind cluster, with the default values.

Commits follow Conventional Commits.
Releases are versioned and tagged automatically from `main`, and published to the Helm repository on this repo's GitHub Pages.
