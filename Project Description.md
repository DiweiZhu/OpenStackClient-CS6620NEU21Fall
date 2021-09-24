# OpenStackClient-CS6620NEU21Fall
** **

# Integrate and plug Cinder functionalities into OpenStackClient Core

OpenStack is open-source software that allows you to create private and public clouds. openstackclient is the unified client for interacting with OpenStack-based clouds. It is a Python-based client that supports most OpenStack services, including the Compute (nova) service, Networking (neutron) service and Block Storage (cinder) service. It also provides a plugin architecture to allow users to extend commands or add support for non-core services. The unified OpenStack client is intended to replace a large number of project-specific clients.
See:
* [openstackclient](https://docs.openstack.org/python-openstackclient/latest/)
* [OpenStack Services](https://www.openstack.org/software/project-navigator/openstack-components#openstack-services)


## 1.   Vision and Goals Of The Project:

The unified OpenStack client, openstackclient or "OSC", aims to provide a single entrypoint to interact with all OpenStack services from the command line, masking differences in the underlying APIs and terminologies. In the past, each service offered their own individual project-specific client as an entrypoint for each service. This client is intended to replace the project-specific clients used so far, such as novaclient and neutronclient, and offer a unified client tool to use OpenStack services. However, while projects such as the Networking service (neutron) have switched to using openstackclient as their primary CLI - either via integration in openstackclient core or an openstackclient plugin - other services have not yet provided full feature parity in openstackclient.

High-level goals of this project include:

* Migrate openstackclient from project-specific clients (like python-cinderclient) to openstacksdk

* Identify CLI gaps in OpenStackClient's support for the Block Storage service (cinder)

* Achieve feature-parity with ([python-cinderclient](https://opendev.org/openstack/python-cinderclient))


## 2. Users/Personas Of The Project:

OpenStackClient (aka OSC) is a command-line client for OpenStack that brings the command set for Compute, Identity, Image, Object Storage and Block Storage APIs together in a single shell with a uniform command structure.

OpenStack is used by large enterprises, governments and startups to build public and private clouds.
Achieving feature-parity between project-specific clients and the unified CLI tool will benefit all users of OpenStack looking for a single easy-to-use client that supports all of OpenStack's services.

** **

## 3.   Scope and Features Of The Project:


* There are a handful of commands and options that exist in the project-specific clients but not in openstackclient. There are a lot of commands already identified by the OpenStack team and have already been transitioned to openstackclient. There are only a few remaining gaps left. The first of those remaining gaps is between novaclient and openstackclient. Once this gap has been closed, the Nova team can consider deprecation process for the novaclient.
* An equally important task is to migrate openstackclient from using project-specific client libraries to using openstacksdk. Block storage commands in openstackclient need to be migrated to openstacksdk instead of using cinderclient in openstackclient. This is to resolve the problem of cinderclient dropping super for v2. This item is causing a big user impact right now.


** **

## 4. Solution Concept

OpenStack provides multiple services as the six core services:
* Nova (compute)
* Neutron (networking)
* Cinder (block storage)
* Glance (image storage)
* Keystone (identity)
* Placement (inventory)

As most services are managed by different teams, they have slightly different APIs, and different "clients" for interacting with the APIs. 

![alt text](https://github.com/DiweiZhu/OpenStackClient-CS6620NEU21Fall/blob/main/OpenStackClient.png)

Each client consists of a command line interface (CLI) and a library. If a user wants to use multiple services, they need to separately deal with multiple CLIs and libraries, with inconsistent styles, leading to bad user experiences.

To improve user experience, OpenStackClient and OpenStackSDK are envisioned. Basically OpenStackClient is a synthesized version of all the CLIs (with some of them as plugins), while OpenStackSDK is a synthesized version of all the libraries (and more). As all the CLIs actually utillize the libraries, OpenStackClient should use OpenStackSDK instead of all the client-specific libraries, for better maintainability and consistency.

In summary, OpenStackClient should utilize methods from OpenStackSDK to provide users with consistent CLI commands for OpenStack services.

The designs for that purpose is:
1. Add commands of services (nova, neutron, cinder, glance, keystone) into OpenStackClient, so that users have a CLI with consistent style. For this, it doesn't matter if the OpenStackClient is using client-specific libraries or OpenStackSDK, because the behavior should be the same.
2. Migrate OpenstackClient from using client-specific libraries to using OpenStackSDK.


## 5. Acceptance criteria
There are two major tasks that are envisioned as an end goal for our project:
OpenStack’s task to transition all project-specific clients to openstackclient has already had significant progress prior to starting our project. Some clients have already been replaced. Our aim is to migrate the client for Cinder(Block Storage service) - cinderclient    to openstacksdk.
The second goal is to identify commands and options that exist in the project-specific clients but not openstackclient and then add these missing commands to openstackclient.

## 6.  Release Planning:

Taiga Board: https://tree.taiga.io/project/amanbatra-cs6620-fall21-openstackclient/timeline

<center>

Sprint | Tasks
------ | ------
1 |
2 |
3 |
4 |
5 |

</center>


Sprint
Task
1
Week 1
Introductions
Project Description

Week 2
Setup:
Create accounts for IRC and Gerrit
Set up a Linux-based development environment
Set up an all-in-one OpenStack deployment that you can use for testing
2
Week 3
Continue with Setup
3
Week 4
  Deliver Goal 1
4
Week 7
  Deliver Goal 2
5
Week 9
  Deliver Goal 3



Open Items: 
Does openstacksdk have all methods (or similar ones) that the project-specific clients support?



