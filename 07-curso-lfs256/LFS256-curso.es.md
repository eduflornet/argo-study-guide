# **Conceptos**

## **¿Qué es un *Canary Deployment*?**

Un **canary deployment** es una estrategia en la que una nueva versión de una aplicación se despliega **solo a un pequeño porcentaje de usuarios** antes de liberarla al resto.

La idea es “probar en producción” con un grupo reducido para detectar errores sin afectar a todos los usuarios.

La idea es “probar en producción” con un grupo reducido para detectar errores sin afectar a todos los usuarios.

Cómo funciona
Se despliega la nueva versión a un 1–5% de los usuarios.

Se monitorean métricas: errores, latencia, consumo, logs.

Si todo va bien, se incrementa gradualmente el porcentaje.

Si algo falla, se revierte rápidamente a la versión estable.

### **Ventajas**

* Riesgo muy bajo.
* Detección temprana de fallos.
* Permite comparar comportamiento entre versiones.

## **¿Qué es un *Blue-Green Deployment*?**

Un **blue-green deployment** consiste en tener **dos entornos completos y paralelos**:

* **Blue**: versión actual en producción.
* **Green**: nueva versión lista para reemplazar a Blue.

Cuando la nueva versión está validada, el tráfico se redirige del entorno Blue al Green **de golpe**, sin interrupciones.

### **Cómo funciona**

* Se prepara la nueva versión en el entorno Green.
* Se realizan pruebas sin afectar a usuarios.
* Cuando todo está listo, se cambia el tráfico al entorno Green.
* El entorno Blue queda como respaldo para un rollback inmediato.

### **Ventajas**

* Cero tiempo de inactividad.
* Rollback instantáneo.
* Entornos totalmente aislados.

## **Diferencias clave**

| Estrategia | Cómo despliega | Riesgo | Rollback | Ideal para |
| ----- | ----- | ----- | ----- | ----- |
| **Canary** | Gradual, por porcentaje de usuarios | Muy bajo | Fácil | Cambios frecuentes, experimentación |
| **Blue-Green** | Cambio total entre entornos | Bajo | Instantáneo | Releases grandes y controlados |


# **Introducción a Argo**

Introducción a Argo
Introducción
Resumen y Objetivos del Capítulo
Antes de empezar a trabajar con Argo y explorar sus potentes características de gestión de flujos de trabajo en Kubernetes, es importante establecer una sólida comprensión de los conceptos básicos. Este capítulo proporciona esa base al presentar las ideas esenciales que necesitarás para entender cómo opera Argo en un entorno de Kubernetes.

Al final de este capítulo, deberías ser capaz de:

Explicar los conceptos clave detrás de Argo.
Describir casos de uso comunes para el conjunto de herramientas de Argo.
Identificar el rol de Kubernetes en el soporte a Argo.
Instalar Kubernetes para preparar tu entorno para el conjunto de herramientas de Argo.

# **Conceptos Esenciales para Argo**

Introducción a Argo

## **Conceptos Esenciales para Argo**

### **¿Qué es GitOps?**

GitOps es un conjunto de principios que guían la entrega y las operaciones de software moderno. Proporciona una forma estructurada y fiable de gestionar la infraestructura y las aplicaciones a través del control de versiones. Exploremos los cinco aspectos clave de GitOps que agilizan, estandarizan y optimizan el desarrollo y el despliegue.

### **Elementos Centrales**

#### **Configuración Declarativa**

El primer principio de GitOps es la configuración declarativa. Esto significa definir cómo debería ser el estado deseado de tu sistema, en lugar de cómo llegar a él. En la práctica, los desarrolladores describen el resultado previsto; por ejemplo, especificando que una aplicación debe ejecutar tres contenedores. Los agentes automatizados comparan entonces el estado actual del sistema con esta configuración deseada y realizan los ajustes necesarios, como añadir o eliminar contenedores. Este enfoque contrasta con el estilo imperativo tradicional, en el que se emiten comandos específicos paso a paso para lograr la configuración deseada.

#### **Almacenamiento Inmutable**

En GitOps, Git no solo sirve como un sistema de control de versiones, sino también como un almacenamiento inmutable para las configuraciones. Una vez que una configuración se confirma en Git, se convierte en un punto de referencia fijo, proporcionando un registro fiable que apoya la reproducibilidad y la trazabilidad. Esto convierte a Git en la única fuente de verdad para el estado deseado de tu sistema. Aunque Git es la herramienta más utilizada para este propósito, los principios básicos de GitOps también se pueden aplicar con otros sistemas de control de versiones.

#### **Automatización**

La automatización es un principio central de GitOps. Se centra en eliminar los pasos manuales después de que los cambios se confirman en el control de versiones. Una vez que se realiza una actualización, los agentes de software toman el control, analizando la diferencia entre el estado actual del sistema y el estado deseado definido en el repositorio. A continuación, aplican los cambios necesarios para implementar la configuración recién declarada, alineando el sistema. Este proceso de reconciliación continua representa la naturaleza de bucle cerrado de GitOps, asegurando que los despliegues permanezcan consistentes, fiables y actualizados sin intervención humana.

#### **Bucle Cerrado (Closed loop)**

En GitOps, el término bucle cerrado se refiere al proceso de retroalimentación continua que compara el estado real del sistema con su estado deseado. Los agentes automatizados monitorean constantemente las diferencias entre los dos y toman medidas correctivas cada vez que el sistema se desvía de la configuración definida en el control de versiones. Esto asegura que el entorno siempre se mueva hacia el estado declarado, manteniendo la consistencia, la fiabilidad y la previsibilidad en las operaciones.

# **Introducción a Argo**

## **Resumen de Argo**

### **¿Qué es Argo?**

Argo es un conjunto de herramientas nativas de Kubernetes que mejoran las capacidades de gestión de flujos de trabajo de Kubernetes. Incluye Argo Continuous Delivery (CD) para la gestión de estado, Argo Workflows para ejecutar trabajos complejos, Argo Events para la gestión de dependencias basada en eventos y Argo Rollouts para la entrega progresiva. Estas herramientas están diseñadas para ayudarte a automatizar y gestionar tareas en un entorno de Kubernetes, facilitando el despliegue, la actualización y la gestión de aplicaciones. Cada una de estas herramientas puede funcionar de forma independiente y no requiere de las otras para funcionar, pero son capaces de trabajar juntas.

Vamos a discutirlas con más detalle.

### **Argo Continuous Delivery (CD)**

Argo CD es una herramienta declarativa de entrega continua (CD) de GitOps diseñada para Kubernetes. Automatiza el proceso de aplicación de manifiestos de Kubernetes desde un repositorio de Git a un clúster y monitorea continuamente el repositorio en busca de cambios. Cuando se detectan actualizaciones, Argo CD sincroniza automáticamente el clúster para que coincida con la configuración deseada definida en Git. Esto lo convierte en una herramienta ideal para gestionar tanto la infraestructura como los despliegues de aplicaciones, asegurando que los entornos de producción siempre reflejen el estado exacto especificado en el control de versiones.

### **Argo Workflows**

Argo Workflows, como extensión de la API de Kubernetes, presenta un nuevo Workflow CRD, que proporciona a las empresas un recurso por lotes altamente adaptable, similar a un trabajo de Kubernetes. Esta extensibilidad hace que Workflows sea versátil y aplicable en diversos dominios, destacando especialmente su eficacia en aprendizaje automático (ML) y canalizaciones de datos. El uso de Argo Workflows en estos contextos se ha convertido en una estrategia para numerosas empresas, desvelando su potencial para orquestar procesos complejos sin problemas.

En esencia, Argo Workflows opera con un conjunto único de características y casos de uso. Cada paso dentro de un flujo de trabajo se ejecuta como un pod, lo que contribuye a la modularidad y escalabilidad del proceso general. Este diseño permite un paralelismo optimizado y un uso eficiente de los recursos. Argo Workflows funciona principalmente bien en escenarios de procesamiento y automatización de datos, como lo demuestra el patrón de entrada-salida (fan-out fan-in). Este patrón permite que el flujo de trabajo distribuya ampliamente las tareas, las ejecute simultáneamente y luego consolide los resultados, lo que convierte a Argo Workflows en una opción robusta para escenarios que exigen la manipulación compleja de datos y procesos automatizados.

### **Argo Events**

Argo Events es un marco de automatización de flujos de trabajo basado en eventos para Kubernetes que permite activar objetos de Kubernetes, flujos de trabajo de Argo, cargas de trabajo sin servidor y otros procesos en respuesta a eventos de diversas fuentes, como webhooks, S3, programaciones, colas de mensajes, GCP PubSub y más. Admite eventos de más de 20 fuentes y permite personalizar la lógica de restricciones a nivel empresarial para la automatización del flujo de trabajo.

Argo Events se compone de dos componentes principales: desencadenadores (Triggers) y fuentes de eventos (Event Sources). Los desencadenadores ejecutan acciones cuando ocurre un evento, mientras que las fuentes de eventos generan eventos.

Algunos casos de uso de Argo Events incluyen la automatización de flujos de trabajo de investigación, el diseño de una canalización completa de CI/CD y la automatización integral mediante la combinación de Argo Events, flujos de trabajo y canalizaciones, CD e implementación.

### **Argo Rollouts**

Argo Rollouts es un controlador de entrega progresivo para Kubernetes. Surgió por necesidad ante la falta de estrategias de implementación más sofisticadas en Kubernetes. Argo Rollouts proporciona estrategias de actualización blue-green y canary, se integra con mallas de servicios y controladores de entrada para gestionar el tráfico, y automatiza la promoción y la reversión según el análisis. Se utiliza para implementar de forma segura nuevas funciones de la aplicación en producción sin necesidad de análisis, pruebas ni intervención manual.

# **Beneficios de Usar Argo**

## **Beneficios de Usar Argo**

Usar Argo para la entrega continua ofrece varios beneficios. Ahora exploraremos algunos de ellos.

Argo es una popular herramienta de código abierto que ofrece la practicidad de GitOps a cualquier usuario de Kubernetes. Al habilitar GitOps, Argo puede mejorar la robustez, la seguridad y la fiabilidad de los entornos de Kubernetes. Esta práctica técnica puede ser muy beneficiosa para los equipos de DevOps, mejorando su eficiencia.

Argo está diseñado desde cero para un entorno contenedorizado moderno. Argo CD, por ejemplo, cuenta con una potente interfaz gráfica de usuario (GUI) y una interfaz de línea de comandos (CLI), lo que permite a ingenieros de todos los niveles trabajar con él e implementar soluciones personalizadas altamente sofisticadas. Argo Rollouts, por otro lado, admite estrategias de entrega progresiva, como las implementaciones canarias y blue-green, que son difíciles de implementar en Kubernetes simple.

La automatización que ofrece Argo agiliza todo el proceso de implementación de nuevas funciones, lo que permite implementaciones más rápidas y frecuentes. Si se detectan problemas según las métricas definidas, la implementación se puede revertir automáticamente a la implementación anterior, lo que permite una resolución de errores más rápida.

Una de las ventajas de usar Git y los principios de GitOps es la auditoría integrada, que permite rastrear todo el historial de su software a lo largo del tiempo. Argo facilita la especificación, programación y coordinación de la ejecución de flujos de trabajo y aplicaciones complejos en Kubernetes.

La integración de Argo con otros proyectos de la Cloud Native Computing Foundation (CNCF) es una ventaja significativa. Utiliza o se integra con proyectos de la CNCF como gRPC, Prometheus, NATS, Helm y CloudEvents. Esta integración permite una interoperabilidad perfecta y una funcionalidad mejorada dentro del ecosistema nativo de la nube. Por ejemplo, Prometheus se puede utilizar para fines de monitoreo y alerta, mientras que Helm puede ayudar a gestionar aplicaciones de Kubernetes. NATS puede proporcionar un sistema de mensajería de alto rendimiento, y CloudEvents puede ofrecer una forma estándar de definir el formato de los datos de eventos de manera consistente en todos los servicios, plataformas y sistemas. Por lo tanto, al usar Argo, puedes aprovechar los beneficios de estos proyectos de la CNCF, lo que conduce a aplicaciones nativas de la nube más robustas, escalables y eficientes.

Ten en cuenta que profundizaremos en cada uno de estos temas en los capítulos siguientes.

# **13.Paso a Paso: Desplegando Kubernetes para Argo**

### **Paso a Paso: Desplegando Kubernetes para Argo**

Esta sección sirve como una guía general para instalar un clúster de Kubernetes localmente. Esta configuración básica de un clúster de Kubernetes local se puede utilizar para cualquier laboratorio a lo largo de este curso.

Kubernetes funciona como la plataforma de orquestación subyacente que facilita la implementación, el escalado y la gestión de aplicaciones en contenedores. Argo, que incluye proyectos como Argo Workflows y Argo CD, aprovecha Kubernetes para integrar y automatizar fluidamente flujos de trabajo y canales de entrega continua. Kubernetes proporciona la infraestructura esencial para la orquestación de contenedores, lo que permite a Argo coordinar y ejecutar tareas eficazmente, gestionar dependencias y garantizar la implementación y el funcionamiento eficientes de las aplicaciones en un entorno distribuido y escalable.

# **14.Instalando Kubernetes Localmente**

Introducción a Argo

### **Instalando Kubernetes Localmente**

Instalando Docker
Asegúrate de que Docker esté instalado y en funcionamiento en tu máquina:

Visita el sitio web de Docker para obtener instrucciones detalladas de instalación y para descargar el software.
Elige la versión apropiada para tu sistema operativo.
Descarga e instala Docker siguiendo las instrucciones en pantalla.
Después de instalar, verifica que Docker esté correctamente instalado abriendo la terminal y escribiendo `docker --version`. Si Docker está instalado correctamente, este comando mostrará la versión instalada de Docker.
Los laboratorios de este curso fueron probados en Ubuntu 24.04 en una máquina de 2 CPU y 8GB.

Si estás en Ubuntu 24.04, puedes instalar Docker usando el script de conveniencia con el siguiente comando:

curl https://get.docker.com | bash

### **Instalando kubectl**

Las instrucciones para descargar e instalar kubectl se pueden encontrar en la documentación oficial de Kubernetes.

Asegúrate de que kubectl esté instalado ejecutando el comando `kubectl version --client` en tu terminal. Si kubectl está instalado correctamente, este comando mostrará la versión instalada de kubectl.

Para instalar kubectl en Ubuntu 24.04, usa los siguientes comandos:

curl \-LO "https://dl.k8s.io/release/$(curl \-L \-s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install \-o root \-g root \-m 0755 kubectl /usr/local/bin/kubectl

## **Instalando kind y Creando un Clúster**

Instalando kind y Creando un Clúster
Descarga e instala kind siguiendo las instrucciones del sitio web oficial de kind.

En Ubuntu 24.04, usa el siguiente comando para descargar el cliente kind más reciente (a fecha de 7 de noviembre de 2025):

\[ $(uname \-m) \= x86\_64 \] && curl \-Lo ./kind https://kind.sigs.k8s.io/dl/v0.30.0/kind-linux-amd64

Luego mueve el binario a un directorio en tu PATH para que puedas usarlo libremente como un usuario regular.

Comandos:

chmod \+x ./kind
sudo mv ./kind /usr/local/bin/kind

Crea un clúster ejecutando el siguiente comando (puede que necesites usar sudo):

kind create cluster

Este comando crea un clúster de Kubernetes de un solo nodo que se ejecuta dentro de un contenedor de Docker y configura automáticamente el archivo de configuración de kubectl y establece el contexto actual para usar kind.

Si instalaste kind con sudo, solo el usuario root puede acceder al clúster. Para acceder al clúster como un usuario regular, crea una copia del kubeconfig de kind que sea propiedad de tu usuario en tu directorio de inicio:

mkdir \-p \~/.kube
sudo cp /root/.kube/config \~/.kube
sudo chown $(whoami):$(whoami) \~/.kube/config

Para verificar que todo funciona, puedes usar el comando `kubectl config current-context` para comprobar el contexto activo actualmente. Debería mostrar `kind-kind` como el contexto actual.

Para comprobar si kubectl puede alcanzar tu clúster de kind, puedes usar el comando `kubectl get nodes`. Este comando mostrará tus nodos de Kubernetes, que en un clúster de kind de un solo nodo solo consiste en el nodo `kind-control-plane`.

#### **Fedora**

#### **1\. Instalar Docker (si aún no lo tienes)**

Fedora usa Podman por defecto, pero Kind requiere Docker.
Instálalo así:

bash
sudo dnf \-y install dnf-plugins-core
sudo dnf config-manager \--add-repo https://download.docker.com/linux/fedora/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io
sudo systemctl enable \--now docker
sudo usermod \-aG docker $USER
Cierra sesión y vuelve a entrar para aplicar el grupo docker.

#### **2\. Descargar el binario oficial de Kind**

La documentación oficial indica que la forma recomendada es descargar el binario desde los releases.

curl \-Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod \+x ./kind
sudo mv ./kind /usr/local/bin/kind

Luego mueve el binario a un directorio en tu PATH para que puedas usarlo libremente como un usuario regular.

Comandos:

chmod \+x ./kind
sudo mv ./kind /usr/local/bin/kind

# **15.Realiza este cuestionario para comprobar tu comprensión**

Felicidades por completar el Capítulo 2 - "Introducción a Argo". Realiza este cuestionario para comprobar tu comprensión de los conceptos que has aprendido hasta ahora.


Instrucciones:

Comienza el cuestionario seleccionando el botón Iniciar Cuestionario a continuación. Para cada pregunta, elige la(s) mejor(es) respuesta(s) y luego selecciona Siguiente Pregunta. Después de haber respondido la pregunta final, selecciona Ver Resultados. Luego puedes elegir entre las opciones en la pantalla: Rehacer Cuestionario, Revisar Todos los Resultados de las Preguntas o Continuar a la Siguiente Sección.

Nota:
Seleccionar las flechas en la parte superior o inferior de la página te sacará del cuestionario y te llevará a la siguiente sección. Si esto sucede, regresa al cuestionario desde el Menú en el lado de tu pantalla.

#### **¿Cuál es el rol de Argo en la gestión de flujos de trabajo de Kubernetes?**

a.
Argo es una herramienta de gestión de bases de datos
Incorrecto
**b.**
Argo proporciona despliegue de aplicaciones controlado por versiones y automatización de flujos de trabajo
Tu Respuesta: **Correcto**
c.
Argo es una plataforma de contenedorización
Incorrecto
d.
Argo es una solución de red para Kubernetes

#### **¿Cuál es el rol principal de Kubernetes en el contexto de las funcionalidades de Argo?**

a.
Kubernetes es un sistema de control de versiones
Incorrecto
**b.**
Kubernetes proporciona la infraestructura esencial para la orquestación de contenedores
Tu Respuesta: **Correcto**
c.
Kubernetes es una herramienta de entrega continua
Incorrecto
d.
Kubernetes es un sistema de gestión de bases de datos

#### **¿Cómo se alinea Argo con las prácticas de desarrollo modernas, asegurando una única fuente de verdad tanto para el código como para la infraestructura?**

a.
Utilizando archivos de Docker Compose
Incorrecto
b.
Imponiendo una arquitectura de microservicios
Incorrecto
c.
Eliminando prácticas obsoletas de Kubernetes
Incorrecto
**d.**
Siguiendo los principios de GitOps
Tu Respuesta: **Correcto**

# **16.Resumen y Objetivos del Capítulo**

## **Resumen y Objetivos del Capítulo**

En este capítulo, exploramos los aspectos detallados de Argo CD, una herramienta fundamental dentro del conjunto de Argo, diseñada específicamente para las prácticas de Entrega Continua (CD) y GitOps. A través de discusiones temáticas y laboratorios prácticos, nuestro objetivo es equiparte con una comprensión integral de la arquitectura, instalación, configuración, consideraciones de seguridad y extensibilidad de Argo CD.

Al final de este capítulo, deberías ser capaz de:

Explicar cómo Argo CD aprovecha los principios de GitOps.
Describir los componentes principales de Argo CD, sus roles y tareas.
Instalar y configurar Argo CD en un clúster de Kubernetes.
Desplegar y actualizar aplicaciones usando Argo CD.

## **¿Qué es Argo CD?**

Argo CD es una herramienta declarativa de entrega continua GitOps para Kubernetes. En comparación con el flujo de trabajo estándar de Kubernetes, Argo CD introduce una serie de mejoras.

### **Avances Clave**

#### **Brechas**

Argo CD impone el estado deseado de tu aplicación tal como se describe en un repositorio Git. Esto significa que todos los cambios en tu aplicación están controlados por versiones y pueden ser rastreados fácilmente. Esta es una mejora significativa sobre el flujo de trabajo estándar de Kubernetes, donde los cambios se realizan directamente en el clúster y pueden ser difíciles de rastrear.

#### **Entrega Continua**

Argo CD monitorea continuamente el repositorio Git de la aplicación y despliega automáticamente cualquier cambio en tu clúster de Kubernetes. Esto elimina la necesidad de ejecutar manualmente el comando `kubectl apply` cada vez que deseas desplegar cambios, haciendo que el proceso de despliegue sea más eficiente, más fácil de rastrear y menos propenso a errores.

#### **Reversiones (Rollbacks)**

Si algo sale mal con un despliegue, Argo CD te permite revertir fácilmente a una versión anterior de tu aplicación. Esto es mucho más difícil con el flujo de trabajo estándar de Kubernetes, donde necesitarías revertir manualmente tus cambios en el clúster o volver a aplicar manifiestos antiguos al clúster.

#### **Gestión Multi-entorno**

Argo CD facilita la gestión de múltiples entornos (como Desarrollo, Staging, Producción) desde un único repositorio de Git. Esta es una mejora significativa sobre el flujo de trabajo estándar de Kubernetes, donde la gestión de múltiples entornos puede ser compleja y propensa a errores.

#### **UI y API**

Argo CD proporciona una interfaz de usuario amigable y una potente API, lo que facilita la gestión y el monitoreo de tus aplicaciones. Esta es una mejora significativa sobre el flujo de trabajo estándar de Kubernetes, que depende en gran medida de herramientas de línea de comandos.

En las próximas secciones, exploraremos aspectos fundamentales de Argo CD. Nuestro enfoque estará en la terminología esencial dentro del ecosistema de Argo CD, los componentes centrales que constituyen Argo CD y el concepto del bucle de reconciliación. Comprender estos elementos es clave para gestionar eficientemente los despliegues de tus aplicaciones.

# 17.La Arquitectura de Argo CD

## **La Arquitectura de Argo CD**

# Vocabulario

En el mundo de Argo CD, entender el vocabulario específico es crucial para gestionar y desplegar aplicaciones de manera efectiva dentro de los entornos de Kubernetes. Estos términos forman el lenguaje central utilizado para describir diversos aspectos y operaciones dentro de Argo CD, una herramienta declarativa de entrega continua GitOps para Kubernetes. Al familiarizarte con este vocabulario, adquieres la capacidad de comunicar y ejecutar con precisión tareas relacionadas con el despliegue y la gestión de aplicaciones. Este conocimiento no solo agiliza tu flujo de trabajo, sino que también mejora la colaboración con los miembros del equipo, ya que todos comparten una comprensión común de estos conceptos clave.

Cada término encapsula un aspecto distinto del ecosistema de Argo CD. Desde la definición de la estructura y las fuentes de tus aplicaciones hasta la comprensión de sus estados operativos y su salud, este vocabulario cubre la amplitud del proceso de despliegue. Conocer la diferencia entre 'Estado objetivo' y 'Estado en vivo', por ejemplo, es esencial para mantener la integridad y la funcionalidad deseada de tus aplicaciones. Del mismo modo, comprender los matices de varios estados y acciones como 'Estado de sincronización' y 'Refrescar' te permite monitorear, solucionar problemas y actualizar eficazmente tus recursos de Kubernetes. En esencia, estos términos son los bloques de construcción para dominar Argo CD y garantizar una canalización de despliegue fluida y eficiente en Kubernetes.

### **Conceptos Principales**

#### **Configuración**

Una Definición de Recurso Personalizado (CRD) proporcionada por Argo CD que define una colección de recursos de Kubernetes y sirve como la interfaz principal para gestionar el software desplegado por Argo CD.
Tipo de fuente de la aplicación: La herramienta utilizada para construir aplicaciones, como Helm o Kustomize.

#### **Estados**

Estado objetivo (Target state): El estado deseado de una aplicación, representado en un repositorio Git, que sirve como fuente de verdad.
Estado en vivo (Live state): El estado actual de la aplicación, que indica los recursos de Kubernetes desplegados.

#### **Estados (Statuses)**

Estado de sincronización (Sync status): El estado que indica si el estado en vivo se alinea con el estado objetivo. Esencialmente, confirma si la aplicación desplegada en Kubernetes coincide con los estados deseados descritos en el repositorio de Git.
Estado de la operación de sincronización (Sync operation status): El estado durante la fase de sincronización, que especifica si ha fallado o tenido éxito.
Estado de salud (Health status): El bienestar de la aplicación, evaluando su condición de funcionamiento y su capacidad para manejar solicitudes.

#### **Acciones**

Refrescar (Refresh): El acto de comparar el código más reciente en Git con el estado en vivo para identificar cualquier diferencia.
Sincronizar (Sync): El proceso de llevar una aplicación al estado objetivo aplicando cambios en el clúster de Kubernetes.

# 18.Componentes Principales

### **Componentes Principales**

#### **Controladores**

Para empezar, echaremos un vistazo a los componentes principales de Argo CD y cómo funcionan entre sí.

Argo CD emplea controladores de Kubernetes para sus funciones principales. Un controlador de Kubernetes, incluidos los de Argo CD, monitorea el estado del clúster y se asegura de que se alinee con el estado deseado, iniciando o solicitando cambios cuando sea necesario. Esto se logra observando los objetos de recursos de Kubernetes, cada uno de los cuales especifica el estado previsto a través de un campo `spec`.

#### **Servidor de API**

El servidor de API en Argo CD sirve como un centro de comunicación central que permite que diferentes sistemas como la interfaz web (Web UI), la CLI, Argo Events y las herramientas de CI/CD interactúen con él. Piénsalo como una torre de control central para gestionar aplicaciones. Su trabajo principal es hacer un seguimiento de las aplicaciones, proporcionando actualizaciones de estado para que sepas qué está sucediendo con ellas. Cuando es el momento de hacer cambios o actualizaciones, el servidor de API desencadena las operaciones necesarias en estas aplicaciones.

Más allá de eso, el servidor de API tiene otras responsabilidades. Se encarga de manejar los repositorios de Git y los clústeres de Kubernetes, asegurando una conexión fluida entre tu código y tu infraestructura. En cuanto a la seguridad, es el guardián, manejando la autenticación y el soporte para Single Sign-On (SSO). Además, el servidor de API aplica políticas de Control de Acceso Basado en Roles (RBAC), asegurándose de que las personas adecuadas tengan el nivel de acceso correcto para mantener todo seguro y organizado. En esencia, es el maestro detrás de escena que se asegura de que todo en tu configuración de Argo CD funcione en armonía. Las siguientes acciones encapsulan sus roles específicos en la gestión de aplicaciones y la facilitación de la comunicación:

Gestiona aplicaciones y proporciona actualizaciones de estado
Desencadena operaciones en las aplicaciones cuando es necesario
Maneja repositorios de Git para el control de versiones
Gestiona las conexiones con los clústeres de Kubernetes
Se encarga de la autenticación y admite Single Sign-On (SSO)
Aplica políticas de Control de Acceso Basado en Roles (RBAC)
Sirve como un centro central para la comunicación con la interfaz web, la CLI, Argo Events y los sistemas de CI/CD

#### **Servidor de Repositorio**

Conectado al Servidor de API está el Servidor de Repositorio de Argo CD, que es responsable de obtener el estado deseado de las aplicaciones desde los repositorios de Git y empaquetarlo en un formato que pueda ser entendido por Kubernetes. Este servidor se comunica con tus repositorios de Git y obtiene la información necesaria. Mantiene una caché local de los repositorios de Git que contienen los manifiestos de la aplicación.

Otros servicios de Argo solicitan manifiestos de Kubernetes utilizando los siguientes argumentos:

URL del repositorio
Revisión de Git
Ruta de la aplicación
Información relevante para la plantilla, como parámetros o el `values.yaml` de Helm

#### **Controlador de Aplicación**

El Controlador de Aplicación de Argo CD es otro componente crucial. Compara continuamente el estado deseado de la aplicación (como se define en tus repositorios de Git) con el estado en vivo en tu clúster de Kubernetes (como se define en los CRDs de Aplicación proporcionados por el usuario). Si detecta alguna discrepancia, tomará medidas correctivas para garantizar que el estado en vivo coincida con el estado deseado.

![][image1]
Diagrama general de Argo CD y sus componentes

# 19.Reconciliación y Control de Sincronización

## **Entendiendo la Reconciliación y el Control de Sincronización**

¿Cómo funciona el Bucle de Reconciliación de Argo CD?
El proceso de reconciliación de Argo CD implica alinear la configuración deseada especificada en un repositorio Git con el estado actual en el clúster de Kubernetes. Este bucle continuo, conocido como el bucle de reconciliación, se ilustra en la siguiente figura.

![][image2]
Bucle de reconciliación de Argo CD


Cuando se usa Helm, Argo CD monitorea el repositorio de Git, emplea Helm para generar el YAML del manifiesto de Kubernetes a través de la ejecución de plantillas, y lo compara con el estado deseado del clúster, conocido como estado de sincronización (sync status). Si se identifican disparidades, Argo CD utiliza **kubectl apply** para implementar los cambios y actualizar el estado deseado de Kubernetes. Es de destacar que Argo CD, en este proceso, opta por **kubectl apply** en lugar de `helm install` para admitir diversas herramientas de plantillas, manteniendo su rol como una herramienta declarativa de GitOps sin estar atado a una herramienta de plantillas específica.

El bucle de reconciliación de Argo CD encarna los principios de GitOps al mantener un repositorio de Git como la única fuente de verdad para las definiciones de infraestructura. Este proceso se alinea con los principios de GitOps de tener un estado deseado declarativo, que está versionado y es inmutable, y es extraído automáticamente por agentes de software. Los agentes observan continuamente el estado real del sistema e intentan reconciliarlo con el estado deseado, reflejando el principio de reconciliación continua de GitOps. Por lo tanto, el bucle de reconciliación de Argo CD es una implementación práctica de los principios de GitOps.

## **Principios de Sincronización**

La fase de sincronización es una operación muy importante y su comportamiento puede personalizarse usando ganchos de recursos (resource hooks) y ondas de sincronización (sync waves). En esta sección, exploramos ambas formas de personalización y aprendemos a usarlas.

### **Soluciones de Personalización**

#### **Ganchos de recursos (Resource hooks)**

Una sincronización, como se describe en la sección "Vocabulario", es la transición de una aplicación al estado objetivo. Hay cinco definiciones posibles de cuándo se puede ejecutar un gancho de recurso:

PreSync, se ejecuta antes de la fase de Sincronización (por ejemplo, crear una copia de seguridad antes de sincronizar)
Sync, se ejecuta después de que todos los ganchos PreSync se completaron con éxito y realiza acciones durante la aplicación de los manifiestos (por ejemplo, para estrategias de despliegue más complejas como azul-verde o canary en lugar de la actualización continua de Kubernetes)
PostSync, se ejecuta después de que todos los ganchos de Sincronización y la aplicación hayan tenido éxito, y todos los recursos estén en estado Saludable (Healthy) (por ejemplo, ejecutar comprobaciones de salud adicionales después del despliegue o ejecutar pruebas de integración)
Skip, indica a Argo CD que omita la aplicación del manifiesto
SyncFail, se ejecuta cuando la Sincronización falla (por ejemplo, operaciones de limpieza)
Para simplificar, los ganchos de recursos utilizan el `kind` de Kubernetes `Job` y se identifican mediante una anotación. Usando la anotación, Argo CD identifica los Jobs y cuándo deben ejecutarse.

El siguiente es un ejemplo de un gancho de recurso para la migración de un esquema de base de datos:

apiVersion: batch/v1
kind: Job
metadata:
  generateName: schema-migrate-
  annotations:
    argocd.argoproj.io/hook: PreSync

Se puede encontrar más información sobre los ganchos de recursos en la documentación oficial de Argo CD.

#### **Ondas de sincronización (Sync waves)**

Las ondas de sincronización son una forma conveniente de dividir y ordenar los manifiestos que se van a sincronizar. Las ondas pueden variar desde valores negativos a positivos y ocurren desde el valor más bajo al más alto. Si no se define, el valor predeterminado es la onda cero. El uso de ondas de sincronización se logra de la misma manera que los ganchos de recursos; mediante el uso de anotaciones.

metadata:
  annotations:
    argocd.argoproj.io/sync-wave: "5"

Ambos enfoques pueden utilizarse simultáneamente. La operación de sincronización en Argo CD se inicia con el ordenamiento de los recursos según los siguientes criterios:

* Anotación de fase del gancho de recurso
* Anotación de onda de sincronización
* Tipo (kind) de Kubernetes (por ejemplo, los namespaces tienen prioridad, seguidos de otros recursos de Kubernetes y luego los recursos personalizados)
* Nombre

Posteriormente, se identifica el siguiente número de onda a aplicar. Este número es siempre el más bajo entre todos los recursos no sincronizados o no saludables. Argo CD luego aplica esta onda.

Este proceso se repite hasta que todas las fases y ondas estén sincronizadas y saludables.

Sin embargo, si una aplicación tiene recursos no saludables en la onda inicial, puede evitar que la aplicación alcance un estado saludable.

Por razones de seguridad, Argo CD añade un retraso de 2 segundos entre cada onda de sincronización. Esto se puede personalizar utilizando la variable de entorno `ARGOCD_SYNC_WAVE_DELAY`.

# **20.Objetos y Recursos**

## **Objetos y Recursos**

### **Simplificando la Gestión de Aplicaciones**

Argo CD simplifica la gestión de tus aplicaciones en entornos de Kubernetes utilizando Definiciones de Recursos Personalizados (CRDs).

# **Elementos Clave**

#### **Application**

Argo CD introduce el CRD Application, que sirve como una representación de la instancia de la aplicación que pretendes desplegar en tu clúster de Kubernetes. Las Applications definen el repositorio de origen y el clúster de destino. También proporcionan configuraciones para la sincronización y las sobreescrituras de configuración de la aplicación. Ejemplo:

URL: https://github.com/argoproj/argocd-example-apps

apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: guestbook
  namespace: argocd
spec:
  project: default
  source:
    repoURL: 'htt‌ps://github.com/argoproj/argocd-example-apps.git'
    targetRevision: HEAD
    path: guestbook
destination:
  server: 'htt‌ps://kubernetes.default.svc'
  namespace: guestbook

#### **AppProject**

Para una organización eficiente, Argo CD proporciona el CRD AppProject. Esto te permite agrupar aplicaciones relacionadas para fines de organización y gobernanza. Un caso de uso de ejemplo implica segregar aplicaciones de servicios de utilidad, mejorando la claridad de la estructura de tu proyecto. Ejemplo:

apiVersion: argoproj.io/v1alpha1
kind: AppProject
metadata:
  name: production
  namespace: argocd
spec:
  description: Aplicaciones de producción
  sourceRepos:
    \- '\*'
  destinations:
    \- namespace: production
      server: 'htt‌ps://kubernetes.default.svc'
  clusterResourceWhitelist:
    \- group: '\*'
      kind: '\*'

#### **Credenciales de Repositorio**

En escenarios del mundo real, es común acceder a repositorios privados. Argo CD facilita esto utilizando Secretos y ConfigMaps de Kubernetes con etiquetas específicas. Para otorgar acceso a Argo CD, crea los recursos Secret de Kubernetes necesarios con una etiqueta específica: `argocd.argoproj.io/secret-type: repository`. Ejemplo:

apiVersion: v1
kind: Secret
metadata:
  name: private-repo-creds
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  url: 'htt‌ps://github.com/private/repo.git'
  username: \<username\>
  password: \<password\>

#### **Credenciales de Clúster**

En casos donde Argo CD gestiona múltiples clústeres de Kubernetes, puede ser necesario un acceso adicional. Para este propósito, Argo CD utiliza un tipo de Secreto distinto con la etiqueta: `argocd.argoproj.io/secret-type: cluster`. Esto garantiza un acceso seguro a clústeres externos que no estaban incluidos inicialmente en los entornos gestionados por Argo CD. Ejemplo:

apiVersion: v1
kind: Secret
metadata:
  name: external-cluster-creds
  labels:
    argocd.argoproj.io/secret-type: cluster
type: Opaque
stringData:
  name: external-cluster
  server: 'ht‌tps://external-cluster-api.com'
  config: |
    {
      "bearerToken": "\<token\>",
      "tlsClientConfig": {
         "insecure": false,
         "caData": "\<certificate encoded in base64\>"
      }
    }

Al entender y utilizar estos objetos fundamentales de Argo CD, puedes agilizar eficazmente el despliegue y la gestión de tus aplicaciones en diversos entornos de Kubernetes.

# **21.Extensiones e Integraciones de Argo CD**

## **Extensiones e Integraciones de Argo CD**

### **Plugins**

La fortaleza de Argo CD reside en su extensibilidad a través de plugins, lo que permite a los usuarios adaptar la herramienta a sus necesidades específicas. En esta sección, exploraremos el concepto de los plugins de Argo CD, entenderemos cómo configurarlos usando ConfigMaps y veremos el plugin de Notificaciones en acción. Además, destacaremos algunos otros plugins notables para mostrar la versatilidad de las extensiones de Argo CD.

### **Entendiendo los Plugins en Argo CD**

Los plugins de Argo CD extienden las funcionalidades principales del sistema, ofreciendo características adicionales más allá de las capacidades predeterminadas. Nos centraremos en los plugins de Notificaciones, que juegan un papel crucial en mantener a los usuarios informados sobre los eventos de despliegue.

### **Extensiones e Integraciones de Argo CD**

#### **Configurando Plugins con ConfigMaps**

Los ConfigMaps en Kubernetes proporcionan una forma de gestionar datos de configuración, y en el contexto de Argo CD, se emplean para configurar plugins. Exploremos la configuración del plugin de Notificaciones usando un ConfigMap:

apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-notifications-cm
data:
  context: |
    region: east
    environmentName: staging

  template.a-slack-template-with-context: |
    message: "Something happened in {{ .context.environmentName }} in the {{ .context.region }} data center\!"

En este ejemplo:

La sección `context` proporciona información contextual como la región y el nombre del entorno.
La sección `template.a-slack-template-with-context` define una plantilla de notificación de Slack usando plantillas de Go. Hace referencia a valores de la sección `context` para personalizar el mensaje.
Este ConfigMap permite a los usuarios ajustar dinámicamente el contenido de las notificaciones basándose en información contextual, proporcionando flexibilidad para responder a diferentes escenarios de despliegue.

#### **Cómo Funcionan los Plugins en Argo CD**

Los plugins en Argo CD siguen un ciclo de vida específico, que incluye las fases de registro, inicialización y ejecución. Durante el inicio, Argo CD descubre y carga los plugins disponibles, inicializándolos para su uso a lo largo del ciclo de vida de la aplicación. Los eventos críticos, como la sincronización o el despliegue de una aplicación, activan los plugins asociados.

#### **Plugins en Acción: Notificaciones y Más Allá**

Apliquemos nuestro conocimiento de forma práctica considerando un escenario donde un despliegue falla en el entorno de staging en la región este. El plugin de Notificaciones, configurado con la plantilla proporcionada, activa una notificación por Slack o correo electrónico como: "¡Algo sucedió en staging en el centro de datos del este!". Esta notificación inmediata permite a los usuarios identificar y abordar rápidamente los problemas de despliegue.

Más allá del plugin de Notificaciones, Argo CD admite varios otros plugins que satisfacen diversas necesidades. Algunos ejemplos notables incluyen:

**Image Updater Plugin**: Automatiza el proceso de actualización de imágenes de contenedores en los manifiestos de Kubernetes, garantizando que las aplicaciones siempre utilicen las últimas versiones.
**ArgoCD Autopilot**: Optimiza los flujos de trabajo de GitOps al automatizar la gestión de las versiones de Helm, lo que facilita el mantenimiento y la implementación de aplicaciones.
**ArgoCD Interlace**: Mejora la interfaz de usuario de Argo CD con funciones adicionales, proporcionando una interfaz interactiva y dinámica para la gestión de aplicaciones e implementaciones.

# **22\. Mejores Prácticas**

## **Mejores Prácticas**

### **Asegurando Argo CD**

#### **Usar RBAC**

Por qué: Usar RBAC es una de las mejores prácticas más importantes para asegurar Argo CD, ya que es vital para gestionar los permisos de los usuarios de manera efectiva, asegurando que solo el personal autorizado tenga acceso a recursos específicos. Este enfoque minimiza el riesgo de cambios o accesos no autorizados, lo cual es crucial en un entorno de despliegue continuo.
Cómo: Para implementar RBAC en Argo CD, define roles con permisos específicos en el ConfigMap `argocd-rbac-cm`, alineándolos con las responsabilidades de los usuarios. Asigna estos roles a usuarios o grupos a través de RoleBindings o ClusterRoleBindings para los niveles de acceso apropiados. Asegura el acceso mínimo necesario por rol, adhiriéndote al principio de menor privilegio. Audita regularmente la configuración de RBAC para reflejar las necesidades actuales y las políticas de seguridad, ajustando roles y permisos según sea necesario. Utiliza los roles predefinidos de Argo CD para patrones de acceso comunes, personalizándolos según sea necesario. Prueba las configuraciones para garantizar la correcta aplicación y seguridad.

#### **Gestionar Secretos de Forma Segura**

Por qué: La gestión de secretos es crítica para salvaguardar información sensible como claves de API, contraseñas y certificados. Una gestión eficaz de los secretos previene el acceso no autorizado y posibles brechas de seguridad. En el contexto de Kubernetes y Argo CD, esto implica manejar los secretos de manera que no queden expuestos ni se registren en logs.
Cómo: Utiliza la gestión de secretos nativa de Kubernetes para almacenar y manejar datos sensibles. Asegúrate de que los secretos estén encriptados en reposo y en tránsito. Para una seguridad mejorada, considera implementar capas adicionales de seguridad como el uso de variables de entorno, las capacidades de encriptación nativas de Kubernetes, o la integración con otras soluciones de gestión de secretos que se ajusten al ecosistema de Kubernetes.

#### **Actualizar Argo CD Regularmente**

Por qué: Mantener Argo CD actualizado es crucial para la seguridad. Las actualizaciones a menudo incluyen parches para vulnerabilidades, mejoras en la funcionalidad y características de seguridad mejoradas. Mantenerse actualizado ayuda a protegerse contra amenazas emergentes y a aprovechar las últimas mejoras de seguridad.
Cómo: Establece una rutina para verificar y aplicar actualizaciones a Argo CD. Esto se puede hacer a través de comprobaciones de actualización automatizadas o monitoreando manualmente los lanzamientos de Argo CD. Utiliza estrategias de despliegue estándar de Kubernetes para aplicar actualizaciones con una interrupción mínima. Asegúrate de que el proceso de actualización incluya pruebas en un entorno de staging antes de desplegar en producción para evitar problemas inesperados.
Para obtener información detallada adicional sobre las prácticas de seguridad de Argo CD, especialmente en temas como Single Sign-On (SSO), Registro de Auditoría y Políticas de Red, consulta la Documentación de Seguridad de Argo CD.

#### **Nota sobre Helm y Kustomize**

Mejorando la Eficiencia del Despliegue con Helm y Kustomize
Transicionando desde despliegues estándar, Helm mejora ArgoCD con plantillas y gestión de paquetes, agilizando los despliegues en diferentes entornos. Kustomize añade capas de personalización a los despliegues sin cambiar los manifiestos base, simplificando los ajustes específicos del entorno. Estas herramientas facilitan despliegues escalables y con menos errores en ArgoCD.

Argo CD utiliza Helm o Kustomize para desplegar aplicaciones dependiendo del contenido de su repositorio de origen.

Para más información, visita las guías de ArgoCD sobre Helm y Kustomize.

# **23\. Ejercicios de Laboratorio**

# **Ejercicios de Laboratorio**

## **Laboratorio 3.1. Instalando Argo CD**

### **Consejos y Mejores Prácticas del Laboratorio**

Cuando trabajes en los ejercicios de laboratorio, por favor ten en cuenta lo siguiente:

Cuando accedas a URLs externas incrustadas en el documento PDF a continuación, utiliza siempre el clic derecho y abre en una nueva pestaña o ventana. Intentar abrir la URL haciendo clic directamente en ella cerrará la ventana/pestaña de tu curso.
Dependiendo de tu visor de PDF, si estás cortando y pegando desde el documento, puedes perder el formato original. Por ejemplo, los guiones bajos pueden desaparecer y ser reemplazados por espacios. Por lo tanto, es posible que necesites editar manualmente el texto. Siempre verifica que el texto pegado sea correcto.

## **Laboratorio 3.2. Gestionando Aplicaciones con Argo CD**

Consejos y Mejores Prácticas del Laboratorio
Cuando trabajes en los ejercicios de laboratorio, por favor ten en cuenta lo siguiente:

Cuando accedas a URLs externas incrustadas en el documento PDF a continuación, utiliza siempre el clic derecho y abre en una nueva pestaña o ventana. Intentar abrir la URL haciendo clic directamente en ella cerrará la ventana/pestaña de tu curso.
Dependiendo de tu visor de PDF, si estás cortando y pegando desde el documento, puedes perder el formato original. Por ejemplo, los guiones bajos pueden desaparecer y ser reemplazados por espacios. Por lo tanto, es posible que necesites editar manualmente el texto. Siempre verifica que el texto pegado sea correcto.

## **Laboratorio 3.3. Seguridad y RBAC en Argo CD**

# **24.Preguntas**

Felicidades por completar el Capítulo 3 - "Argo CD". Realiza este cuestionario para comprobar tu comprensión de los conceptos que has aprendido hasta ahora.



Instrucciones:

Comienza el cuestionario seleccionando el botón Iniciar Cuestionario a continuación. Para cada pregunta, elige la(s) mejor(es) respuesta(s) y luego selecciona Siguiente Pregunta. Después de haber respondido la pregunta final, selecciona Ver Resultados. Luego puedes elegir entre las opciones en la pantalla: Rehacer Cuestionario, Revisar Todos los Resultados de las Preguntas o Continuar a la Siguiente Sección.

Nota:
Seleccionar las flechas en la parte superior o inferior de la página te sacará del cuestionario y te llevará a la siguiente sección. Si esto sucede, regresa al cuestionario desde el Menú en el lado de tu pantalla.

Prueba de Conocimiento 3.1

#### **¿Qué sirve como única fuente de verdad en Argo CD?**

a.
Un repositorio Git
b.
Estado objetivo
c.
Estado en vivo
d.
Manifiesto de la aplicación

Prueba de Conocimiento 3.2

#### **¿Qué parte de la arquitectura de Argo CD es responsable de obtener el estado deseado de una aplicación?**

a.
Servidor de aplicación
b.
Gancho de recurso
c.
Servidor de repositorio
Correcto
d.
Servidor de API

Prueba de Conocimiento 3.3
¿Qué Definición de Recurso Personalizado de Argo CD permite agrupar aplicaciones?

a.
AutoPilot
b.
Deployments
c.
Application
d.
AppProject

✅ Respuesta correcta: d. AppProject
Por qué:
Application define una aplicación individual.

AppProject es el CRD que permite agrupar varias Applications bajo un mismo proyecto lógico, aplicando políticas comunes como:

repos permitidos

destinos permitidos (clusters/namespaces)

roles RBAC

restricciones de sincronización

En otras palabras:

👉 AppProject \= agrupación \+ políticas \+ límites para múltiples Applications.

#### **¿Qué función utiliza Argo CD internamente para implementar el estado deseado?**

a.
helm install
b.
**kubectl apply**
c.
argocd apply
d.
argo install

✅ Respuesta correcta: b. kubectl apply
Por qué:
Argo CD no inventa su propio mecanismo para aplicar manifiestos.
Internamente utiliza el mismo mecanismo que usarías tú manualmente:

👉 kubectl apply

Esto le permite:

aplicar manifiestos declarativos

hacer diffs entre el estado deseado y el real

reconciliar continuamente

respetar server-side apply cuando corresponde

Argo CD simplemente automatiza y controla este proceso, pero la herramienta subyacente es la misma.

Prueba de Conocimiento 3.5

#### **Quieres ejecutar una copia de seguridad antes de que se realicen cambios en el despliegue y hacer comprobaciones de salud adicionales después del despliegue. ¿Qué ganchos de recursos (resource hooks) usas?**

a.
PreSync y Sync
b.
PostSync y Sync
c.
**PreSync y PostSync**
d.
Sync y Skip

La respuesta correcta es **c. PreSync and PostSync**.

## **🧩 ¿Por qué?**

Argo CD tiene varios *resource hooks* que permiten ejecutar acciones en distintos momentos del ciclo de sincronización. Para tu caso:

### **🔹 PreSync**

Se ejecuta **antes** de que Argo CD aplique los cambios.

👉 Ideal para hacer **backups**, validaciones previas o tareas de preparación.

### **🔹 PostSync**

Se ejecuta **después** de que la sincronización ha terminado.

👉 Perfecto para **health checks adicionales**, notificaciones o tareas de verificación.

## **🧠 Resumen**

| Hook | Cuándo se ejecuta | Para qué sirve |
| ----- | ----- | ----- |
| PreSync | Antes del deploy | Backups, validaciones |
| Sync | Durante el deploy | Aplicación de cambios |
| PostSync | Después del deploy | Health checks, notificaciones |

Así que la combinación correcta para tu escenario es:

👉 **PreSync \+ PostSync**

# **25\. Argo Workflows**

# **Argo Workflows**

## **Introducción**

### **Resumen y Objetivos del Capítulo**

En este capítulo, exploraremos los detalles de Argo Workflows, una extensión de Argo, una popular herramienta de GitOps diseñada para la entrega continua declarativa de aplicaciones de Kubernetes. Argo Workflows te permite definir y gestionar flujos de trabajo complejos como código, proporcionando una forma de orquestar y automatizar procesos de múltiples pasos dentro del entorno de Kubernetes.

Al final de este capítulo, deberías ser capaz de comprender los conceptos básicos y la arquitectura de Argo Workflows. Esto implica entender sus componentes clave, cómo interactúan y los conceptos fundamentales que rigen la ejecución de los flujos de trabajo. Aquí están los objetivos de aprendizaje para adquirir competencia en Argo Workflows:

Definir y explicar la estructura de un Argo Workflow.
Reconocer elementos clave como metadatos, spec, entrypoint y templates.
Comprender el rol de las plantillas (templates) en los flujos de trabajo.
Identificar y explicar los componentes principales de Argo Workflows, incluyendo el Controlador de Flujo de Trabajo (Workflow Controller) y la interfaz de usuario (UI).
Entender cómo Argo Workflows programa y ejecuta tareas.
Profundizar en las responsabilidades del Controlador de Flujo de Trabajo.

### **Workflow (Flujo de Trabajo)**

Un flujo de trabajo (workflow) es una serie de tareas, procesos o pasos que se ejecutan en una secuencia específica para lograr un objetivo o resultado particular. Los flujos de trabajo son prevalentes en diversos dominios, incluyendo negocios, desarrollo de software y gestión de proyectos. En el contexto de Argo y otras herramientas de DevOps, un flujo de trabajo se refiere específicamente a una secuencia de pasos automatizados involucrados en el despliegue, prueba y promoción de aplicaciones de software.

En Argo, el término Workflow es un Recurso Personalizado de Kubernetes que representa una secuencia de tareas o pasos que se definen y orquestan para lograr un objetivo específico. Es una abstracción de nivel superior que permite a los usuarios describir procesos complejos, dependencias y condiciones de una manera estructurada y declarativa. Un Workflow también mantiene el estado de un flujo de trabajo.

A continuación, echaremos un vistazo a las especificaciones de un Workflow simple.

La parte principal de la especificación de un Workflow contiene un `entrypoint` y una lista de `templates`, como se muestra en el ejemplo a continuación:

apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: hello-world-
spec:
  entrypoint: whalesay
  templates:
\- name: whalesay
  container:
    image: docker/whalesay
    command: \[cowsay\]
    args: \["hello world"\]

La especificación de un Workflow tiene dos partes centrales:

Entrypoint: Especifica el nombre de la plantilla que sirve como punto de entrada para el flujo de trabajo. Define el punto de partida de la ejecución del flujo de trabajo.
Templates: Una plantilla representa un paso o tarea en el flujo de trabajo que debe ejecutarse. Hay seis tipos de plantillas que presentaremos a continuación.

### **Conceptos Centrales de Argo Workflows**

#### **Tipos de Plantillas (Templates)**

Una plantilla (template) puede ser de tipo contenedor, script, DAG (Grafo Acíclico Dirigido), u otros tipos dependiendo de la estructura del flujo de trabajo y se divide en dos grupos: definiciones de plantillas (que definen el trabajo a realizar) e invocadores de plantillas (que invocan/llaman a otras plantillas y proporcionan control de ejecución).

##### **Container**

Un `Container` es el tipo de plantilla más común y representa un paso en el flujo de trabajo que ejecuta un contenedor. Es adecuado para ejecutar aplicaciones o scripts en contenedores.

Ejemplo:

\- name: whalesay
  container:
    image: docker/whalesay
    command: \[cowsay\]
    args: \["hello world"\]

##### **Resource**

Un `Resource` representa una plantilla para crear, modificar o eliminar recursos de Kubernetes. Es útil para realizar operaciones en objetos de Kubernetes en el clúster donde se está ejecutando Workflows.

Ejemplo:

\- name: k8s-owner-reference
  resource:
    action: create
    manifest: |
      apiVersion: v1
      kind: ConfigMap
      metadata:
        generateName: owned-eg-
      data:
        some: value

##### **Script**

Un `Script` es similar a la plantilla de contenedor pero permite especificar el script en línea sin hacer referencia a una imagen de contenedor externa. Se puede usar para scripts simples o de una sola línea.

Ejemplo:

\- name: gen-random-int
  script:
    image: python:alpine3.6
    command: \[python\]
    source: |
      import random
      i \= random.randint(1, 100\)
      print(i)

##### **Suspend**

Un `Suspend` es una plantilla que suspende la ejecución, ya sea por una duración o hasta que se reanude manualmente. Se puede reanudar usando la CLI, el punto final de la API o la interfaz de usuario.

Ejemplo:

\- name: delay
  suspend:
    duration: "20s"

#### **Invocadores de Plantillas**

##### **DAG (Directed-Acyclic Graph)**

Un `DAG` permite definir nuestras tareas como un grafo de dependencias. Es beneficioso para flujos de trabajo con dependencias complejas y ejecución condicional.

Ejemplo:

\- name: diamond
  dag:
    tasks:
    \- name: A
      template: echo
    \- name: B
      dependencies: \[A\]
      template: echo
    \- name: C
      dependencies: \[A\]
      template: echo
    \- name: D
      dependencies: \[B, C\]
      template: echo

##### **Steps**

Los `Steps` definen múltiples pasos dentro de una plantilla, ya que varios pasos deben ejecutarse secuencialmente o en paralelo.

\- name: hello-hello-hello
  steps:
  \- \- name: step1
      template: prepare-data
  \- \- name: step2a
      template: run-data-first-half
    \- name: step2b
      template: run-data-second-half

### **Salidas (Outputs)**

En Argo Workflows, la sección `outputs` dentro de una plantilla de paso te permite definir y capturar salidas que pueden ser accedidas por pasos posteriores o referenciadas en la definición del flujo de trabajo. Las salidas son útiles cuando quieres pasar datos, valores o artefactos de un paso a otro. Aquí tienes un resumen de cómo funcionan las salidas en Argo Workflows. La Salida comprende dos conceptos clave:

**Definición de Salidas**: Defines las salidas dentro de una plantilla de paso usando la sección `outputs`. Cada salida tiene un nombre y una ruta dentro del contenedor donde se produce el dato o artefacto.

**Acceso a Salidas**: Puedes hacer referencia a las salidas de un paso usando expresiones de plantilla en los pasos posteriores o en la definición del flujo de trabajo.

Consideremos un ejemplo simple donde un paso genera un parámetro de salida y un artefacto de salida, y otro paso los consume:

apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: artifact-passing-
spec:
  entrypoint: artifact-example
  templates:
  \- name: artifact-example
    steps:
    \- \- name: generate-artifact
        template: whalesay
    \- \- name: consume-artifact
        template: print-message
        arguments:
          artifacts:
          \- name: message
            from: "{{steps.generate-artifact.outputs.artifacts.hello-art}}"

  \- name: whalesay
    container:
      image: docker/whalesay:latest
      command: \[sh, \-c\]
      args: \["cowsay hello world | tee /tmp/hello\_world.txt"\]
    outputs:
      artifacts:
    \- name: hello-art
      path: /tmp/hello\_world.txt

  \- name: print-message
    inputs:
      artifacts:
      \- name: message
        path: /tmp/message
    container:
      image: alpine:latest
      command: \[sh, \-c\]
      args: \["cat /tmp/message"\]

La plantilla `whalesay` crea un archivo llamado `/tmp/hello-world.txt` usando el comando `cowsay`. A continuación, emite este archivo como un artefacto llamado `hello-art`. La plantilla `artifact-example` proporciona el artefacto `hello-art` generado como una salida del paso `generate-artifact`. Finalmente, la plantilla `print-message` toma un artefacto de entrada llamado `message` y lo consume desempaquetándolo en la ruta `/tmp/message` y usando el comando `cat` para imprimirlo en la salida estándar.

### **WorkflowTemplate (Plantilla de Flujo de Trabajo)**

En Argo Workflows, un `WorkflowTemplate` es un recurso que define una plantilla de flujo de trabajo reutilizable y compartible, permitiendo a los usuarios encapsular la lógica del flujo de trabajo, los parámetros y los metadatos. Esta abstracción promueve la modularidad y la reutilización, permitiendo la creación de flujos de trabajo complejos a partir de plantillas predefinidas.

Aquí hay un ejemplo de una definición simple de **WorkflowTemplate** en **Argo** **Workflows**:

apiVersion: argoproj.io/v1alpha1
kind: WorkflowTemplate
metadata:
  name: sample-template
spec:
  templates:
   \- name: hello-world
     inputs:
       parameters:
         \- name: msg
           value: "Hello World\!"
     container:
       image: docker/whalesay
       command: \[cowsay\]
       args: \["{{inputs.parameters.msg}}"\]

En este ejemplo:

El **WorkflowTemplate** se llama `sample-template`
Contiene una plantilla: `hello-world`
La plantilla `hello-world` toma un parámetro `message` (con un valor predeterminado de "Hello, World\!") y genera un archivo con el mensaje especificado.
Una vez definido, este `WorkflowTemplate` puede ser referenciado e instanciado dentro de múltiples flujos de trabajo. Por ejemplo:

apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: hello-world-
spec:
entrypoint: whalesay
templates:
  \- name: whalesay
    steps:
      \- \- name: hello-world
          templateRef:
            name: sample-template
            template: hello-world

Este flujo de trabajo hace referencia al **WorkflowTemplate** llamado `sample-template`, heredando efectivamente la estructura y la lógica definidas en la plantilla.

Usar **WorkflowTemplates** es beneficioso cuando quieres estandarizar y reutilizar patrones de flujo de trabajo específicos, lo que facilita la gestión, el mantenimiento y el intercambio de definiciones de flujo de trabajo dentro de tu organización. También ayudan a reforzar la consistencia y a reducir la redundancia en múltiples flujos de trabajo.

# **26\. Definiendo Argo Workflows y sus Componentes**

# **Definiendo Argo Workflows y sus Componentes**

Argo Workflows es una plataforma de orquestación de flujos de trabajo de código abierto diseñada para Kubernetes. Permite a los usuarios definir, ejecutar y gestionar flujos de trabajo complejos utilizando Kubernetes como entorno de ejecución subyacente. Todos los componentes de Argo Workflows son independientes de otros proyectos de Argo y generalmente se despliegan en su propio espacio de nombres (namespace) de Kubernetes en un clúster.

## **Bloques de Construcción**

### **Servidor de Argo (Argo Server)**

El Servidor de Argo es un componente central que gestiona los recursos, el estado y las interacciones generales del flujo de trabajo. Expone una API REST para la presentación, monitoreo y gestión de flujos de trabajo. El servidor mantiene el estado de los flujos de trabajo y su ejecución, e interactúa con el servidor de la API de Kubernetes para crear y gestionar recursos.

### **Controlador de Flujo de Trabajo (Workflow Controller)**

El Controlador de Flujo de Trabajo de Argo es un componente crítico dentro del sistema Argo Workflows. Es responsable de gestionar el ciclo de vida de los flujos de trabajo, interactuar con el servidor de la API de Kubernetes y garantizar la ejecución de los flujos de trabajo según sus especificaciones. El Controlador de Flujo de Trabajo de Argo observa continuamente el servidor de la API de Kubernetes en busca de cambios relacionados con los Recursos Personalizados (CRs) de Argo Workflows. El CR principal involucrado es el `Workflow`, que define la estructura y los pasos del flujo de trabajo. Al detectar la creación o modificación de un CR de `Workflow`, el controlador inicia la ejecución del flujo de trabajo correspondiente. El controlador es responsable de gestionar el ciclo de vida completo de un flujo de trabajo, incluyendo su creación, ejecución, monitoreo y finalización. También resuelve las dependencias entre los pasos dentro de un flujo de trabajo. Se asegura de que los pasos se ejecuten en el orden correcto, basándose en las dependencias especificadas en la definición del flujo de trabajo.

### **Interfaz de Usuario de Argo (Argo UI)**

La Interfaz de Usuario de Argo es una interfaz de usuario basada en web para monitorear y gestionar visualmente los flujos de trabajo. Permite a los usuarios ver el estado del flujo de trabajo, los registros y los artefactos, así como enviar nuevos flujos de trabajo.

Tanto el Controlador de Flujo de Trabajo como el Servidor de Argo se ejecutan en el espacio de nombres `argo`. Podemos optar por una de las instalaciones a nivel de clúster o de espacio de nombres; sin embargo, los Workflows y los Pods generados se ejecutarán en el espacio de nombres respectivo.

**Bloques de construcción de Argo Workflow**

![][image3]

Un usuario define un flujo de trabajo usando archivos YAML o JSON, especificando la secuencia de pasos, dependencias, entradas, salidas, parámetros y cualquier otra configuración relevante. Luego, el archivo de definición del flujo de trabajo se envía al clúster de Kubernetes donde está desplegado **Argo** **Workflows**. Este envío se puede hacer a través de la **CLI de Argo**, la **UI de Argo**, o programáticamente a través de clientes de la API de Kubernetes.

El componente **Workflow Controller** de **Argo Workflows** monitorea continuamente el clúster de Kubernetes en busca de nuevos envíos de flujos de trabajo o actualizaciones de flujos de trabajo existentes. Cuando se envía un nuevo flujo de trabajo, el Workflow Controller analiza la definición del flujo de trabajo para validar su sintaxis y semántica. Si hay errores o inconsistencias, el **Workflow Controller** los informa al usuario para su corrección.

Una vez que se valida la definición del flujo de trabajo, el **Workflow Controller** crea los recursos de Kubernetes necesarios para representar el flujo de trabajo, como los CRDs de Workflow (Definiciones de Recursos Personalizados) y los Pods, Services, ConfigMaps y Secrets asociados.

Finalmente, el Workflow Controller comienza a ejecutar los pasos definidos en el flujo de trabajo. Cada paso puede implicar la ejecución de contenedores, la ejecución de scripts o la realización de otras acciones especificadas por el usuario. Argo Workflows se asegura de que los pasos se ejecuten en el orden correcto basándose en las dependencias definidas en el flujo de trabajo.

### **Resumen del Flujo de Trabajo de Argo**

Cada Paso y DAG causa la generación de un Pod que comprende tres contenedores:

init: una plantilla que contiene un contenedor `init` que realiza tareas de inicialización. En este caso, muestra un mensaje y duerme durante 30 segundos, pero puedes reemplazar estos comandos con tus pasos de inicialización reales.
main: una plantilla que contiene el contenedor `main` que ejecuta el proceso principal una vez que la inicialización está completa.
wait: un contenedor que ejecuta tareas como limpieza, guardado de parámetros y artefactos.
Para obtener más información sobre los Experimentos, consulta la documentación oficial.

### **Ejemplos**

Argo Workflows es una herramienta versátil con una amplia gama de casos de uso en el contexto de Kubernetes y entornos en contenedores. Aquí hay algunos casos de uso comunes donde Argo Workflows puede ser beneficioso:

Para orquestar canalizaciones de procesamiento de datos de extremo a extremo, incluyendo tareas de extracción, transformación y carga (ETL).
En proyectos de aprendizaje automático, Argo Workflows puede orquestar tareas como el preprocesamiento de datos, el entrenamiento de modelos, la evaluación y el despliegue.
Argo Workflows puede servir como la base para las canalizaciones de integración y despliegue continuo (CI/CD). Permite la automatización de la construcción, prueba y despliegue de aplicaciones en un entorno de Kubernetes.
Para el procesamiento por lotes y tareas periódicas, Argo Workflows se puede configurar para que se ejecute a intervalos especificados o basándose en programaciones cron. Esto es útil para automatizar tareas rutinarias, generación de informes y otros trabajos programados.

# **27\. Preguntas\_Workflows**

Felicidades por completar el Capítulo 4 - "Argo Workflows". Realiza este cuestionario para comprobar tu comprensión de los conceptos que has aprendido hasta ahora.

### **Prueba de Conocimiento 4.1**

¿Qué es una 'plantilla' (template) en el contexto de Argo Workflows?

a.
Un Recurso Personalizado de Kubernetes
b.
**Una unidad de trabajo reutilizable**
c.
Un lenguaje de programación
d.
Un esquema de base de datos

### **Prueba de Conocimiento 4.2**

En Argo Workflows, ¿qué se utiliza para intercambiar datos entre pasos en un flujo de trabajo?

a.
**Artefactos**
b.
Contenedor
c.
Bucles
d.
DAG

💡 Explicación rápida
En Argo Workflows, los **artefactos** son el mecanismo utilizado para pasar datos entre pasos. Permiten que un paso produzca un archivo u objeto que un paso posterior puede consumir. Esto es especialmente útil para datos más grandes que no encajan bien en los parámetros.

### **Prueba de Conocimiento 4.3**

¿Cuál es el rol principal del Controlador de Flujo de Trabajo (Workflow Controller) en la arquitectura de Argo Workflows?

a.
Mantiene una caché del repositorio de Git que contiene los manifiestos de la aplicación
b.
Gestionar los Pods de Kubernetes
c.
**Ejecutar flujos de trabajo**
d.
Servir la API REST y la interfaz de usuario

🚀 Por qué esta es la elección correcta
**El Workflow Controller** es el motor principal de Argo Workflows. Su principal responsabilidad es:

* Interpretar el YAML del flujo de trabajo
* Orquestar y ejecutar los pasos del flujo de trabajo
* Gestionar el estado y las transiciones del flujo de trabajo
* Crear y monitorear los Pods de Kubernetes en los que se ejecuta cada paso

Así que, aunque interactúa con los Pods de Kubernetes, su rol general es más amplio: ejecuta y gestiona todo el ciclo de vida del flujo de trabajo.

### **Prueba de Conocimiento 4.4**

¿Qué tipo de plantilla de Workflow puede crear, modificar o eliminar recursos de Kubernetes?

a.
DAG
b.
Steps
c.
Suspend
d.
**Resource**

La respuesta correcta es d. **Resource**.

🛠️ Por qué esto es correcto
Una plantilla de tipo `Resource` en Argo Workflows está diseñada específicamente para:

* Crear recursos de Kubernetes
* Modificar recursos existentes
* Eliminar recursos

Esencialmente, permite que un flujo de trabajo interactúe directamente con la API de Kubernetes usando operaciones de estilo `kubectl` definidas declarativamente en el YAML del flujo de trabajo.

### **Prueba de Conocimiento 4.5**

¿Cuál de los siguientes no es un componente de Argo Workflows?

a.
Argo Server
b.
Argo UI
c.
**Argo Repo Server**
d.
Workflow Controller

La respuesta correcta es c. Argo Repo Server.

🧩 Por qué esta es la elección correcta
Argo Workflows consta de componentes clave como:

Argo Server – proporciona la interfaz de usuario y la API

Argo UI – la interfaz web (ahora parte de Argo Server)

Workflow Controller – ejecuta y gestiona los flujos de trabajo

El Argo Repo Server, sin embargo, no forma parte de Argo Workflows. Pertenece a Argo CD, un proyecto separado centrado en la entrega continua de GitOps.

# **28\. Argo Rollouts**

# **Argo Rollouts**

## **Introducción**

### **Resumen y Objetivos del Capítulo**

En este capítulo, profundizamos en Argo Rollouts, una herramienta fundamental dentro del conjunto de Argo, diseñada específicamente para las prácticas de Entrega Continua (CD) y GitOps. Argo Rollouts se puede utilizar como una herramienta independiente y, por lo tanto, no requiere ningún conocimiento previo de ArgoCD (u otras herramientas relacionadas con Argo). A través de discusiones temáticas y laboratorios prácticos, nuestro objetivo es equiparte con una comprensión integral de la arquitectura, instalación y uso de Argo Rollouts.

Al final de este capítulo, deberías ser capaz de:

Comprender y diferenciar varios patrones de Entrega Progresiva y decidir cuándo usar cada uno.
Tener una comprensión profunda de qué es Argo Rollouts y en qué escenarios podría ayudar.
Tener una visión general de la arquitectura y funcionalidad de Argo Rollouts.

### **Introducción a la Entrega Progresiva**

#### **Fundamentos de CI/CD y Entrega Progresiva en el Desarrollo de Software**

La Integración Continua (CI), la Entrega Continua (CD) y la Entrega Progresiva son conceptos clave en el desarrollo de software moderno, particularmente en el contexto de DevOps y las prácticas ágiles. Representan diferentes etapas o enfoques en el proceso de lanzamiento de software. Los discutiremos más en este capítulo.

#### **Integración Continua**

La Integración Continua es una práctica de desarrollo donde los desarrolladores integran frecuentemente su código en un repositorio compartido, preferiblemente varias veces al día. Cada integración es luego verificada por una compilación automatizada y pruebas automatizadas.

#### **Características de CI**

Commits de código frecuentes
Anima a los desarrolladores a integrar a menudo su código en la rama principal, reduciendo los desafíos de integración.

#### **Pruebas automatizadas**

Cubren los commits de código frecuentes. Ejecutan automáticamente pruebas sobre el nuevo código para asegurar que se integra bien con el código base existente. Esto no solo incluye pruebas unitarias, sino también cualquier otro método de prueba de orden superior, como pruebas de integración o de extremo a extremo.

#### **Detección inmediata de problemas**

Permite la detección y corrección rápida de problemas de integración.

#### **Reducción de problemas de integración**

Ayuda a minimizar los problemas asociados con la integración de nuevo código.

El objetivo principal de la CI es proporcionar retroalimentación rápida para que, si se introduce un defecto en la base de código, se identifique y corrija lo antes posible.

Una vez que el código está en nuestra rama principal, no se despliega en producción ni se lanza. Aquí es donde entra en juego el concepto de Entrega Continua.

### **Entrega Continua**

La Entrega Continua es una extensión de la CI, que asegura que el software se pueda lanzar de manera fiable en cualquier momento. Implica la automatización de todo el proceso de lanzamiento de software.

#### **Proceso de lanzamiento automatizado**

Cada cambio que pasa las pruebas automatizadas puede ser lanzado a producción a través de un proceso automatizado.

#### **Despliegues fiables**

Asegura que el software esté siempre en un estado desplegable.

#### **Ciclos de lanzamiento rápidos**

Facilita ciclos de lanzamiento frecuentes y más rápidos.

#### **Colaboración estrecha entre equipos**

Se requiere una alineación estrecha entre los equipos de desarrollo, QA y operaciones.

El objetivo de la Entrega Continua es establecer un proceso donde los despliegues de software se vuelvan predecibles, rutinarios y puedan ejecutarse bajo demanda.

### **Entrega Progresiva**

La entrega progresiva a menudo se describe como una evolución de la entrega continua. Se centra en lanzar actualizaciones de un producto de manera controlada y gradual, reduciendo así el riesgo del lanzamiento, acoplando típicamente la automatización y el análisis de métricas para impulsar la promoción o reversión automatizada de la actualización.

#### **Características de la Entrega Progresiva**

##### **Lanzamientos Canary**

Despliega gradualmente el cambio a un pequeño subconjunto de usuarios antes de desplegarlo a toda la base de usuarios.

##### **Feature flags (Banderas de características)**

Controla quién puede ver qué característica en la aplicación, permitiendo un despliegue selectivo y dirigido.

##### **Experimentos y pruebas A/B**

Prueba diferentes versiones de una característica con diferentes segmentos de la base de usuarios.

##### **Despliegues por fases**

Despliega lentamente las características a segmentos cada vez más grandes de la base de usuarios, monitoreando y ajustando según la retroalimentación.

El objetivo principal de la Entrega Progresiva es reducir el riesgo asociado con el lanzamiento de nuevas características y permitir una iteración más rápida al obtener retroalimentación temprana de los usuarios.

### **Estrategias de Despliegue**

Cada sistema de software es diferente, y el despliegue de sistemas complejos a menudo requiere pasos y verificaciones adicionales. Es por eso que diferentes estrategias de despliegue surgieron con el tiempo para gestionar el proceso de despliegue de nuevas versiones de software en un entorno de producción.

Estas estrategias son una parte integral de las prácticas de DevOps, especialmente en el contexto de los flujos de trabajo de CI/CD. La elección de una estrategia de despliegue puede impactar significativamente la disponibilidad, fiabilidad y experiencia del usuario de una aplicación o servicio de software.

En las siguientes páginas, presentaremos las cuatro estrategias de despliegue más importantes y discutiremos su impacto en la experiencia del usuario durante el despliegue:

Despliegue Recrear/fijo
Actualización continua (Rolling update)
Despliegue Azul-verde
Despliegue Canary

### **Introducción a la Entrega Progresiva**

#### **Despliegue Recrear/Fijo**

Un despliegue de tipo Recrear elimina la versión antigua de la aplicación antes de levantar la nueva versión. Como resultado, esto asegura que dos versiones de la aplicación nunca se ejecuten al mismo tiempo, pero hay tiempo de inactividad durante el despliegue. Esta estrategia es una opción para el objeto Deployment de Kubernetes.

#### **Actualización Continua (Rolling Update)**

Una Actualización Continua reemplaza lentamente la versión antigua con la nueva. A medida que la nueva versión se levanta, la versión antigua se escala hacia abajo para mantener el número total de instancias de la aplicación. Esto reduce el tiempo de inactividad y el riesgo, ya que la nueva versión se despliega gradualmente. Esta es la estrategia predeterminada del objeto Deployment de Kubernetes.

#### **Despliegue Azul-Verde**

Un despliegue azul-verde (a veces llamado rojo/negro) tiene tanto la versión nueva como la antigua de la aplicación desplegadas al mismo tiempo. Durante este tiempo, solo la versión antigua de la aplicación recibirá tráfico de producción. Esto permite a los desarrolladores ejecutar pruebas contra la nueva versión antes de cambiar el tráfico en vivo a la nueva versión. Una vez que la nueva versión está lista y probada, el tráfico se cambia (a menudo a nivel del balanceador de carga) del entorno antiguo al nuevo. La ventaja aquí es una reversión rápida en caso de problemas y un tiempo de inactividad mínimo durante el despliegue.

Un inconveniente importante de un despliegue azul-verde es que se crea el doble de instancias durante el tiempo del despliegue. Esto es un obstáculo común para este patrón.

Para obtener más información sobre el despliegue azul-verde, consulta el artículo de Martin Fowler.

#### **Despliegue Canary**

Un pequeño subconjunto de usuarios es dirigido a la nueva versión de la aplicación mientras que la mayoría todavía usa la versión antigua. Basándose en la retroalimentación y el rendimiento de la nueva versión, el despliegue se extiende gradualmente a más usuarios. Esto reduce el riesgo al afectar inicialmente a una pequeña base de usuarios, permite pruebas A/B y retroalimentación del mundo real. Si bien esto es técnicamente posible en Kubernetes nativo ajustando manualmente los Selectores de Servicio entre las versiones "antigua" y "nueva" de un despliegue, tener una solución automatizada es más ideal.

Se puede encontrar información más detallada en el artículo Canary Release de Danilo Sato.

### **Estrategias para Lanzamientos Fluidos y Confiables**

En resumen, las estrategias de despliegue son fundamentales en el desarrollo y las operaciones de software moderno para garantizar lanzamientos de software fluidos, seguros y eficientes. Satisfacen la necesidad de equilibrar el despliegue rápido con la estabilidad y fiabilidad de los entornos de producción.

#### **Tabla 5.1: Beneficios de Introducir Estrategias de Despliegue**

Beneficio Descripción
**Mitigación de riesgos**: Permiten despliegues más seguros al reducir el riesgo de introducir errores o problemas de rendimiento en el entorno de producción. Estrategias como los despliegues canary permiten una exposición gradual a los nuevos cambios.
**Experiencia del usuario**: Mantener una experiencia de usuario consistente y de alta calidad es esencial. Estrategias como los despliegues azul-verde minimizan el tiempo de inactividad y las posibles interrupciones en la experiencia del usuario.
**Retroalimentación y pruebas**: Proporcionan un marco para recopilar retroalimentación de usuarios del mundo real. Los despliegues canary, en particular, son valiosos para comprender cómo se desempeñan los cambios en un entorno en vivo.
**Capacidades de reversión**: En caso de que las nuevas versiones tengan problemas críticos, estrategias como los despliegues azul-verde permiten reversiones rápidas a la versión estable anterior.


#### **Tabla 5.2: Casos de Uso Comunes para Cada Estrategia**

Estrategia	Soportado Por	Casos de Uso Comunes
Despliegue fijo → Nativo de Kubernetes
La forma más básica de desplegar una carga de trabajo es cuando el tiempo de inactividad es aceptable.
A menudo, las cargas de trabajo con estado (por ejemplo, bases de datos) requieren una "recreación" para evitar la corrupción de datos.
Actualización continua	→Nativo de Kubernetes
Comúnmente utilizado para cargas de trabajo sin estado y de bajo mantenimiento como proxies, APIs RESTful, etc.
Despliegue azul-verde → Argo Rollouts
Úsalo cuando a) puedes permitirte el costo extra de ejecutar el doble de recursos y b) necesitas una opción de reversión rápida y fácil.
B/G también puede ser útil para escenarios de experimentación.
Puede ser ventajoso para actualizar servicios que dependen de conexiones con estado, por ejemplo, a través de WebSockets.
Despliegue Canary → Argo Rollouts
Úsalo siempre que se desee un despliegue parcial (experimentación con un subconjunto de usuarios, deseo de un despliegue gradual durante horas o días, querer que el despliegue dependa de ciertas condiciones).
Puede ser una buena alternativa si los despliegues son demasiado grandes y el costo de infraestructura de ejecutar un azul-verde completo es demasiado alto.

# **29\. Bloques de Construcción de Argo Rollouts**

# **Bloques de Construcción de Argo Rollouts**

En esta sección, discutiremos los bloques de construcción de Argo Rollouts. Para darte una idea de qué esperar, describiremos brevemente los componentes relevantes de una configuración de Argo Rollouts antes de descubrirlos con más detalle.

![][image4]

Visión general de los componentes que participan en un despliegue gestionado por Argo Rollouts
Fuente de la imagen: Argo

## **Componentes de Argo Rollouts**

### **Controlador de Argo Rollouts**

Un operador que gestiona los Recursos de Argo Rollout. Lee todos los detalles de un rollout (y otros recursos) y asegura el estado deseado del clúster.

### **Recurso Argo Rollout**

Un recurso personalizado de Kubernetes gestionado por el Controlador de Argo Rollouts. Es en gran medida compatible con el recurso nativo de Deployment de Kubernetes, añadiendo campos adicionales que gestionan las etapas, umbrales y técnicas de estrategias de despliegue sofisticadas, incluyendo despliegues canary y azul-verde.

### **Ingress y la API Gateway**

El recurso Ingress de Kubernetes se utiliza para habilitar la gestión del tráfico para varios proveedores de tráfico como las mallas de servicios (por ejemplo, Istio o Linkerd) o los Controladores de Ingress (por ejemplo, Nginx Ingress Controller).

La API Gateway de Kubernetes también es compatible con un plugin separado y proporciona una funcionalidad similar.

### **Service (Servicio)**

Argo Rollouts utiliza el recurso Service de Kubernetes para redirigir el tráfico de entrada a la versión de la carga de trabajo respectiva añadiendo metadatos específicos a un Service.

### **ReplicaSet**

Recurso estándar de ReplicaSet de Kubernetes utilizado por Argo Rollouts para hacer un seguimiento de las diferentes versiones de un despliegue de aplicación.

### **AnalysisTemplate y AnalysisRun**

El Análisis es una característica opcional de Argo Rollouts y permite la conexión de los Rollouts a un sistema de monitoreo. Esto permite la automatización de promociones y reversiones. Para realizar un análisis, un AnalysisTemplate define una consulta de métrica y su resultado esperado. Si la consulta coincide con la expectativa, un Rollout progresará o se revertirá automáticamente, si no lo hace. Un AnalysisRun es una instanciación de un AnalysisTemplate (similar a los Jobs de Kubernetes).

### **Proveedores de Métricas**

Los proveedores de métricas se pueden utilizar para automatizar las promociones o reversiones de un rollout. Argo Rollouts proporciona integración nativa para proveedores de métricas populares como Prometheus y otros sistemas de monitoreo.

Ten en cuenta que no todos los componentes mencionados son obligatorios para cada configuración de Argo Rollouts. El uso de recursos de Análisis o proveedores de métricas es completamente opcional y relevante para casos de uso más avanzados. También ten en cuenta que los componentes de Argo Rollouts son independientes de otros proyectos de Argo (como Argo CD o Argo Workflows) y no los requieren para funcionar correctamente.

## **Arquitectura y Componentes Centrales de Argo Rollouts**

### **Un Repaso: El ReplicaSet de Kubernetes**

Para comprender el funcionamiento de Argo Rollouts en el manejo de cargas de trabajo, es esencial entender algunos conceptos básicos de Kubernetes. Esencialmente, Argo Rollouts funciona de una manera bastante similar a los recursos de Deployment de Kubernetes. Lo que es menos conocido es que los Deployments proporcionan otra capa de abstracción para la gestión de cargas de trabajo. El recurso Deployment fue una adición relativamente tardía a Kubernetes, debutando en la versión 1.5 como parte de la API `apps/v1beta1` y alcanzando la estabilidad en la versión 1.9 con la API `apps/v1`. Antes de la introducción de los Deployments, la gestión de cargas de trabajo se realizaba utilizando ReplicaSets. ¡Y bajo el capó, se utilizan hasta el día de hoy!

Un ReplicaSet de Kubernetes es un recurso utilizado para asegurar que un número específico de réplicas de pod estén en funcionamiento en un momento dado. Esencialmente, es una forma de gestionar el ciclo de vida de los pods. La función principal de un ReplicaSet es mantener un conjunto estable de réplicas de pod en funcionamiento en un momento dado. Lo hace programando pods según sea necesario para alcanzar el número deseado.

Si un pod falla, el ReplicaSet lo reemplazará; si hay más pods de los necesarios, terminará los pods adicionales. Los ReplicaSets se utilizan para lograr redundancia y alta disponibilidad dentro de las aplicaciones de Kubernetes.

Para una orquestación más sofisticada como actualizaciones continuas, reversiones o escalado, un ReplicaSet no es suficiente. Kubernetes introdujo un concepto de nivel superior (y generalmente más conocido) llamado recurso Deployment que gestiona tanto el despliegue como la actualización de las aplicaciones.

Un despliegue es gestionado por el controlador de despliegue de Kubernetes y es responsable de actualizar los ReplicaSets proporcionando actualizaciones declarativas para ellos.

Vamos a crear un Deployment de proxies nginx para demostrar la relación de propiedad entre Deployment y ReplicaSet.

Comando:
$ kubectl create deploy nginx-deployment \--image=nginx \--replicas=3

Salida:

deployment.apps/nginx-deployment created

Ahora asegúrate de que se haya escalado correctamente.

Comando:

$ kubectl get deployment

Salida:

NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           47s

Comando:

$ kubectl get replicaset
Salida:

NAME                          DESIRED   CURRENT   READY   AGE
nginx-deployment-66fb7f764c   3         3         3       47s

El **ReplicaSet** `nginx-deployment-66fb7f764c` es gestionado por `nginx-deployment`. Puedes saberlo inspeccionando el ReplicaSet con el siguiente comando:

kubectl get replicaset nginx-deployment-66fb7f764c \-ojsonpath='{.metadata.ownerReferences}' | jq
\[
  {
    "apiVersion": "apps/v1",
    "blockOwnerDeletion": true,
    "controller": true,
    "kind": "Deployment",
    "name": "nginx-deployment",
    "uid": "1dd44efd-aab5-4475-aff2-32670201e2ef"
  }
\]

Como vemos, los `ownerReferences` del ReplicaSet indican que este recurso es "propiedad" de un recurso Deployment con el uid "1dd44efd-aab5-4475-aff2-32670201e2ef". Y de hecho, este uid coincide con el otro Deployment que acabamos de crear.

Comando:

$ kubectl get deployment nginx-deployment \-ojsonpath='{.metadata.uid}'

Salida:

1dd44efd-aab5-4475-aff2-32670201e2ef

Los Deployments son una gran invención de Kubernetes nativo y son una abstracción exitosa. Rara vez la gente gestiona sus pods manualmente a través de ReplicaSets. Los Deployments son el estándar.

Pero a pesar de todos los elogios, los recursos de Deployment todavía están limitados en sus capacidades. Todavía no admiten todas las estrategias de despliegue que describimos en la sección anterior, "Introducción a la Entrega Progresiva".

¡Hablemos de Argo Rollouts!

# **30\. Arquitectura y Componentes Centrales de Argo Rollouts**

# **Arquitectura y Componentes Centrales de Argo Rollouts**

## **Argo Rollouts**

Aquí, exploraremos el recurso Argo Rollouts, que es el elemento central en Argo Rollouts, permitiendo estrategias de despliegue avanzadas. Un Rollout, en esencia, es un recurso de Kubernetes que refleja de cerca la funcionalidad de un objeto Deployment de Kubernetes. Sin embargo, interviene como un sustituto más avanzado de los objetos Deployment, particularmente en escenarios que exigen un despliegue intrincado de técnicas de entrega progresiva.

### **Características Clave de Argo Rollouts**

Las implementaciones de Argo superan a las implementaciones regulares de Kubernetes gracias a varias características mejoradas.

#### **Funcionalidades de Argo Rollouts**

##### **Despliegues azul-verde**

Este enfoque minimiza el tiempo de inactividad y el riesgo al cambiar el tráfico entre dos versiones de la aplicación.

##### **Despliegues canary**

Despliega gradualmente los cambios a un subconjunto de usuarios para garantizar la estabilidad antes del despliegue completo.

##### **Enrutamiento de tráfico avanzado**

Se integra sin problemas con controladores de entrada y mallas de servicios, facilitando una gestión de tráfico sofisticada.

##### **Integración con proveedores de métricas**

Ofrece información analítica para despliegues azul-verde y canary, permitiendo tomar decisiones informadas.

##### **Toma de decisiones automatizada**

Promueve o revierte automáticamente los despliegues basándose en el éxito o fracaso de las métricas definidas.

El recurso Rollout es un recurso personalizado de Kubernetes introducido y gestionado por el Controlador de Argo Rollouts. Este controlador de Kubernetes monitorea los recursos de tipo Rollout y se asegura de que el estado descrito se refleje en el clúster.

El recurso Rollout mantiene una alta compatibilidad con el recurso convencional de Deployment de Kubernetes, pero se aumenta con campos adicionales. Estos campos son instrumentales para gobernar las fases, umbrales y metodologías de enfoques de despliegue avanzados, como las estrategias canary y azul-verde.

Es crucial entender que el controlador de Argo Rollouts está sintonizado exclusivamente con los cambios en los recursos de Rollout. Permanece inactivo para los recursos de despliegue estándar. En consecuencia, para utilizar Argo Rollouts para los Deployments existentes, se requiere una migración de los Deployments tradicionales a Rollouts.

En general, los recursos de Deployment y Rollout se ven bastante similares. Consulta la siguiente tabla para entender las diferencias mínimas entre ambos.

| Recurso Deployment | Recurso Argo Rollout | Comentario |
| :---- | :---- | :---- |
| apiVersion: apps/v1 kind: Deployment metadata:   name: nginx-deployment spec: | apiVersion: argoproj.io/v1alpha1 kind: Rollout metadata:   name: nginx-rollout spec: | Metadatos básicos del recurso. |
|   replicas: 3 |   replicas: 3 | Número de pods deseados. Por defecto es 1. |
|   selector:     matchLabels:       app: nginx |   selector:     matchLabels:       app: nginx | Selector de etiquetas para los pods. |
|   template:     metadata:       labels:         app: nginx     spec:       containers:       \- name: nginx         image: nginx |   template:     metadata:       labels:         app: nginx     spec:       containers:       \- name: nginx         image: nginx | Describe la plantilla de pod que se utilizará para instanciar los pods. La plantilla no difiere. |
|   strategy:     type: RollingUpdate |   strategy:     blueGreen: {} | Una estrategia de Deployment puede ser "RollingUpdate" (predeterminada) o "Recreate". Una estrategia de Rollout puede ser "blueGreen" o "canary". |


Por supuesto, hay muchas más opciones de configuración para controlar el comportamiento de un Rollout. Consulta la especificación oficial de Argo Rollouts para más opciones.

## **Migrando Despliegues Existentes a Rollouts**

La similitud de la especificación de Deployments y Rollouts facilita la conversión de un tipo de recurso a otro. Argo Rollouts admite una excelente manera de migrar los recursos de Deployment existentes a Rollouts.

Al proporcionar un `spec.workloadRef` en lugar de `spec.template`, un Rollout puede hacer referencia a la plantilla de un Deployment:

apiVersion: argoproj.io/v1alpha1
kind: Rollout
metadata:
  name: nginx-rollout
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  workloadRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx-deployment
\[...\]

El Rollout obtendrá la información de la plantilla del Deployment (en nuestro ejemplo llamado `nginx-deployment`) e iniciará el número de pods especificado en el Rollout.

Ten en cuenta que los ciclos de vida de Deployment y Rollouts son distintos y son gestionados por sus respectivos controladores. Esto significa que el controlador de Deployment de Kubernetes no comenzará a gestionar los Pods creados por el Rollout. Además, el Rollout no comenzará a gestionar los pods que son controlados por el Deployment.

Esto permite una introducción sin tiempo de inactividad de Argo Rollouts a tu clúster existente. Además, hace posible la experimentación con múltiples escenarios de despliegue.

### **Discusión: ¿Crear Rollouts o Referenciar Deployments desde Rollouts?**

Como los recursos de Rollout pueden existir y operar sin los Deployments nativos, puede surgir la siguiente pregunta: ¿Debería siempre referenciar Deployments o es mejor empezar de nuevo con un recurso de Rollout independiente, sin la dependencia de una referencia?

Y la respuesta simple es... depende.

Generalmente, `workloadRef` se inventó para permitir una forma simple y fluida de migrar de Deployments a Rollouts. Incluso lo consideramos útil, ya que los administradores que no están familiarizados con Argo Rollouts podrían confundirse si ven una serie de Pods en ejecución pero ni un Deployment ni un StatefulSet en funcionamiento. Para reducir la barrera, referenciar Deployments existentes desde un Rollout puede ser una buena opción.

Si utilizas la referencia a Deployments, el controlador de Argo copiará el número de generación del Deployment referenciado y lo almacenará en un campo de estado llamado `workloadObservedGeneration`. Por lo tanto, la propia anotación `rollout.argoproj.io/workload-generation` del rollout siempre debe coincidir con la generación del despliegue. Esto ayuda a identificar desviaciones debido a la manipulación de cualquiera de los recursos.

Sin embargo, la referencia conlleva el costo de otra dependencia de recursos. ¡Otro recurso más que verificar en caso de fallo!

Por lo tanto, si estás seguro de que quieres trabajar con Argo Rollouts, utiliza el Recurso de Rollout nativo.

Pista: También es posible migrar un recurso de Rollout a un Deployment nativo. Consulta la documentación oficial para más información.

Recursos de aprendizaje adicionales:

Para explorar la especificación detallada de un Rollout, visita Especificación de Argo Rollouts.
Para obtener orientación sobre la transición de un Deployment a un Rollout, consulta Migrando un Deployment a un Rollout.

### **Bloques de Construcción Adicionales**

Si bien el Controlador de Argo Rollouts y el recurso de Rollout correspondiente son los componentes principales, existen otros bloques de construcción que habilitan y extienden la funcionalidad de Argo Rollouts.

![][image5]

Visión general de los componentes que participan en un despliegue gestionado por Argo Rollouts
Fuente de la imagen: Argo.

### **Recursos de Ingress y Service**

Recursos Relevantes para el Enrutamiento de Tráfico

#### **Kubernetes Ingress**

Un Ingress de Kubernetes es un recurso nativo de Kubernetes que gestiona el acceso externo a los servicios en un clúster (generalmente a través de HTTP). Un Ingress permite definir reglas para que las conexiones entrantes lleguen a los Servicios de Kubernetes internos del clúster. Como tal, son una abstracción importante para controlar programáticamente el flujo del tráfico de red entrante. Incluso se pueden utilizar para la terminación de SSL/TLS.

Este enfoque se amplía con la API Gateway de Kubernetes. La API Gateway divide el enfoque de Ingress en el Gateway de Kubernetes y el HTTPRoute de Kubernetes, este último gestionado por Argo Rollouts como lo hacía con Ingress. La ventaja es que la API Gateway proporciona modos de acceso adicionales más allá de HTTP/HTTPS y no requiere código específico del controlador como lo hacía Ingress.

#### **Kubernetes Service**

Un Service de Kubernetes es un recurso que abstrae cómo exponer una aplicación que se ejecuta en un conjunto de Pods. Los Services pueden balancear el tráfico y proporcionar descubrimiento de servicios dentro del clúster. El rol principal de un Service es proporcionar una dirección IP y un número de puerto consistentes para acceder a la aplicación en ejecución, independientemente de los cambios en los pods.

En el contexto de Argo Rollouts, estos recursos juegan un papel fundamental cuando se trata, por ejemplo, de despliegues canary. El comportamiento general de los recursos Service e Ingress no es diferente cuando se usan con Argo. Argo Rollouts utiliza los Servicios de Kubernetes para gestionar el flujo de tráfico a diferentes versiones de una aplicación durante un proceso de rollout y lo hacen aumentando el servicio con metadatos adicionales.

#### **Hash de la Plantilla del Pod (Pod Template Hash)**

Argo Rollouts utiliza el Hash de la Plantilla del Pod, que identifica de forma única los Pods de un ReplicaSet común. Así que para cambiar el tráfico entrante del ReplicaSet "antiguo" a nuestro nuevo ReplicaSet, el controlador de Argo Rollouts muta el `spec.selector` del Service para que coincida con el nuevo Hash de la Plantilla del Pod.

![][image6]

Los Servicios de Kubernetes tienen selectores que encuentran los pods correspondientes según su conjunto de etiquetas; la etiqueta `pod-template-hash` se añade a cada ReplicaSet y se utiliza para tomar decisiones de enrutamiento.

#### **ReplicaSets Estables/Canary**

Al introducir un "servicio estable" y "servicios canary" en la especificación de los Rollouts, Argo no solo puede cambiar el tráfico a los ReplicaSets Estables/Canary, sino también decidir sobre la distribución de qué ReplicaSet debe recibir cuánto tráfico.

#### **Análisis y Experimentos de Rollout**

La capacidad de dividir el tráfico entre cargas de trabajo estables y canary es buena. Pero, ¿cómo decidimos si la carga de trabajo canary está funcionando bien y, por lo tanto, se considera "estable"? ¡Exacto, métricas! Un operador observaría de cerca el sistema de monitoreo (por ejemplo, Prometheus, VMWare Wavefront u otros) en busca de ciertas métricas que indiquen que la aplicación está funcionando bien. Si estás pensando que esta "observación de métricas y toma de decisiones" podría automatizarse, ¡tienes razón!

Argo Rollouts permite al usuario ejecutar "Análisis" durante el proceso de entrega progresiva. Se centra principalmente en evaluar y garantizar el éxito del despliegue basándose en criterios definidos. Estos criterios pueden incluir métricas personalizadas de tu proveedor de monitoreo de métricas específico (consulta la documentación oficial para una lista concluyente de proveedores de métricas compatibles).

El proceso de análisis en Argo Rollouts implica los siguientes recursos personalizados que trabajan de la mano con los recursos ya discutidos.

 Tabla 5.4: Definiciones de Recursos Personalizados de Análisis

Plantillas	Descripción/Caso de Uso
AnalysisTemplate	Esta plantilla define las métricas a consultar y las condiciones para el éxito o el fracaso. El AnalysisTemplate especifica qué métricas deben ser monitoreadas y los umbrales para determinar el éxito o el fracaso de un despliegue. Puede ser parametrizado con valores de entrada para hacerlo más dinámico y adaptable a diferentes situaciones.
AnalysisRun	Un AnalysisRun es una instanciación de un AnalysisTemplate. Es un recurso de Kubernetes que se comporta de manera similar a un job en el sentido de que se ejecuta hasta su finalización. El resultado de un AnalysisRun puede ser exitoso, fallido o inconcluso, y este resultado impacta directamente en la progresión de la actualización del Rollout. Si el AnalysisRun es exitoso, la actualización continúa; si falla, la actualización se aborta; y si es inconcluso, la actualización se pausa.

Los recursos de análisis permiten a Argo Rollouts tomar decisiones informadas durante el proceso de despliegue, como promover una nueva versión, revertir a una versión anterior o pausar el rollout para una mayor investigación, basándose en datos en tiempo real y criterios de éxito predefinidos.
**AnalysisRuns** admite varios proveedores como Prometheus o múltiples otras soluciones de monitoreo para obtener mediciones para el análisis. Esas mediciones pueden luego utilizarse para automatizar las decisiones de promoción.
Además de solo mirar las métricas, hay otras formas de decidir si tu rollout va bien. La más básica (pero comúnmente utilizada) podría ser el proveedor de "Job" de Kubernetes: si un job tiene éxito, la métrica se considera "exitosa". Si el job devuelve cualquier cosa que no sea el código de retorno cero, la métrica se considera "fallida".
El proveedor Web ayuda con la integración perfecta a servicios personalizados para ayudar a tomar decisiones de promoción.
Recuerda, no es obligatorio usar análisis y métricas cuando estás desplegando actualizaciones en Argo Rollouts.
Si quieres, puedes controlar el rollout tú mismo. Esto significa que puedes detener o avanzar el rollout cuando lo desees. Puedes hacerlo a través de la API o la línea de comandos. Además, no tienes que depender de métricas automáticas para usar Argo Rollouts. Está totalmente bien combinar pasos automáticos, como los basados en análisis, con tus propios pasos manuales.

### **Experimentos**

Los Experimentos son una característica extendida de Argo Rollouts diseñada para probar y evaluar cambios en dos o más versiones de una aplicación en un entorno controlado y temporal. El recurso personalizado Experiment puede lanzar AnalysisRuns junto con ReplicaSets. Esto es útil para confirmar que los nuevos ReplicaSets están funcionando como se esperaba.

Puedes usar experimentos en Argo Rollouts para probar diferentes versiones de tu aplicación al mismo tiempo. Esto es como hacer pruebas A/B/C. Puedes configurar cada experimento con su propia versión de la aplicación para ver cuál funciona mejor. Cada experimento utiliza una plantilla para definir su versión específica de la aplicación.

Lo bueno de estos experimentos es que puedes ejecutar varios de ellos simultáneamente, y cada uno es independiente de los demás. Esto significa que no interfieren entre sí.

Para obtener más información sobre Análisis o Experimentos, consulta la documentación oficial.

# **31.Preguntas sobre Argo Rollouts**

## **Prueba de Conocimiento 5.1**

Selecciona todas las respuestas que apliquen. ¿Qué tipo de estrategias de despliegue no son posibles con Kubernetes nativo (vanilla)?
a. Despliegue Recrear/fijo
b. Actualización continua
c. Despliegue Azul-verde
d. Despliegue Canary

Kubernetes nativo (out-of-the-box) **solo admite dos estrategias de despliegue integradas**:

* **Recreate**
* **RollingUpdate**

Cualquier cosa más allá de eso requiere herramientas adicionales (Argo Rollouts, Flagger, enrutamiento de malla de servicios, controladores personalizados, etc.).

### **✅ Respuestas correctas**

**c. Despliegue Azul-verde**

**d. Despliegue Canary**

### **Por qué**

* **Recreate (a)** → Soportado nativamente
* **Rolling update (b)** → Soportado nativamente
* **Blue-green (c)** → No soportado sin herramientas externas
* **Canary (d)** → No soportado sin herramientas externas

## **Prueba de Conocimiento 5.2**

¿Verdadero o Falso? Al migrar un Deployment existente a un recurso Argo Rollout, el controlador de Argo Rollout toma posesión del ReplicaSet del Deployment.

a.
Verdadero
b.
Falso

✅ Respuesta correcta: b. False
📌 Explicación clara
Cuando conviertes un Deployment de Kubernetes en un Argo Rollout, lo que realmente haces es crear un nuevo recurso Rollout que reemplaza al Deployment.

Pero aquí está el detalle importante:

❗ El controlador de Argo Rollouts no toma control del ReplicaSet que pertenecía al Deployment original.
¿Por qué?

El Deployment y el Rollout son controladores distintos, cada uno administra sus propios ReplicaSets.

Cuando migras, el Deployment original sigue siendo dueño de su ReplicaSet.

El nuevo Rollout creará su propio ReplicaSet, independiente del anterior.

Por eso, la migración no implica que Argo Rollouts “herede” o “adopte” el ReplicaSet previo.

🧩 Ejemplo rápido para visualizarlo
Tienes un Deployment llamado mi-app.

Ese Deployment controla un ReplicaSet mi-app-abc123.

Migras a un Rollout llamado mi-app.

El Rollout crea un nuevo ReplicaSet mi-app-xyz789.

El ReplicaSet abc123 sigue siendo propiedad del Deployment (hasta que lo elimines).

## **Prueba de Conocimiento 5.3**

¿Cuál de los siguientes recursos no son recursos de Argo Rollout?

a.
ExperimentTemplate
b.
AnalysisRuns
c.
Rollouts
d.
Experiments

✅ Respuesta correcta: a. ExperimentTemplate
📌 Explicación
Argo Rollouts define varios tipos de recursos propios. Entre ellos están:

✔️ Recursos que SÍ pertenecen a Argo Rollouts
Rollouts → El recurso principal que reemplaza a un Deployment.

AnalysisRuns → Ejecutan análisis (por ejemplo, métricas) durante un rollout.

Experiments → Permiten probar múltiples versiones simultáneamente.

❌ Recurso que NO pertenece a Argo Rollouts
ExperimentTemplate → No existe en Argo Rollouts.

Lo que sí existe es AnalysisTemplate, pero no ExperimentTemplate.

Por eso, la única opción incorrecta (no perteneciente a Argo Rollouts) es a.

## **Prueba de Conocimiento 5.4**

El administrador del clúster eliminó accidentalmente el recurso de despliegue del controlador de Argo Rollout. ¿Qué sucede con mis rollouts existentes?

a.
Nada; siguen funcionando normalmente y solo las actualizaciones no se propagarán al ReplicaSet.
b.
Todos los recursos de rollout y los respectivos ReplicaSets en el clúster se terminarán y los pods se detendrán de forma elegante.
c.
El panel de control de Argo Rollout dejará de funcionar.

✅ Respuesta correcta: a. Nada; siguen funcionando normalmente y solo las actualizaciones no se propagarán al ReplicaSet.
📌 Explicación clara y directa
El controlador de Argo Rollout es el componente que:

Gestiona los Rollouts

Crea y actualiza ReplicaSets

Ejecuta análisis, pausas, steps, etc.

Pero una vez que un Rollout ya creó sus ReplicaSets y Pods, estos recursos son controlados por Kubernetes, no por el controlador.

Por eso:
✔️ Los rollouts existentes siguen funcionando
Los Pods y ReplicaSets ya creados continúan ejecutándose sin interrupción.

❗ Lo que sí se pierde:
No podrás actualizar los Rollouts.

No se ejecutarán steps, análisis, pausas, ni progresiones.

No se crearán nuevos ReplicaSets.

❗ Lo que NO ocurre:
❌ No se borran los Rollouts

❌ No se borran los ReplicaSets

❌ No se detienen los Pods

❌ No se termina la aplicación

Sobre la opción (c):
El dashboard de Argo Rollouts depende del controlador, así que probablemente dejará de mostrar información actualizada, pero esa no es la respuesta principal que busca la pregunta.

## **Prueba de Conocimiento 5.5**

¿Verdadero o Falso? Aunque Argo Rollouts permite el enrutamiento de tráfico avanzado, solo es compatible con Controladores de Ingress.

a.
Verdadero
b.
Falso

✅ Respuesta correcta: b. False
📌 Explicación sencilla
Argo Rollouts sí permite enrutamiento avanzado de tráfico, pero no se limita únicamente a Ingress Controllers.

De hecho, Argo Rollouts puede integrarse con:

✔️ Ingress Controllers
NGINX Ingress

ALB Ingress

GKE Ingress

✔️ Service Meshes (muy común para canary y blue‑green avanzados)
Istio

Linkerd

SMI (Service Mesh Interface)

App Mesh

✔️ Gateways / API Gateways
Traefik

Ambassador

Por eso, decir que “solo funciona con Ingress Controllers” es incorrecto.

# **32.Argo Events**

# **Argo Events**

## **Introducción**

### **Resumen y Objetivos del Capítulo**

En este capítulo, discutimos Argo Events, explorando su rol en la implementación de la arquitectura orientada a eventos dentro de Kubernetes. Comenzando con una visión general conceptual, entenderemos los componentes clave de Argo Events: Fuentes de Eventos (Event Sources), Sensores (Sensors), Bus de Eventos (EventBus) y Desencadenadores (Triggers), y su importancia en Kubernetes. El capítulo luego transita al aprendizaje práctico, con laboratorios enfocados en la configuración de fuentes de eventos y desencadenadores, y la integración de Argo Events con sistemas externos como webhooks y colas de mensajes.

Al final de este capítulo, deberías ser capaz de:

Aprender cómo las fuentes de eventos inician el proceso orientado a eventos en Kubernetes.
Comprender el mecanismo de detección y respuesta de los sensores y desencadenadores en sistemas orientados a eventos.
Entender la importancia del EventBus en la gestión del flujo de eventos dentro de Argo Events.

## **Los Componentes Principales**

### **Arquitectura Orientada a Eventos**

En esta sección, exploramos el concepto de arquitectura orientada a eventos (EDA) y su aplicación práctica en entornos de Kubernetes. A diferencia de las arquitecturas tradicionales donde los componentes operan de manera lineal, de solicitud-respuesta, la EDA se basa en un modelo más dinámico y fluido. Este modelo es particularmente relevante en Kubernetes, un sistema que gestiona aplicaciones en contenedores a través de clústeres y prospera en la capacidad de respuesta y adaptabilidad.

En el núcleo de Kubernetes se encuentran los eventos: estas son diversas acciones o cambios dentro del sistema, como cambios en el ciclo de vida de los pods o actualizaciones de servicios. La EDA en Kubernetes implica responder a estos eventos de una manera que sea tanto automatizada como escalable. Este método de operación permite un manejo más eficiente del estado en constante cambio dentro de un clúster de Kubernetes.

Argo Events entra en escena como una herramienta diseñada para Kubernetes, destinada a facilitar la implementación de paradigmas orientados a eventos. No es solo un complemento, sino una integración que amplifica las capacidades de Kubernetes. Echemos un vistazo a los componentes principales de Argo Events.

Fuente de Eventos (Event Source): Aquí es donde se generan los eventos. Las fuentes de eventos en Argo Events pueden ser cualquier cosa, desde un simple webhook o un mensaje de una cola de mensajes, hasta un evento programado. Comprender las fuentes de eventos es clave para saber cómo interactuará tu sistema con diversos estímulos externos e internos. A continuación se proporciona un ejemplo de una fuente de eventos:

apiVersion: argoproj.io/v1alpha1
kind: EventSource
metadata:
  name: webhook-example
spec:
  service:
    ports:
      \- port: 12000
        targetPort: 12000
  webhook:
    example-endpoint:
      endpoint: /example
      method: POST

Sensor: Los sensores son los escuchadores de eventos en Argo Events. Esperan eventos específicos de las fuentes de eventos y, al detectar estos eventos, activan acciones predefinidas. Comprender los sensores implica saber cómo responder a diferentes tipos de eventos. Un sensor se especificaría con la siguiente especificación:

apiVersion: argoproj.io/v1alpha1
kind: Sensor
metadata:
  name: webhook-sensor
spec:
  dependencies:
    \- name: example-dep
      eventSourceName: webhook-example
      eventName: example-endpoint
  triggers:
    \- template:
        name: k8s-trigger
        k8s:
          group: batch
          version: v1
          resource: jobs
          operation: create
          source:
            resource:
              apiVersion: batch/v1
              kind: Job
              metadata:
                generateName: webhook-job-
              spec:
                template:
                  spec:
                    containers:
                      \- name: hello
                        image: busybox
                        command: \["echo", "Hello from Argo Sensor\!"\]
                    restartPolicy: Never

EventBus: El EventBus actúa como la columna vertebral para la distribución de eventos dentro de Argo Events. Es responsable de gestionar la entrega de eventos desde las fuentes a los sensores. Comprender el EventBus es crucial para gestionar el flujo de eventos dentro de tu sistema.

apiVersion: argoproj.io/v1alpha1
kind: EventBus
metadata:
  name: default
spec:
  nats:
    native:
      replicas: 1

Trigger: Los desencadenadores en Argo Events son los mecanismos que responden a los eventos detectados por los sensores. Pueden realizar una amplia gama de acciones, desde iniciar un flujo de trabajo hasta actualizar un recurso. Comprender los desencadenadores es esencial para automatizar las respuestas a los eventos. Los desencadenadores se definen dentro de la especificación de un sensor, por lo que el siguiente extracto se centra en el desencadenador en sí:

trigger:
  template:
    name: argo-workflow-trigger
    argoWorkflow:
      source:
        resource:
          apiVersion: argoproj.io/v1alpha1
          kind: Workflow
          metadata:
            generateName: hello-world-
          spec:
            entrypoint: whalesay
            templates:
            \- name: whalesay
              container:
                image: docker/whalesay:latest
                command: \[cowsay\]
                args: \["Hello from Argo Events\!"\]

Arquitectura de Argo Events

![][image7]

La imagen muestra la arquitectura de Argo Events, con tres componentes principales: Fuente de Eventos, Bus de Eventos y Sensor, cada uno con un controlador y un despliegue. La Fuente de Eventos recibe varios eventos (como SNS, SQS, GCP PubSub, S3, Webhooks, etc.), que son gestionados por el Controlador de la Fuente de Eventos y pasados al Despliegue de la Fuente de Eventos. Esto se conecta al Bus de Eventos con NATS Streaming a través del Controlador del Bus de Eventos. Finalmente, el Controlador del Sensor gestiona el Despliegue del Sensor, que desencadena flujos de trabajo en Kubernetes y funciones en AWS Lambda, ilustrado por los respectivos iconos.

# **33.Preguntas sobre Argo Events**

## **Prueba de Conocimiento 6.1**

¿Cuál es la función principal de un 'Sensor' en Argo Events y cómo interactúa con las Fuentes de Eventos (Event Sources)?

a.
Los sensores crean nuevos eventos para el EventBus
b.
Los sensores esperan eventos específicos de las Fuentes de Eventos y activan acciones predefinidas al detectarlos
c.
Los sensores sirven como un mecanismo de almacenamiento para los datos de los eventos
d.
Los sensores gestionan la distribución de eventos dentro del sistema

✅ Respuesta correcta: b. Los sensores esperan eventos específicos de las Fuentes de Eventos y activan acciones predefinidas al detectarlos
📌 Explicación sencilla
En Argo Events, los componentes principales son:

Event Sources → generan eventos (por ejemplo: webhook, S3, Kafka, cron, etc.)

EventBus → transporta los eventos

Sensors → escuchan eventos y ejecutan acciones

Entonces, ¿qué hace un Sensor?
✔️ Un Sensor escucha eventos provenientes de uno o varios Event Sources
✔️ Cuando detecta un evento que coincide con su condición, dispara una acción

Estas acciones pueden ser:

Crear un Workflow de Argo Workflows

Enviar una notificación

Ejecutar un script

Crear un recurso de Kubernetes

Llamar a un servicio externo

❗ Lo que NO hace un Sensor
❌ No crea eventos (eso lo hacen los Event Sources)

❌ No almacena eventos

❌ No distribuye eventos dentro del sistema

## **Prueba de Conocimiento 6.2**

¿Cuál es la principal responsabilidad del 'EventBus' en Argo Events y por qué es crítico en una arquitectura orientada a eventos?

a.
Actúa como una herramienta de monitoreo para todos los eventos en el sistema
b.
Sirve como la columna vertebral para la distribución de eventos, gestionando la entrega de eventos desde las fuentes a los sensores
Correcto
c.
Es responsable de activar acciones en respuesta a los eventos
d.
Genera eventos basados en los cambios del sistema

✅ Respuesta correcta: b. Sirve como la columna vertebral para la distribución de eventos, gestionando la entrega de eventos desde las fuentes a los sensores
📌 Explicación sencilla y directa
En Argo Events, el EventBus es el componente que transporta los eventos.
Es literalmente el “sistema circulatorio” del framework.

✔️ ¿Qué hace el EventBus?
Recibe eventos desde los Event Sources

Los distribuye de forma confiable a los Sensors

Garantiza que los eventos no se pierdan

Asegura que los sensores reciban los eventos en el orden correcto

Por eso se dice que es el backbone (columna vertebral) del sistema.

❗ Lo que NO hace el EventBus
❌ No monitorea eventos (eso sería más propio de un observador)

❌ No dispara acciones (eso lo hacen los Sensors)

❌ No genera eventos (eso lo hacen los Event Sources)

## **Prueba de Conocimiento 6.3**

¿Cuál es el rol principal de un 'Trigger' en Argo Events dentro de un entorno de Kubernetes?

a.
Responder a los eventos detectados por los sensores, realizando acciones como iniciar un flujo de trabajo
Correcto
b.
Monitorear el rendimiento del clúster de Kubernetes
c.
Almacenar y archivar datos de eventos para su análisis
d.
Gestionar el despliegue de nuevos pods

✅ Respuesta correcta: a. Responder a los eventos detectados por los sensores, realizando acciones como iniciar un flujo de trabajo
📌 Explicación clara y directa
En Argo Events, el flujo funciona así:

Event Source → genera un evento

EventBus → transporta el evento

Sensor → detecta el evento

Trigger → ejecuta la acción asociada

Entonces, ¿qué es un Trigger?
✔️ Un Trigger es la acción que se ejecuta cuando un Sensor detecta un evento.
Puede hacer cosas como:

Lanzar un Argo Workflow

Crear un recurso de Kubernetes (Deployment, Job, ConfigMap…)

Enviar una notificación

Llamar a un webhook externo

Publicar un mensaje en un sistema externo

❗ Lo que NO hace un Trigger
❌ No monitorea el clúster

❌ No almacena eventos

❌ No gestiona despliegues de pods

## **Prueba de Conocimiento 6.4**

¿Cuál es el propósito de usar el comando de reenvío de puertos (port-forwarding) en el proceso de configuración de Argo Events dentro del entorno de Kubernetes?

a.
Aumentar la velocidad de procesamiento del clúster de Kubernetes
b.
Permitir la comunicación directa entre la máquina local y el pod de EventSource
Correcto
c.
Crear una copia de seguridad de los datos del clúster de Kubernetes
d.
Instalar nodos de Kubernetes adicionales automáticamente

## **Prueba de Conocimiento 6.5**

¿Cuál de los siguientes no es un componente de Argo Events?

a.
Event Source
b.
Sensor
c.
EventBus
d.
Event Logger

# **35\. Preguntas Finales**

## **Pregunta 1**

¿Cuál es la secuencia correcta de componentes que manejan eventos en Argo Events, desde la generación hasta la acción?

a.
Trigger → EventBus → Sensor → Event Source
b.
EventBus → Sensor → Event Source → Trigger
c.
Event Source → EventBus → Sensor → Trigger
d.
Event Source → Trigger → Sensor → EventBus

## **Pregunta 2**

¿Cuáles de las siguientes herramientas deben instalarse localmente para crear un clúster de Kubernetes para las pruebas de Argo?

a.
Terraform y Ansible
b.
Docker, kubectl y kind
c.
Jenkins y Git
d.
Helm y Prometheus

## **Pregunta 3**

¿Cuál de los siguientes es un caso de uso válido para Argo Workflows?

a.
Gestionar políticas RBAC de Argo CD
b.
Orquestar canalizaciones de aprendizaje automático
c.
Reemplazar la red de Docker
d.
Servir como un controlador de entrada de Kubernetes

✅ Respuesta correcta: b. Orchestrating machine learning pipelines
📌 Explicación clara
Argo Workflows es un motor de workflow orchestration nativo de Kubernetes.
Su propósito es ejecutar tareas complejas, secuenciales o paralelas, especialmente en entornos de automatización.

✔️ Casos de uso típicos de Argo Workflows
Pipelines de machine learning (entrenamiento, validación, despliegue)

Procesamiento de datos (ETL, batch jobs)

Automatización CI/CD

Tareas científicas o de computación intensiva

Workflows multi‑paso con dependencias

Por eso, la opción b es totalmente correcta.

## **Pregunta 4**

En Argo Workflows, ¿qué te permite encapsular lógica reutilizable que se puede compartir entre múltiples flujos de trabajo?

a.
*WorkflowTemplate*
b.
Workflow DAGs
c.
Output Parameters
d.
Argo Server

✅ Respuesta correcta: a. WorkflowTemplate
📌 Explicación sencilla
En Argo Workflows, un WorkflowTemplate es un recurso que te permite:

Definir pasos, tareas, contenedores o DAGs una sola vez

Reutilizar esa lógica en múltiples Workflows

Mantener consistencia entre pipelines

Evitar duplicar YAML en cada workflow

Es como crear una “plantilla” de workflow que luego puedes invocar desde:

Workflows normales

CronWorkflows

Otros WorkflowTemplates

❌ Por qué las otras opciones no aplican
b. Workflow DAGs
Un DAG organiza tareas con dependencias, pero no es reutilizable por sí mismo.

c. Output Parameters
Sirven para pasar datos entre pasos, no para reutilizar lógica.

d. Argo Server
Es la interfaz/API, no encapsula lógica.

## **Pregunta 5**

¿Qué afirmación describe mejor cómo Argo Rollouts utiliza los recursos de Service de Kubernetes durante un despliegue Azul-Verde?

a.
Argo Rollouts elimina el Service al cambiar el tráfico
b.
Argo Rollouts modifica el serviceAccount para actualizar las etiquetas de los pods
c.
Argo Rollouts reemplaza manualmente el hash de la plantilla del pod
d.
Argo Rollouts actualiza el selector del Service para enrutar el tráfico al nuevo ReplicaSet

✅ Respuesta correcta: d. Argo Rollouts updates the selector of the Service to route traffic to the new ReplicaSet
📌 Explicación clara y directa
En un despliegue Blue‑Green, Argo Rollouts utiliza dos Services:

✔️ activeService
Apunta a la versión actualmente en producción (el “blue”).

✔️ previewService
Apunta a la nueva versión que está lista para ser promovida (el “green”).

Cuando decides hacer el switch:

👉 Argo Rollouts actualiza el selector del activeService
para que deje de apuntar al ReplicaSet viejo y empiece a apuntar al nuevo.

No borra Services, no toca serviceAccounts, no manipula hashes manualmente.
Todo se basa en cambiar el selector del Service, que es la forma estándar de Kubernetes para redirigir tráfico.

## **Pregunta 6**

¿Cuál de las siguientes opciones describe mejor el propósito del Controlador de Aplicación de Argo CD?

a.
Proporciona la interfaz de usuario para Argo CD
b.
Maneja las credenciales del repositorio de Git
c.
Almacena los Secretos de Kubernetes utilizados para la autenticación
d.
Compara continuamente los estados deseado y en vivo y aplica los cambios según sea necesario

✅ Respuesta correcta: d. It continuously compares the desired and live states and applies changes as needed
📌 Explicación clara
El Application Controller es el corazón de Argo CD.
Su función principal es garantizar que lo que está en tu clúster coincida exactamente con lo que está definido en Git.

✔️ ¿Qué hace realmente?
Observa continuamente el estado deseado (Git)

Compara ese estado con el estado actual del clúster

Si encuentra diferencias, puede:

Sincronizar automáticamente

Aplicar cambios

Revertir drift

Reportar estado OutOfSync

Este proceso se conoce como GitOps reconciliation loop.

❌ Por qué las otras opciones no son correctas
a. It provides the user interface for Argo CD
La UI la proporciona argocd-server, no el Application Controller.

b. It handles Git repository credentials
Eso lo gestionan los Secrets y el repo-server, no el controller.

c. It stores Kubernetes Secrets used for authentication
Argo CD usa Secrets de Kubernetes, pero el controller no los almacena.

## **Pregunta 7**

¿Verdadero o Falso? El servidor de API de Argo CD es responsable de aplicar manifiestos directamente al clúster de Kubernetes.

a.
Verdadero
b.
Falso

✅ Respuesta correcta: b. False
📌 Explicación clara
El Argo CD API Server NO aplica manifiestos directamente al clúster.
Su función principal es:

Proveer la API y la interfaz web

Autenticar usuarios

Exponer operaciones de Argo CD

Servir como punto de entrada para CLI y UI

Pero no realiza la reconciliación ni aplica cambios.

Entonces, ¿quién aplica realmente los manifiestos?
👉 El Application Controller
Es el componente que:

Compara el estado deseado (Git) con el estado real (clúster)

Aplica los manifiestos

Sincroniza recursos

Repara drift

Es decir, es el “cerebro” operativo de Argo CD.

## **Pregunta 8**

¿Cuál es la función principal de Argo CD?

a.
Gestionar entornos de ejecución de contenedores
b.
Ejecutar canalizaciones de ML utilizando trabajos basados en datos
c.
Desplegar aplicaciones en Kubernetes basándose en repositorios de Git
d.
Monitorear el tráfico de red dentro de Kubernetes

✅ Respuesta correcta: c. Deploying applications to Kubernetes based on Git repositories
📌 Explicación clara
Argo CD es una herramienta de GitOps para Kubernetes.
Su propósito principal es:

✔️ Tomar el estado deseado desde un repositorio Git
(manifests, Helm charts, Kustomize, etc.)

✔️ Compararlo con el estado real del clúster
✔️ Aplicar los cambios necesarios para mantenerlos sincronizados
Es decir, Argo CD convierte Git en la fuente de verdad para tus despliegues.

## **Pregunta 9**

¿Qué utiliza Argo Rollouts para determinar si una nueva versión de una aplicación debe ser promovida o revertida automáticamente durante un despliegue?

a.
ReplicaSets
b.
Feature Flags
c.
AnalysisRuns y métricas
d.
Helm Charts

✅ Respuesta correcta: c. AnalysisRuns and metrics
📌 Explicación clara y directa
Argo Rollouts permite despliegues avanzados como canary, blue‑green, progressive delivery, etc.
Para decidir si una nueva versión es segura o si debe revertirse, utiliza:

✔️ AnalysisRuns
Son ejecuciones de análisis basadas en plantillas (AnalysisTemplates).
Cada AnalysisRun puede:

* Consultar métricas de Prometheus
* Revisar logs
* Llamar a un webhook
* Validar KPIs de negocio
* Comprobar errores o latencia

✔️ Métricas
Los AnalysisRuns evalúan métricas como:

* Tasa de errores
* Latencia
* Disponibilidad
* Conversiones
* Cualquier métrica personalizada

Si las métricas pasan → promoción automática
Si fallan → rollback automático

❌ Por qué las otras opciones no aplican
a. ReplicaSets
Son solo versiones del despliegue, no deciden promoción.

b. Feature Flags
Pueden usarse junto con Rollouts, pero no determinan promoción.

d. Helm Charts
Son solo una forma de empaquetar manifests, no influyen en la lógica de promoción.

## **Pregunta 10**

¿Qué componente es responsable de iniciar el proceso orientado a eventos generando eventos en Argo Events?

a.
Event Source
b.
Sensor
c.
EventBus
d.
Trigger

✅ Respuesta correcta: a. Event Source
📌 Explicación clara
En Argo Events, el flujo siempre empieza con un Event Source.

✔️ ¿Qué hace un Event Source?
Es el productor de eventos.

Detecta algo que ocurre en el mundo real o en un sistema externo.

Genera un evento y lo envía al EventBus.

Los Event Sources pueden escuchar cosas como:

* Webhooks
* Cron (eventos programados)
* S3 / GCS / MinIO
* Kafka / NATS
* GitHub / GitLab
* Mensajes HTTP
* Cambios en colas o streams

Sin un Event Source, no hay eventos, y por lo tanto, nada que procesar.

❌ Por qué las otras opciones no son correctas
b. Sensor
Escucha eventos, pero no los genera.

c. EventBus
Transporta eventos, pero no los crea.

d. Trigger
Ejecuta acciones cuando un Sensor detecta un evento, pero no inicia el proceso.