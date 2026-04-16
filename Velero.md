# Velero

Velero is a Kubernetes backup/restore and disaster-recovery tool. It saves your cluster state (API objects like Deployments, Services, CRDs, etc.) to object storage (e.g., S3) and can also capture persistent volume data (via snapshots or file-system backups). You can run on-demand backups or schedules, then restore the whole cluster or specific namespaces/resources.