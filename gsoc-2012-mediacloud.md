---
layout: page
title: Media Cloud multi-language support via plug-in infrastructure, plus an automatic stemmer and a stopword list generator
---

*(Accepted and successfully completed project; [original proposal](https://www.google-melange.com/archive/gsoc/2012/orgs/berkman/projects/pypt.html).)*

I propose adding multi-language support to Media Cloud in such a way that the new languages (French, German, ...) could be added as plug-ins (adding a new language would not involve further modifications to the core code). Additionally, the process of adding a language would also be made simpler by providing automatic (experimental) stopword list and stemmer generators.


## About Me

### Description

My name is Linas [pronounced lee-nahs], I'm from Vilnius, Lithuania. Currently, I'm an undergraduate journalism student in Vilnius University, Faculty of Communication.

I've started programming with Perl when I was fourteen years old. Strangely enough, Perl was the first programming language that I've learned; I was introduced to it by a "hacker" magazine that I've bought (those were very popular at the time). The magazine claimed that Perl is "cool" and would, in fact, make me a "hacker".

It wasn't long before I found out that Perl is, indeed, pretty cool. This landed me a first-ever job while still being fourteen. I'm not too proud of the nature of the job (so to say), but here it is: I had to create a bot that would automatically "vote" for a certain music band on a radio station's online "Top 40" listings. The bot would work by downloading a list of HTTP / SOCKS 5 proxies from a certain website and then submitting a vote through each of them.

Again, that wasn't a flagship project of mine (and it happened ten years ago anyway), but I hope it shows that I had some early experience developing server-side applications in Perl. Thankfully, the "client" for this particular software got busted (my guess is that the radio station managers decided that it is unlikely that so many fans of the band X live in Zimbabwe), and I moved on into web and mobile development (as exhibited in my CV).

Later, while studying journalism in Vilnius University, I had various experience in developing public transport software (figuring out how to calculate shortest paths in huge, country-wide graphs), online media websites (learning the "do's" and "dont's" of a 1000 rq/s site), English-Lithuanian dictionaries (thanks to that, now I know what a stemmer is) and iPhone applications (varying from a simple news reader to a public transport timetable viewer with an ability to calculate those shortest paths client-side).

Last year, I was lucky enough to contribute to the wxWidgets project by bridging Cocoa Touch (Objective-C API used for developing iOS applications) and wxWidgets UI library together. The wxWidgets project (which was a part of GSoC 2011) taught me about how to get into and understand a huge, previously unknown code base.

Right now, I would like to try to find my way back to crawler programming, extend my knowledge of natural language processing and have a chance to play with a huge amount of data. Thus, I'm writing this proposal to the Media Cloud project.

### Personal Gains

By participating in the Media Cloud project, I hope to refresh my Perl skills, gain some more NLP experience, help opening up an interesting and useful software to a wider (international) audience, and to further improve my ability to read someone else's code and research papers.

### Why I'm interested

I got interested in the Media Cloud project because, as a journalism student, I see a great potential of such software being applied to the online media. Media Cloud could help answering many important questions and caching various ongoing trends in real-time (as they happen), and it's just that the adaptability of this software could be improved by implementing certain features (e.g. multi-language support, which I propose in this document).

Frankly, having to work with someone from Harvard is a motivator too.

### Dates and times

I have reviewed the Important dates and times for GSoC 2012. I have no conflicts with the schedule.

Although I live in UTC+3 timezone and Boston, MA is in UTC-4, the work hours in Boston [translate to "afternoon - midnight" in Lithuania](https://docs.google.com/spreadsheet/ccc?key=0AmV1dqYR3iqPdFpyeDVkV2pFVFN5N2ZjQmk5cmxtOWc#gid=0), so I have no problem with that too.

I do understand that Google Summer of Code is a serious commitment, equivalent to a full-time job.

### Code samples

* Git mirror of Media Cloud with a limited Lithuanian language support: <https://github.com/pypt/mediacloud>
wxiOS, an effort to create a wxWidgets based C++ wrapper over Cocoa Touch (GSoC 2011 project):
    * Description: [wxiOS](gsoc-2011-wxios)
    * Demo: <http://www.youtube.com/watch?v=PTbFj9dqROs> (2:26)
    * SVN branch: <http://svn.wxwidgets.org/viewvc/wx/wxWidgets/branches/SOC2011_WXIOS/> (I'm 'LV')
    * Tarball with a huge patch: <http://code.google.com/p/google-summer-of-code-2011-wxwidgets/downloads/detail?name=Linas_Valiukas.tar.gz&can=2&q=#makechanges>
* WikiShoot, an iOS (iPhone and iPad) application allowing one to take photos of unphotographed locations and upload them to Wikimedia Commons (or some other MediaWiki deployment): <https://github.com/pypt/WikiShoot>
    * CocoaMW, an Objective-C (Cocoa) library for accessing MediaWiki's API: <https://github.com/pypt/CocoaMW> (sub-project of the parent one)


## Proposal

### Overview

I propose adding multi-language support to Media Cloud in such a way that the new languages (French, German, ...) could be added as plug-ins (adding a new language would not involve further modifications to the core code). Additionally, the process of adding a language would also be made simpler by providing automatic (experimental) stopword list and stemmer generators.

Objectives:

* Move the language-dependent code and assets (stopword list, stemmer, "next page" detection algorithm, sentence stop detection algorithm, ...) for each of the currently supported languages (English, Russian, Chinese) into separate plug-ins.
* Modify the core code so that it would use a particular configured language plug-in for the inner processes.
* Create an experimental stopword list generator in order to make it easier for the end-user to provide a stopword list if they don't have one for their language.
* Create an experimental stemmer generator in order to make it easier for the end-user to provide a language stemmer if they don't have one for their language.
* Add a new language (Lithuanian) in order to test how the newly created plug-in architecture and experimental generators work.
* Document the process of adding a new language in a README.

Languages and technology: Perl, PostgreSQL, Snowball stemmer, Subversion (GIT + SVN).

### Detailed

At the time of writing, Media Cloud supports a limited set of natural languages (English, Russian, Chinese), and the support for those languages is implemented right in the core of the application in a (`isEnglish()) { ... } elsif (isChinese()) { ... } elsif (isRussian()) { ... }` manner.

I propose adding multi-language support to Media Cloud in such a way that the new languages (French, German, ...) could be added as plug-ins. With this approach, adding support for a new language would not require one to modify the core code, and the whole process would be easier to accomplish.

Benefits:

* Adding support for their own language would become easier for the end-user because he / she would know where to look at and the exact requirements for adding a new language.
* Experimental stopword list / stemmer generators might be used for creating stopword lists and a stemmer if none is available.
* Experimental stopword list / stemmer generators could be reused elsewhere (in other projects).
* Hopefully, multi-language support would bring the Media Cloud project more developers from the international community.

Objectives in detail:

1. **Move the language-dependent code and assets into separate plug-ins.**
    * This would be done for each of the currently supported languages (English, Russian, Chinese).
        * Later, for the Lithuanian language too (as a test case).
    * The plug-in for each language would allow one to configure:
        * a stopword list
        * a word stemmer (either a language code passed to the Lingua::Stem::Snowball CPAN module or a custom subroutine altogether)
        * a way to detect a link to a next page of an article (either via keyword(s) or a subroutine)
        * a way to detect the boundaries of a sentence (either by looking for a full stop symbol '.' or with a subroutine)
        * a way to extract separate words from a sentence (either by looking for a whitespace symbol ' ' or with a subroutine)
        * other means of language-specific setup
2. **Modify the core code so that it would use a particular configured language plug-in for the inner processes.**
    * The main language would have to be set in a main configuration file, mediawords.yml, or some other place.
    * Then, the tagger (sentence / word extractor) would initialize a plug-in for that particular language and use the language-specific data provided by the plug-in to split texts into sentences, sentences into words, words into their stems, detect "next pages", etc.
    * Unit tests will have to be adapted accordingly so that they both test the new language support, and the tests do pass.
3. **Create an experimental stopword list generator.**
    * The experimental stopword list generator would make it easier for the end-user to provide a stopword list if they don't have one for their language.
    * The stopword list generator would employ one, several or a mix of the methods described in:
        * Lo, R.T.-W., He, B., Ounis, I., 2005. [Automatically building a stopword list for an information retrieval system.](http://terrierteam.dcs.gla.ac.uk/publications/rtlo_DIRpaper.pdf) The Journal on Digital Information Management: special issue on the 5th Dutch-Belgian Information Retrieval Workshop (DIR 2005) 3, 3–8.
        * Makrehchi, M., Kamel, M.S., 2008. [Automatic extraction of domain-specific stopwords from labeled documents](http://books.google.lt/books?id=VAy1N54gwloC&lpg=PA222&ots=QfcRbc3NAu&dq=Automatic%20Extraction%20of%20Domain-Specific%20Stopwords%20from%20Labeled%20Documents&hl=lt&pg=PA222#v=onepage&q=Automatic%20Extraction%20of%20Domain-Specific%20Stopwords%20from%20Labeled%20Documents&f=false), in: Proceedings of the IR Research, 30th European Conference on Advances in Information Retrieval, March 30-April 03. Glasgow, UK.
        * Blanco, R., Barreiro, A., 2007. [Static pruning of terms in inverted files](http://www.dc.fi.udc.es/~barreiro/publications/blanco_barreiro_ecir2007.pdf). Advances in information retrieval, Vol. 4425 of lecture notes in computer science 64–75.
        * (the exact method that would be used is not strictly limited to this list; a better way to do this might come up after discussions with a mentor)
4. **Create an experimental stemmer generator.**
    * The experimental stemmer generator would make it easier for the end-user to provide a language stemmer if they don't have one for their language.
    * The stemmer generator would produce a [Snowball](http://snowball.tartarus.org/) language source of a stemmer (or some other - possibly Perl - data structure that would fit the nature of the stemmer) which could be used by `Lingua::Stem` or `Lingua::Stem::Snowball`.
    * The stemmer generator would employ one, several or a mix of the methods described in:
        * Di Nunzio, G.M., Ferro, N., Melucci, M., Orio, N., 2004. [Experiments to Evaluate Probabilistic Models for Automatic Stemmer Generation and Query Word Translation](http://books.google.lt/books?id=JYgg4nIXvhsC&lpg=PA220&ots=jH5MmoUPRc&dq=Experiments%20to%20Evaluate%20Probabilistic%20Models%20for%20Automatic%20Stemmer%20Generation%20and%20Query%20Word%20Translation&hl=lt&pg=PA220#v=onepage&q=Experiments%20to%20Evaluate%20Probabilistic%20Models%20for%20Automatic%20Stemmer%20Generation%20and%20Query%20Word%20Translation&f=false). Comparative evaluation of multi-lingual information access systems, Vol. 3237 of lecture notes in computer science 220–235.
        * Xu, J., Croft, W.B., 1998. [Corpus-based stemming using cooccurrence of word variants](http://cui.unige.ch/~ehrler/Project/Yassin/Articles/CorpusBasedStemmingUsingCoOccurrenceOfWord.pdf). ACM Transactions on Information Systems 16, 61–81.
        * Goldsmith, J.A., Higgins, D., Soglasnova, S., 2000. [Automatic Language-Specific Stemming in Information Retrieval](http://hum.uchicago.edu/~jagoldsm/Papers/CLEF.pdf), in: CLEF '00 Revised Papers. Presented at the Workshop of Cross-Language Evaluation, Forum on Cross-Language Information Retrieval and Evaluation.
        * (the exact method that would be used is not strictly limited to this list; a better way to do this might come up after discussions with a mentor)
5. **Add a new language (Lithuanian) in order to test how the newly created plug-in architecture and experimental generators work.**
    * Not a major language of the world, but it would serve as a good test case of a newly created plug-in architecture.
    *  Corpus for Lithuanian should be easy to find (I know some places to ask for it, and `lt.wikipedia.org` might be a last retreat).
6. **Document the process of adding a new language in a `README`.**
    * The `README` would be a step-by-step guide describing a way to add a new language to Media Cloud.


## Project Plan

Project plan distinguishes several levels of objective completeness. The levels are loosely defined as follows:

* *alpha* - development of the software has been started; software is more like a functional, incomplete "demo" than an integral part of Media Cloud; software is able to demonstrate that a particular chosen method (in case of stemmer / stopword generator) is able to complete the task at hand;
* *beta* - software is in the process of being integrated into Media Cloud; software is already a part of the Media Cloud system, but is still buggy, contains FIXMEs, marginal cases were not tested;
* *final* - software is an integral part of Media Cloud; code does not contain FIXMEs; marginal cases were tested; software is ready for production.

Plan:

* *April 23*: **Accepted student proposals announced on the Google Summer of Code 2012 site.**
* *April 24 - May 20* (27 days): **Community Bonding Period**
    * Get to know the mentor, find out the frequency of mini-evaluations and other communication that would be suitable for both parties.
    * Brush up my Perl skills.
    * Get to know the Media Cloud code by trying and breaking a few things.
    * Set up my development environment (find a good Perl debugger, etc.)
    * Set up a experimental Media Cloud deployment (used for testing) at my own expense which would crawl Lithuanian online media (or retain the one I'm currently running).
        * (This would cost just ~$20 / mo with the local prices, and I probably could get a dedicated Ubuntu server for free anyway).
    * Find a Lithuanian language corpus of considerable size and quality to be used in automatic stemmer / stopword list generator testing.
    * Assess available research on automatic stemmers and stopword generators, implement a few of them, choose one from each category with the help of a mentor.
    * Reach *alpha* level on objective #3 ('Create an experimental stopword list generator').
    * Reach *alpha* level on objective #4 ('Create an experimental stemmer generator').
* *May 21 - July 12* (53 days): **Interim Period #1**
    * Reach *final* level on objective #1 ('Move the language-dependent code and assets into separate plug-ins').
    * Reach *final* level on objective #2 ('Modify the core code so that it would use a particular configured language plug-in for the inner processes').
    * Reach *beta* level on objective #3 ('Create an experimental stopword list generator').
    * Reach *beta* level on objective #4 ('Create an experimental stemmer generator').
    * Reach *alpha* level on objective #5 ('Add a new language (Lithuanian) in order to test how the newly created plug-in architecture and experimental generators work').
* *July 13*: **Mid-term evaluations**
    * Hold a private mid-term review with a mentor in order to find out how I'm doing: too fast, too slow, messy code, code too pedantic so he / she is not sure if I'm going to finish on time, ...?
* *July 14 - August 19* (37 days): **Interim Period #2**
    * Reach *final* level on objective #3 ('Create an experimental stopword list generator').
    * Reach *final* level on objective #4 ('Create an experimental stemmer generator').
    * Reach *final* level on objective #5 ('Add a new language (Lithuanian) in order to test how the newly created plug-in architecture and experimental generators work').
    * Reach *final* level on objective #6 ('Document the process of adding a new language in a README').
