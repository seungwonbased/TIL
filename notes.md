```
apiVersion: v1
kind: Service
metadata:
	name: postgresql-svc 
	labels: app: drawinglots spec: ports: - port: 5432 selector: app: drawinglots tier: postgresql clusterIP: None --- apiVersion: v1 kind: PersistentVolumeClaim metadata: name: postgresql-pv-claim labels: app: drawinglots spec: accessModes: - ReadWriteOnce resources: requests: storage: 10Gi --- apiVersion: apps/v1 kind: Deployment metadata: name: drawinglots-postgresql labels: app: drawinglots spec: selector: matchLabels: app: drawinglots tier: postgresql strategy: type: Recreate template: metadata: labels: app: drawinglots tier: postgresql spec: containers: - image: postgres:10 name: postgresql env: - name: POSTGRES_PASSWORD valueFrom: secretKeyRef: name: db-secret key: POSTGRES_PASSWORD - name: POSTGRES_DB valueFrom: secretKeyRef: name: db-secret key: POSTGRES_DB - name: PGDATA // Postgres는 요고 하지않으면 오류남 value: /var/lib/postgresql/data/pgdata ports: - containerPort: 5432 name: postgresql volumeMounts: - name: postgresql-persistent-storage mountPath: /var/lib/postgresql/data volumes: - name: postgresql-persistent-storage persistentVolumeClaim: claimName: postgresql-pv-claim



https://willbfine.tistory.com/477