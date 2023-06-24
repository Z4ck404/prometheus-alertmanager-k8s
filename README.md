This repository contains the following examples for setting up monitoring and alerting using Prometheus and Alertmanager.

## PrometheusRule

The PrometheusRule defines a set of rules for Prometheus to evaluate and generate alerts. It is configured with the following specifications:

    Name: z4ck404-alerts
    Namespace: monitoring
    Groups:
        Name: blackbox.alerts
        Rules:
            Alert: backbox probe check failed!
            Expression: probe_success != 1
            For: 2 minutes
            Labels:
                severity: warning
                namespace: monitoring
            Annotations:
                summary: BlackBox probe check failed. Service might be down
                description: 'Target {{{{}} printf "%v" $labels.target {{}}}} probe is down.'

This rule is used to monitor and generate alerts when the specified BlackBox probe check fails.

## AlertmanagerConfig

The AlertmanagerConfig specifies the configuration for Alertmanager, which handles the routing and dispatching of alerts. The configuration includes:

    Name: z4ck404-alertmanagerconfig
    Route:
        Group By: ['alertname', 'job']
        Group Wait: 10 seconds
        Group Interval: 5 minutes
        Repeat Interval: 12 hours
        Continue: true
        Matchers:
            Name: severity
            Match Type: "="
            Value: "warning"
        Receiver: slack
    Receivers:
        Name: slack
            Slack Configs:
                Channel: #alerts
                API URL:
                    Key: slack_webhook
                    Name: alertmanager-slack-notification
                Send Resolved: true
                Title: Conditional title based on firing or resolved status
                Text: Conditional text based on firing or resolved status and alert details
                Footer: "z4ck404-alertmanager"

This configuration sets up Alertmanager to route alerts based on the severity level, and sends them to a Slack channel using the specified webhook URL.

## Secret

The Secret named `alertmanager-slack-notification` is used to store sensitive information, such as the Slack webhook URL, securely. The secret is of type Opaque and contains the following data:

    slack_webhook: "https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX"

Make sure to configure the slack_webhook with your actual Slack webhook URL to enable alert notifications to Slack.

Please refer to the individual files in this repository for more details and configuration options.