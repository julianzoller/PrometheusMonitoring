# Implement Prometheus

## Create .yaml files to implement custom resources (CRDs)

CRDs allow users to create new types of resources without adding another API server. You do not need to understand API Aggregation to use CRDs.

Regardless of how they are installed, the new resources are referred to as Custom Resources to distinguish them from built-in Kubernetes resources (like pods).

- CRDs to create:
    - prometheuses.yaml
    - alertmanagerconfigs.yaml
    - alertmanagers.yaml
    - podmonitors.yaml
    - probes.yaml
    - prometheusrules.yaml
    - servicemonitors.yaml
    - thanosrulers.yaml (optional)
- integrate created CRDs
    
    ```bash
    kubectl apply -f crds
    ```
    
- check if certain CRDs are already integrated
    
    ```bash
    kubectl get crds
    ```
    
    ![image](https://user-images.githubusercontent.com/64065672/150694937-99a7122f-6cf3-46c7-97c0-e8bdf3e11f7c.png)

## Install Prometheus-Operator

- Prometheus-Operator is deployed as a pod in the namespace ‘monitoring’
- configuration files to create:
    - namespace.yaml
    - service-account.yaml
    - cluster-role.yaml
    - cluster-role-binding.yaml
    - deployment.yaml
    - service.yaml
    - service-monitor.yaml
    
    ```bash
    kubectl apply -f prometheus-operator
    ```
    
- check if pod is running
    
    ```bash
    kubectl get pods -n monitoring
    ```
    
    ![image](https://user-images.githubusercontent.com/64065672/150694963-d895f3f3-09da-4eed-b3e1-df835f88cdf1.png)

## Install Prometheus

- Prometheus is deployed as a pod in the namespace ‘monitoring’
- configuration files to create:
    - service-account.yaml
    - cluster-role.yaml
    - cluster-role-binding.yaml
    - prometheus.yaml
    - service-monitor.yaml
    
    ```bash
    kubectl apply -f prometheus
    ```
    
- check if pod is running
    
    ```bash
    kubectl get pods -n monitoring
    ```
    
    ![image](https://user-images.githubusercontent.com/64065672/150695038-348ef7dc-8a5d-456c-9106-6d0099d3b0d3.png)
    
- check if services are running
    
    ```bash
    kubectl get svc -n monitoring
    ```
    
    ![image](https://user-images.githubusercontent.com/64065672/150695026-c0082798-82a0-4390-8c60-3662c0543e87.png)
    
- now you can check if Prometheus is working on localhost:9090
    
    ```bash
    kubectl -n monitoring port-forward svc/prometheus-operated 9090:9090
    ```
    
    ![image](https://user-images.githubusercontent.com/64065672/150695016-8ae51df3-1990-441e-b439-c6a45c5a159c.png)
    
    ![image](https://user-images.githubusercontent.com/64065672/150695003-1a540da3-0600-404c-adc4-22183e492956.png)
