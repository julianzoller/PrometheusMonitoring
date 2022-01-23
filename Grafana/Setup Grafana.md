## Install Grafana

- configuration files to create:
    - service-account.yaml
    - configmap.yaml
    - secret.yaml
    - deployment.yaml
    - service.yaml
    - service-monitor.yaml
    
    ```bash
    kubectl apply -f grafana
    ```
    
- check if pod is running
    
    ```bash
    kubectl get pods -n monitoring
    ```
    
    ![image](https://user-images.githubusercontent.com/64065672/150695105-c6293c6f-0af9-474c-ad66-9f5665943f30.png)
    
- check if services are running
    
    ```bash
    kubectl get svc -n monitoring
    ```
    
    ![image](https://user-images.githubusercontent.com/64065672/150695091-7d6238f6-12c5-4f5b-9601-813ab5fc146a.png)
    
- get Endpoint of prometheus-operated
    
    ```bash
    k get endpoints -n monitoring
    ```
    
    ![image](https://user-images.githubusercontent.com/64065672/150695078-30fa006a-04c4-4cdf-89e9-a95bb3c99b6a.png)
    
- now you can check if Grafana is working on localhost:3030
    
    ```bash
    kubectl -n monitoring port-forward svc/grafana 3000:3000
    ```
    
    ![image](https://user-images.githubusercontent.com/64065672/150695064-42a663d8-5f3d-4a8e-a4de-b046953bf96d.png)
    
- log in and add prometheus as data source
- type in “http://<endpoint of prometheus operated>”
- Check if datasource is working
