# Evidence and Limitations

This page explains the scope of evidence included in the TaskHub Kubernetes
infrastructure lab. The PNG files in `screenshots/` were extracted without
content alteration from the completed submission report. They are historical
screenshots of a local kind/WSL2 environment, not live monitoring results.

## What is directly visible

| Evidence | Supported observation |
|---|---|
| `cluster-nodes-ready.png` | One control plane and two worker nodes report Ready. |
| `workload-status.png` | Frontend/backend Deployments have 2/2 available replicas; PostgreSQL has 1/1; the worker DaemonSet has 2/2; the CronJob is suspended in that capture. |
| `networking-and-storage.png` | Ingress is present; database and NFS PVCs are Bound; local LoadBalancer external IP is pending. |
| `ingress-http-responses.png` | The frontend and `/api` endpoints return HTTP 200 through localhost:8081. |
| `postgresql-test-row.png` | The `persistence_test` row exists in PostgreSQL at the time of the query. |
| `nfs-shared-file.png` | One NFS demo Pod writes a file and the other reads the same contents. |
| `jobs-and-cronjob.png` | Jobs complete; maintenance Pods/logs show completed scheduled executions. |
| `rbac-authorization.png` | The test ServiceAccount can list Pods in `taskhub` but cannot delete Pods, get Secrets, read Pod logs or list Pods in `default`. |
| `frontend-self-healing.png` | A frontend Pod is deleted and a replacement is subsequently Running with the desired number of ready replicas. |
| `readiness-not-ready.png` and `readiness-recovered.png` | An unready Pod is observed during a rollout; a later capture shows a successful healthy rollout. |

## Evidence boundaries

- **Database persistence:** The visible SQL query confirms a stored row. The
  screenshot does not include the full delete/recreate/query sequence, so it
  does not independently prove the claimed persistence test from start to end.
- **Readiness failure and recovery:** The not-ready state and later recovery
  are shown, but neither the deliberately broken probe configuration nor the
  specific repair or rollback command appears in these captures.
- **NFS node placement:** The writer/reader screenshot confirms cross-Pod file
  sharing. It does not display which nodes hosted those Pods. The Kubernetes
  manifests select different worker nodes, but runtime placement is not
  shown in that image.
- **High availability:** Two replicas inside one local kind cluster do not
  prove resilience to host, storage, or availability-zone failure.
- **Application scope:** The backend is a simple HTTP echo service; it does
  not execute database queries. PostgreSQL is a separate infrastructure lab.
- **Local setup:** The NFS server address in `storage/nfs-pv-pvc.yaml` is
  specific to the original WSL2 setup and must be changed to reproduce it.
- **Secrets:** `config/secret.yaml` remains intentionally excluded from Git.
  A user recreating the project must supply their own lab credentials as
  described in the README.

The [submission report](../reports/TaskHub_Kubernetes_YousefElsayed.pdf)
includes the longer sequence and captions, with qualifications about what
each historical screenshot supports.
