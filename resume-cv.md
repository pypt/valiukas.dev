---
layout: page
title: Resume / CV
---

*(last updated on February 19, 2022)*

## Linas Valiukas

Lithuania • <linas.valiukas@gmail.com> • [+370 687 65870](tel:+37068765870) • [LinkedIn](http://linkedin.com/in/linasvaliukas) • [GitHub](https://github.com/pypt)

Accomplished backend developer and versatile technologist with proven ability to contribute to software development, systems administration, and database administration initiatives. Experience includes 10 years as a remote employee for a US-based news media research project affiliated with MIT and Harvard, several significant personal projects, and multiple freelance assignments. Effective communicator and collaborator with proven success in remote roles with limited supervision.


### Experience


#### Software Engineer, Media Ecosystems Analysis Group - Remote

*(2012 - 2022)*

Joined Media Cloud ([GitHub](https://github.com/mediacloud/backend)) joint venture project that has been an effort in partnership with Harvard’s Berkman Center for Internet & Society (now Berkman Klein Center), MIT Media Lab, and now the umbrella nonprofit Media Ecosystems Analysis Group. Media Cloud is an open source, open data platform that allows researchers to answer quantitative questions about the content of online media.

* Assigned to a small development team and inherited a ~100K LOC, 10+ year-old codebase with development focused on Perl, Python, Solr, and PostgreSQL. 
* Implemented and maintained core features critical to fundraising efforts, including continuous collection of unstructured data from the web (news articles), NLP text preprocessing and analysis in 20+ languages, large scale URL impact statistics collection, automatic transcription of audio / video podcast episodes, word2vec model generation, and periodic XML sitemap traversal.
* Proposed and implemented multiple team efficiency improvements including migration from Subversion to Git, introduction of CI and automated deployments, database schema versioning, containerization with Docker, and server provisioning with Ansible.
* Reimplemented elements of the monolith into a job-based system to separate concerns and divide load between servers. Proposed and executed gradual codebase migration from Perl to Python and introduced workflow management system through Temporal.
* Maintained and scaled a growing 30+ TB PostgreSQL dataset. Optimized queries, improved configuration, partitioned/sharded large tables, managed file system-level (ZFS snapshot) and PITR (WAL-G) backups, and tested and executed upgrades.
* Managed and mentored new backend hires, external users of the backend product, and multiple Google Summer of Code interns.


#### Freelance Developer

*(2005 - present)*

Client Project Highlights:

* **Fotonija** (Lithuanian language software company)
    * Built a proprietary first-of-its-kind Lithuanian language support module for an open-source proofreading software featuring a spellchecker, a Hunspell-based morphological analyzer, and support for identifying most frequent Lithuanian language errors. The module is being used to analyze various Lithuanian language corpora. ([Link](https://www.semantika.lt/Analysis/TextAnalysis))
    * Wrote iOS and OS X versions of a leading English-Lithuanian dictionary now in its 10th version in [iTunes](https://itunes.apple.com/app/apple-store/id381153256).
* **Institute of Lithuanian Language** - Wrote a Qt-based Lithuanian language dictionary app for Windows, macOS, and Linux. ([store link](https://sites.fastspring.com/lkilt/product/lkz))
* **Merakas** (transport technology company) - Developed a website that provides real-time public transport data for ~10 cities using client-side JavaScript to reduce hosting costs. ([Link](https://www.stops.lt/vilnius/#vilnius/en))


### Personal Projects

* **iOS Keyboard** - Developed a third-party Lithuanian keyboard layout for iOS that replicates the standard system keyboard, featuring support for adjustable height, keys for popular emoticons / Emoji, and a bug report system. The app earned 400+ paid downloads in its first month. ([iTunes link](https://itunes.apple.com/app/apple-store/id993465092))
* **Google Summer of Code 2011 Project** - Developed an open source wxWidgets-based C++ wrapper over Cocoa Touch (iOS) that implemented a subset of wxWidgets API for touch-based devices. The API enabled wxWidgets users to port their C++ applications to iOS devices, and the interface could be reused to make wxWidgets available to other mobile platforms. 
* **Public Transportation** - Published iOS / Java ME applications for using public transportation timetables while offline. iOS versions of the apps contain automatic timetable data updates, an offline trip planner, and a full city map.
* **Fervor** - Built software library for inclusion in Qt-based applications to enable autoupdates. ([repo](https://github.com/pypt/fervor))
* **News App** - Created iOS app for a local news organization (~2008-09) that beame the top app locally and led to the website transforming to mobile-first.
* **FreeBSD ports** - Ported [Quake III](https://www.freshports.org/games/quake3/), [KCHM](https://www.freshports.org/deskutils/kchm/) (ebook reader), and [tspc2](https://www.freshports.org/net/tspc2/) (IPv6 tunnel utility) to FreeBSD (~2005). 


### Skills

* **Programming:** Python, Java, C, C++, Qt, Perl (+ gradually porting legacy Perl apps to Python), PHP, Bash, some Rust
* **Databases:** PostgreSQL (schema design, maintenance, query optimization, PITR backups, streaming / logical replication, updates, sharding with Citus Data), MySQL, SQLite3
* **OS:** Linux (administration, performance, debugging), macOS, FreeBSD
* **Web:** HTML 5, CSS, JavaScript
* **Mobile:** Objective-C (iOS SDK), Android SDK / NDK
* **Tools:** AWS (migrations, EC2, S3, networking, pricing estimates), Google Cloud (Compute Engine, Cloud Storage, Elastic Container Registry, Speech-to-Text, networking, pricing estimates), Docker, Temporal.io, Uber Cadence, Netflix Conductor, Airflow, Ansible, ELK stack, RabbitMQ, CI (GitHub Actions, Travis, CircleCI, Jenkins), Git, Solr, LXC, LaTeX, Kubernetes


### Education

**BA, Journalism**, Vilnius University - Vilnius, Lithuania (2012)


### Volunteering & Interests

* Volunteer (2020 - 2021), Lithuanian Red Cross Covid-19 vaccination center. 
* Technical Presentations:
    * Dealing with Gigantic Tables (extended version): @ PGConf APAC 2019, Singapore ([Summary](https://pgconfapac.org/events/pgconfapac2019/schedule/session/10-dealing-with-gigantic-tables/), [Video Link](https://www.youtube.com/watch?v=3uh8jnBa0l4))
    * Dealing with Gigantic Tables @ PostgresConf Silicon Valley 2018, San Jose, CA ([Summary](https://postgresconf.org/conferences/SiliconValley2018/program/proposals/dealing-with-gigantic-tables))
* Hobbies: Surfing, reading
