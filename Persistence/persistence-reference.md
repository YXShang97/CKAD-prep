# Persistence — Single Reference (CKAD)

Concise, exam-focused reference covering volumes, storage, persistence, StorageClass, and StatefulSet usage. All guidance and quick examples are below; full YAML examples live in the `examples/` directory.

1. Quick summary

- Storage: physical or cloud-backed capacity (disks, NFS, cloud volumes). External to Kubernetes until exposed by a provisioner/CSI driver.
- Volume (Pod): a spec entry that makes storage available inside containers (`emptyDir`, `hostPath`, `persistentVolumeClaim`, etc.).
- PersistentVolume (PV): cluster-level representation of real storage.
- PersistentVolumeClaim (PVC): a user's request for storage; binds to a PV (or triggers dynamic provisioning via a StorageClass).
- StorageClass: policy for dynamic provisioning (which provisioner and parameters to use).
- StatefulSet: workload controller that provides stable network identity and per-replica persistent storage with `volumeClaimTemplates`.

2. Key concepts (concise)

- Ephemeral volumes: `emptyDir`, container FS — data lost when Pod removed or container deleted.
- Durable volumes: PV + PVC — data survives Pod restarts/rescheduling because backing storage remains.
- Access modes: `ReadWriteOnce` (RWO), `ReadOnlyMany` (ROX), `ReadWriteMany` (RWX).
- ReclaimPolicy: `Retain` keeps underlying data after PVC deletion; `Delete` removes it.
- Dynamic provisioning: PVC with `storageClassName` triggers the StorageClass provisioner to create a PV.

3. Typical flows

- Static: admin creates `PersistentVolume` → user creates `PersistentVolumeClaim` → PVC binds to PV → Pod mounts PVC.
- Dynamic: admin creates `StorageClass` → user creates PVC referencing `storageClassName` → provisioner creates PV → Pod mounts PVC.
- StatefulSet: `volumeClaimTemplates` creates one PVC per replica (names: `<claim>-<sts-name>-<ordinal>`).

4. Quick `kubectl` checklist

- View resources: `kubectl get pv,pvc,sc`
- Inspect: `kubectl describe pv <name>`, `kubectl describe pvc <name>`, `kubectl describe sc <name>`
- Apply examples: `kubectl apply -f Persistence/<file>.yaml`
- Debug pod mounts: `kubectl exec -it <pod> -- df -h` and `kubectl exec -it <pod> -- ls /data`

5. Troubleshooting checklist (common exam scenarios)

- PVC `Pending`: check `kubectl get pv` for matching `storageClassName`, `capacity`, `accessModes`, or ensure a provisioner exists for the StorageClass.
- Pod mount failure: `kubectl describe pod <pod>` and `kubectl describe pvc <pvc>` for events; check CSI provisioner logs in the cluster.
- Data “gone” after PVC deletion: check PV `persistentVolumeReclaimPolicy` (if `Retain`, underlying volume remains and must be reclaimed manually).
- Permission issues in mount: use `securityContext` or an initContainer to `chown` the mount path.

6. Exam tips (very short)

- Use `kubectl get pv,pvc -o wide` often.
- Practice both static PV+PVC and dynamic PVC via `StorageClass`.
- Practice StatefulSet `volumeClaimTemplates` and inspect created PVC names.

7. Examples (in this folder)

- `pv.yaml` — static PV (moved here)
- `pvc.yaml` — combined examples: static PV+PVC and dynamic PVC
- `storageclass.yaml` — StorageClass for no-provisioner (or adapt for cloud CSI)
- `pod-pvc.yaml` — Pod mounting a PVC
- `statefulset.yaml` — StatefulSet using `volumeClaimTemplates`
- `emptydir-pod.yaml` — ephemeral `emptyDir` volume example
- `hostpath-pod.yaml` — `hostPath` volume example (node-local)

8. Quick apply & validate (copy/paste in shell)

```bash
kubectl apply -f Persistence/storageclass.yaml
kubectl apply -f Persistence/pv.yaml
kubectl apply -f Persistence/pvc.yaml
kubectl apply -f Persistence/pod-pvc.yaml
kubectl get pv,pvc
kubectl exec -it pod-using-pvc -- sh -c "echo hello > /data/hello && cat /data/hello"
```

If you want a tiny diagram (Mermaid) or a one-page printable cheatsheet, I can add it — preference?
