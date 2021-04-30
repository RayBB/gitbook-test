# Google-Summer-Of-Code-2018\(Plan\)

## Open Library - Google Summer of Code 2018

### Index

1. [Milestones and Deliverables](google-summer-of-code-2018-plan.md#milestones-and-deliverables)
   * [Community Bonding Period](google-summer-of-code-2018-plan.md#community-bonding-period)
   * Phase 1
   * Phase 2
   * Phase 3
   * Weekly plan
     * Community Bonding Period \(Apr 23 - May 14\)
     * Week 1 + Week 2 \(May 14 - May 27\)
     * Week 3 \(May 28 - June 3\)
     * Week 4 \(June 4 - June 10\)
     * Week 5 \(June 11 - June 17\)
     * Week 6 \(June 18 - June 24\)
     * Week 7 \(June 25 - July 1\)
     * Week 8 \(July 2 - July 8\)
     * Week 9 \(July 9 - July 15\)
     * Week 10 \(July 16 - July 22\)
     * Week 11 \(July 23 - July 29\)
     * Week 12 \(July 30 - August 5\)
2. [Project Overview](google-summer-of-code-2018-plan.md#Project-Overview)
3. [Technical Details](google-summer-of-code-2018-plan.md#Technical-Details)

## Milestones and Deliverables

The entire project period is divided into 3 phases. The deliverable at the end of each phase is as mentioned below :

### Community Bonding Period

* Learning how to use Open Library Client and using the Open Library Client to create bots to import new records to the Library.

### Phase 1 \(Week 1-4\)

* In the first 4 weeks, a new repository called openlibrary-bots will be created. A new bot will be created to add this to the repository which uses the OL client to process our 50,000 ONIX feeds and load the relevant books into Open Library.
* After this phase, books will be imported via MARC \(Machine-Readable Cataloging\) and all MARC feeds will be imported via the OL Client itself. These documentation for this phase will be covered in the Wiki section of the new repository and it will also be linked to the main Open Library Documentation.

### Phase 2 \(Week 5-8\)

Work with Docker to build a Production Based Deployment of Docker Images. Currently a Dockerfile using Xenial as a Base Image is present and the Vagrantfile has to be converted to a Dockerfile. The objectives that we look to achieve in Phase II are as follows:

* Switch Open Library to use Ansible \(a software that automates software provisioning, configuration management, and application deployment\). Have a Production as well as a Development Playbook. Playbooks are Ansible’s configuration, deployment, and orchestration language. They can describe a policy you want your remote systems to enforce, or a set of steps in a general IT process.   
* Deprecate Vagrant to use a combination of Ansible and Docker-Connector to build a Docker based VM.  
* Use Ansible Vault which is a feature of ansible that allows keeping sensitive data such as passwords or keys in encrypted files. This will replace the current system of having a olsystem.
* Explore having our development ansible playbook setup multiple Docker Images \(for db, solr, mecached, haproxy, web, covers, etc\) and Kubernetes to manage all the Docker containers. \(Provided we have time\).

### Phase 3 \(Week 9-12\)

This phase will primarily focus on improving the User Experience of an Open Library User. This month will deal with features that are prioritized for Q2 for Open Library as discussed on the meeting on 20th February, 2018. In a nutshell these three weeks will cover the following tasks:

* UI to Merging Works + Editions UI
* Enhance social share for public Reading Log
* Ability to export reading log
* Add ability to import reading log / bookshelves from Goodreads

### Weekly plan

#### Community Bonding Period \(April 23 - May 14\)

* Learning how to use Open Library Client and using the Open Library Client to create bots to import new records to the Library.

#### Week 1 + Week 2 \(May 14 - May 27\)

* Create a new repository called openlibrary-bots and create a bot to add to the repository which uses the OL client to process the 50,000 ONIX feeds and load the relevant books into Open Library

#### Week 3 \(May 28 - June 3\)

* Second set of MARC Books feeds are imported into Open Library with all links and the distinguished work.  

#### Week 4 \(June 4 - June 10\)

* Documentation and Testing for MARC and ONIX and closing all ONIX Related Issues. 
* Write first report for work done on ONIX and MARC and work done to import all the books using the Open Library Client. 
* Address all comments in code review for all the code added during the previous three weeks.

#### Week 5 \(June 11 - June 17\)

Modifying existing Vagrant-based system so that instead of using scripts/bootstrap.sh, all provisioning is done via Ansible. A different playbook for development and production will be present by the end of this week.

#### Week 6 \(June 18 - June 24\)

* This week will be spent on addressing the comments of the first report and making necessary changes as suggested. 
* Deprecate Vagrant and replace the Vagrant based system. Make the developer ansible playbook provision a Docker image using Ansible’s docker-connector. Ansible will create a Docker image and then provision the Docker image so that it can be used as Vagrant is presently being used on the Open Library Dev and Productions systems. 

#### Week 7 \(June 25 - July 1\)

* Setting up ansible-vault to use different keys for the `developer` versus the `production` playbooks. This will reduce Open Library’s dependency on using the olsystem “secrets” repository. This will also add much of the ol-system mechanism into ansible for production environment. 

#### Week 8 \(July 2 - July 8\)

* This week will involve submitting a second report and documenting the work done during this time. 
* In case we have extra time, the ansible Playbook can be changed to create Multiple Docker images. Another possibility to explore is how Kubernetes can be used to monitor and manage the multiple Docker Images.

#### Week 9 \(July 9 - July 15\)

* This week will involve adding new features and enhancing User Experience for the Feature public Reading Log. 
* Enhancing the Reading Log Social Share option\(by using the archive.org API\) and Exporting  Reading Logs in the form of JSON and CSV will be incorporated in this week. 

#### Week 10 \(July 16 - July 22\)

* This week will involve addressing the comments of the second report and making changes accordingly. 
* After that we will look into developing a new interface for Merging Different works where there will be a simplified interface as compared to the current interface where merging is basically done manually.

#### Week 11 \(July 23 - July 29\)

* Goodreads provides a way to download/export a list of books from one's bookshelves. It would allow users to take an exported dump\(in the form of a CSV or JSON\) of their reading log from Goodreads and then add each of these books to their Open Library account.

#### Week 12 \(July 30 - August 5\)

* This week will involve submitting a third report and documenting the work done during the entire project. 
* Documentation for all the code added during the entire Project Duration will be added and any pending work that has been remaining can be can be completed during this week.

## Project Overview

### Project Name

Open Library

### Student

[Salman Shah](https://github.com/salman-bhai)

### Mentor

Mek Kerapeles

### Abstract

Currently Open Library has over 1.75M Members who borrow millions of books every year. This Project looks to develop Open Library further by improving it with the addition of new options to the newly added feature of Public Reading Logs and creating a new UI for merging Workflows. Furthermore, adding more books to Open Library via ONIX and MARC feeds and creating a standard bot to import new book metadata records from 3rd party partners \(like BetterWorldBooks APIs, Onix Feeds, MARCs\). The project also focuses on improving System Reliability by shifting from a Vagrant based system to Docker and Ansible. Currently the Project focuses on three main categories: 1. **Ingestion** - Using MARC and ONIX to import new books into Open Library 2. **Devops** - Using Docker for better System Reliability 3. **Features** - Adding Features to Open Library for Merging Works and Public Reading Log

### Code

[Open Library](https://github.com/internetarchive/openlibrary)

## Technical Details

### Language Used

* Python

### Technologies Used

* Ansible
* Docker
* Infogami

