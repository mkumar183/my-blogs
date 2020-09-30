---
layout: post
title: Chef 
---

![_config.yml]({{ site.baseurl }}/images/bamboo.png) 

[Chef Tutorial](https://www.tutorialspoint.com/chef/chef_overview.htm)

##### What is Knife ?
Knife is a command-line tool that provides an interface between your workstation 
and the Chef server. The knife enables you to upload your cookbooks to the 
Chef server and interact with nodes, the servers that you manage.

##### Attributes 
Attributes represent details about a node. 
Chef has automatic attributes, which are the attributes collected by a 
tool called Ohai and containing information about the system 
(such as platform, hostname and default IP address), 
but it also lets you define your own custom attributes.

An attribute is a specific detail about a node. Attributes are used by Chef Infra Client to understand:

The current state of the node
What the state of the node was at the end of the previous Chef Infra Client run
What the state of the node should be at the end of the current Chef Infra Client run
Attributes are defined by:

The state of the node itself
Attributes passed via JSON on the CLI
Cookbooks (in attribute files and/or recipes)
Roles
Environments
Policyfiles
During every Chef Infra Client run, Chef Infra Client builds the attribute list using:

Attributes passed via JSON on the CLI
Data about the node collected by [Ohai].
The node object that was saved to the Chef Infra Server at the end of the previous Chef Infra Client run.
The rebuilt node object from the current Chef Infra Client run, after it is updated for changes to cookbooks (attribute files and/or recipes), roles, and/or environments, and updated for any changes to the state of the node itself.
After the node object is rebuilt, all of the attributes are compared, and then the node is updated based on attribute precedence. At the end of every Chef Infra Client run, the node object that defines the current state of the node is uploaded to the Chef Infra Server so that it can be indexed for search.

So how does Chef Infra Client determine which value should be applied? Keep reading to learn more about how attributes work, including more about the types of attributes, where attributes are saved, and how Chef Infra Client chooses which attribute to apply.

Attribute Persistence
All attributes except for normal attributes are reset at the beginning of a Chef Infra Client run. Attributes set via chef-client -j with a JSON file have normal precedence and are persisted between Chef Infra Client runs. Chef Infra Client rebuilds these attributes using automatic attributes collected by Ohai at the beginning of each Chef Infra Client run, and then uses default and override attributes that are specified in cookbooks, roles, environments, and Policyfiles. All attributes are then merged and applied to the node according to attribute precedence. The attributes that were applied to the node are saved to the Chef Infra Server as part of the node object at the conclusion of each Chef Infra Client run.

###### Attribute Sources
Attributes are provided to Chef Infra Client from the following locations:
- JSON files passed via the chef-client -j
- Nodes (collected by Ohai at the start of each Chef Infra Client run)
- Attribute files (in cookbooks)
- Recipes (in cookbooks)
- Environments
- Roles
- Policyfiles


##### About Recipes
A recipe is the most fundamental configuration element within the organization. A recipe:

- Is authored using Ruby, which is a programming language designed to read and behave in a predictable manner
- Is mostly a collection of resources, defined using patterns (resource names, attribute-value pairs, and actions); helper code is added around this using Ruby, when needed
- Must define everything that is required to configure part of a system
- Must be stored in a cookbook
- May be included in another recipe
- May use the results of a search query and read the contents of a data bag (including an encrypted data bag)
- May have a dependency on one (or more) recipes
- Must be added to a run-list before it can be used by Chef Infra Client
- Is always executed in the same order as listed in a run-list


###### Recipe Attributes
An attribute can be defined in a cookbook (or a recipe) and then used to 
override the default settings on a node. When a cookbook is loaded during a 
Chef Infra Client run, these attributes are compared to the attributes that 
are already present on the node. Attributes that are defined in attribute
files are first loaded according to cookbook order. For each cookbook, 
attributes in the default.rb file are loaded first, and then additional 
attribute files (if present) are loaded in lexical sort order. 
When the cookbook attributes take precedence over the default attributes, 
Chef Infra Client applies those new settings and values during a Chef 
Infra Client run on the node.