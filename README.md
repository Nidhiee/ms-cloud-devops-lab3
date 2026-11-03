# Lab 3 — hello-app on Kubernetes with Terraform

Name: Nidhie Balaji

Roll number:261100680029

## Part 1: first `terraform plan`

Paste the `Plan:` line and the summary above it.
nidhie@Nidhies-MacBook-Pro ms-cloud-devops-lab3 % terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # kubernetes_deployment_v1.hello will be created
  + resource "kubernetes_deployment_v1" "hello" {
      + id               = (known after apply)
      + wait_for_rollout = true

      + metadata {
          + generation       = (known after apply)
          + name             = "hello-app"
          + namespace        = "default"
          + resource_version = (known after apply)
          + uid              = (known after apply)
        }

      + spec {
          + min_ready_seconds         = 0
          + paused                    = false
          + progress_deadline_seconds = 600
          + replicas                  = "3"
          + revision_history_limit    = 10

          + selector {
              + match_labels = {
                  + "app" = "hello-app"
                }
            }

          + strategy (known after apply)

          + template {
              + metadata {
                  + generation       = (known after apply)
                  + labels           = {
                      + "app" = "hello-app"
                    }
                  + name             = (known after apply)
                  + resource_version = (known after apply)
                  + uid              = (known after apply)
                }
              + spec {
                  + automount_service_account_token  = true
                  + dns_policy                       = "ClusterFirst"
                  + enable_service_links             = true
                  + host_ipc                         = false
                  + host_network                     = false
                  + host_pid                         = false
                  + hostname                         = (known after apply)
                  + node_name                        = (known after apply)
                  + restart_policy                   = "Always"
                  + scheduler_name                   = (known after apply)
                  + service_account_name             = (known after apply)
                  + share_process_namespace          = false
                  + termination_grace_period_seconds = 30

                  + container {
                      + image                      = "shripada/hello-app:v1"
                      + image_pull_policy          = (known after apply)
                      + name                       = "hello-app"
                      + restart_policy             = (known after apply)
                      + stdin                      = false
                      + stdin_once                 = false
                      + termination_message_path   = "/dev/termination-log"
                      + termination_message_policy = (known after apply)
                      + tty                        = false

                      + port {
                          + container_port = 80
                          + protocol       = "TCP"
                        }

                      + resources (known after apply)
                    }

                  + image_pull_secrets (known after apply)

                  + readiness_gate (known after apply)
                }
            }
        }
    }

  # kubernetes_service_v1.hello will be created
  + resource "kubernetes_service_v1" "hello" {
      + id                     = (known after apply)
      + status                 = (known after apply)
      + wait_for_load_balancer = true

      + metadata {
          + generation       = (known after apply)
          + name             = "hello-app"
          + namespace        = "default"
          + resource_version = (known after apply)
          + uid              = (known after apply)
        }

      + spec {
          + allocate_load_balancer_node_ports = true
          + cluster_ip                        = (known after apply)
          + cluster_ips                       = (known after apply)
          + external_traffic_policy           = (known after apply)
          + health_check_node_port            = (known after apply)
          + internal_traffic_policy           = (known after apply)
          + ip_families                       = (known after apply)
          + ip_family_policy                  = (known after apply)
          + publish_not_ready_addresses       = false
          + selector                          = {
              + "app" = "hello-app"
            }
          + session_affinity                  = "None"
          + type                              = "LoadBalancer"

          + port {
              + node_port   = (known after apply)
              + port        = 8080
              + protocol    = "TCP"
              + target_port = "80"
            }

          + session_affinity_config (known after apply)
        }
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Changes to Outputs:
  + url = "http://localhost:8080"

─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.


## Part 1: `terraform output -raw url`

 nidhie@Nidhies-MacBook-Pro ms-cloud-devops-lab3 % terraform output -raw url
http://localhost:8080%  


## Part 2: plan for the v1 → v2 image change

Paste the lines that show the image change, and the `Plan:` line.
(base) nidhie@Nidhies-MacBook-Pro ms-cloud-devops-lab3 % terraform plan
kubernetes_deployment_v1.hello: Refreshing state... [id=default/hello-app]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create
  ~ update in-place

Terraform will perform the following actions:

  # kubernetes_deployment_v1.hello will be updated in-place
  ~ resource "kubernetes_deployment_v1" "hello" {
        id               = "default/hello-app"
        # (1 unchanged attribute hidden)

      ~ spec {
            # (5 unchanged attributes hidden)

          ~ template {
              ~ spec {
                    # (18 unchanged attributes hidden)

                  ~ container {
                      ~ image                      = "shripada/hello-app:v1" -> "shripada/hello-app:v2"
                        name                       = "hello-app"
                        # (10 unchanged attributes hidden)

                        # (2 unchanged blocks hidden)
                    }
                }

                # (1 unchanged block hidden)
            }

            # (2 unchanged blocks hidden)
        }

        # (1 unchanged block hidden)
    }

  # kubernetes_service_v1.hello will be created
  + resource "kubernetes_service_v1" "hello" {
      + id                     = (known after apply)
      + status                 = (known after apply)
      + wait_for_load_balancer = true

      + metadata {
          + generation       = (known after apply)
          + name             = "hello-app"
          + namespace        = "default"
          + resource_version = (known after apply)
          + uid              = (known after apply)
        }

      + spec {
          + allocate_load_balancer_node_ports = true
          + cluster_ip                        = (known after apply)
          + cluster_ips                       = (known after apply)
          + external_traffic_policy           = (known after apply)
          + health_check_node_port            = (known after apply)
          + internal_traffic_policy           = (known after apply)
          + ip_families                       = (known after apply)
          + ip_family_policy                  = (known after apply)
          + publish_not_ready_addresses       = false
          + selector                          = {
              + "app" = "hello-app"
            }
          + session_affinity                  = "None"
          + type                              = "LoadBalancer"

          + port {
              + node_port   = (known after apply)
              + port        = 8080
              + protocol    = "TCP"
              + target_port = "80"
            }

          + session_affinity_config (known after apply)
        }
    }

Plan: 1 to add, 1 to change, 0 to destroy.

─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.


## Part 3: plan that found the drift after `kubectl scale`
(base) nidhie@Nidhies-MacBook-Pro ms-cloud-devops-lab3 % kubectl scale deployment hello-app --replicas=5
deployment.apps/hello-app scaled
(base) nidhie@Nidhies-MacBook-Pro ms-cloud-devops-lab3 % kubectl get pods
NAME                         READY   STATUS    RESTARTS      AGE
hello-app-56c454cc46-5gbbg   1/1     Running   0             3m52s
hello-app-56c454cc46-5xjhr   1/1     Running   0             3m54s
hello-app-56c454cc46-7dshk   1/1     Running   0             8s
hello-app-56c454cc46-fgwmf   1/1     Running   0             8s
hello-app-56c454cc46-l7zj9   1/1     Running   0             3m53s
web-7887448d46-rzvwz         1/1     Running   3 (35m ago)   21d


## Part 4: `kubectl get svc -A` showing dev and prod
(base) nidhie@Nidhies-MacBook-Pro prod % kubectl get svc -A
NAMESPACE     NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                  AGE
default       hello-app    LoadBalancer   10.96.142.255   <pending>     8080:30425/TCP           25m
default       kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP                  21d
dev           hello-app    LoadBalancer   10.96.7.27      172.18.0.5    8080:31966/TCP           36m
kube-system   kube-dns     ClusterIP      10.96.0.10      <none>        53/UDP,53/TCP,9153/TCP   21d
prod          hello-app    LoadBalancer   10.96.179.5     172.18.0.8    8090:31897/TCP           36m


## Break it: the error after deleting `terraform.tfstate`

╷
│ Error: Error creating namespace
│ 
│   with module.hello.kubernetes_namespace_v1.env,
│   on ../../modules/hello-app/main.tf line 37, in resource "kubernetes_namespace_v1" "env":
│   37: resource "kubernetes_namespace_v1" "env" {
│ 
│ namespaces "dev" already exists

## Break it: the wrong selector

What did you see, and how did you find the cause? One or two sentences.

After changing the Service selector from `app = "hello-app"` to `app = "helo-app"`, the Development application stopped working even though Terraform applied successfully. I checked the EndpointSlices and found that there were no matching Pod endpoints because the Service selector did not match the Pod labels.


## Screenshot

Save a browser screenshot as `screenshot.png` in the repository root.
