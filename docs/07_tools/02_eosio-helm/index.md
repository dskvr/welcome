---
content_title: EOSIO Nodeos Helm Charts
link_text: EOSIO Nodeos Helm Charts
---

## Summary

* [Overview](#overview)
* [Installation](#installation)
    * [Local Deployment](#local-deployment)
    * [Cloud Deployment](#cloud-deployment)
* [Configuration](#configuration)
    * [Customize Values](#customize-values)
	* [Configure Cloud Provider](#configure-cloud-provider)
* [Tutorial](tutorial.md)

## Overview

EOSIO Helm Charts provide direct access to infrastructure-as-code (IaC) for rapid deployment of EOSIO nodes via Kubernetes. This includes support for common cloud providers such as Amazon Web Services (AWS) and Google Cloud Platform (GCP). EOSIO Helm currently available artefacts include three Helm charts:

  * `eosio`: provides the configuration umbrella for the EOSIO ecosystem.
  * `common`: provides scaffolding for infrastructure extensions support.
  * `nodeos`: provides the configuration options for the EOSIO node(s).

[[info | Note]]
| The [EOSIO Web IDE](https://github.com/EOSIO/eosio-web-ide) is another tool that allows developers to get an EOSIO network up and running quickly. It also allows developers to build and deploy EOSIO applications quickly from within a Docker-enabled web-based IDE.

## Installation

EOSIO Helm Charts can be installed and run locally via Docker Desktop or run via a cloud deployment. For quick prototyping and rapid testing you may want to select a local deployment. For production environments or distributed testing, a cloud deployment is recommended.

### Local Deployment

To install EOSIO Helm Charts for local deployment via Docker Desktop, follow these instructions:

1. Install Docker Desktop:
    * [Docker Official: Desktop](https://docs.docker.com/desktop)
1. Install Kubernetes command-line tool (kubectl):
    * [Kubernetes Official: Install and Set Up Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl)
1. Install Helm:
    * [Helm Official: Installing Helm](https://helm.sh/docs/intro/install)
1. Configure Helm and Kubernetes for deployment:
    1. Helm repo add `eosio.helm` public repository.
    1. Set Kubernetes to use the Docker Desktop context.
        1. Verify the current context using `kubectl config current-context`.
        1. If the current context is not Docker Desktop, then
            1. Get a list of contexts via `kubectl config get-contexts`.
            1. Set the current context to the Docker Desktop Kubernetes namespace via `kubectl config use-context <docker local>`.
1. Deploy a Helm Chart for EOSIO applications:
    1. Clone the EOSIO Help Charts github repository via `git clone https://github.com/EOSIO/eosio.helm.git`.
    1. Configure Helm Chart subpackages via `helm dependency update`.
        * File: [eosio/scripts/helm-dependency-update.sh](https://github.com/EOSIO/eosio.helm/blob/master/eosio/scripts/helm-dependency-update.sh)
    1. Deploy EOSIO via `helm upgrade --install eosio eosio -f eosio/local.yaml -f eosio/nodeos_config.yaml`
        * File: [eosio/local.yaml](https://github.com/EOSIO/eosio.helm/blob/master/eosio/local.yaml)
        * File: [eosio/nodeos_config.yaml](https://github.com/EOSIO/eosio.helm/blob/master/eosio/nodeos_config.yaml)

### Cloud Deployment

To install EOSIO Helm Charts for cloud deployment using a cloud provider like AWS or GCP, follow these instructions:

1. Deploy a Helm Chart for EOSIO applications:
    1. Clone the EOSIO Help Charts github repository via `git clone https://github.com/EOSIO/eosio.helm.git`.
    1. Configure Helm Chart subpackages for chain via `helm dependency update`.
        * File: [eosio/scripts/helm-dependency-update.sh](https://github.com/EOSIO/eosio.helm/blob/master/eosio/scripts/helm-dependency-update.sh)
    1. Deploy EOSIO via `helm upgrade --install eosio eosio -f eosio/primary.yaml -f eosio/nodeos_config.yaml`
        * File: [eosio/primary.yaml](https://github.com/EOSIO/eosio.helm/blob/master/eosio/primary.yaml)
        * File: [eosio/nodeos_config.yaml](https://github.com/EOSIO/eosio.helm/blob/master/eosio/nodeos_config.yaml)

## Configuration

You can customize specific values to be passed to the Helm charts. You can also configure specific artefacts within the cloud provider.

### Customize Values

1. Specify additional value files as desired using additional `-f` options. Remember that order matters and values are overridden from left-to-right in specification.
    * [Helm Official: Value Files](https://helm.sh/docs/chart_template_guide/values_files)
1. Specify values directly as command line arguments.
    * [Helm Official: Using Helm](https://helm.sh/docs/intro/using_helm)

### Configure Cloud Provider

* Cloud provider: `.Values.global.cloudProvider`
* Autoscaler: `.Values.global.nodeSelector`