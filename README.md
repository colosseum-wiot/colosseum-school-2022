# Colosseum Master Class 2021
### November 1-3, 2021

<p align="middle">
  <img src="images/colosseum-1.jpg" width="49%" /> 
  <img src="images/colosseum-2.jpg" width="49%" />
</p>

Large-scale experimentation is a core component of wireless research. However, experimental capabilities are as effective and useful as their ability of capturing diverse wireless environments and conditions realistically, in a controlled environment that is highly accessible, programmable and where experiments can be repeated for fair and informative comparison among solutions. Up until now, the research community lacked widespread access to testbeds offering such critical capabilities, especially at scale.

The past few years have seen the emergence of larger facilities with the characteristics required for repeatable wireless experimentation at scale. Examples include the testbeds of the NSF Platform for Advanced Wireless Research (PAWR) program and Colosseum, which, with its capability of emulating over 60k wireless channels, is hailed as the world's largest wireless network emulator.

Colosseum is a massive RF and computational facility enabling large-scale experiments through a pool of 128 Software-defined Radios (SDRs) controlled by dedicated and remotely-accessible host computers called Standard Radio Nodes (SRNs). It emulates wireless signals traversing space and reflecting off multiple objects and obstacles as they travel from transmitters to receivers, through its Massive Channel Emulator (MCHEM) that consists of an additional array of 128 SDRs and 64 FPGAs. As such, Colosseum can create virtual worlds, as if the radios are operating in an open field, downtown area, shopping mall, or a desert, by generating more than 52 terabytes of data per second.

Colosseum is hosted at Northeastern University, and is freely accessible by anybody in the research community with an active grant on wireless research. We believe that a tutorial on Colosseum is timely and relevant to the MobiCom community as it will offer to the attendees a clear introduction on how to access the emulator, and how to run repeatable wireless experiments at scale on it, emphasizing its capabilities of modeling a vast variety of scenarios, channel conditions, and traffic and mobility patterns. Particularly, we will show how to use Colosseum in a set of scenario relevant to most wireless research: Local area networking (e.g., WiFi-based networks), cellular networks, and wireless ad hoc scenarios (e.g., aerial or vehicular networking). Out tutorial will further explain how wireless emulated experiments can be ported to other real-world wireless testbeds, including the PAWR platforms, thus facilitating full-cycle experimental wireless research: Design, experiments and tests at scale in a fully controlled and observable environment, and testing in the field.


## Colosseum Young Gladiators Master Class

The Colosseum team at the Institute for the Wireless Internet of Things at Northeastern University is organizing a 3 day in-person master class for PhD students and Postdoc researchers at U.S. institutions who are interested in using the Colosseum channel emulator for their research. The course will include tutorial-like presentations from Colosseum experts, hands-on assignments for the attendees, invited talks by Colosseum lead users, and a site visit to the Colosseum data center facility.

By the end of this master class the attendees will learn about the wireless emulation, how Colosseum does it, and how to access and use the Colosseum wireless network emulator. We will explain the fundamentals of wireless network emulation, its use for experimental wireless research, and how Colosseum does it at scale and with hardware-in-the-loop. We will describe the Colosseum architecture and its unique emulation system, called MCHEM, and will talk about Radio Frequency (RF) and traffic scenarios in Colosseum and how they capture realistic wireless environments and conditions.

After the initial overview of Colosseum, attendees will be guided to the usage of the emulator with hands-on experiments on how to access Colosseum and run experiments. The Colosseum containerized system will be explained in detail with hands-on exercises on how to instantiate containers on Colosseum, how to work with them, and how to save them for a later use. We will then show how to run actual experiments using customized containers in Colosseum and its channel emulation system. Practical demonstrations will be given of Colosseum use cases, including WiFi and cellular networking and multi-hop ad hoc networks.


## Agenda
The agenda for the Colosseum Master Class can be found [here](https://www.northeastern.edu/colosseum/master-class2021/).


## Requirements for the attendees

- The use of Linux-based systems, which is the operating system of most Colosseum components, is advised for hands-on sessions.
Familiarity with basic Linux commands and its command line interface (e.g., ssh, scp/rsync) is recommended.
Basic knowledge of computer networking and wireless concepts is also useful.
- The attendees are required to bring their own laptop for the hands-on sessions.
- The attendees are required to setup in advance their ssh keys (see [Upload SSH Public Keys](https://colosseumneu.freshdesk.com/en/support/solutions/articles/61000253402-upload-ssh-public-keys)) and ssh proxy (see [SSH Proxy Setup](https://colosseumneu.freshdesk.com/en/support/solutions/articles/61000253369-ssh-proxy-setup))
- The attendees are required to verify in advance that they are able to successfully access Colosseum portal and resources (see [Accessing Colosseum Resources](https://colosseumneu.freshdesk.com/en/support/solutions/articles/61000253362-accessing-colosseum-resources)), e.g., log into the ssh gateway and file-proxy servers.
