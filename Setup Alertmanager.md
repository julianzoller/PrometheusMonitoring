## Install Alertmanager

- configuration files to create:
    - config.yaml
    - alertmanager.yaml
    - service-monitor.yaml
    - rule.yaml
    
    ```bash
    kubectl apply -f alertmanager
    ```
    
- check if pod is running
    
    ```bash
    kubectl get pods -n monitoring
    ```
    
    ![image](https://user-images.githubusercontent.com/64065672/150695201-ef7c97bf-e0d6-4ae8-9a83-5b55eda3eed0.png)
    
- check if services are running
    
    ```bash
    kubectl get svc -n monitoring
    ```
    
    ![image](https://user-images.githubusercontent.com/64065672/150695194-e1cfe166-636a-40fb-996e-ed16a118d424.png)
    
- now you can check if the Alertmanager is working on localhost:9093
    
    ```bash
    kubectl -n monitoring port-forward svc/alertmanager-operated 9093:9093
    ```
    
    ![image](https://user-images.githubusercontent.com/64065672/150695186-48b0f4d3-13ee-4018-92c5-46e3fda24e0c.png)
