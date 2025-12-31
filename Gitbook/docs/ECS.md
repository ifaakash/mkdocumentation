# ECS

## How to deploy a DAEMON service in ECS

```
############################ DATADOG SERVICE ############################

resource "aws_ecs_service" "datadog" {
  name                    = "${var.environment}-${var.project_name}-datadog"
  cluster                 = aws_ecs_cluster.main.id
  task_definition         = var.datadog_task_definition_arn
  scheduling_strategy     = "DAEMON"
  enable_ecs_managed_tags = true
  propagate_tags          = "TASK_DEFINITION"
  enable_execute_command  = false
  launch_type = "EC2"

  deployment_circuit_breaker {
    enable   = true
    rollback = true
  }

  tags = {
    Name = "${var.environment}-${var.project_name}-datadog"
  }
}
```

<div style="background-color: #fff3cd; border-left: 4px solid #ffc107; padding: 10px; margin: 10px 0;">
📌 <strong>NOTE:</strong> A Daemon service dont need the capacity provider block. This is not supported for daemon service. 
Daemon service will need to have launch_type parameter configured. This is used to mention the infra over which this service will be running. 
</div>

If launch_type is EC2, then we will have the containers running over every EC2 instnace, that is configured to run inside the default capacity provider of the ECS Cluster
The daemon service dont need:
capacity_provider_strategy
load_balancer
ordered_placement_strategy

### How DAEMON Scheduling Works

`scheduling_strategy = "DAEMON"`

ECS runs exactly one Datadog agent task per EC2 instance in your cluster
It does NOT run one per service
It runs at the host/instance level, not the service level
Let's say your ASG scales to 3 EC2 instances:

```
Instance 1:
├── Datadog Agent (1 task) ← DAEMON
├── Laravel Task 1 ← REPLICA
└── MODX Task 1 ← REPLICA
Instance 2:
├── Datadog Agent (1 task) ← DAEMON
├── Laravel Task 2 ← REPLICA
└── MODX Task 2 ← REPLICA
Instance 3:
├── Datadog Agent (1 task) ← DAEMON
└── Laravel Task 3 ← REPLICA
```

### Configuration Requirement
For the agent to monitor your Laravel and MODX containers, your Datadog task definition should have the Docker socket mounted:
json

```
"mountPoints": [
  {
    "sourceVolume": "docker_sock",
    "containerPath": "/var/run/docker.sock",
    "readOnly": true
  }
]
```

This allows the Datadog agent to discover and monitor all containers on the host.
