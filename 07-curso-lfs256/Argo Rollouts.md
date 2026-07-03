# domain: Argo Rollouts

## Introduction

### Chapter Overview and Objectives

In this chapter, we delve into Argo Rollouts, a pivotal tool within the Argo suite, designed specifically for Continuous Delivery (CD) and GitOps practices. Argo Rollouts can be used as a stand-alone tool and therefore does not require any prior knowledge of ArgoCD (or other Argo-related tools). Through thematic discussions and hands-on labs, we aim to equip you with a comprehensive understanding of Argo Rollouts’ architecture, installation, and usage.

By the end of this chapter, you should be able to:

*   Understand and differentiate various Progressive Delivery patterns and decide when to use which.
*   Have a thorough understanding of what Argo Rollouts is and in what scenarios it might help.
*   Have an overview of Argo Rollouts architecture and functionality.

### A Primer on Progressive Delivery

#### Essentials of CI/CD and Progressive Delivery in Software Development

Continuous Integration (CI), Continuous Delivery (CD), and Progressive Delivery are key concepts in modern software development, particularly in the context of DevOps and agile practices. They represent different stages or approaches in the software release process. We will discuss them more in this chapter.

#### Continuous Integration

Continuous Integration is a development practice where developers frequently integrate their code into a shared repository, preferably several times daily. Each integration is then verified by an automated build and automated tests.

#### CI Features

*   **Frequent code commits**: Encourage developers to often integrate their code into the main branch, reducing integration challenges.
*   **Automated tests**: Cover frequent code commits. Automatically running tests on the new code to ensure it integrates well with the existing codebase. This does not only include unit tests, but also any other higher-order testing method, such as integration- or end-to-end tests.
*   **Immediate problem detection**: Allows for quick detection and fixing of integration issues.
*   **Reduced integration problems**: Help to minimize the problems associated with integrating new code.

The main goal of CI is to provide rapid feedback so that if a defect is introduced into the code base, it is identified and corrected as soon as possible.

Once code is in our main branch, it is not deployed in production or even released. This is where the concept of Continuous Delivery comes into play.

### Continuous Delivery

Continuous Delivery is an extension of CI, ensuring the software can be reliably released anytime. It involves the automation of the entire software release process.

#### Automated release process

*   Every change that passes the automated tests can be released to production through an automated process.
*   Reliable deployments: Ensure that the software is always in a deployable state.
*   Rapid release cycles: Facilitate frequent and faster release cycles.
*   Close collaboration between teams: A close alignment between development, QA, and operations teams is required.

The objective of Continuous Delivery is to establish a process where software deployments become predictable, routine, and can be executed on demand.

### Progressive Delivery

Progressive delivery is often described as an evolution of continuous delivery. It focuses on releasing updates of a product in a controlled and gradual manner, thereby reducing the risk of the release, typically coupling automation and metric analysis to drive the automated promotion or rollback of the update.

#### Progressive Delivery Features

##### Canary releases

Gradually roll out the change to a small subset of users before rolling it out to the entire user base.

##### Feature flags

Control who gets to see what feature in the application, allowing for selective and targeted deployment.

##### Experiments & A/B testing

Test different versions of a feature with different segments of the user base.

##### Phased rollouts

Slowly roll out features to incrementally larger segments of the user base, monitoring and adjusting based on feedback.

The primary goal of Progressive Delivery is to reduce the risk associated with releasing new features and to enable faster iteration by getting early feedback from users.

### Deployment Strategies

Every software system is different, and deploying complex systems oftentimes requires additional steps and checks. This is why different deployment strategies emerged over time to manage the process of deploying new software versions in a production environment.

These strategies are an integral part of DevOps practices, especially in the context of CI/CD workflows. The choice of a deployment strategy can significantly impact the availability, reliability, and user experience of a software application or software service.

On the following pages, we will present the four most important deployment strategies and discuss their impact on user experience during deployment:

*   Recreate/fixed deployment
*   Rolling update
*   Blue-green deployment
*   Canary deployment

# Building Blocks of Argo Rollouts

In this section, we will discuss the building blocks of Argo Rollouts. To give you an overview of what to expect, we’ll briefly describe the relevant components of an Argo Rollouts setup before we discover them in more detail.

## Argo Rollouts Components

### Argo Rollouts Controller

An operator that manages Argo Rollout Resources. It reads all the details of a rollout (and other resources) and ensures the desired cluster state.

### Argo Rollout Resource

A custom Kubernetes resource managed by the Argo Rollouts Controller. It is largely compatible with the native Kubernetes Deployment resource, adding additional fields that manage the stages, thresholds, and techniques of sophisticated deployment strategies, including canary and blue-green deployments.

### Ingress and the Gateway API

The Kubernetes Ingress resource is used to enable traffic management for various traffic providers such as service meshes (e.g., Istio or Linkerd) or Ingress Controllers (e.g., Nginx Ingress Controller).

The Kubernetes Gateway API is also supported with a separate plugin and provides similar functionality.

### Service

Argo Rollouts utilizes the Kubernetes Service resource to redirect ingress traffic to the respective workload version by adding specific metadata to a Service.

### ReplicaSet

Standard Kubernetes ReplicaSet resource used by Argo Rollouts to keep track of different versions of an application deployment.

### AnalysisTemplate and AnalysisRun

Analysis is an optional feature of Argo Rollouts and enables the connection of Rollouts to a monitoring system. This allows automation of promotions and rollbacks. To perform an analysis an AnalysisTemplate defines a metric query and their expected result. If the query matches the expectation, a Rollout will progress or rollback automatically, if it doesn’t. An AnalysisRuns is an instantiation of an AnalysisTemplate (similar to Kubernetes Jobs).

### Metric Providers

Metric providers can be used to automate promotions or rollbacks of a rollout. Argo Rollouts provides native integration for popular metric providers such as Prometheus and other monitoring systems.

Please note, that not all of the mentioned components are mandatory to every Argo Rollouts setup. The usage of Analysis resources or metric providers is entirely optional and relevant for more advanced use cases. Also note that the Argo Rollouts components are independent of other Argo projects (like Argo CD or Argo Workflows) and do not require them to function properly.

# Argo Rollouts Architecture and Core Components

## Argo Rollouts

Here, we will explore the Argo Rollouts resource, which is the central element in Argo Rollouts, enabling advanced deployment strategies. A Rollout, in essence, is a Kubernetes resource that closely mirrors the functionality of a Kubernetes Deployment object. However, it steps in as a more advanced substitute for Deployment objects, particularly in scenarios demanding intricate deployment of progressive delivery techniques.

### Key Features of Argo Rollouts

Argo Rollouts outshine regular Kubernetes Deployments with several enhanced features.

#### Argo Rollouts Functionalities

##### Blue-green deployments

This approach minimizes downtime and risk by switching traffic between two versions of the application.

##### Canary deployments

Gradually roll out changes to a subset of users to ensure stability before full deployment.

##### Advanced traffic routing

Integrates seamlessly with ingress controllers and service meshes, facilitating sophisticated traffic management.

##### Integration with metric providers

Offers analytical insights for blue-green and canary deployments, enabling informed decisions.

##### Automated decision making

Automatically promote or roll back deployments based on the success or failure of defined metrics.

The Rollout resource is a custom Kubernetes resource introduced and managed by the Argo Rollouts Controller. This Kubernetes controller monitors resources of type Rollout and ensures that the described state will be reflected in the cluster.

The Rollout resource maintains high compatibility with the conventional Kubernetes Deployment resource but is augmented with additional fields. These fields are instrumental in governing the phases, thresholds, and methodologies of advanced deployment approaches, such as canary and blue-green strategies.

It’s crucial to understand that the Argo Rollouts controller is attuned exclusively to changes in Rollout resources. It remains inactive for standard deployment resources. Consequently, to use the Argo Rollouts for existing Deployments, a migration from traditional Deployments to Rollouts is required.

### Migrating Existing Deployments to Rollouts

The similarity of Deployments and Rollouts spec makes it easier to convert from one to the other resource type. Argo Rollouts supports a great way to migrate existing Deployment resources to Rollouts.

By providing a `spec.workloadRef` instead of `spec.template` a Rollout can refer to a Deployments template.

The Rollout will fetch the template information from the Deployment and start the in the Rollout specified number of pods.

Please note, that lifecycles of Deployment and Rollouts are distinct and managed by their respective controllers. This means that the Kubernetes Deployment controller will not start to manage Pods created by the Rollout. Also, the Rollout will not start to manage pods that are controlled by the Deployment.

This enables a zero-downtime introduction of Argo Rollouts to your existing cluster. It furthermore makes experimentation with multiple deployment scenarios possible.

### Experiments

Experiments are an extended feature of Argo Rollouts designed to test and evaluate changes in two or more versions of an application in a controlled, temporary environment. The Experiment custom resource can launch AnalysisRuns alongside ReplicaSets. This is useful to confirm that new ReplicaSets are running as expected.

You can use experiments in Argo Rollouts to test different versions of your app at the same time. This is like doing A/B/C testing. You can set up each experiment with its own version of the app to see which one works best. Each experiment uses a template to define its specific version of the app.

The great thing about these experiments is that you can run several of them simultaneously, and each one is separate from the others. This means they don't interfere with each other.