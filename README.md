Welcome to the **GSLB tool** project! This README is an introduction to the project. **GSLB tool is in beta status**.

**GSLB tool allows the automatic creation of GSLB DNS** entries in [F5 CloudServices](https://clouddocs.f5.com/cloud-services/latest/)' DNS LB service. This DNS LB service is a SaaS offering thus allowing to provision GSLB in minutes without provisioning of new infrastructure. 

**Verify that this GSLB solution suits your environment without any commitment**. You can try this tool and CloudService's in your existing production infrastructure without any change into your existing deployments.

In this page you will find an overview of the project. ![See the Wiki for setup and usage documentation](https://github.com/f5devcentral/f5-bd-cloudservices-gslb-tool/wiki). 

GSLB tool automatically retrieves **Layer 7 routes** (ie: `https://www.mycompany.com/shop`) from your infrastructure and automatically generates the GSLB configuration. The tool has several utilities which allow the move the workloads across the different deployments.

At present this tool focuses in Openshift clusters and [Route resources](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html#route-types) but it could be easily extended to support any other Kubernetes with Ingress resources. GSLB tool has been tested with Openshift 3.x and 4.x.

Please note that **GSLB tool is not tied to any specific Openshfit Router implementation**. GSLB tool can use either RedHat's default Router implementation, BIG-IP, any other implementation or a combination of these.

The different members of a DevOps team can have the tool in their laptops and share the desired configuration in a git repository (source of truth). The overall architecture can be seen in the next diagram.

![Overall Architecture](https://raw.githubusercontent.com/f5devcentral/f5-bd-cloudservices-gslb-tool/master/diagrams/Diagram%20overall%20architecture.png)

GSLB tool doesn't require any special installation and doesn't modify your Openshift/K8s cluster (it only performs read-only operations). It is a set of Ansible playbooks and roles to allow  automation happen with a large degree of flexibility. GSLB tool is compromised of the following commands.

**project-** commands operates on a projec/namespace basis (it's always a parameter) and performs operations only on the local version of the config, prior to updating the source of truth and updating the active/published GSLB config in CloudServices. These commands are:

* **project-retrieve**: retrieves all the routes of the given project/namespace and deployment.
* **project-populate**: populates (copies) the routes of a given project/namespace from one deployment to another.
* **project-evacuate**: evacuates (removes) from GSLB all the routes being hosted in the given project/namespace and deployment.
* **project-ratios**: sets the GSLB ratio for each deployment for a given project/namespace.


Whilst the **project-** commands operates on all the routes of a given project/namespace at a time, the commands with the **gslb-** prefix don't have as parameter a project/namespace. Instead these operates on all of them:

* **gslb-commit**: publishes into F5 Cloud Services the local GSLB configuration and after success stores the succesful change into the source of truth.
* **gslb-rollback**: sets in the local config the configuration prior to the last commit. Needs that **gslb-commit** is run afterwards to make effective the rollback.

Please note that when a **gslb-commit** command is executed it commits all the changes or doesn't commit any since the previous **gslb-commit** no matter how many **project-** operations have been performed previously.

The next animation will give you a hint of tool's operation.

![Operations animation](https://raw.githubusercontent.com/f5devcentral/f5-bd-gslb-tool/master/diagrams/Diagram%20Operations%20overview.gif)

To see an actual demo of the tool please check ![this video in youtube](https://www.youtube.com/watch?v=TiAMINSBPns).

gslb-tool is released to the community under the [Apache v2 license](https://www.apache.org/licenses/LICENSE-2.0.txt). It is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.

[![CLA assistant](https://cla-assistant.io/readme/badge/f5devcentral/f5-bd-gslb-tool)](https://cla-assistant.io/f5devcentral/f5-bd-gslb-tool)


