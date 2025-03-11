# SUSE Edge Components Versions

This tool retrieves version information for Helm charts, pods, and nodes in a SUSE Edge Kubernetes cluster.

## Quick start

```bash
podman run -it --rm --name components-versions -v /path/to/your/kubeconfig:/kubeconfig ghcr.io/suse-edge/suse-edge-support-tools:components-versions
```

*NOTE:* Running this as a container directly on a K3s/RKE2 host requires to use the `--network=host` flag as the RKE2/K3s kubeconfig points to `127.0.0.1:6443`

```bash
podman run -it --rm --name components-versions --network=host -v /etc/rancher/rke2/rke2.yaml:/kubeconfig ghcr.io/suse-edge/suse-edge-support-tools:components-versions
```

## Features

* Retrieves Helm chart versions, namespaces, revisions, and resources.
* Retrieves pod versions (container images) for each Helm chart.
* Retrieves node information (architecture, kernel version, kubelet version, operating system, OS image).
* Outputs the information in JSON or table format.
* Detects the Edge version.

## Usage

0.  **Prerequisites:**

    * python3
    * helm

    Ideally a virtualenv should be created:

    ```bash
    python3 -m venv .venv
    source .venv/bin/activate
    ```

1.  **Install dependencies:**

    ```bash
    pip install -r requirements.txt
    ```

2.  **Run the script:**

    ```bash
    python components-versions.py -k <kubeconfig_path> -c <chart_names> -o <output_format>
    ```

    * `-k`: Path to the kubeconfig file (optional). If not provided, defaults to `/kubeconfig`
    * `-c`: Comma-separated list of Helm chart names (optional). If not provided, defaults to `metallb, endpoint-copier-operator, 
                  rancher, longhorn, cdi,
                  kubevirt, neuvector, elemental-operator,
                  sriov-network-operator, akri, metal3, system-upgrade-controller & rancher-turtles`.
    * `-o`: Output format: `json` (default) or `table`.
    * `--get-resources`: Get (and print) resources created by the helm chart.
    * `-h`: Show help.

**Example:**

```bash
python components-versions.py -k /path/to/kubeconfig -c cert-manager,metallb -o table
```

## Container

A Dockerfile is provided to build a containerized version of the tool.

**Build the image:**

```bash
podman build -t components-versions .
```

**Run the container:**

```bash
podman run -it --rm --name components-versions -v /path/to/your/kubeconfig:/kubeconfig components-versions -c <chart_names>
```

*NOTE:* Just in case, there is a prebuilt image available at `ghcr.io/e-minguez/suse-edge-support-tools:components-versions` (`linux/amd64` & `linux/arm64` compatible).

```bash
podman run -it --rm --name components-versions -v /path/to/your/kubeconfig:/kubeconfig ghcr.io/e-minguez/suse-edge-support-tools:components-versions -c <chart_names>
```

*NOTE:* Running this as a container directly on a K3s/RKE2 host requires to use the `--network=host` flag as the RKE2/K3s kubeconfig points to `127.0.0.1:6443`

```bash
podman run -it --rm --name components-versions --network=host -v /etc/rancher/rke2/rke2.yaml:/kubeconfig ghcr.io/e-minguez/suse-edge-support-tools:components-versions -c <chart_names>
```

## Example output

* Json

```bash
{
  "helm_charts": {
    "endpoint-copier-operator": {
      "version": "302.0.0+up0.2.1",
      "namespace": "endpoint-copier-operator",
      "revision": 1,
      "pods": {
        "endpoint-copier-operator-7bf97b9d45-psrs7": [
          "registry.suse.com/edge/3.2/endpoint-copier-operator:0.2.0"
        ],
        "endpoint-copier-operator-7bf97b9d45-z2xws": [
          "registry.suse.com/edge/3.2/endpoint-copier-operator:0.2.0"
        ]
      }
    },
    "longhorn": {
      "version": "105.1.0+up1.7.2",
      "namespace": "longhorn-system",
      "revision": 1,
      "pods": {
        "csi-attacher-787fd9c6c8-nnmkb": [
          "rancher/mirrored-longhornio-csi-attacher:v4.7.0"
        ],
        "csi-attacher-787fd9c6c8-sd6p5": [
          "rancher/mirrored-longhornio-csi-attacher:v4.7.0"
        ],
        "csi-attacher-787fd9c6c8-xt64s": [
          "rancher/mirrored-longhornio-csi-attacher:v4.7.0"
        ],
        "csi-provisioner-74486b95c6-4ghrf": [
          "rancher/mirrored-longhornio-csi-provisioner:v4.0.1-20241007"
        ],
        "csi-provisioner-74486b95c6-mfrz2": [
          "rancher/mirrored-longhornio-csi-provisioner:v4.0.1-20241007"
        ],
        "csi-provisioner-74486b95c6-tvt5l": [
          "rancher/mirrored-longhornio-csi-provisioner:v4.0.1-20241007"
        ],
        "csi-resizer-859d4557fd-9rjjf": [
          "rancher/mirrored-longhornio-csi-resizer:v1.12.0"
        ],
        "csi-resizer-859d4557fd-hg6fw": [
          "rancher/mirrored-longhornio-csi-resizer:v1.12.0"
        ],
        "csi-resizer-859d4557fd-zjm4w": [
          "rancher/mirrored-longhornio-csi-resizer:v1.12.0"
        ],
        "csi-snapshotter-6f69c6c8cc-9vmfp": [
          "rancher/mirrored-longhornio-csi-snapshotter:v7.0.2-20241007"
        ],
        "csi-snapshotter-6f69c6c8cc-hhx22": [
          "rancher/mirrored-longhornio-csi-snapshotter:v7.0.2-20241007"
        ],
        "csi-snapshotter-6f69c6c8cc-wm925": [
          "rancher/mirrored-longhornio-csi-snapshotter:v7.0.2-20241007"
        ],
        "engine-image-ei-4623b511-6gq8h": [
          "rancher/mirrored-longhornio-longhorn-engine:v1.7.2"
        ],
        "engine-image-ei-4623b511-d4d6d": [
          "rancher/mirrored-longhornio-longhorn-engine:v1.7.2"
        ],
        "engine-image-ei-4623b511-kczg5": [
          "rancher/mirrored-longhornio-longhorn-engine:v1.7.2"
        ],
        "instance-manager-48853cc97a50c7a5ea009b0a9c863cf2": [
          "rancher/mirrored-longhornio-longhorn-instance-manager:v1.7.2"
        ],
        "instance-manager-6711c17377f76cde3330e9931efa1a8c": [
          "rancher/mirrored-longhornio-longhorn-instance-manager:v1.7.2"
        ],
        "instance-manager-846e57fb61de6ad34a4b71c9e98b8878": [
          "rancher/mirrored-longhornio-longhorn-instance-manager:v1.7.2"
        ],
        "longhorn-csi-plugin-hpxhx": [
          "rancher/mirrored-longhornio-csi-node-driver-registrar:v2.12.0",
          "rancher/mirrored-longhornio-livenessprobe:v2.14.0",
          "rancher/mirrored-longhornio-longhorn-manager:v1.7.2"
        ],
        "longhorn-csi-plugin-k55rp": [
          "rancher/mirrored-longhornio-csi-node-driver-registrar:v2.12.0",
          "rancher/mirrored-longhornio-livenessprobe:v2.14.0",
          "rancher/mirrored-longhornio-longhorn-manager:v1.7.2"
        ],
        "longhorn-csi-plugin-mtkls": [
          "rancher/mirrored-longhornio-csi-node-driver-registrar:v2.12.0",
          "rancher/mirrored-longhornio-livenessprobe:v2.14.0",
          "rancher/mirrored-longhornio-longhorn-manager:v1.7.2"
        ],
        "longhorn-driver-deployer-55f9c88499-hg842": [
          "rancher/mirrored-longhornio-longhorn-manager:v1.7.2"
        ],
        "longhorn-manager-4ptjf": [
          "rancher/mirrored-longhornio-longhorn-manager:v1.7.2",
          "rancher/mirrored-longhornio-longhorn-share-manager:v1.7.2"
        ],
        "longhorn-manager-4qj8r": [
          "rancher/mirrored-longhornio-longhorn-manager:v1.7.2",
          "rancher/mirrored-longhornio-longhorn-share-manager:v1.7.2"
        ],
        "longhorn-manager-69fzv": [
          "rancher/mirrored-longhornio-longhorn-manager:v1.7.2",
          "rancher/mirrored-longhornio-longhorn-share-manager:v1.7.2"
        ],
        "longhorn-ui-59c85fcf94-5jqmw": [
          "rancher/mirrored-longhornio-longhorn-ui:v1.7.2"
        ],
        "longhorn-ui-59c85fcf94-t25pf": [
          "rancher/mirrored-longhornio-longhorn-ui:v1.7.2"
        ]
      }
    },
    "metal3": {
      "version": "302.0.0+up0.9.0",
      "namespace": "metal3-system",
      "revision": 1,
      "pods": {
        "baremetal-operator-controller-manager-86dbf5fb5f-pzgs7": [
          "registry.suse.com/edge/3.2/baremetal-operator:0.8.0",
          "registry.suse.com/edge/3.2/kube-rbac-proxy:0.18.1"
        ],
        "metal3-metal3-ironic-758d5dcb89-fxlpx": [
          "registry.suse.com/edge/3.2/ironic:26.1.2.0",
          "registry.suse.com/edge/3.2/ironic:26.1.2.0",
          "registry.suse.com/edge/3.2/ironic:26.1.2.0"
        ]
      }
    },
    "metallb": {
      "version": "302.0.0+up0.14.9",
      "namespace": "metallb-system",
      "revision": 1,
      "pods": {
        "metallb-controller-5756c8898-cd2hw": [
          "registry.suse.com/edge/3.2/metallb-controller:v0.14.8"
        ],
        "metallb-speaker-5j84l": [
          "registry.suse.com/edge/3.2/metallb-speaker:v0.14.8"
        ],
        "metallb-speaker-fnt4s": [
          "registry.suse.com/edge/3.2/metallb-speaker:v0.14.8"
        ],
        "metallb-speaker-t2gg5": [
          "registry.suse.com/edge/3.2/metallb-speaker:v0.14.8"
        ]
      }
    },
    "rancher": {
      "version": "2.10.1",
      "namespace": "cattle-system",
      "revision": 1,
      "pods": {
        "rancher-7899bd4998-hf6mq": [
          "registry.rancher.com/rancher/rancher:v2.10.1"
        ],
        "rancher-webhook-79798674c5-8tbps": [
          "registry.rancher.com/rancher/rancher-webhook:v0.6.2"
        ],
        "system-upgrade-controller-56696956b-ctxxt": [
          "registry.rancher.com/rancher/system-upgrade-controller:v0.14.2"
        ]
      }
    },
    "rancher-turtles": {
      "version": "302.0.0+up0.14.1",
      "namespace": "rancher-turtles-system",
      "revision": 1,
      "pods": {
        "rancher-turtles-cluster-api-operator-ccc87c444-mfb8b": [
          "registry.rancher.com/rancher/cluster-api-operator:v0.14.0"
        ],
        "rancher-turtles-controller-manager-69d67966d4-2glt2": [
          "registry.rancher.com/rancher/rancher/turtles:v0.14.1"
        ]
      }
    },
    "sriov-network-operator": {
      "version": "302.0.0+up1.4.0",
      "namespace": "sriov-system",
      "revision": 1,
      "pods": {
        "sriov-network-operator-66566b589b-lzhnk": [
          "rancher/hardened-sriov-network-operator:v1.4.0-build20241113"
        ],
        "sriov-network-operator-sriov-nfd-gc-5cc766bbb4-xnmqm": [
          "rancher/hardened-node-feature-discovery:v0.15.7-build20241113"
        ],
        "sriov-network-operator-sriov-nfd-master-676d988589-drbvd": [
          "rancher/hardened-node-feature-discovery:v0.15.7-build20241113"
        ],
        "sriov-network-operator-sriov-nfd-worker-8vx5r": [
          "rancher/hardened-node-feature-discovery:v0.15.7-build20241113"
        ],
        "sriov-network-operator-sriov-nfd-worker-nsg4d": [
          "rancher/hardened-node-feature-discovery:v0.15.7-build20241113"
        ],
        "sriov-network-operator-sriov-nfd-worker-thgvs": [
          "rancher/hardened-node-feature-discovery:v0.15.7-build20241113"
        ]
      }
    },
    "system-upgrade-controller": {
      "version": "105.0.1",
      "namespace": "cattle-system",
      "revision": 1,
      "pods": {
        "rancher-7899bd4998-hf6mq": [
          "registry.rancher.com/rancher/rancher:v2.10.1"
        ],
        "rancher-webhook-79798674c5-8tbps": [
          "registry.rancher.com/rancher/rancher-webhook:v0.6.2"
        ],
        "system-upgrade-controller-56696956b-ctxxt": [
          "registry.rancher.com/rancher/system-upgrade-controller:v0.14.2"
        ]
      }
    }
  },
  "nodes": {
    "host1rke2": {
      "architecture": "amd64",
      "kernelVersion": "6.4.0-18-default",
      "kubeletVersion": "v1.31.3+rke2r1",
      "operatingSystem": "linux",
      "osImage": "SUSE Linux Micro 6.0"
    },
    "host2rke2": {
      "architecture": "amd64",
      "kernelVersion": "6.4.0-18-default",
      "kubeletVersion": "v1.31.3+rke2r1",
      "operatingSystem": "linux",
      "osImage": "SUSE Linux Micro 6.0"
    },
    "host3rke2": {
      "architecture": "amd64",
      "kernelVersion": "6.4.0-18-default",
      "kubeletVersion": "v1.31.3+rke2r1",
      "operatingSystem": "linux",
      "osImage": "SUSE Linux Micro 6.0"
    }
  },
  "detected_edge_version": {
    "release": "3.2.0",
    "items_matching": {
      "kubeversion": "v1.31.3+rke2r1",
      "endpoint-copier-operator": "302.0.0+up0.2.1",
      "longhorn": "105.1.0+up1.7.2",
      "metal3": "302.0.0+up0.9.0",
      "metallb": "302.0.0+up0.14.9",
      "rancher": "2.10.1",
      "rancher-turtles": "302.0.0+up0.14.1",
      "sriov-network-operator": "302.0.0+up1.4.0",
      "system-upgrade-controller": "105.0.1"
    },
    "items_not_matching": {}
  }
}
```

* Table

```
Release: endpoint-copier-operator
+-----------+--------------------------+
| Version   | 302.0.0+up0.2.1          |
+-----------+--------------------------+
| Namespace | endpoint-copier-operator |
+-----------+--------------------------+
| Revision  | 1                        |
+-----------+--------------------------+

  Pods:
+-------------------------------------------+-----------------------------------------------------------+
| Pod Name                                  | Images                                                    |
+===========================================+===========================================================+
| endpoint-copier-operator-7bf97b9d45-psrs7 | registry.suse.com/edge/3.2/endpoint-copier-operator:0.2.0 |
+-------------------------------------------+-----------------------------------------------------------+
| endpoint-copier-operator-7bf97b9d45-z2xws | registry.suse.com/edge/3.2/endpoint-copier-operator:0.2.0 |
+-------------------------------------------+-----------------------------------------------------------+

Release: longhorn
+-----------+-----------------+
| Version   | 105.1.0+up1.7.2 |
+-----------+-----------------+
| Namespace | longhorn-system |
+-----------+-----------------+
| Revision  | 1               |
+-----------+-----------------+

  Pods:
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Pod Name                                          | Images                                                                                                                                                                |
+===================================================+=======================================================================================================================================================================+
| csi-attacher-787fd9c6c8-nnmkb                     | rancher/mirrored-longhornio-csi-attacher:v4.7.0                                                                                                                       |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| csi-attacher-787fd9c6c8-sd6p5                     | rancher/mirrored-longhornio-csi-attacher:v4.7.0                                                                                                                       |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| csi-attacher-787fd9c6c8-xt64s                     | rancher/mirrored-longhornio-csi-attacher:v4.7.0                                                                                                                       |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| csi-provisioner-74486b95c6-4ghrf                  | rancher/mirrored-longhornio-csi-provisioner:v4.0.1-20241007                                                                                                           |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| csi-provisioner-74486b95c6-mfrz2                  | rancher/mirrored-longhornio-csi-provisioner:v4.0.1-20241007                                                                                                           |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| csi-provisioner-74486b95c6-tvt5l                  | rancher/mirrored-longhornio-csi-provisioner:v4.0.1-20241007                                                                                                           |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| csi-resizer-859d4557fd-9rjjf                      | rancher/mirrored-longhornio-csi-resizer:v1.12.0                                                                                                                       |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| csi-resizer-859d4557fd-hg6fw                      | rancher/mirrored-longhornio-csi-resizer:v1.12.0                                                                                                                       |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| csi-resizer-859d4557fd-zjm4w                      | rancher/mirrored-longhornio-csi-resizer:v1.12.0                                                                                                                       |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| csi-snapshotter-6f69c6c8cc-9vmfp                  | rancher/mirrored-longhornio-csi-snapshotter:v7.0.2-20241007                                                                                                           |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| csi-snapshotter-6f69c6c8cc-hhx22                  | rancher/mirrored-longhornio-csi-snapshotter:v7.0.2-20241007                                                                                                           |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| csi-snapshotter-6f69c6c8cc-wm925                  | rancher/mirrored-longhornio-csi-snapshotter:v7.0.2-20241007                                                                                                           |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| engine-image-ei-4623b511-6gq8h                    | rancher/mirrored-longhornio-longhorn-engine:v1.7.2                                                                                                                    |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| engine-image-ei-4623b511-d4d6d                    | rancher/mirrored-longhornio-longhorn-engine:v1.7.2                                                                                                                    |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| engine-image-ei-4623b511-kczg5                    | rancher/mirrored-longhornio-longhorn-engine:v1.7.2                                                                                                                    |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| instance-manager-48853cc97a50c7a5ea009b0a9c863cf2 | rancher/mirrored-longhornio-longhorn-instance-manager:v1.7.2                                                                                                          |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| instance-manager-6711c17377f76cde3330e9931efa1a8c | rancher/mirrored-longhornio-longhorn-instance-manager:v1.7.2                                                                                                          |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| instance-manager-846e57fb61de6ad34a4b71c9e98b8878 | rancher/mirrored-longhornio-longhorn-instance-manager:v1.7.2                                                                                                          |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| longhorn-csi-plugin-hpxhx                         | rancher/mirrored-longhornio-csi-node-driver-registrar:v2.12.0, rancher/mirrored-longhornio-livenessprobe:v2.14.0, rancher/mirrored-longhornio-longhorn-manager:v1.7.2 |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| longhorn-csi-plugin-k55rp                         | rancher/mirrored-longhornio-csi-node-driver-registrar:v2.12.0, rancher/mirrored-longhornio-livenessprobe:v2.14.0, rancher/mirrored-longhornio-longhorn-manager:v1.7.2 |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| longhorn-csi-plugin-mtkls                         | rancher/mirrored-longhornio-csi-node-driver-registrar:v2.12.0, rancher/mirrored-longhornio-livenessprobe:v2.14.0, rancher/mirrored-longhornio-longhorn-manager:v1.7.2 |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| longhorn-driver-deployer-55f9c88499-hg842         | rancher/mirrored-longhornio-longhorn-manager:v1.7.2                                                                                                                   |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| longhorn-manager-4ptjf                            | rancher/mirrored-longhornio-longhorn-manager:v1.7.2, rancher/mirrored-longhornio-longhorn-share-manager:v1.7.2                                                        |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| longhorn-manager-4qj8r                            | rancher/mirrored-longhornio-longhorn-manager:v1.7.2, rancher/mirrored-longhornio-longhorn-share-manager:v1.7.2                                                        |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| longhorn-manager-69fzv                            | rancher/mirrored-longhornio-longhorn-manager:v1.7.2, rancher/mirrored-longhornio-longhorn-share-manager:v1.7.2                                                        |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| longhorn-ui-59c85fcf94-5jqmw                      | rancher/mirrored-longhornio-longhorn-ui:v1.7.2                                                                                                                        |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| longhorn-ui-59c85fcf94-t25pf                      | rancher/mirrored-longhornio-longhorn-ui:v1.7.2                                                                                                                        |
+---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Release: metal3
+-----------+-----------------+
| Version   | 302.0.0+up0.9.0 |
+-----------+-----------------+
| Namespace | metal3-system   |
+-----------+-----------------+
| Revision  | 1               |
+-----------+-----------------+

  Pods:
+--------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
| Pod Name                                               | Images                                                                                                                             |
+========================================================+====================================================================================================================================+
| baremetal-operator-controller-manager-86dbf5fb5f-pzgs7 | registry.suse.com/edge/3.2/baremetal-operator:0.8.0, registry.suse.com/edge/3.2/kube-rbac-proxy:0.18.1                             |
+--------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------+
| metal3-metal3-ironic-758d5dcb89-fxlpx                  | registry.suse.com/edge/3.2/ironic:26.1.2.0, registry.suse.com/edge/3.2/ironic:26.1.2.0, registry.suse.com/edge/3.2/ironic:26.1.2.0 |
+--------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------+

Release: metallb
+-----------+------------------+
| Version   | 302.0.0+up0.14.9 |
+-----------+------------------+
| Namespace | metallb-system   |
+-----------+------------------+
| Revision  | 1                |
+-----------+------------------+

  Pods:
+------------------------------------+-------------------------------------------------------+
| Pod Name                           | Images                                                |
+====================================+=======================================================+
| metallb-controller-5756c8898-cd2hw | registry.suse.com/edge/3.2/metallb-controller:v0.14.8 |
+------------------------------------+-------------------------------------------------------+
| metallb-speaker-5j84l              | registry.suse.com/edge/3.2/metallb-speaker:v0.14.8    |
+------------------------------------+-------------------------------------------------------+
| metallb-speaker-fnt4s              | registry.suse.com/edge/3.2/metallb-speaker:v0.14.8    |
+------------------------------------+-------------------------------------------------------+
| metallb-speaker-t2gg5              | registry.suse.com/edge/3.2/metallb-speaker:v0.14.8    |
+------------------------------------+-------------------------------------------------------+

Release: rancher
+-----------+---------------+
| Version   | 2.10.1        |
+-----------+---------------+
| Namespace | cattle-system |
+-----------+---------------+
| Revision  | 1             |
+-----------+---------------+

  Pods:
+-------------------------------------------+----------------------------------------------------------------+
| Pod Name                                  | Images                                                         |
+===========================================+================================================================+
| rancher-7899bd4998-hf6mq                  | registry.rancher.com/rancher/rancher:v2.10.1                   |
+-------------------------------------------+----------------------------------------------------------------+
| rancher-webhook-79798674c5-8tbps          | registry.rancher.com/rancher/rancher-webhook:v0.6.2            |
+-------------------------------------------+----------------------------------------------------------------+
| system-upgrade-controller-56696956b-ctxxt | registry.rancher.com/rancher/system-upgrade-controller:v0.14.2 |
+-------------------------------------------+----------------------------------------------------------------+

Release: rancher-turtles
+-----------+------------------------+
| Version   | 302.0.0+up0.14.1       |
+-----------+------------------------+
| Namespace | rancher-turtles-system |
+-----------+------------------------+
| Revision  | 1                      |
+-----------+------------------------+

  Pods:
+------------------------------------------------------+-----------------------------------------------------------+
| Pod Name                                             | Images                                                    |
+======================================================+===========================================================+
| rancher-turtles-cluster-api-operator-ccc87c444-mfb8b | registry.rancher.com/rancher/cluster-api-operator:v0.14.0 |
+------------------------------------------------------+-----------------------------------------------------------+
| rancher-turtles-controller-manager-69d67966d4-2glt2  | registry.rancher.com/rancher/rancher/turtles:v0.14.1      |
+------------------------------------------------------+-----------------------------------------------------------+

Release: sriov-network-operator
+-----------+-----------------+
| Version   | 302.0.0+up1.4.0 |
+-----------+-----------------+
| Namespace | sriov-system    |
+-----------+-----------------+
| Revision  | 1               |
+-----------+-----------------+

  Pods:
+----------------------------------------------------------+---------------------------------------------------------------+
| Pod Name                                                 | Images                                                        |
+==========================================================+===============================================================+
| sriov-network-operator-66566b589b-lzhnk                  | rancher/hardened-sriov-network-operator:v1.4.0-build20241113  |
+----------------------------------------------------------+---------------------------------------------------------------+
| sriov-network-operator-sriov-nfd-gc-5cc766bbb4-xnmqm     | rancher/hardened-node-feature-discovery:v0.15.7-build20241113 |
+----------------------------------------------------------+---------------------------------------------------------------+
| sriov-network-operator-sriov-nfd-master-676d988589-drbvd | rancher/hardened-node-feature-discovery:v0.15.7-build20241113 |
+----------------------------------------------------------+---------------------------------------------------------------+
| sriov-network-operator-sriov-nfd-worker-8vx5r            | rancher/hardened-node-feature-discovery:v0.15.7-build20241113 |
+----------------------------------------------------------+---------------------------------------------------------------+
| sriov-network-operator-sriov-nfd-worker-nsg4d            | rancher/hardened-node-feature-discovery:v0.15.7-build20241113 |
+----------------------------------------------------------+---------------------------------------------------------------+
| sriov-network-operator-sriov-nfd-worker-thgvs            | rancher/hardened-node-feature-discovery:v0.15.7-build20241113 |
+----------------------------------------------------------+---------------------------------------------------------------+

Release: system-upgrade-controller
+-----------+---------------+
| Version   | 105.0.1       |
+-----------+---------------+
| Namespace | cattle-system |
+-----------+---------------+
| Revision  | 1             |
+-----------+---------------+

  Pods:
+-------------------------------------------+----------------------------------------------------------------+
| Pod Name                                  | Images                                                         |
+===========================================+================================================================+
| rancher-7899bd4998-hf6mq                  | registry.rancher.com/rancher/rancher:v2.10.1                   |
+-------------------------------------------+----------------------------------------------------------------+
| rancher-webhook-79798674c5-8tbps          | registry.rancher.com/rancher/rancher-webhook:v0.6.2            |
+-------------------------------------------+----------------------------------------------------------------+
| system-upgrade-controller-56696956b-ctxxt | registry.rancher.com/rancher/system-upgrade-controller:v0.14.2 |
+-------------------------------------------+----------------------------------------------------------------+

Node Information:
+-----------+----------------+------------------+-------------------+--------------------+----------------------+
| Node      | Architecture   | Kernel Version   | Kubelet Version   | Operating System   | OS Image             |
+===========+================+==================+===================+====================+======================+
| host1rke2 | amd64          | 6.4.0-18-default | v1.31.3+rke2r1    | linux              | SUSE Linux Micro 6.0 |
+-----------+----------------+------------------+-------------------+--------------------+----------------------+
| host2rke2 | amd64          | 6.4.0-18-default | v1.31.3+rke2r1    | linux              | SUSE Linux Micro 6.0 |
+-----------+----------------+------------------+-------------------+--------------------+----------------------+
| host3rke2 | amd64          | 6.4.0-18-default | v1.31.3+rke2r1    | linux              | SUSE Linux Micro 6.0 |
+-----------+----------------+------------------+-------------------+--------------------+----------------------+

SUSE Edge detected version: 3.2.0

SUSE Edge items matching:
+---------------------------+------------------+
| Component                 | Version          |
+===========================+==================+
| kubeversion               | v1.31.3+rke2r1   |
+---------------------------+------------------+
| endpoint-copier-operator  | 302.0.0+up0.2.1  |
+---------------------------+------------------+
| longhorn                  | 105.1.0+up1.7.2  |
+---------------------------+------------------+
| metal3                    | 302.0.0+up0.9.0  |
+---------------------------+------------------+
| metallb                   | 302.0.0+up0.14.9 |
+---------------------------+------------------+
| rancher                   | 2.10.1           |
+---------------------------+------------------+
| rancher-turtles           | 302.0.0+up0.14.1 |
+---------------------------+------------------+
| sriov-network-operator    | 302.0.0+up1.4.0  |
+---------------------------+------------------+
| system-upgrade-controller | 105.0.1          |
+---------------------------+------------------+
```

## GitHub Actions

A GitHub Actions workflow is included to automatically build and push the Docker image to the GitHub Container Registry on every push to the main branch.

## Contributing

Contributions are welcome! Feel free to open issues or pull requests.