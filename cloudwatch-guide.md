# 📊 CloudWatch — Monitoring, Alarms & Log Groups

A practical guide to using AWS CloudWatch for infrastructure monitoring and alerting.

---

## What is CloudWatch?

AWS CloudWatch is a monitoring and observability service. It collects metrics, logs, and events from AWS resources and lets you set alarms, visualise data, and respond to operational changes automatically.

---

## Key CloudWatch Concepts

| Concept | Description |
|---|---|
| **Metrics** | Numerical data points (e.g. CPU usage, network in/out) |
| **Alarms** | Triggers an action when a metric crosses a threshold |
| **Log Groups** | Containers for log streams from services or instances |
| **Dashboards** | Custom visualisations of your metrics |

---

## Viewing EC2 Metrics

CloudWatch automatically collects these metrics for every EC2 instance:

| Metric | Description |
|---|---|
| `CPUUtilization` | % CPU in use |
| `NetworkIn / NetworkOut` | Bytes transferred |
| `DiskReadOps / DiskWriteOps` | Disk activity |
| `StatusCheckFailed` | Instance health check status |

To view: **CloudWatch → Metrics → EC2 → Per-Instance Metrics**

---

## Creating a CPU Alarm

Set an alarm to notify you when CPU usage exceeds 80%:

1. Go to **CloudWatch → Alarms → Create Alarm**
2. Click **"Select Metric"** → EC2 → Per-Instance Metrics
3. Choose **`CPUUtilization`** for your instance
4. Configure the condition:

```
Threshold type:    Static
Condition:         Greater than
Value:             80
Period:            5 minutes
Datapoints:        1 out of 1
```

5. Set notification action:
   - Create a new **SNS topic** (e.g. `cpu-alert-topic`)
   - Add your email address as a subscriber
   - Confirm the subscription in your inbox

6. Name the alarm: `High-CPU-EC2-Alert`

> 💡 You'll get an email whenever CPU spikes above 80% for 5 consecutive minutes.

---

## Setting Up CloudWatch Logs (EC2)

To push system logs from EC2 to CloudWatch:

### Install the CloudWatch Agent

```bash
# Amazon Linux 2
sudo yum install amazon-cloudwatch-agent -y

# Ubuntu
sudo apt install amazon-cloudwatch-agent -y
```

### Create a config file

```json
{
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/syslog",
            "log_group_name": "my-ec2-syslogs",
            "log_stream_name": "{instance_id}"
          }
        ]
      }
    }
  }
}
```

### Start the agent

```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -a fetch-config \
  -m ec2 \
  -c file:/path/to/config.json \
  -s
```

---

## Creating a Simple Dashboard

1. Go to **CloudWatch → Dashboards → Create Dashboard**
2. Name it (e.g. `EC2-Overview`)
3. Add widgets:
   - **Line graph** → CPUUtilization
   - **Number widget** → NetworkIn
   - **Alarm status widget** → your CPU alarm

---

## Key Takeaways

- CloudWatch is the central monitoring hub for all AWS services
- Alarms + SNS = automated alerting to email, SMS, or Lambda
- The CloudWatch agent unlocks memory and disk metrics (not collected by default)
- Log Insights allows powerful querying of log data using a SQL-like syntax
