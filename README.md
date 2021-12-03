
# OpenStackClient
** **

# Integrate and plug Cinder functionalities into OpenStackClient Core [need refine]

OpenStack is an open-source platform that allows you to create private and public clouds. openstackclient is the unified client for interacting with OpenStack-based clouds. It is a Python-based client that supports most OpenStack services, including the Compute (nova) service, Networking (neutron) service, and Block Storage (cinder) service. It also provides a plugin architecture to allow users to extend commands or add support for non-core services. The unified OpenStack client is intended to replace a large number of project-specific clients.
See:
* [Openstackclient](https://docs.openstack.org/python-openstackclient/latest/)
* [OpenStack Services](https://www.openstack.org/software/project-navigator/openstack-components#openstack-services)


## 1.   Vision and Goals Of The Project: [need refine]

openstackclient is intended to replace the project-specific clients used in the past, such as novaclient and neutronclient, and offer a unified client tool to use OpenStack services. However, while projects such as the Networking service (neutron) have switched to using openstackclient as their primary CLI - either via integration in openstackclient core or an openstackclient plugin - other services have not yet provided full feature parity in openstackclient.

High-level goals of this project include:

* Migrate openstackclient from project-specific python-cinderclient to openstacksdk

* Identify CLI gaps in OpenStackClient's support for the Block Storage service (cinder)

* Achieve feature-parity with ([python-cinderclient](https://opendev.org/openstack/python-cinderclient))

## 2. Users/Personas Of The Project:

OpenStackClient (aka OSC) is a command-line client for OpenStack that brings the command set for Compute, Identity, Image, Object Storage, and Block Storage APIs together in a single shell with a uniform command structure.

OpenStack is used by large enterprises, governments, and startups to build public and private clouds. Achieving feature-parity between project-specific clients and the unified CLI tool will benefit all users of OpenStack looking for a single easy-to-use client that supports most of OpenStack's services.

** **

## 3.   Scope and Features Of The Project:  [need refine]

* There are a handful of commands and options that exist in the project-specific clients but not in openstackclient. There are a lot of commands already identified by the OpenStack team and have already been transitioned to openstackclient. There are a few gaps left. The first of those remaining gaps is within novaclient and openstackclient. Once these gaps have been closed, the Nova team can consider deprecating novaclient.
	* All nova commands already exist in OpenStackClient.
	* Must inspect these commands to check if their syntax is consistent, and if they work as intended.
	* For those that do not perform as desired, modify corresponding code, either using nova client methods or migrate to OpenStackSDK.
	* For those that are not using OpenStackSDK, have them use OpenStackSDK.

* An equally important task is to migrate openstackclient from using project-specific client libraries to openstacksdk. Block storage commands in openstackclient must be migrated to openstacksdk instead of using cinderclient in openstackclient. This is to resolve the issue of cinderclient dropping support for v2 of the API. This item is impacting users and is a priority for the community.
	* Commands are listed in https://github.com/openstack/python-openstackclient/blob/master/doc/source/cli/data/cinder.csv
	* Block storage operations can be found here https://opendev.org/openstack/python-openstackclient/src/branch/master/openstackclient/volume
	* These commands should be properly grouped and treated as groups
	* Commands using cinderclient methods should be modified to use OpenStackSDK methods
	* As commands will use OpenStackSDK, they will not rely on v2 APIs anymore

It may be a challenge anticipating how long our tasks may take given the stringent contributor standards, processes, and general unfamiliarity with the massive codebase. Based on our mentors' conservative estimate, we may only be able to complete one of these tasks. The second service (cinder) is harder than the first (nova) as our mentors have already done part of nova, we may set out to complete nova first, and treat cinder as a stretch goal.

** **

## 4. Solution Concept [need refine]

OpenStack provides multiple services:
* Nova (compute)
* Neutron (networking)
* Cinder (block storage)
* Glance (image storage)
* Keystone (identity)
* Placement (inventory)

As most services are managed by different teams, they have slightly different APIs, and different "clients" to interact with the APIs.

![alt text](https://github.com/DiweiZhu/OpenStackClient-CS6620NEU21Fall/blob/main/SolutionConcept.png)

Each client consists of a command-line interface (CLI) and a library. If a user wants to use multiple services, they need to separately deal with multiple CLIs and libraries, with inconsistent styles, leading to a bad user experience.

To improve user experience, OpenStackClient and OpenStackSDK are envisioned. OpenStackClient is a synthesized version of all the CLIs (with some of them as plugins), while OpenStackSDK is a synthesized version of all the libraries (and more). As all the CLIs utilize the libraries, OpenStackClient should use OpenStackSDK instead of all the client-specific libraries, for better maintainability and consistency.

In summary, OpenStackClient should utilize methods from OpenStackSDK to provide users with consistent CLI commands for OpenStack services.

The design for that purpose is:
1. Add commands of services (nova, neutron, cinder, glance, keystone) into OpenStackClient, so that users have a CLI with a consistent style. For this, it doesn't matter if the OpenStackClient is using client-specific libraries or OpenStackSDK, because the behavior should be the same.
2. Migrate OpenstackClient from using client-specific libraries to using OpenStackSDK.


## 5. Acceptance criteria [need refine]

Two major tasks are envisioned as an end goal for our project: OpenStack’s task to transition all project-specific clients to openstackclient has already had significant progress prior to starting our project. Some clients have already been replaced. Our aim is to migrate the client for Cinder(Block Storage service) - cinderclient to openstacksdk. The second goal is to identify commands and options that exist in the project-specific clients but not openstackclient and then add these missing commands to openstackclient.

## 6. Accomplishment

## 7. What Learned

## 8. Main Obstacles

## 9. Future Work

During the entirity of our work for OpenStackClient we were faced with some challenging situations and their resolution depended on implementing new changes in the OpenStackSDK before we could migrate some commands. As discussed in the obstacles <>
## 10. Release Notes: [need refine]

Taiga Board: https://tree.taiga.io/project/amanbatra-cs6620-fall21-openstackclient/timeline

Sprint | Tasks
:------: | ------
1 | Setup the OpenStackClient environment on the Cloud VM: <br><ul><li>Create accounts for IRC and Gerrit</li><li>Set up a Linux-based development environment</li><li>Set up an all-in-one OpenStack deployment that you can use for testing</li></ul></li></ul>
2 | The next step is to go through the source code and get familiar with the functionality of Nova client service. This is crucial in understanding the gaps to figure out how to migrate the remaining commands that still use native Nova API’s and libraries and migrate them to OpenStackSDK. To completely understand the OpenStackClient functionality we need to execute the basic set of commands like create and modify Cloud environments.
3 |We shall focus on bringing the commands that are missing in OpenStackSDK for Nova client. This is done by adding the proxy classes/methods in the OpenStackSDK for Nova. After this, it is critical to run a proper testing suite to make sure there is no impact on the system.
4 |The next task is to plug the gaps in commands that are already in the OpenStackSDK. We do this by debugging why the command is not working as expected and adding/fixing the code.

### Mentors:
* Artem Goncharov <artem.goncharov@gmail.com>
* Stephen Finucane <stephenfin@redhat.com>
* Kendall Nelson <kennelson11@gmail.com>

