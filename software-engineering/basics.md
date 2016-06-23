### Software Engineering

* __Software__ - Computer programs and associated documentation, may be developed:
    * for a particular customer - customer own the specification, customer make decisions on software changes
    * for a general market - stand alone systems, decision of software change are made by the developer, developer
    own the specification
* __Attributes of good software__ :
    * Maintainability - can evolve to meet the changing needs
    * Dependability - include reliability, security and safety
    * Efficiency - not waste system resources (memory, processor)
    * Usability - acceptable to the users
* __Software engineering__ - Software engineering is an engineering discipline that is concerned with all aspects of
software production.
* __SE issues__ :
    * Heterogeneity - distributed systems across networks that include different types of computer and mobile devices
    * Business and social change
    * Security and trust
    * Scale
* __System engineering__ - System engineering is concerned with all aspects of computer-based systems development
including hardware, software and process engineering. Software engineering is part of this more general process.
* __Computer Science__ - Computer science focuses on theory and fundamentals; software engineering is concerned with
the practicalities of developing and delivering useful software.
* __Cost of software engineering__ - Roughly 60% of software costs are development costs, 40% are testing costs. For
custom software, evolution costs often exceed development costs.
* __Web SE__ - :
    * Software reuse - Software reuse is the dominant approach for constructing web-based systems. 	When building
    these systems, you think about how you can assemble them from pre-existing software components and systems.
    * Incremental and agile development - Web-based systems should be developed and delivered incrementally. It is
    now generally recognized that it is impractical to specify all the requirements for such systems in advance.
    * Service-oriented systems - Software may be implemented using service-oriented software engineering, where
     the software components are stand-alone web services (SOA).
    * Rich interfaces - Interface development technologies such as AJAX and HTML5 have emerged that support the
    creation of rich interfaces within a web browser.
* __Issues of professional responsibility__ :
    * Confidentiality
    * Competence
    * Intellectual property rights
    * Computer misuse
* __Ethical principles__ :
    * Public
    * Client and employer
    * Product
    * Judgment
    * Management
    * Profession
    * Colleagues
    * Self

===

### Software Processes

* __Software process__ - A set of activities whose goal is the development or evolution of software, general activities:
    * Specification - defining what the system should do
    * Development - defining the organization of the system and implementing the system, stages:
        * Design, main activities:
            * Architectural design, where you identify the overall structure of the system, the principal components
            (subsystems or modules), their relationships and how they are distributed.
            * Database design, where you design the system data structures and how these are to be represented in a database.
            * Interface design, where you define the interfaces between system components.
            * Component selection and design, where you search for reusable components. If unavailable, you design how it will
             operate.
        * Implementation
        * Design and implementation are interleaved activities for most types of software system
    * Validation - checking that it does what the customer, stages:
        * Component testing
        * System testing
        * Customer testing (acceptance testing)
    * Evolution - changing the system in response to changing customer needs
* __Software processess__ :
    * __Plan-driven processes__ - processes where all of the process activities are planned in advance and progress is
measured against this plan:
        * A plan-driven approach to software engineering is based around separate development stages with the outputs
         to be produced at each of these stages planned in advance.
        * Not necessarily waterfall model – plan-driven, incremental development is possible
        * Iteration occurs within activities.
    * __Agile processess__ - planning is incremental and it is easier to change the process to reflect changing customer
requirements:
        * Specification, design, implementation and testing are inter-leaved and the outputs from the development
        process are decided through a process of negotiation during the software development process.
    * In practice, most practical processes include elements of both plan-driven and agile approaches.
* __Software process model__ - abstract representation of a process, it presents a description of a process from some
 particular perspective, types:
    * __Waterfall model__ - Plan-driven model. Separate and distinct phases of specification and development
    * __Incremental development__ - Specification, development and validation are interleaved. May be plan-driven or
    agile
    * __Integration and configuration__ - The system is assembled from existing configurable components. May be
    plan-driven or agile, reused elements may be configured to adapt their behaviour and functionality to a user’s
    requirements, types of reusable software:
        * Stand-alone application systems (COTS - commercial off the shelf)
        * Collections of objects that are developed as a package to be integrated with a component framework such as
        .NET or J2EE
        * Web services that are developed according to service standards and which are available for remote invocation
    * In practice, most large systems are developed using a process that incorporates elements from all of these models
* __The waterfall model__ :
    * requirements definition
    * system and software design
    * implementation and unit testing
    * integration and system testing
    * operation and maintenance
* __Coping with changes__ :
    * System prototyping, where a version of the system or part of the system is developed quickly to check the
    customer’s requirements and the feasibility of design decisions. This approach supports change anticipation,
    process:
        * prototype objectives
        * prototype functionality
        * develop prototype
        * evaluate prototype
    * Incremental delivery (ex: Agile), where system increments are delivered to the customer for comment and
    experimentation.
    This supports both change avoidance and change tolerance
        * User requirements are prioritised and the highest priority requirements are included in early increments.
* __Process improvement activities__ :
    * Process measurement - You measure one or more attributes of the software process or product. These measurements
     forms a baseline that helps you decide if process improvements have been effective.
    * Process analysis - The current process is assessed, and process weaknesses and bottlenecks are identified.
    Process models (sometimes called process maps) that describe the process may be developed.
    * Process change - Process changes are proposed to address some of the identified process weaknesses. These are
    introduced and the cycle resumes to collect data about the effectiveness of the changes.

===

### Agile

* Rapid development and delivery is now often the most important requirement for software systems:
    * Businesses operate in a fast –changing requirement and it is practically impossible to produce a set of stable
    software requirements
    * Software has to evolve quickly to reflect changing business needs
* __Agile development__ :
    * Specification,  development (design and implementation) and testing are inter-leaved
    * The system is developed as a series of versions or increments with stakeholders involved in version
    specification and evaluation
    * Extensive tool support (e.g. automated testing tools) used to support development
    * Minimal documentation – focus on working code, reduce overheads in the software process to be able to respond
    quickly to changing requirements without excessive rework
* __Agile principles__ :
    * Customer involvement - This depends on having a customer who is willing and able to spend time with the
    development team and who can represent all system stakeholders. Often, customer representatives have other
    demands on their time and cannot play a full part in the software development.
    * Incremental delivery - Rapid iterations and short-term planning for development does not always fit in with the
     longer-term planning cycles of business planning and marketing. Marketing managers may need to know what product
      features several months in advance to prepare an effective marketing campaign.
    * People not process - Individual team members may not have suitable personalities for the intense involvement
    that is typical of agile methods, and therefore may not interact well with other team members.
    * Embrace change - Prioritizing changes can be extremely difficult, especially in systems for which there are
    many stakeholders. Typically, each stakeholder gives different priorities to different changes.
    * Maintain simplicity - Under pressure from delivery schedules, team members may not have time to carry out
    desirable system simplifications.
* __Agile development techniques__ :
    * Extreme Programming (XP) - extreme approach to iterative development
        * New versions may be built several times per day
        * Increments are delivered to customers every 2 weeks
        * All tests must be run for every build and the build is only accepted if tests run successfully
        * EP practices:
            * incremental planning - requirements are recorded on story cards and the stories to be included in a
            release are determined by the time available and their relative priority. The developers break these
            stories into development ‘Tasks’.
            * small releases - the minimal useful set of functionality that provides business value is developed
            first. Releases of the system are frequent and incrementally add functionality to the first release
            * simple design - enough design is carried out to meet the current requirements and no more
            * test first development - an automated unit test framework is used to write tests for a new piece of
            functionality before that functionality itself is implemented
            * refactoring - all developers are expected to refactor the code continuously as soon as possible code
            improvements are found. This keeps the code simple and maintainable
            * pair programming - developers work in pairs, checking each other’s work and providing the support to
            always do a good job
            * collective ownership - the pairs of developers work on all areas of the system, so that no islands of
            expertise develop and all the developers take responsibility for all of the code. Anyone can change anything
            * continuous integration - as soon as the work on a task is complete, it is integrated into the whole
            system. After any such integration, all the unit tests in the system must pass
            * sustainable pace - large amounts of overtime are not considered acceptable as the net effect is often
            to reduce code quality and medium term productivity
            * on-site customer - a representative of the end-user of the system (the customer) should be available
            full time for the use of the XP team. In an extreme programming process, the customer is a member of the
            development team and is responsible for bringing system requirements to the team for implementation
    * Agile development uses practices from XP, the method as originally defined is not widely used, key practices:
        * user stories for specification
        * test driven development
        * refactoring
        * pair programming
* __Scrum__ :
    * Scrum is an agile method that focuses on managing iterative development rather than specific agile practices,
    three phases in Scrum:
        * The initial phase is an outline planning phase where you establish the general objectives for the project
        and design the software architecture.
        * This is followed by a series of sprint cycles, where each cycle develops an increment of the system.
        * The project closure phase wraps up the project, completes required documentation such as system help frames
        and user manuals and assesses the lessons learned from the project.
    * terminology:
        * Development team - a self-organizing group of software developers, which should be no more than 7 people.
        They are responsible for developing the software and other essential project documents
        * Potentially shippable software - the software increment that is delivered from a sprint. The idea is that
        this should be ‘potentially shippable’ which means that it is in a finished state and no further work, such
        as testing, is needed to  incorporate it into the final product. In practice, this is not always achievable
        * Product backlog - this is a list of ‘to do’ items which the Scrum team must tackle. They may be feature
        definitions for the software, software requirements, user stories or descriptions of supplementary tasks that
         are needed, such as architecture definition or user documentation
        * Product owner - an individual (or possibly a small group) whose job is to identify product features or
        requirements, prioritize these for development and continuously review the product backlog to ensure that the
         project continues to meet critical business needs. The Product Owner can be a customer but might also be a
         product manager in a software company or other stakeholder representative
        * Scrum - a daily meeting of the Scrum team that reviews progress and prioritizes work to be done that day.
        Ideally, this should be a short face-to-face meeting that includes the whole team
        * ScrumMaster - the ScrumMaster is responsible for ensuring that the Scrum process is followed and guides the
         team in the effective use of Scrum. He or she is responsible for interfacing with the rest of the company
         and for ensuring that the Scrum team is not diverted by outside interference. The Scrum developers are
         adamant that the ScrumMaster should not be thought of as a project manager. Others, however, may not always
         find it easy to see the difference
        * Sprint - A development iteration. Sprints are usually 2-4 weeks long
        * Velocity - an estimate of how much product backlog effort that a team can cover in a single sprint.
        Understanding a team’s velocity helps them estimate what can be covered in a sprint and provides a basis for
        measuring improving performance.
* __Problems with agile methods__ :
    * The informality of agile development is incompatible with the legal approach to contract definition that is
    commonly used in large companies.
    * Agile methods are most appropriate for new software development rather than software maintenance. Yet the
    majority of software costs in large companies come from maintaining their existing software systems.
    * Agile methods are designed for small co-located teams yet much software development now involves worldwide
    distributed teams

===

### Requirements engineering

* __Requirement__ - It may range from a high-level abstract statement of a service or of a system constraint to a
detailed mathematical functional specification, it should show WHAT the system should do rather than HOW it should do
 it, types:
    * __User requirements__ - Statements in natural language plus diagrams of the services the system provides and
    its operational constraints. Written for customers.
    * __System requirements__ - A structured document setting out detailed descriptions of the system’s functions,
    services and operational constraints. Defines what should be implemented so may be part of a contract between
    client and contractor.
* __Stakeholder types:__
    * __End users__
    * __System managers__
    * __System owners__
    * __External stakeholders__
* Agile methods usually use incremental requirements engineering ("user stories"), detailed system requirements is a
waste of time as requirements change so quickly
* __Requirements types:__
    * __Functional:__
        * Describe functionality or system services.
        * Depend on the type of software, expected users and the type of system where the software is used.
        * Functional user requirements may be high-level statements of what the system should do.
        * Functional system requirements should describe the system services in detail.
    * __Non-functional:__
        * These define system properties and constraints e.g. reliability, security, response time and storage
        requirements. Constraints are I/O device capability, system representations, etc.
        * Process requirements may also be specified mandating a particular IDE, programming language or development
        method.
        * Non-functional requirements may be more critical than functional requirements. If these are not met, the
        system may be useless.
        * Often apply to the system as a whole rather than individual features or services.
* __Metrics for nonfunctional requirements:__
    * speed, size, ease of use, reliability, robustness, portability
* __Requirements engineering processes:__
    * __elicitation__
        * Software engineers work with a range of system stakeholders to find out about the application domain, the
        services that the system should provide, the required system performance, hardware constraints, other
        systems, etc, activities:
        * Requirements discovery - Interacting with stakeholders to discover their requirements. Domain requirements
        are also discovered at this stage.
        * Requirements classification and organisation - Groups related requirements and organises them into coherent
        clusters.
        * Prioritisation and negotiation - Prioritising requirements and resolving requirements conflicts.
        * Requirements specification - Requirements are documented and input into the next round of the spiral.
        * You can use a range of techniques for requirements elicitation including interviews and ethnography. User
        stories and scenarios may be used to facilitate discussions.
    * __analysis__
    * __validation:__
        * Validity. Does the system provide the functions which best support the customer’s needs?
        * Consistency. Are there any requirements conflicts?
        * Completeness. Are all functions required by the customer included?
        * Realism. Can the requirements be implemented given available budget and technology
        * Verifiability. Can the requirements be checked?
    * __management:__
        * Requirements management is the process of managing changing requirements during the requirements
        engineering process and system development.

===

### System modeling

* System modeling is the process of developing abstract models of a system, with each model presenting a different
view or perspective of that system.
* System modeling has now come to mean representing a system using some kind of graphical notation, which is now
almost always based on notations in the Unified Modeling Language (UML).
* System modelling helps the analyst to understand the functionality of the system and models are used to communicate
with customers.
* __UML diagram types:__
    * __Activity diagram__ - show the activities involved in a process or in data processing
    * __Use case diagram__ - show the interactions between a system and its environment
    * __Sequence diagram__ - show interactions between actors and the system and between system components
    * __Class diagram__ - show the object classes in the system and the associations between these classes
    * __State diagram__ - show how the system reacts to internal and external events
* Context models are used to illustrate the operational context of a system - they show what lies outside the system
boundaries.
* System boundaries are established to define what is inside and what is outside the system.
* __Behavioral models:__
    * Behavioral models are models of the dynamic behavior of a system as it is executing. They show what happens or
    what is supposed to happen when a system responds to a stimulus from its environment, 2 types of stimuli:
        * data (Data-driven modelling)
        * events (Event-driven modelling)
    * State machine models

===

#### Model-driven engineering

* Model-driven engineering (MDE) is an approach to software development where models rather than programs are the
principal outputs of the development process (models that can be automatically transformed to executable code)
* Model-driven architecture (MDA) was the precursor of more general model-driven engineering
* MDA is a model-focused approach to software design and implementation that uses a subset of UML models to describe a
 system.
* __Types of model:__
    * __Computation independent model (CIM)__ - These model the important domain abstractions used in a system. CIMs
    are sometimes called domain models.
    * __Platform independent model (PIM)__ - These model the operation of the system without reference to its
    implementation. The PIM is usually described using UML models that show the static system structure and how it
    responds to external and internal events.
    * __Platform specific models (PSM)__ - These are transformations of the platform-independent model with a separate
    PSM for each application platform. In principle, there may be layers of PSM, with each layer adding some
    platform-specific detail.

===

### Architectural design

* Architectural design is concerned with understanding how a software system should be organized and designing the
overall structure of that system.
* Architectural design is the critical link between design and requirements engineering, as it identifies the main
structural components in a system and the relationships between them.
* The output of the architectural design process is an architectural model that describes how the system is organized
as a set of communicating components.
* __Architectural views:__
    * __logical view__
    * __process view__
    * __development view__
    * __physical view__
    * related using use cases of scenarios
* __Architectural patterns:__
    * An architectural pattern is a stylized description of good design practice, which has been tried and tested in
    different environments, examples:
        * __MVC__
        * __Layered architecture__
            * Data layer (database)
            * Data access layer
            * Business layer
            * Presentation layer
        * __Repository architecture__
            * Sub-systems must exchange data. This may be done in two ways:
                * Shared data is held in a central database or repository and may be accessed by all sub-systems;
                * Each sub-system maintains its own database and passes data explicitly to other sub-systems.
        * __Client - Server architecture__
            * distributed system model
            * set of stand-alone servers which provide services
            * set of clients which call on these services
            * network which allows clients to access servers
        * __Pipe and filter architecture__
            * The processing of the data in a system is organized so that each processing component (filter) is
            discrete and carries out one type of data transformation. The data flows (as in a pipe) from one
            component to another for processing.

===

### Design and implementation

* Software design and implementation is the stage in the software engineering process at which an executable software
 system is developed.
* Software design and implementation activities are invariably inter-leaved.
    * Software design is a creative activity in which you identify software components and their relationships, based
    on a customer’s requirements.
    * Implementation is the process of realizing the design as a program.
* Architectural design
* Design patterns - A pattern is a description of the problem and the essence of its solution, elements:
    * Name
    * Problem description
    * Solution description
    * Consequences
* Implementation issues














