\# 🚀 Kubernetes Logging Pipeline (Fluentd + Elasticsearch + Grafana)



\## 📌 Project Overview



This project demonstrates a complete logging pipeline in Kubernetes using:



\* Fluentd (log collection \& processing)

\* Elasticsearch (log storage)

\* Grafana (log visualization)



The setup collects container logs from all nodes, enriches them with Kubernetes metadata, and makes them searchable and visualizable.



\---



\## 🏗️ Architecture



```

Pods → Fluentd (DaemonSet) → Elasticsearch → Grafana

```



\* Fluentd runs on every node and collects logs from `/var/log/containers`

\* Logs are enriched with Kubernetes metadata (pod name, namespace)

\* Logs are stored in Elasticsearch

\* Grafana is used to query and visualize logs



\---



\## ⚙️ Tech Stack



\* Kubernetes (Kind cluster)

\* Fluentd

\* Elasticsearch

\* Grafana



\---



\## 📂 Project Structure



```

manifests/

&#x20; ├── fluentd/

&#x20; ├── elasticsearch.yaml/

&#x20; ├── grafana.yaml

```



\---



\## 🚀 Setup Instructions



\### 1. Deploy Elasticsearch



```

kubectl apply -f manifests/elasticsearch.yaml

```



\### 2. Deploy Fluentd



```

kubectl apply -f manifests/fluentd/

```



\### 3. Deploy Grafana



```

kubectl apply -f manifests/grafana.yaml

```



\### 4. Generate Logs



```

kubectl run log-generator --image=busybox -- /bin/sh -c "while true; do echo 'Hello from Kubernetes logging'; sleep 10; done"

```



\---



\## 🔍 Verify Logs in Elasticsearch



```

curl http://localhost:9200/k8s-logs-\*/\_search?q=Hello\&pretty

```



\---



\## 📊 View Logs in Grafana



1\. Open Grafana UI

2\. Add Elasticsearch datasource

3\. Go to Explore

4\. Query:



```

message:Hello

```



\---



\## ✅ Output Example



```

"message": "Hello from Kubernetes logging",

"kubernetes": {

&#x20; "pod\_name": "log-generator",

&#x20; "namespace\_name": "default"

}

```



\---



\## ⚠️ Issues Faced \& Fixes



\### 1. Fluentd CrashLoopBackOff



\* Cause: Missing RBAC permissions

\* Fix: Added ClusterRole and ServiceAccount



\### 2. Kubernetes metadata not visible



\* Cause: Incorrect filter pattern

\* Fix: Changed to `kubernetes.\*\*`



\### 3. Old logs showing without metadata



\* Cause: Existing Elasticsearch indices

\* Fix: Deleted indices and restarted pipeline



\### 4. Buffer overflow error



\* Cause: High log volume

\* Fix: Tuned Fluentd buffer settings



\---



\## 📈 What I Learned



\* Kubernetes logging architecture

\* DaemonSet usage for node-level agents

\* Fluentd configuration and filters

\* Elasticsearch indexing and querying

\* Grafana log exploration



\---



\## 🔥 Future Improvements



\* Add Loki + Promtail

\* Create Grafana dashboards

\* Add alerting

\* Use persistent volumes for Elasticsearch



\---



\## 👨‍💻 Author



Kiran Kumatkar



