# Strategy for the software ecosystem of the Open Telekom Cloud

Thanks to its extensive services, the Open Telekom Cloud offers users many options for designing and operating application setups in a flexible and scalable manner. There are several options for managing and orchestrating these setups:

1. **Browser-based user interface (UI):** The UI allows access to the managed resources of the Services via a browser. This is intuitive at first, but only scales to a limited extent, as many resources often have to be managed on a recurring basis in the long term. In this way, comprehensive automation of administrative tasks is only possible to a limited extent.

2. **API Control:** Each service of the Open Telekom Cloud is accessible and controllable via an API, which allows users to automate the control of these interfaces. The interfaces are usually implemented as REST endpoints, through which JSON-encoded data objects are exchanged. This path is very generic, but also small-step and extensive.

3. **Management Tools and Orchestration Platforms:** These tools combine both approaches and provide task-based management capabilities to set up, manage, operate, and monitor setups in the cloud. Examples are infrastructure-as-code platforms such as Terraform, Ansible or Chef as well as orchestration tools for clusters such as Kubernetes, Rancher or Nomad. These tools consist of a general part and a connector that connects the software to the specific cloud via the API.

The entire set of tools and tools that can be used to manage a cloud is what we call the Open Telekom Cloud ecosystem. The **Ecosystem Squad** of the Open Telekom Cloud is the team that takes care of the connection of these tools, reviews and tests the offer and, in selected cases, strives to connect and maintain these tools.

In view of the large number of management tools and platforms as well as the need to adapt to all or as many services as possible, it is necessary to prioritise the supported offers. This prioritization is based on two core aspects:

1. **Market and business considerations:** You will investigate whether there is a need for a particular tool, how high the demand is for it and how it fits in with the rest of the Open Telekom Cloud's portfolio of offerings.

2. **Architectural analyses:** These assess the technical resilience, compatibility and suitability of a solution for typical products. The programming language used, the required process environment and other technical considerations are also considered.

This strategy document contains two key investigations: First, the typical and widespread infrastructure management solutions that are under an open source license and platform-independent are compiled so that they can be used with the Open Telekom Cloud. Document then evaluates the benefits of these solutions from a technical point of view from the customer's and user's perspective. These assessments serve as a basis for product management to select the products that are to be adapted to the Open Telekom Cloud as part of the implementation process by the Ecosystem Squad.

## Categories of Cloud Platform Management Solutions

These tools are platform-independent, are under an open-source license and offer the possibility to manage and orchestrate cloud environments.

### Infrastructure as Code and Configuration Management

These tools directly manage the resources provided by the cloud. The creation, arrangement, and correlation of these resources is known as provisioning. An essential task of these tools is the comparison between the target and actual state. Users describe the desired target state, and the tool takes care of the identification and the correct sequence or parallelism of the steps to be carried out. Another important function is the condition management of the managed clouds. If the tool's idea of the presence of certain resources deviates from reality, it is called config drift. A tool should be able to detect, report and resolve this drift.

#### Infrastructure as Code

Infrastructure as Code (IaC) tools provision cloud resources. In doing so, they usually abstract the specific APIs of the respective cloud platforms in order to be able to provision setups on several platforms more or less easily with a description in order to create a certain degree of multi-cloud capability. The main representatives of this category are:

**Terraform:** Automation and orchestration of multi-cloud infrastructures.

Terraform is considered a technological market and innovation leader in the field of IaC. Terraform support is virtually mandatory for any cloud today, especially for users who want to provision cloud-native environments quickly and easily. Terraform uses an easy-to-learn, domain-specific language that defines individual resources and describes their relationships. Terraform calculates the actual sequence in a planning phase and performs all the necessary steps. The importance of Terraform is also reflected in the fact that many planning tools now use Terraform templates as an output format.

Terraform itself was under Mozilla's MPL 2.0 license for a long time until HashiCorp changed the license to the non-OSI-compliant BSL 1.1 in August 2023. Although this has only a minor impact on users at first, this step could have long-term legal implications.

In response to the license change, the OpenTOFU project was founded, which aims to remain Terraform-compatible and continue to use existing Terraform providers. It remains to be seen to what extent this will be guaranteed in the long term. As long as the effects of the license change remain low, the focus of the Ecosystem Squad will remain on the classic Terraform for the time being.

The Ecosystem Squad has developed the Terraform provider for the Open Telekom Cloud and is overseeing its further development together with the Terraform community on GitHub. It is licensed under the Mozilla MPL license.

Terraform is currently the highest priority among the Ecosystem Squad's supported platforms to meet user demand and support customers' cloud enablement. In the medium term, an expansion of resources is necessary to ensure support and expand the offer, for example through additional documentation and training materials.

**Ansible:** Automation and configuration management for infrastructure and applications.

Ansible was originally created as a configuration management tool and is one of the best known and most widely used tools of its kind. It is developed in Python, which makes it flexible, but sometimes also less performant.

Ansible allows resources to be defined via YAML files, stored in a complex directory structure, and then executes the necessary steps to achieve the goal. Ansible playbooks offer versionability that makes it possible to define complex infrastructure and application solutions.

Through the integration of Ansible with Apimon and the CI server Zuul, the support of Ansible Collections has a major impact on the internal processes of the Ecosystem Squad.

Ansible is licensed under the GPL, the collections are released under the Apache License 2.0.

**Pulumi:** Programming language-based IaC solution for multi-cloud infrastructures.

Pulumi has a very similar approach to Terraform structurally and can almost be seen as a greenfield reimplementation. The most important difference is in the language of the Domain Specific Language, because here users have much more choice, as well-known programming languages such as Typescript, Python, Go or even Java are possible. They then also have the possibility to integrate imperative constructs such as loops or conditions into their IaC COde. This makes Pulumi a very modern and powerful tool. Nevertheless, it has only found a limited distribution.

The Ecosystem Squad has not yet decided to actively support Pulumi.

**Juju:** Canonical's tool for managing applications in the cloud.

Together with the infrastructure manager MaaS, Juju is a complex, integrated tool for deploying, managing, and orchestrating applications. It has strengths in managing complex, distributed applications that consist of multiple components and are distributed across different cloud or server environments. The combination of Juju/MaaS is developed by the Ubuntu manufacturer Canonical, but has only a very low distribution outside this ecosystem, as it implements many approaches of the otherwise usual stack of Terraform and Ansible in a "proprietary" and incompatible way, so to speak.

The Ecosystem Squad has not yet decided to actively support Juju.

**TOSCA:** Open standard format for infrastructure modeling.

The Topology and Orchestration Specification for Cloud Applications (TOSCA) is an open OASIS standard that defines a unified language for modeling and managing complex cloud applications and infrastructure. The goal of TOSCA is to enable portability and interoperability between different cloud environments by standardizing applications and their dependencies and defining them as reusable modules. TOSCA can be understood as a meta-abstraction of the principles described in this section. It is also not a tool per se, but only a language definition that then works together with other tools, such as a modeler who uses a UI to assemble the components and an orchestrator who translates the descriptions into directly processable templates, for example to Terraform or Ansible.

Due to TOSCA's high degree of abstraction, it is necessary to provide a complete application environment that integrates the individual components. This goes beyond the resources of the Ecosystem Squad, but is offered by the Cloud Create Squad as a complete application.

### Cluster and container orchestration

In addition to virtual machines, containers in particular are playing an increasingly important role as a lighter alternative in modern cloud-native setups, often in the context of more complex application setups that are put together according to the concept of microservices. Since VMs and COntainers only consider a single host as an expiration platform, orchestration solutions address the roles and relationships of such VMs and servers with each other, for example in the form of clusters of redundant nodes that enable horizontal scaling during operation: 

Kubernetes: Container orchestration and cluster management.

In the meantime, Kubernetes is considered the undisputed core building block of most orchestration platforms (Platform as a Service, PaaS). The framework, originally designed by Google, realizes a number of abstract platform resources and distributes them transparently to existing infrastructure components (nodes). Kubernetes brings the data structure of Custom Resource Definitions (CRDs), a universal way to define and manage certain basic building blocks of clusters. This can be used, for example, to implement scalable redundancy clusters, scale-in and scale-out, or to set up failover scenarios. For application developers, Kubernetes is now considered a generalized workflow environment for distributed, cloud-native applications.

Kubernetes can handle multiple IaaS implementations of networks (CNI), storage providers (CSI) and other implementations. Kubernetes can either be built directly on IaaS resources of the Open Telekom CLoud (for example, with the help of the provisioning tools described) or used in the form of the existing Cloud Container Engine (CCE) service of the Open Telekom Cloud. In this respect, Kubernetes can also be used without a concrete support component by the Ecosystem Squad.

**Rancher:** Platform for multi-cluster Kubernetes environments.

Since the setups running on Kubernetes usually have a certain complexity and consist of several components, but Kubernetes itself only has a limited UI, management tools such as Rancher fill this gap. You are able to set up, manage, and monitor Kubernetes clusters based on a CLuster service such as the CCE of the open Telekom CLoud or on the basis of an abstracted virtual NMechine (node driver).

The Ecosystem Squad offers both forms of drivers for the Open Telekom CLoud as a proof-of-concept. Rancher has achieved a certain market penetration and is mainly used for pragmatic reasons, as it does not offer much added value to the managed, actual cluster.

**OpenShift:** Comprehensive platform for cluster environments

The architectural approach of OpenShift is comparable to that of Rancher, although the software offered by Red Hat as a platform is even more extensive, because in addition to the derangent flow platform, it comes with many other additional components, including a library and management of its own custom resources, for example for databases and other microservices.

Provisioning OpenShift clusters is a complex task and requires the availability of a whole range of custom resources. Currently, the Open Telekom Cloud is only provisioned on the basis of a predefined topology that exists for the Cloud Create tool of the same name.

**Nomad:** Orchestration for containers, VMs, and more.

Nomad is a Cluster platform from Hashicorp and can be seen as a core feature and alternative implementation of Kubernetes, but it has only found little distribution.

Nomad is currently not offered by the Ecosystem Squad.

### Resource Management Platforms

Many application developers for cloud setups struggle with the IaaS and PaaS abstraction layers for their applications. Anyone who accesses the services of the Open Telekom Cloud via their APIs at the IaaS level must use a different access paradigm than they are used to from Kubernetes on the PaaS level. Therefore, there is a trend towards packing the resources of the IaaS layer into objects of the PaaS layer. These shells are the Custom Resource Definitions (CRDs). There are different sentences of them, which address different focal points:

**Cloud COntainer Management (CCM):** Connection of resources from core services

A CCM implementation specifies a set of frequently used CRDs that IaaS clouds usually already provide in one form or another.
xxx where are we here with the OTC? Xxx

**Crossplane:** xxx as above, but for more servcies
xx The same principle is the same as CCM, but broader and beyond the core services. would require extensive realization.
