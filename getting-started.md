# Getting started

This project is continuously evolving and based on a lot of previous work. Before continuing this project it is deemed important to read the listed works to get a fundamental understanding of what is desired and what was already tried and determined before. Some of these works are not available online and it is important to ask project supervisors for these resources quickly. If for any reasons the supervisors fail to be able to supply the desired documents in a feasible amount of time your team is allowed to contact [Dantali0n](https://dantalion.nl/contact/). Please note that the previous works that are not readily available online should not be put online and that the one's supplied these documents take full responsibility for handling them correctly.

* [Alice Technical Design Report (TDR) - Upgrade off the Offline Online computing system](https://cds.cern.ch/record/2011297/files/ALICE-TDR-019.pdf)
* Load Balancing High Volume Network Traffic at ALICE After the Second Long Shutdown of CERN’s LHC
* Effect of ticktime on the Re-initialization and Blacklist load balancing algorithms during a fail-over for ALICE
* [Effects of the EPNs and FLPs on the Information Node for ALICE](https://github.com/hexoxide/Thesis)
* [Team Hexoxide Technical Design Report](https://github.com/hexoxide/documentation/tree/master/technical-design-report)

These documents were listed in chronological order and are best read in that order. Next to these documents are the following online resources which might be of significant interest.

* [Team Hexoxide github organization page](https://github.com/hexoxide)
* [Team Hexoxide Travis-CI](https://travis-ci.com/hexoxide)
* [Team Hexoxide CodeCoverage](https://codecov.io/gh/hexoxide)
* [Alice O2 C++ coding guidelines](https://rawgit.com/AliceO2Group/CodingGuidelines/master/coding_guidelines.html)
* [Original O2Balancer](https://github.com/valvy/BalancerScripts)
* [Original O2Balancer support scripts](https://github.com/valvy/O2-Balancer)
* [Software For Science github organization page](https://github.com/SoftwareForScience)

## Using the team Hexoxide software

The O2Balancer software has been completely rewritten by team Hexoxide but still relies on almost the same dependencies as previous works. The software is specifically aimed at being compatible with ARM based processors and was tested on Raspberry Pi models 3+. As referenced by the TDR the list of dependencies and how to install them can be found in the repository: [Raspberry-Dependency](https://github.com/hexoxide/raspberry-dependency). The dependency scripts was tested on Debian based systems and the latest image of Raspbian is highly recommended as image to use.

## Recommended background information

The following background is recommended for being able to comprehend information found in documents and get started with the continuation of the project. Do not be alarmed if not all recommended background is immediately familiar.

* Intermediate Linux - How to use git from the commandline, copy image files with dd, navigate folders and searching directories, edit files, understanding of permissions, basic networking with netstat and ifconfig, port and host discovery using nmap, using ssh for tunneling and configuring pki authentication, compiling c++ projects with cmake and gcc, monitoring and killing running processes, using dhcpd with commandline parameters to create an network on a specific interface.
* Intermediate C++ - familiar with cmake and gcc, familiar with components of the STL library, aware of coding practices introduced by c++2011 such as dynamic memory management (unique_ptr, shared_ptr, weak_ptr), familiar with gcc parameters such as -O3, -G3 and -pendantic, being familiar with shared and static libraries and how to link against them as well as being able to compile c++ code as shared library, understand boost unit tests and how to add additional tests in cmake as well as executing these tests using make.
* Intermediate Bash - Ability to read and understand operations such as if, while, for. Ability to understand storing command outputs into variables, ability to understand [] conditions with parameters such as -f or -d and understanding the exportation of variables using the export command.
* Intermediate networking - Ability to configure Network Address Translation (NAT) with port forwarding to allow for segmented but connected networks, configuring port forwarding on virtual machines to allow access from outside the host machine but not to the host machine itself and being able to compute netmasks.
* Beginner ZeroMQ - Knowing about the different topologies and configurations that are available while using ZeroMQ. Understanding the pro's and con's of configurations as well as their limitations and knowing about zero-copy messaging.
* Beginner travis - Ability to read and understand the travis.yml files, ability to add badges to readme's and being able to monitor builds and branches in the travis dashboard.
* Beginner gdb - Ability to start gdb from the commandline, being aware of limitations introduced to gdb when compiling with certain gcc parameters, ability to enable TUI mode for gdb, being able to set breakpoints and being able to display the contents of variables. 