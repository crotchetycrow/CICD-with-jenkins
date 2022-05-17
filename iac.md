# IAC

## What is IAC

Infrastructure as Code (IaC) is the managing and provisioning of infrastructure through code instead of through manual processes.

With IaC, configuration files are created that contain your infrastructure specifications, which makes it easier to edit and distribute configurations. It also ensures that you provision the same environment every time.

## What are the benefits of IAC

- Cost reduction

  - By removing the manual component, people are able to refocus their efforts towards other tasks.

- Speed
- IaC allows faster execution when configuring infrastructure and aims at providing visibility to help other teams across the enterprise work quickly and more efficiently.

- Reduced risk

  - Automation removes the risk associated with human error, like manual misconfiguration; removing this can decrease downtime and increase reliability.

- Test

  - Infrastructure as Code enables DevOps teams to test applications in production-like environments early in the development cycle.

- Stable and scalable environments

  - IaC delivers stable environments rapidly and at scale. Teams avoid manual configuration of environments and enforce consistency by representing the desired state of their environments via code.
  - DevOps teams can work together with a unified set of practices and tools to deliver applications and their supporting infrastructure rapidly, reliably, and at scale.

- Enhanced security
  - Because all compute, storage, and networking services are provisioned with code, they are deployed the same way every time. This means that security standards can be easily and consistently deployed across company without having to have a security gatekeeper review and approve every change.

## Use cases for IaC

### Software development

When the environments are uniform across the whole cycle, the chances of bugs arising are much lower, as well as the time required for deployment and configuration of all the required environments. Build, testing, staging and production environment deployments will be repeatable, predictable and error-free.

### Cloud infrastructure management

That all actions that can be automated will be automated. In this case, multiple scenarios emerge, where provisioning and configuring the system components with Terraform and Kubernetes helps save time, money and effort. All kinds of tasks, from database backups to new feature releases can be done faster and better.

### Cloud monitoring

Cloud monitoring, logging and alerting tools also need to run in some environments and deliver new system components. Solutions like ELK stack, FluentD, SumoLogic, Datadog, Prometheus + Grafana — all of these can be quickly provisioned and configured for your project using IaC best practices.

## What tools are available with IAC

- Terraform

  - A declarative provisioning and infrastructure orchestration tool that lets engineers automate provisioning of all aspects of their enterprise cloud-based and on-premises infrastructure.

- Chef

  - One of the most popular configuration management tools that organizations use in their continuous integration and delivery processes
  - Chef is cloud-agnostic it is compatible with any cloud infrastructure and can be moved to and from different cloud environments without any operational issues

- Puppet

  - Using Puppet, you can define the desired end state of your infrastructure and exactly what you want it to do
  - Puppet automatically enforces the desired state and fixes any incorrect changes

- Ansible

  - Models your infrastructure by describing how your components and system relate to one another, as opposed to managing systems independently

## What is configuration management and Orchestration under IAC?

- Configuration Management (CM):

  - Maintains the consistency of an application’s performance, as well as its functional and physical inputs along with requirements, overall design, and operations throughout the lifespan of the product.
  - Essentially involves the migration of configs between different environments backed by a control system to configure an infrastructure so it’s prepared for deployment.

  - Configuration Management is a way to configure servers. The configuration could be:

    - Installing applications
    - Ensuring services are stopped or started
    - Installing updates
    - Opening up ports

  - Realistically and technically, you can use CM for IaC. The biggest problem is configuration drift:

    - Configuration drift is when someone automates a deployment with CM and a person goes into the server and changes the config. No one will know and there isn't an actual blocker to stop the person from doing that.

- Orchestration:

  - The automated configuration, management, and coordination of computer systems, applications, and services.
  - Can automate a process or workflow that involves many steps across multiple different systems
  - Can automate IT processes such as server provisioning, incident management, cloud orchestration, database management

  - In orchestration, the person designing the process dictates the desired result. However, the computer can make decisions based on changing circumstances. This capability makes orchestration significantly more complex than task automation. An orchestrated system can:

    - Make decisions based on varying factors.
    - React to different events.
    - Keep track across various IT environments (apps, mobile devices, databases, etc.)

## What tools are available for configuration management and Orchestration?

- CM tools:

  - Ansible
    - Can be used to execute the same command for a list of servers from the command line.
    - Can be understood by tech and non tech because it's written in plain english
  - Chef
  - Puppet
  - CFEngine
    - Open source configuration management system
    - Uses agents running on a server to pull the config from a central repository
    - Requires technical knowledge

- Orchestration tools:

  - Kubernetes
    - Open-source platform that was originally designed by Google
    - Can help to automate deployment, scaling, and management of containerized workload and services.
  - OpenShift
    - Redhat PaaS
    - Helps in the automation of applications on secure and scalable resources in hybrid cloud environments
    - Platforms for building, deployment, and managing containerized applications
  - Docker Swarm
    - Defines the desired state of the service, and Docker will maintain that state
  - Docker Compose
    - Defines and runs multi-container applications that work together
    - Describes groups of interconnected services that share software dependencies and are orchestrated and scaled together

## What is the difference between configuration management and Orchestration?

Orchestration is about automating processes, and configuration management is automating all the processes that keeps the infrastructure secure, up to date, and available.

## Why should we use configuration management and Orchestration?

- CM:

  - Reduced risk of outages and security breaches through visibility and tracking of the changes to your systems.
  - Cost reduction by having detailed knowledge of all the elements of your configuration, avoiding wasteful duplication of your technology assets.
  - Improved experience for your customers and internal staff by rapidly detecting and correcting improper configurations that could negatively impact performance.
  - Strict control of your processes by defining and enforcing formal policies and procedures that govern asset identification, status monitoring, and auditing.
  - Greater agility and faster problem resolution, enabling you to provide a higher quality of service and reduce software engineering costs.
  - Efficient change management by knowing your baseline configuration, and having the visibility to design changes that avoid problems.
  - Quicker restoration of service. In an outage, you'll be able to recover quickly as your configuration is documented and automated.
  - Better release management and clear status accounting.

- Orchestration:

  - Minimizing team interactions
    - Team collaboration is certainly valuable, but it can also introduce friction into projects and processes. Orchestrating certain workflows and processes can minimize this friction among teams
  - Increasing productivity
    - Instead of using human power on rote tasks, you can work on projects that require human thought, decision making, and skills.
  - Standardizing workflows and products
    - Standardizing processes and products across the spectrum means your processes and products are consistent and reliable

## What is pull & push configuration management - What tools are available?

## Mutability vs Immutability
