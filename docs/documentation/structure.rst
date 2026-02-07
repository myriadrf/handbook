Structure
#########

The documentation structure should be consistent across MyriadRF projects of the same type, e.g. hardware, gateware and software. This makes it easier for users to find information and for maintainers to manage updates. 

General guidance is provided below, along with some specific guidance for hardware and software projects. 

Note that the structure of gateware projects is still to be defined, but it is likely to be similar to that of software projects.

General Guidance
****************

Focus on the Audience
---------------------

Documentation should be focused on the needs of the intended audience. For example, the user manual for a hardware project should be focused on helping users to get up and running with the project, while the developer guide for software should cover developer topics and not repeat information that is already covered in the user guide.

Minimise Duplication
--------------------

How to split content across projects, e.g. software instructions for hardware.

For example, developer documentation should not contain information that is already covered in the user guide, but it should link to the relevant sections of the user guide where appropriate. 

.. warning::
   It is vitally important to minimise duplication and to ensure that the documentation is well linked together, using Intersphinx where possible. This is because it is very easy for duplicated content to become out of date, which can lead to confusion for users and extra work for maintainers.

Maintain Consistency
--------------------

Each documentation project should have a consistent structure, with the same types of documents in the same places. This makes it easier for users to find information and for maintainers to manage updates.

Avoid Overly Long Pages
-----------------------

Pages should not be too long, as this can make them difficult to read and navigate. If a page is getting too long, it should be split into multiple pages with clear links between them.

[*this page is getting a bit long!*]

Hardware
********

Hardware projects should have the following structure:

Introduction
------------

:code:`/index.rst`

The project root index serves as a landing page for users and should contain a summary of the project, along with a photograph of the hardware. This page will be displayed when users navigate to a project from the MyriadRF website, so it should be welcoming and informative.

The introduction page for a hardware project should serve as a datasheet, with a summary of the project, along with key features and specifications, and a purchasing link if hardware is available to buy.

User Manual
-----------

:code:`/user/*`

This should have an index page which provides a short introduction. 

The User Manual should include at least the following sections: 

* Board Versions
* Hardware Setup 
* Troubleshooting

.. note::
   Once again ensure that content is not duplicated. E.g. the Troubleshooting section of the User Manual should for an SDR project should include links to the relevant sections of the software documentation, such as how to run tests to validate correct operation. However, it should contain details of how to check the hardware for faults, such as bad cabling and missing jumpers etc.

Depending upon the project, it may also include sections on things like:

* Software
* Advanced topics, e.g. connecting an external reference clock to an SDR, etc.

Where hardware is supported by software which has its own documentation, the user manual should be brief and link to the relevant sections of this.

.. note::
   The User Manual should be focused on helping users to get up and running with the project, and to understand how to use it. It should not contain detailed technical information about a design or the implementation of the project, as this is covered elsewhere.  

Hardware Reference
------------------

:code:`/reference/*`

The hardware reference should contain detailed technical information about the design. This might include things like detailed block diagrams, component information, connector pinouts and interface specifications etc. 

Resources
---------

:code:`/resources.rst`

The resources page for a hardware project should contain links to:

* The hardware design GitHub repository
* The documentation for associated resources, such as software, gateware and firmware repositories (Intersphinx links!)
* The MyriadRF forum category for the project

Software
********

Software projects should have the following structure:

Introduction
------------

:code:`/index.rst`

The project root index serves as a landing page for users and should contain a summary of the project, a featured image such as a logo or diagram, plus details of key features and other important information.

This page will be displayed when users navigate to a project from the MyriadRF website, so it should be welcoming and informative.

User Guide
----------

:code:`/user/*`

This should have an index page which provides a short introduction. 

The User Guide should include at least the following sections: 

* Installation
* Getting Started
* Troubleshooting

Depending upon the project, it may also include sections on things like:

* Advanced Topics

.. note::
   The User Guide should not contain detailed technical information about the implementation of the project, as this is covered elsewhere.  

Developer Guide
---------------

:code:`/developer/*`

The Developer Guide should contain detailed technical information about the implementation of the project, such as code structure, API documentation and examples etc.

Resources
---------

:code:`/resources.rst`

The Resources page for a software project should contain links to:

* The GitHub repository
* The documentation for associated resources
* The MyriadRF forum category for the project
