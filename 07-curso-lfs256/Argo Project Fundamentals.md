#  fundamental: Argo Project Fundamentals

## ¿Qué es un *Canary Deployment*?

Un **canary deployment** es una estrategia en la que una nueva versión de una aplicación se despliega **solo a un pequeño porcentaje de usuarios** antes de liberarla al resto.

La idea es “probar en producción” con un grupo reducido para detectar errores sin afectar a todos los usuarios.

Cómo funciona
Se despliega la nueva versión a un 1–5% de los usuarios.

Se monitorean métricas: errores, latencia, consumo, logs.

Si todo va bien, se incrementa gradualmente el porcentaje.

Si algo falla, se revierte rápidamente a la versión estable.

### **Ventajas**

*   Riesgo muy bajo.
*   Detección temprana de fallos.
*   Permite comparar comportamiento entre versiones.

## ¿Qué es un *Blue-Green Deployment*?

Un **blue-green deployment** consiste en tener **dos entornos completos y paralelos**:

*   **Blue**: versión actual en producción.
*   **Green**: nueva versión lista para reemplazar a Blue.

Cuando la nueva versión está validada, el tráfico se redirige del entorno Blue al Green **de golpe**, sin interrupciones.

### **Cómo funciona**

*   Se prepara la nueva versión en el entorno Green.
*   Se realizan pruebas sin afectar a usuarios.
*   Cuando todo está listo, se cambia el tráfico al entorno Green.
*   El entorno Blue queda como respaldo para un rollback inmediato.

### **Ventajas**

*   Cero tiempo de inactividad.
*   Rollback instantáneo.
*   Entornos totalmente aislados.

## Diferencias clave

| Estrategia | Cómo despliega | Riesgo | Rollback | Ideal para |
| --- | --- | --- | --- | --- |
| **Canary** | Gradual, por porcentaje de usuarios | Muy bajo | Fácil | Cambios frecuentes, experimentación |
| **Blue-Green** | Cambio total entre entornos | Bajo | Instantáneo | Releases grandes y controlados |

# Introduction to Argo

By the end of this chapter, you should be able to:

*   Explain the key concepts behind Argo.
*   Describe common use cases for the Argo suite.
*   Identify Kubernetes's role in supporting Argo.
*   Install Kubernetes to prepare your environment for the Argo suite.

# Essential Concepts for Argo

## What Is GitOps?

GitOps is a collection of principles that guide modern software delivery and operations. It provides a structured, reliable way to manage infrastructure and applications through version control. Let's explore the five key aspects of GitOps that streamline, standardize, and optimize development and deployment.

### Core Elements

#### Declarative configuration

The first principle of GitOps is declarative configuration. This means defining what the desired state of your system should look like, rather than how to get there. In practice, developers describe the intended outcome—for example, specifying that an application should run three containers. Automated agents then compare the current system state to this desired configuration and make the necessary adjustments, such as adding or removing containers. This approach contrasts with the traditional imperative style, in which specific commands are issued step by step to achieve the desired setup.

#### Immutable storage

In GitOps, Git serves not only as a version-control system but also as an immutable storage for configurations. Once a configuration is committed to Git, it becomes a fixed reference point, providing a reliable record that supports reproducibility and traceability. This makes Git the single source of truth for your system’s desired state. Although Git is the most commonly used tool for this purpose, the core principles of GitOps can also be applied with other version control systems.

#### Automation

Automation is a core principle of GitOps. It focuses on removing manual steps after changes are committed to version control. Once an update is made, software agents take over, analyzing the difference between the system’s current state and the desired state defined in the repository. They then apply the necessary changes to implement the newly declared configuration, bringing the system into alignment. This continuous reconciliation process represents the closed-loop nature of GitOps, ensuring that deployments remain consistent, reliable, and up to date without human intervention.

#### Closed loop

In GitOps, the term closed loop refers to the continuous feedback process that compares the system’s actual state with its desired state. Automated agents constantly monitor for differences between the two and take corrective action whenever the system drifts from the configuration defined in version control. This ensures the environment is always moving toward the declared state, maintaining consistency, reliability, and predictability in operations.

# Argo Overview

## What Is Argo?

Argo is a set of Kubernetes-native tools that enhance the workflow management capabilities of Kubernetes. It includes Argo Continuous Delivery (CD) for state management, Argo Workflows for running complex jobs, Argo Events for event-based dependency management, and Argo Rollouts for progressive delivery. These tools are designed to help you automate and manage tasks in a Kubernetes environment, making it easier to deploy, update, and manage applications. Each of these tools can run independently and do not require the others to work, but they are capable of working together.

# Benefits of Using Argo

Using Argo for continuous delivery offers several benefits. We will now explore some of them.

Argo is a popular open source tool that brings the practicality of GitOps to anyone who uses Kubernetes. By enabling GitOps, Argo can enhance the robustness, security, and reliability of Kubernetes environments. This technical practice can be highly beneficial to DevOps teams, improving their efficiency.

Argo is designed from the ground up for a modern containerized environment. Argo CD as an example is equipped with a powerful GUI and CLI, enabling all seniority levels of engineers to work with it and implement highly sophisticated custom solutions. Argo Rollouts on the other hand supports progressive delivery strategies like canary and blue-green deployments, which are difficult to implement in plain Kubernetes.

The automation provided by Argo speeds up the entire process of pushing new features, leading to faster and more frequent deployments. If any issues are detected based on defined metrics, the deployment can be automatically rolled back to the previous working deployment, allowing for faster error resolution.

One of the benefits of using Git and GitOps principles is the built-in auditing, which gives you the benefit of having the entire history of your software tracked over time. Argo makes it easy to specify, schedule, and coordinate the running of complex workflows and applications on Kubernetes.

Argo's integration with other Cloud Native Computing Foundation (CNCF) projects is a significant advantage. It uses or integrates with CNCF projects like gRPC, Prometheus, NATS, Helm, and CloudEvents. This integration allows for seamless interoperability and enhanced functionality within the cloud native ecosystem. For instance, Prometheus can be used for monitoring and alerting purposes, while Helm can help manage Kubernetes applications. NATS can provide a high-performance messaging system, and CloudEvents can offer a standard way to define the format of event data in a consistent way across services, platforms, and systems. Therefore, by using Argo, you can leverage the benefits of these CNCF projects, leading to more robust, scalable, and efficient cloud native applications.

# Step-by-Step: Deploying Kubernetes for Argo

This section serves as a general guide to installing a Kubernetes cluster locally. This basic setup of a local Kubernetes cluster can be used for any labs throughout this course.

Kubernetes serves as the underlying orchestration platform that facilitates the deployment, scaling, and management of containerized applications. Argo, which includes projects like Argo Workflows and Argo CD, leverages Kubernetes to seamlessly integrate and automate workflows and continuous delivery pipelines. Kubernetes provides the essential infrastructure for container orchestration, allowing Argo to effectively coordinate and execute tasks, manage dependencies, and ensure the efficient deployment and operation of applications in a distributed and scalable environment.

# Installing Kubernetes Locally

Installing Docker
Ensure Docker is installed and running on your machine:

*   Visit the Docker website for detailed installation instructions and to download the software.
*   Choose the appropriate version for your operating system.
*   Download and install Docker by following the on-screen instructions.
*   After installing, verify that Docker is correctly installed by opening the terminal and typing docker –version. If Docker is installed correctly, this command will display the installed version of Docker.
*   The labs in this course were tested on Ubuntu 24.04 on a 2 CPU, 8GB machine.

If you are on Ubuntu 24.04, you can install Docker using the convenience script with the following command:

`curl https://get.docker.com | bash`

### Installing kubectl

Instructions for downloading and installing kubectl can be found in the Kubernetes official documentation.

Make sure kubectl is installed by running the kubectl version –client command in your terminal. If kubectl is installed correctly, this command will display the installed version of kubectl.

To install kubectl on Ubuntu 24.04, use the following commands:

`curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"`
`sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl`