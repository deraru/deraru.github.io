# Wordpress

## Set up

1. gcloud components update preview
1. gcloud config set account deraru@example.com
1. gcloud auth login
1. gcloud init my-project-id
1. cd ~/my-project-id
1. gcloud config set compute/zone asia-east1-c

## Create Wordpress website

1. gcloud preview container clusters create my-cluster-name --num-nodes 1 --machine-type g1-small
1. gcloud preview container pods create --name my-pod-name --image tutum/wordpress --port 80
1. gcloud compute firewall-rules create my-firewall-name --allow tcp:80 --target-tags k8s-my-cluster-name-node

## Check the website

1. gcloud preview container pods list
1. http://host-ip-address

## Clean up

1. gcloud preview container clusters delete my-cluster-name
1. gcloud compute firewall-rules delete my-firewall-name

## Reference

- [Hello Wordpress - Google Container Engine — Google Cloud Platform](https://cloud.google.com/container-engine/docs/hello-wordpress)

# Guestbook

## Setup

1. wget https://cloud.google.com/container-engine/docs/downloads/guestbook.zip
1. unzip guestbook.zip

## Create Guestbook web service

1. gcloud preview container clusters create my-cluster-name
1. gcloud preview container pods create --config-file redis-master-pod.json
1. gcloud preview container services create --config-file redis-master-service.json
1. gcloud preview container replicationcontrollers create --config-file redis-worker-controller.json
1. gcloud preview container services create --config-file redis-worker-service.json
1. gcloud preview container replicationcontrollers create --config-file guestbook-controller.json
1. gcloud preview container services create --config-file guestbook-service.json
1. gcloud compute firewall-rules create my-firewall-name --allow tcp:3000 --target-tags k8s-my-cluster-name-node

## Check the web service

1. gcloud compute forwarding-rules list
1. http://forwarding-rule-ip-address:3000

## Clean up

1. gcloud preview container clusters delete my-cluster-name
1. gcloud compute forwarding-rules delete guestbook
1. gcloud compute target-pools delete guestbook
1. gcloud compute firewall-rules delete my-firewall-name

## Reference

- [Getting Started - Google Container Engine — Google Cloud Platform](https://cloud.google.com/container-engine/docs/guestbook)

# Google Cloud Platform Tips

## Connect to VM

- gcloud compute ssh my-vm-name
