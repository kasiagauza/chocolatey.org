Title: Hosting Your Own Chocolatey Package Server
Published: 20160106
Author: Rob Reynolds
Tags: HowTo, Server, Packaging
Keywords: howto, host, chocolatey, packages
Summary: Hosting your own server can be easy. Here's what you need to know.
---
**Note:** Originally posted on [Puppet's blog](https://puppet.com/blog/chocolatey-hosting-your-own-server). *Reposted with permission.*

When folks think of Chocolatey, they often conflate the Chocolatey framework with the default location for packages, [Chocolatey.org](https://chocolatey.org/). However, Chocolatey was designed as a decentralized framework to allow you to use multiple (including internal) locations to get packages. This is one of the most important differentiators of Chocolatey compared to other packaging frameworks for Windows, especially for organizational use.

Lots of folks use Chocolatey.org, which has a feed of packages provided and maintained by the community (it is also known as the community feed). Packages on the community feed usually download software from official distribution points. When software owners move download locations or other breakages occur because of the internet, the package is broken until the new location is specified in an updated version of the package.

For folks in the community that are not using Chocolatey for production purposes, it is fine to use the community feed. For organizations that have a low tolerance for breakages and require a higher level of security, control, and trust, a self-hosted Chocolatey server is the recommended option. This guarantees that your installs, upgrades, and uninstalls will always work every time. This gives you complete control over what software gets installed. Also because you are vetting newer versions, you control when those newer versions are available for upgrade.

How is this different than the process you are following now outside of Chocolatey? I believe you are thinking, we are doing something similar now as an organization, what benefits do I get by adding Chocolatey into the mix?

- You have a single unified interface for installing/upgrading and uninstalling software on Windows.
- You define dependencies between software so installation order is not a manual process.
- Chocolatey has some really great smarts built-in to be able to handle things like uninstalling software automatically.
- You can easily package your own internal software using technologies you are familiar with (PowerShell).
- Chocolatey has great PowerShell helper functions for many software scenarios, so many times a one line function call is all you need to handle complex installation software.
- You can define any additional functionality a native installer file may have missed setting up for you, such as putting some directory in the PATH variable.
- You control how installs, upgrades and uninstalls are handled.
- Package maintenance is quite simple.

Chocolatey has great documentation on what is available for [hosting your own internal server](https://github.com/chocolatey/choco/wiki/How-To-Host-Feed), including[non-Windows options](https://github.com/chocolatey/choco/wiki/How-To-Host-Feed#non-windows-hosting).

There are three different ways to host your own server:

- Local folder / UNC Share (CIFS)
- Simple Server
- Package Gallery

Using a [local folder](https://chocolatey.org/docs/how-to-host-feed#local-folder-unc-share-cifs) allows you to store your packages in a directory, which may be the easiest option for quickly ramping up on Chocolatey.

As your needs increase, you can easily move your local folder to a [CIFS (UNC) share](https://chocolatey.org/docs/how-to-host-feed#local-folder-unc-share-cifs). Both local folder and CIFS are quick to set up. It’s as easy as putting packages in a location and pointing to that location as the source. Permissions are based on what is already there — if you can get to the packages, you can install from there, and if you can put packages in the location, you have push permission.

A [Simple Server](https://chocolatey.org/docs/how-to-host-feed#simple-server) is ideal when you need more control over pushing packages or reaching a CIFS share is not easily possible. A simple server allows you to push and install from HTTP/HTTPS. The disadvantage here is that there is one shared key for pushing packages, so anyone that has push access can push any package. There is no fancy web site to view packages. All querying is done through choco.exe (or Chocolatey GUI). Most options for hosting packages are based on the simple server setup, including non-Windows options like [Sonatype Nexus](http://www.sonatype.org/nexus/go/) and [JFrog’s Artifactory](https://www.jfrog.com/artifactory/).

The final option for the most complex scenarios is the [Package Gallery](https://chocolatey.org/docs/how-to-host-feed#package-gallery). A couple of examples are[Chocolatey.org](https://chocolatey.org/) and [NuGet.org](https://www.nuget.org/). You can have multiple types of package store, from file system to Azure blobs, and AWS S3. It has a database, indexing, and caching backing the package information, so it is super performant. And it has the idea of multiple users and rights assigned for each user to packages they maintain. Some disadvantages are that currently it only hosts on Windows and it requires more setup time.

With these options in mind your head may be spinning trying to decide which one is right for you. Going between a folder location and the simple server in many cases is simply adding an HTTP endpoint, so it’s really not hard to get started there! If you are using something like Puppet, there is already a simple way to set up a simple server for use with Chocolatey. The [Puppet Chocolatey Server module](https://forge.puppetlabs.com/chocolatey/chocolatey_server) will handle all aspects of setting up an internal host for you to use!