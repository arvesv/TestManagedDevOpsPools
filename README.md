# Experimenting with Azure Managed DevOps pools

## What is Managed DevOps pools?

[Managed DevOps Pools](https://learn.microsoft.com/en-us/azure/devops/managed-devops-pools/?view=azure-devops) is a new type of "agent pool" in Azure DevOps. And this repo is my playground
for testing this new pool.

Azure Devops have basically 3 different types of agent pools.

* The Microsoft hosted agents - This is the best option if you can use them IMHO
* Self hosted agents - these agents can do anything, but complex to setup and run (and are not really scaleable[^1].)
* Scale Set agents - Scalable agents that has the size and software you configure, but complex to setup and run

The normal Microsoft hosted agents are great if you can use them. They are low maintenance, always updated and they scales well (if you pay). But there are some standard reasons
why they may not work for you:

* not enough free disk space (limit is 10 GB) or maybe too slow (2 core machine).
* does not have the software you need
* can't access your private networks

I think Managed Devops Pool aims to fix some of these problems without creating too many new. A Managed DevOps pools is created in Azure Dev Spaces and connects to your Azure DevOps organization. It
allows you to use the existing Microsoft hosted agent images on machines where you decide the disk and machine size. You can also use your own images similar to Scale Set based agent pools. There is
also a way to inject access to your private network from these agents.

## Impressions

I saw a [this presentation](https://www.youtube.com/watch?v=9e6Q8PSGiXU).  After that it took me ~10 minutes to setup a Dev Space with a Managed DevOps pool with the standard images. And it took almost the same time changing a YAML pipeline to use the new pool. I do want to save money and set 0 agents on standby, but this will probably cost me some startup time.

Startup time for spinning up a new agent seems to be 2-4 minutes, but I have not done extensive testing on this. My impression is that this is slower that the normal hosted agents but comparable to Scale Set based agents. If an agent is available then the startuptime is almost 0.

You can have the Managed DevOps Agents connection to your private networks, but this has not been relevant for me so I don't know how good this works.

I used to operate Scale Set based agent pools to work get around the too small disk size and missing software.  The Managed DevOps Pools can use the same images used in the Scale Set and the features are comparable. If you are running Scale Set agents, you can migrate that to Managed Devops Pools but the reason to do so is less clear. Not all machine sizes are available for Magned Devops pool agents, and sometimes you have to request a quota. I think Scale Set based agents can choose between more machine sizes. You have to choose the disk type (standard/SSD/premium). But unline Scale Set's you cannot choose ephemeral disks.

### The good

* You can use the Microsoft hosted on machine sizes of your choosing
* Better UI to configure the pool

### The not so good

* Not all machine sizes are available
* No ephemeral disks

## Preliminary recomendations

If you just need bigger disks and faster agents that use the Microsoft hosted images, then there is no reason to wait.

If you need your special software on you own OS-image, then an optimized Scale Set based agents can probably be better as is supports more sizes and ephemeral disks at the moment. But it is easier to setup Managed Devops pools.

Managed DevOps pools is in public preview. More stuff will come before GA, like cheaper SPOT (like cheaper spot instances and )

[^1]: I guess there are people who can do crazy stuff with K8S, KEDA and self-regestering agents.
