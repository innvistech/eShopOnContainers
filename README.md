# eShopOnContainers - Microservices Architecture and Containers based Reference Application (**ALPHA state** - VS 2017 and CLI environments compatible)
Sample .NET Core reference application, powered by Microsoft, based on a simplified microservices architecture and Docker containers. <p>

> ### DISCLAIMER
> IMPORTANT: The current state of this sample application is ALPHA, therefore, many areas could change significantly while refactoring and getting improvements. Feedback and pull requests from the community will be appreciated. 
>
> This reference application proposes a simplified microservice oriented architecture implementation (as mentioned, currently in ALPHA state) to introduce technologies like .NET Core with Docker containers through a comprehensive but simplified application. However, this reference application it is not trying to solve all the problems in a large and mission-critical distributed system, it is just a bootstrap for developers to easily get started in the world of Docker containers and microservices with .NET Core. 
> <p>For example, the next step (still not covered here) after understanding Docker containers and microservices is to select a microservice cluster/orchestrator like Docker Swarm, Kubernetes or DC/OS (in Azure Container Service) or Azure Service Fabric which in most of the cases will require additional partial changes to your application's configuration (although the present architecture should work on most orchestrators with small changes). In the future we might fork this project and make multiple versions targeting specific microservice cluster/orchestrators. <p>
> Read the planned <a href='https://github.com/dotnet/eShopOnContainers/wiki/Roadmap-and-Milestones-for-future-releases'>Roadmap and Milestones for future releases of eShopOnContainers</a> within the Wiki for further info about possible new implementations and provide feedback at the ISSUES level if you'd like to see any specific scenario implemented.

<p>
This reference application is cross-platform either in the server and client side, thanks to .NET Core services capable of running on Linux or Windows containers depending on your Docker host, and to Xamarin for mobile apps running on Android, iOS or Windows/UWP plus any browser for the client web apps. 
<p>
<img src="img/eshop_logo.png">
<img src="img/eShopOnContainers_Architecture_Diagram.png">
<p>

> ### Important Note on Database Servers/Containers
> In this solution's current configuration for a development environment, the SQL databases are automatically deployed with sample data into a single SQL Server for Linux container (a single shared Docker container for SQL databases) so the whole solution can be up and running without any dependency in the cloud or server. Each database could also be deployed as a single Docker container, but then you'd need more then 8GB or memory RAM in your development machine in order to be able to run 3 SQL Server Docker containers in your Docker Linux host in "Docker for Windows" or "Docker for Mac" development environments.
> <p> A similar case is defined in regards Redis cache running as a container for the development environment.
> <p> However, in a real production environment it is recommended to have persistance (SQL Server and Redis) in HA services like Azure SQL Database, Redis as a service or any other clustering system. If you want to change to a production configuration, you'll just need to change the connection strings once you have set up the servers in HA cloud or on-premises.

## Related documentation and guidance
While developing this reference application, we've been creating a reference Guide/eBook named <b>"Architecting and Developing Containerized and Microservice based .NET Applications"</b> which explains in detail how to develop this kind of architectural style (microservices, Docker containers, Domain-Driven Design for certain microservices) plus other simpler architectural styles, like monolithic that can also live as Docker containers.
<p>
There's also an additional eBook focusing on Containers/Docker lifecycle (DevOps, CI/CD, etc.) with Microsoft Tools, already published.
You can start reviewing these Guides/eBooks here:
<p>
You can download both eBooks from here:

| Architecting & Developing | Containers Lifecycle & CI/CD |
| ------------ | ------------|
| <a href='docs/architecting-and-developing-containerized-and-microservice-based-net-applications-ebook-early-draft.pdf'><img src="img/ebook_arch_dev_microservices_containers_cover.png"> </a> | <a href='https://aka.ms/dockerlifecycleebook'> <img src="img/ebook_containers_lifecycle.png"> </a> |
| <a href='docs/architecting-and-developing-containerized-and-microservice-based-net-applications-ebook-early-draft.pdf'>Download (Confidential DRAFT until published)</a> | <a href='https://aka.ms/dockerlifecycleebook'>Download</a>   |


<p>However, we encourage to review the "Architecting/Developing" eBook because the architectural styles and architectural patterns and technologies explained in the guidance are using this reference application when explaining many sample implementations.


## Overview of the application code
In this repo you can find a sample reference application that will help you to understand how to implement a microservice architecture based application using <b>.NET Core</b> and <b>Docker</b>.

The example business domain or scenario is based on an eShop or eCommerce which is implemented as a multi-container application. Each container is a microservice deployment (like the basket-microservice, catalog-microservice, ordering-microservice and the  identity-microservice) which are developed using ASP.NET Core running on .NET Core so they can run either on Linux Containers and Windows Containers.
The screenshot below shows the VS Solution structure for those microservices/containers and client apps.
- Open <b>eShopOnContainers.sln</b> for a solution containing all the projects (All client apps and services).
- Open <b>eShopOnContainers-ServicesAndWebApps.sln</b> for a solution containing just the server-side projects related to the microservices and web applications.
- Open <b>eShopOnContainers-MobileApps.sln</b> for a solution containing just the client mobile app projects (Xamarin mobile apps only).


<img src="img/vs-solution-structure.png">

Finally, those microservices are consumed by multiple client web and mobile apps, as described below.

<b>*MVC Application (ASP.NET Core)*</b>: Its an MVC 6 application where you can find interesting scenarios on how to consume HTTP-based microservices from C# running in the server side, as it is a typical ASP.NET Core MVC application. Since it is a server-side application, access to other containers/microservices is done within the internal Docker Host network with its internal name resolution.
<img src="img/eshop-webmvc-app-screenshot.png">

<b>*SPA (Single Page Application)*</b>: Providing similar "eShop business functionality" but developed with Angular 2, Typescript and slightly using ASP.NET Core MVC 6. This is another approach for client web applications to be used when you want to have a more modern client behavior which is not behaving with the typical browser round-trip on every action but behaving like a Single-Page-Application which is more similar to a desktop app usage experience. The consumption of the HTTP-based microservices is done from TypeScript/JavaScript in the client browser, so the client calls to the microservices come from out of the Docker Host internal network (Like from your network or even from the Internet). 
<img src="img/eshop-webspa-app-screenshot.png">

<b>*Xamarin Mobile App (For iOS, Android and Windows/UWP)*</b>: It is a client mobile app supporting the most common mobilee OS platforms (iOS, Android and Windows/UWP). In this case, the consumption of the microservices is done from C# but running on the client devices, so out of the Docker Host internal network (Like from your network or even the Internet).

<img src="img/xamarin-mobile-App.png">

> ### Note on tested Docker Containers/Images
> The development and testing of this project was (as of January 2017) done <b>only on Docker Linux containers</b> running in development machines with "Docker for Windows" and the default Hyper-V Linux VM (MobiLinuxVM) installed by "Docker for Windows". 
The <b>Windows Containers scenario has not been implemented/tested yet</b>, but the application should be able to run on Windows Containers based on different Docker base images, as well, as the .NET Core services have also been tested running on plain Windows (with no Docker).
The app was also partially tested on "Docker for Mac" using a development MacOS machine with .NET Core and VS Code installed. However, that is still a scenario using Linux containers running on the VM setup in the Mac by the "Docker for Windows" setup.
 
## Setting up your development environment for eShopOnContainers
### Visual Studio 2017 and Windows based
This is the more straightforward way to get started:
https://github.com/dotnet/eShopOnContainers/wiki/02.-Setting-eShopOnContainer-solution-up-in-a-Visual-Studio-2017-environment

### CLI and Windows based
For those who prefer the CLI on Windows, using dotnet CLI, docker CLI and VS Code for Windows: 
https://github.com/dotnet/eShopOnContainers/wiki/03.-Setting-the-eShopOnContainers-solution-up-in-a-Windows-CLI-environment-(dotnet-CLI,-Docker-CLI-and-VS-Code)

### CLI and Mac based
For those who prefer the CLI on a Mac, using dotnet CLI, docker CLI and VS Code for Mac: 
https://github.com/dotnet/eShopOnContainers/wiki/04.-Setting-eShopOnContainer-solution-up-in-a-Mac,-VS-Code-and-CLI-environment--(dotnet-CLI,-Docker-CLI-and-VS-Code)

> ### Note on Windows Containers
> As mentioned, the development and testing of this project (January 2017 version) was done on Docker Linux containers running in development machines with "Docker for Windows" and the default Hyper-V Linux VM (MobiLinuxVM) installed by "Docker for Windows". 
In order to run the application on Windows Containers you'd need to change the base images used by each container:
> - Official .NET Core base-image for Windows Containers, at Docker Hub: https://hub.docker.com/r/microsoft/dotnet/ (Using the Windows Nanoserver tag)
> - Official base-image for SQL Server on Windows Containers, at Docker Hub: https://hub.docker.com/r/microsoft/mssql-server-windows
