I should get started explaining all the services in this cluster so I won't miss anything

## Control plane

1. Helm charts

First, update helm charts, some CRDs can be installed by Helm. List of helms:
- Bitnami: https://charts.bitnami.com/bitnami
- Elastic: https://helm.elastic.co
- Emberstack: https://emberstack.github.io/helm-charts/
- External-DNS: https://kubernetes-sigs.github.io/external-dns
- Gitlab: https://charts.gitlab.io/
- Hajimari: https://hajimari.io
- Jetstack: https://charts.jetstack.io/
- k8s-at-home: https://k8s-at-home.com/charts/ - MUST REVIEW THIS CHART
- k8s-gateway: https://ori-edge.github.io/k8s_gateway/
- Kube Reboot: https://kubereboot.github.io/charts/
- MetalLB: https://metallb.github.io/metallb
- Metrics Server: https://kubernetes-sigs.github.io/metrics-server
- Prometheus-community: https://prometheus-community.github.io/helm-charts
- Stakater: https://stakater.github.io/stakater-charts
- Strimzi: https://strimzi.io/charts/
- Traefik: https://helm.traefik.io/traefik

2. Configs

Some global cluster settings with optional secrets sops private key
- Timezone
- MetalLB range
- K8s-gateway address
- Traefik address
- Sops private key

3. Crds

All the custom resources definitions required to install other services:
- Cert-manager: Can be installed directly with only 1 file
- Strimzi: Can be installed directly with only 1 file
- System-upgrade-controller: by Rancher, install with 1 file
- ECK: This CRD can be installed via helm chart

Some may require to be installed from git:
- Traefik: https://github.com/traefik/traefik-helm-chart/tree/master/traefik/crds
- MetalLB: https://github.com/metallb/metallb/tree/main/config/crd
- Kube-Prometheus-Stack: https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack/crds

4. Core

Manager services
- Cert Manager [Jetstack]:
  - Manage certificates with Staging / Production issuers
  - TODO:
    - Set DNS to the local DNS server
- Elastic Operator [Elastic]:
  - Manage Elastic search cluster in k8s, CURRENTLY DISABLED
  - TODO:
    - Somehow elastic operators can't run on Pi, investigate the issue
- Kube-VIP [?]:
  - TODO
- MetalLB [MetalLB]:
  - Give out physical IPs in the network
  - TODO:
    - Give out IPs from multiple network interfaces? Is it possible?
- Strimzi Kafka [Strimzi]:
  - Manage Kafka cluster in k8s

## Apps
1. System upgrade

System upgrade service for OS upgrades, currently disabled

2. Kube System

Other managers that are not core services:
- Kured [kubereboot]:
  - KUbernetes REboot Daemon performs safe automatic node reboots
- metrics-server [metrics-server]:
  - Collects resource metrics from Kubelets and exposes them in Kubernetes API server
- reflector [emberstack]:
  - Monitor changes to resources (secrets and configmaps) and reflect changes
- reloader [stakater]:
  - Reloader can watch changes in ConfigMap and Secret and do rolling upgrades on Pods

3. Networking

Network management services:
- cloudflared [cloudflared]:
  - Cloudflare Tunnel client, hook to Cloudflare network
- error-pages [error-pages]:
  - Show error pages when Traefik can not find underlying resources
- external-dns [external-dns]:
  - Update Cloudflare DNS to point hostname to Cloudflare tunnel
- k8s-gateway [k8s-gateway]:
  - Expose k8s API to the network using MetalLB
- Traefik [Traefik]:
  - Load balancer

4. Flux System

Flux listener to reconcile from Github repository
From Flux receiver, guide: https://fluxcd.io/flux/guides/webhook-receivers/

5. NFS Provisioning

From nfs-subdir-external-provisioner, guide: https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner

6. Monitoring

Using Prometheus + Grafana to monitor the cluster

7. Elastic

Elastic cluster deployment, currently disabled
TODO: Move to "defaults" app

8. Defaults

All other apps
- echo-server [echo-server]:
  - Test
- Hajimari [hajimari]:
  - Home page, like Heimdall
- Home-assistance [linux-server]:
  - Test Home Assistant on k8s, currently disabled
  - Test ESPHome on k8s
  - TODO:
    - Get it to work :)
- Kafka [Strimzi]:
  - Kafka cluster deployment
  - TODO:
    - Figure out a way to smartly manage topics, users, and brokers' certificates
    - Don't hard code IPs
- Proxmox [Traefik]:
  - Ingress route for Proxmox cluster from the network
  - TODO:
    - Don't hard code IPs
- Spark [bitnami]:
  - Test spark job on k8s, currently disabled
  - TODO:
    - Get it to work :)
    - Maybe install airflow
- Uptime kuma [louislam/uptime-kuma]:
  - Monitor services health
