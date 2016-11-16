---
layout: post
title: "A proposal: doing name authorities better with non-hierarchical alignments"
date: 2016-11-16 10:35:01
categories: Infrastructure Projects
image: "/assets/black_goat_100.png"
short: 	"Many projects in digital humanities use name authorities (e.g. VIAF, DBPedia) to encode more precise references to people, places, or other concepts in data, and to leverage linked data. We often run into a situation in which we need to keep track of alignments between entries in different authorities that represent the same concepts. The typical solution is to create one's own master authority system that aggregate those existing authorities, but this can cause major headaches when it comes to data integration later on. Black Goat is an attempt to reduce those headaches through non-hierarchical mappings."
---

Recently, my colleague [Julia Damerow](http://jdamerow.users.sourceforge.net/) and I started to sketch out a model for **non-hierarchical alignment** among name authority services. My first attempt at an implementation is a bare-bones web service called [Black Goat](https://diging.github.io/black-goat/). In this post I outline what I take to be a looming problem for digital humanities projects that use name authorities, and roughly describe what I think is a good start on a solution.

## What's a name authority, and what is it for?

Name authorities are an extremely important part of any project that requires precise references to historical actors, places, organisms, or other concepts. In its simplest form, a name authority is a list of standardized representations of concepts in a domain, e.g. a list of preferred spellings of people's names. Name authorities have been around forever (the [Library of Congress Subject Headings](http://id.loc.gov/authorities/subjects.html) dates back to the late 19th century), and are used primarily for indexing.

For example, if every book written by Darwin is indexed using the standardized representation ``Darwin, Charles, 1809-1882``, then finding a comprehensive list of his works is easy -- I can perform a single search using that representation, and have high confidence that the list of results is complete. Without such a standardized representation, I may have to perform several searches -- "Darwin", "Charles Darwin", "Darwin, C", etc. -- and even then may not be confident that I have found every work by Darwin held by the library.

We face the same problem when dealing with data about people in a research context. For example, in the [Genecology Project](http://www.genecology.org) we have several researchers collecting data about a whole bunch of people: where they went to school, where they worked, what organisms they studied, etc. Without a standardized way of referring to those people it would be a real pain to pull out, say, all of the data that we have about [Irene Manton](https://en.wikipedia.org/wiki/Irene_Manton). Suppose one researcher wrote her name "I. Manton", another "Irene Manton", and yet another (assuming that there was only one Manton) simply wrote "Manton". Perhaps I could search for any name containing the string "Manton", but then my results would be polluted by data about "Joseph Manton".

In the age of the internet, we refer to people using **URIs (Uniform Resource Identifiers)**. An URI is a unique alphanumeric string that refers to a resource (in this case, an entry in an authority record). Usually an URI is also an URL (Uniform Resource Locator), which means that it also spells out precisely where that resource can be found. Instead of referring to Irene Manton as ``Manton, I.`` (per the traditional LoC record), we refer to her as [``http://id.loc.gov/authorities/names/no96059757``](http://id.loc.gov/authorities/names/no96059757) (LoC URI). This lets us encode data using [semantic web](https://en.wikipedia.org/wiki/Semantic_Web) technology, and (we all hope) [makes it possible](http://linkeddata.org/) for computers to more precisely and efficiently integrate data from a variety of sources.

## Parallel universes: the problem of multiple authorities

An authority system provides a shared precise vocabulary for concepts. If two projects use the same authority system, their data is much more readily interoperable. If two projects use different authority systems, however, then there is no clear procedure for integrating their data. That's a pretty big bummer.

In the United States, the Library of Congress name authority contains entries for the authors of all of the books in its index. Meanwhile, in Germany, the [Deutsche National Bibliothek](http://www.dnb.de/) maintains a similar name authority. The [National Library Board in Singapore](https://www.nlb.gov.sg/) has one, too. They're all over the place. Each authority system contains quite a few unique records, but also a substantial number of records that overlap with other authority systems.

[In the late '90s](https://www.oclc.org/viaf/history.en.html) the LoC and the DNB started trying to "align" their authority records -- that is, to create a mapping so that someone searching for works by Darwin, for example, in databases that used the LoC authority could also find information in databases that used the DNB authority. This led to a project called the [Virtual Internet Authority File (VIAF)](http://viaf.org), which now contains consolidated records from around 45 authority different systems.

The basic idea is that instead of using URIs from those "local" authority systems, clients can use URIs issued by VIAF instead. We now can refer to Irene Manton as [``http://viaf.org/viaf/26661923``](http://viaf.org/viaf/26661923), and by extension refer to entries in (in this case) seven different local authority systems.

I'll refer to this approach as **"hierarchical alignment"** -- records from several contributing authority systems are aggregated into consolidated records that are supposed to be a more general substitute for the original records.

## Local dialects & scalability

VIAF is an extremely powerful tool, and for those of us working in digital history and philosophy of science it is the no-brainer go-to authority service. But its coverage is only as good as the authority systems that contribute to it. In historical research projects we often deal with historical actors who do not appear in VIAF.

What then? Well, we create our own.

[Here](http://situatingchemistry.org/node/974) is an entry for Claude Louis Berthollet, an 18th-century French chemist, in the the [Situating Chemistry](http://situatingchemistry.org/) project. [Here](https://data.isiscb.org/isis/authority/CBA000008489/) is an entry for Berthollet in the [Isis Bibliography for the History of Science](https://data.isiscb.org/). For digital history and philosophy of science projects in at Arizona State University, we use an in-house system called [Conceptpower](https://chps.asu.edu/conceptpower). The [Genecology Project](http://www.genecology.org), [VogonWeb](http://www.vogonweb.net), and many other projects use that platform.

Now suppose I want to integrate data from the Genecology Project with data from Situating Chemistry.

In many cases, entries in Conceptpower (used by the Genecology Project) and Situation Chemistry have references to VIAF entries -- another layer of hierarchical alignment. This is helpful: for those records with VIAF references, we have a clear mapping from one project to the other. But if contributors to those projects have added entries for the same people in each respective project-generated authority system, and there are no corresponding VIAF entries for those people, I'm stuck.

Well, I have some options, but they're not great options.

1. We could talk VIAF into aggregating our project-generated authority records, so that we can all just use VIAF for everything.
2. We could create our own VIAF-like system that aggregates VIAF and all of our project-generated authority records into combined records. We could then both use identifiers from that new system in our projects. Another layer of **hierarchical alignment**.
3. We could store references to each others' authority records in our respective authority systems. So entries in Conceptpower could store references to entries in Situating Chemistry, and visa-versa. I'll call this **peer-to-peer alignment**.

Option 1 is unlikely: there is a potentially limitless number of domain-specific projects out there, and the chance of us all talking OCLC into aggregating our records is slim.

The big problem with both options 2 and 3 is that they aren't very scalable. Suppose that a third project comes along. Perhaps option 2 (more hierarchical alignment) would have us grow our home-grown-VIAF-like solution; can we keep that up for four, five, fifty, N projects? Meanwhile, how many other home-grown-VIAF-like systems are out there? Do we need another layer of aggregation on top of those aggregators? How many layers of aggregation emerge? Do we all change the identifiers in our datasets every time a new layer of aggregation is added? What if one of those layers breaks?

On the other hand, option 3 (peer-to-peer alignment) gets out of control even more quickly: if I want to make my dataset maximally interoperable, than I would need to store references to corresponding records in every authority system that I encounter. This leads to a situation in which we are all replicating everyone else's authority systems over and over again.

## Wish-list

What I really want is...

1. to be able to use existing authority records where ever possible,
2. but also be able to generate my own records as needed,
3. keep track of mappings between the authority records that I generate and those generated by other projects,
4. and easily discover mappings to other authority records in other projects,
5. all while avoiding the runaway maintenance overhead of hierarchical or peer-to-peer alignment models.

As a research community, we want our old datasets to remain fully usable. And we want to be able to leverage the mappings between pairs of projects for alignment across a whole bunch of datasets.

So, how do we do that?

## A proposal for a non-hierarchical alignment system

### The basic idea

In a non-hierarchical alignment, I would continue to use whatever authority system(s) meets my needs. When I encounter or infer mappings between records in different authority systems (e.g. I discover that an entry in Conceptpower that I'm currently using has a corresponding entry in Situating Chemistry), I would submit that mapping (we'll call it an **identity**) to a third party whose sole job is to keep track of such things. We'll call that third party an **identity store**.

If John over at Situating Chemistry wants to find out what I've got about people in his dataset, he can ask that identity store for any mappings with authority records that he is using. If Stephen at IsisCB has already gone to the trouble of finding mappings between records in his authority system and the Situating Chemistry authority system, and then decides that he also wants to pull out data that uses entries in Conceptpower, he can get most of those mappings for free just by asking the identity store. If Stephen also discovers a bunch of mappings between IsisCB and VIAF, and reports those to the identity store, then I automatically have access to mappings from Conceptpower to VIAF by way of Situating Chemistry and IsisCB. Glorious.

This is a bit like option 3 (peer-to-peer alignment), above, with a crucial difference: **Neither I, nor John, nor Stephen need to store or maintain any information about those mappings ourselves.** We let the identity store do that. It's also a bit like option 2 (home-grown hierarchical alignment), but with a crucial difference: **Neither I, nor John, nor Stephen need to change the identifiers that we use.** We go on using our own preferred identifiers, and when it comes time to pull in data from somewhere else we can ask our trusty identity store whether it knows about mappings to authority systems that those data sources are using. And, of course, it's not hierarchical: the identity store doesn't propose new authority records to "cover" all of our project-generated authority records -- it just keeps track of identity relations among them.

### Building sideways

One worry is that this system depends on buy-in. That is, it only works for projects that use the same identity store. That's a problem in a lot of cases: if I'm developing a new project, I'm going to want to be pretty sure that the owner of the identity store is committed to keeping it available and functioning. It also seems pretty dangerous to build the system around a single identity store: users are not just consuming data (as in the case of VIAF), but actively generating lots of data (identity relations), and unless the owner of the identity store has quite a bit of money to burn there will be a practical upper bound to the amount of traffic that can be accommodated. There may also be legal, civil, or social constraints on data sharing, such that a project may not be able to contribute identity data to a public resource.

For all of those reasons, we imagine identity stores operating as a **de-centralized federation or network** rather than a central facility. Identity stores should be able to talk to each other: I should be able to set up a local identity store where I can register identity relations, but also pull in identity relations from other public identity stores. If my identity store knows about at least one other identity store, it should be able to find and query other identity stores by traversing the network.

Obviously the system works best when identity stores share as much as possible with each other. But this isn't always possible. For example, I may not have access to a public web server where I can make my identity store available to others, but I should still be able to run an identity store locally and pull in relations from other identity stores. It's also possible that I am not extremely confident about the identity relations that I am creating, and so I may want to prevent other identity stores from storing or cacheing my relations.

It's important to me that I know where identity relations are coming from, so the system needs to keep track of accession data (who created the relations, when), and those data need to be preserved as identity relations get passed around among services. This way I can set limits on whose assertions I want to trust.

## Implementation: Black Goat

Over the last few days I've been mocking up a prototype identity service called [Black Goat](https://diging.github.io/black-goat). It implements the basic identity relation functionality described above, via a simple REST API. The prototype is running at [http://black-goat.herokuapp.com](http://black-goat.herokuapp.com) (this is for testing purposes only -- data won't be preserved here, and should not be considered reliable). Clients must register with the service and obtain an access token before they are able to submit new identity relations. Anyone can query for identity relations by passing an URI ([example](https://black-goat.herokuapp.com/identical/?identifier=http://www.digitalhps.org/concepts/CON1ea962e1-b109-43f7-bff7-67d65adcecc7)).

Another feature that I'm kind of proud of is Black Goat's configuration-driven joint search mechanism. I thought that it would be nice if I could call the same service for both registering identity relations and for finding authority records. Clients can register new authority services via the REST API, and can include a [configuration](https://diging.github.io/black-goat/services.html) that describes how to search and retrieve records from that authority service. Clients can then call a search endpoint that farms out the query to all known authority services and returns a combined result set.

One of the next steps will be implementing the bits that make one Black Goat instance "aware" of other instances, and able to share authority and identity searches.

I'd also like to add a simple graphical interface so that human users can enter identity data directly into the system.
