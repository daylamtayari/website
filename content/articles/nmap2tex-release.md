---
title: "Nmap2Tex Release"
description: "Releasing Nmap2Tex, an Nmap to LaTeX converter and formatter."
slug: nmap2tex-release
date: 2022-01-30T22:34:40-07:00
draft: false
toc: false
images:
tags: ['project', 'nmap2tex', 'nmap', 'lateX']
---

## Introduction

What is Nmap2Tex? Well as it's name suggests, it converts a Nmap (a network scanner) scan result into a nicely pre-formatted LaTeX document containing all of the important information from the scan and also colour coding some components such as vulnerabilities according to their CVSS or hosts based on how vulnerable they are.<br/>
<br/>
The inspiration of this project really stems from my experience competing in the national Collegiate Cyber Defense (CCDC) competition, a collegiate cybersecurity competition which aims to help give students experience in a blue team environment, providing each team with a whole network infrastructure with everything from a firewall, a variety of both Linux and Windows machines and occasionally also VoIP devices. <br/>
Throughout this competition, we must execute injects (task requests by the organisers) all while being constantly attacked by a professional red team and having to perform the respective incident response.<br/>
One particular inject that is present in every competition and which is requested in the beginning stages of the competition is a network inventory. In this report, we must include every device running on the network, their information (OS, hostname, IP) and the list of open ports and their respective services but also the list of vulnerabilities present on each machine on the system.<br/>
We usually rely on using Nmap to scan the network and retrieve all necessary information for the report.<br/>
<br/>
Now at one recent invitational, I was tasked with performing this inject and let me just say that manually typing up the results of a Nmap scan on around a dozen devices, each running a large number of services, is incredibly frustrating to say the least which is exacerbated by how time intensive it is.<br/>
It was while completing that inject that I made the decision that I would never again let myself complete this inject in such a painstaking way, let alone let someone else on my team also suffer through this.<br/>
<br/>
Since Nmap has an option to output into XML and due to my fondness for LaTeX, I decided that I would build a tool that would take in the Nmap scan result in an XML format and then convert it into a nicely formatted LaTeX document based on a provided template.<br/>
<br/>
This is the result of that, here releasing version 0.1 which contains full support for Nmap network scan and vulnerability scan conversions among other things.<br/>
Version 0.1 is the initial version of the program and it's version number will continue to increase until I release version 1.0 when I am satisfied with the project and it's state.<br/>
<br/>
**To download the release:** [GitHub](https://github.com/daylamtayari/Nmap2Tex/releases/tag/v0.1)

<br/>

**This is not a guide on how to install and use Nmap2Tex, please check my upcoming guide or the `README.md` on the project page.**


## Features

While initially I only intended for it to be able to convert simple network scans, as I decided to turn a script into a project I decided to include a significantly greater variety of features.<br/>
The features that are currently supported in release 0.1 of Nmap2Tex are:
- Complete support for Nmap network scans.
- Vulnerability reports with smart highlighting according to each individual vulnerability and host's risk.
- External vulnerability Nmap scans allowed as inputs in addition of another existing network scan.
- Custom LaTeX template file with automated retrieval and update.
- Service renaming to human-readable alternatives.
- User table with special highlighting of superusers.
- Template and services automated retrieval and updates.

<br/>
I had more ideas for features to implement in the near future such as extended vulnerability reports but as the Western CCDC regional qualifiers were shortly upcoming, I decided to polish it's iteration and then after testing it, release it as 0.1 to be able to utilise it for the competition and also be able to share it with my teammates to allow whoever the person to be assigned the network inventory inject is able to just deploy it on their machine and use the tool.<br/>
I go in greater details on the planned upcoming features in the <a href="#roadmap">roadmap</a> section down below.


## Program Design

Nmap2Tex was designed to have 2 core inputs, an Nmap scan file in XML and an output file which while will be in LaTeX, is not required to be a `.tex` file.<br/>

The Nmap file has to be in XML and originate from Nmap as the program will go parse the XML input and search for a specific set of tags and values. It only performs the XML parsing absolutely necessary to obtain the information necessary to present in the output.<br/>
The program will go and parse through every host, retrieving the set of tags necessary before going on to the next host to parse.<br/>

In order to store the values, a variety of object classes are used.
- `Host`: Contains the information about the host and also contains a list of objects that apply to it. It stores all the general information such as the IP, operating system or hostname for example.
- `Port`: Contains all the information of interest of a specific port such as what port is it running on, using which protocol and when applicable, running which service.
- `Service`: Stores the name, product and version number of a specific service.
- `Vuln`: Contains information about a vulnerability such as its CVE and CVSS but also which port does it affect.
- `User`: Simply stores the name of a user provided and whether or not it is a superuser.
<br/>
Following the parsing, the program will import the LaTeX template and create the output file to which it will add to over the running of the program. The LaTeX operations are hard-coded into the proram so while a user may use a custom LaTeX template, the names of the environments are not to be changed to ensure full compatibility.<br/>
It will then go through all of the data it retrieved from the input Nmap scan and using the proper environments, it will output all the values in a specific format into the output file.

Nmap2Tex also supports inputs such as custom services list or a list of users in the domain which will then be appended to the output file, in their respective formatting.

The program will then end just after closing out the output LaTeX document.<br/>
Now all that is left of the user is to compile the LaTeX into a PDF, and they have their report ready.

This exact structure is very likely going to change to a considerable extent when I finish rewriting this program in Go but I will certainly post a blog post on the release of `v0.3` with all the major architectural changes and obviously the change list.


## Roadmap

While this version is fully functional and completes everything that I initially intended on support, that does not mean that this project is over, far from that.<br/>
I have numerous ideas on improvements and feature implementations that I plan on integrating into this program.<br/>
<br/>

### Upcoming Improvements

- Go rewrite - Rewriting Nmap2Tex entirely in Go primarily for performance and usage benefits but also to increase my experience with the Go language.
- Extended vulnerability report which contains a mini subsection for each vulnerability detected with a summary about it, affected systems, the patch, etc. This will be primarily achieved by retrieving corresponding data from the NVD API.
- Detailed error handling.
- Services and template files version checks and updates.
- Implement custom denominator on services and template files which disables update checks and prompts.
- Test and implement various other types of Nmap scans.
- Convert its major LaTeX environments and features into a LaTeX package.

### Release Schedule:

- `v0.2`: Go rewrite.
- `v0.3`: Error handling, services and template files updates, custom denominator on LaTeX services and templates files.
- `v0.4`: Extended vulnerability report with NVD API integration.

<br/>
Undetermined future: LaTeX package.


## Support

If this project interests you, please check out the repos (linked down below) and star it to make sure you're informed of when new versions are released.<br/>
<br/>
If you find any issues or have any recommendations, please create an issue on the GitHub page.<br/>
<br/>
Thank you very much and I truly hope that this tool has being able to be useful!


### Repository
- [GitHub.com/daylamtayari/Nmap2Tex](https://github.com/daylamtayari/Nmap2Tex)

<br/>
