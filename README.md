# CloudWatch Exporter Installation (Using Helm)


### Adding Repository
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

```

### Optional : Inspect Helm Chart Values
```
helm show values prometheus-community/prometheus-cloudwatch-exporter
```
### Creating cloudwatch-exporter.yaml
```
vim cloudwatch-exporter.yaml
```
```
helm install cloudwatch-exporter  prometheus-community/prometheus-cloudwatch-exporter -n monitoring --values cloudwatch-exporter.yaml
```

### Exposing the service using Port Forward 
```
kubectl port-forward svc/cloudwatch-exporter-prometheus-cloudwatch-exporter -n monitoring --address 0.0.0.0 80:9106
```

### Note :- CloudWatch Exporter runs on 9106 Port Number (Make Sure it is exposed)




# Prometheus MS Teams Integration

1. Create a Team > From Scratch > Private > Monitoring Team
2. Add Members to your team
3.  Create a channel named alerts
4. Now, Select Connectors > Search for "Incoming Webhook" > Add 
5. After Adding if the configuration option comes Great ! If not click on channel > connectors > internal webhook > configure
6. Configure the Webhook name & Logo > Copy the URL webhook link & put it somewhere safe.

## Install & Configure Prometheus MSTeams

```
helm repo add prometheus-msteams https://prometheus-msteams.github.io/prometheus-msteams/
```
```
helm repo update
```

### Configuring Prometheus-MSTeams Helm Chart 
### Check chart values
```
helm show values prometheus-msteams/prometheus-msteams
```
```
vim values.yaml
```

```
connectors:
- delvex: <"WEBHOOK URL">
```
```
helm upgrade --install prometheus-msteams prometheus-msteams/prometheus-msteams  --namespace monitoring --values values.yaml
```

## NOTE :- This is meant to act as proxy between Prometheus MSTeams, as Prometheus can't POST to webhook url directly which Teams expect 


### Now configure AlertManager to send the alert to prometheus-msteams service 

1. Add Receiver in Alertmanager to receive the data.
2. Now configure routes to bind the alert & receiver.
3. For simplicity, we're configuring KubeControllerManagerDown Alert.
4. Checkout harsh-poc > alertmanager directory (For configuring Alertmanager Helm Chart)
5. Checkout harsh-poc > alertmanager > prometheus-msteams (For configuring prometheus-msteams)





## Email Configuration to Send Email using Gmail SMTP

1. Enable 2FA following this [URL](https://support.google.com/accounts/answer/185839?hl=en&co=GENIE.Platform%3DAndroid)
2. After enabling 2FA search for App Password on the same webpage
3. Create a new App Password & keep copy of it .
4. Now create a receiver of type email_config in values.yaml
5. Check harsh-poc > alertmanager > values.yaml





