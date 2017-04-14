---
layout: post
title: "Leveraging linked data for non-technical users"
date: 2017-04-13 9:45:02
categories:	Infrastructure BigPicture
short: 	"One of the growing back-eddies in the current data deluge is a family of efforts by scholars in both the sciences and humanities to develop and curate datasets and data-centric platforms that supplement 'big data' with  high-quality localized domain knowledge. Many of those scholars understand and covet the promises of large-scale knowledge management technology like the semantic web, but lack the technical experience to take full advantage. This  technology gap that impedes the ability of scholarly communities to fully benefit from advances in knowledge representation and the growth of large-scale online knowledge-bases. In this post I attempt to sketch the contours of this problems, and organize my thoughts on what to do about it."
---

## Summary
* The concept of the semantic web is being realized in large-scale projects like Google's Knowledge Graph and Wikipedia/DBpedia, rendering a large volume of human knowledge available for a wide range of everyday computer-assisted tasks.
* But the semantic web is also highly relevant for scholarship, especially for **scholarly data curation**.
* Scholarly data curation refers to a wide range of activities in the sciences and humanities that creates intellectual value by developing high-quality datasets from less-structured digital materials. For example:
    * Field- or discipline-specific digital bibliographies, such as [Oxford Bibliographies](http://www.oxfordbibliographies.com/) and [Isis Current Bibliography for the history of science](https://data.isiscb.org);
    * Data integration projects, such as [literature-driven protein interaction databases](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2683745/) and historico-geographical databases like [Situating Chemistry](http://situatingchemistry.org/);
    * Scholarly annotation projects.
* Semantic web technology can solve all kinds of problems for scholarly data curation, but using that technology usually requires a skillset that the average scholar lacks.


When Tim Berners-Lee and co. [introduced the concept of the semantic web in the early 2000s](http://ldc.usb.ve/~yudith/docencia/UCV/ScientificAmerican_FeatureArticle_TheSemanticWeb_May2001.pdf), they imagined a form of knowledge representation that would allow machines to perform complex reasoning and data-shuffling tasks on behalf of their human users, based on comfortably abstract instructions. [That future](https://www.scientificamerican.com/article/semantic-web-in-actio/) is  [here](http://www.theverge.com/2017/4/12/15277278/google-home-burger-king-whopper-ad-campaign): "Ok Google!" my wife will exclaim, ["Who invented the digital computer?"](https://www.google.com/search?q=Who+was+the+inventor+of+the+first+computer&oq=Who+was+the+inventor+of+the+first+computer&aqs=chrome..69i57j69i59.233j0j7&sourceid=chrome&ie=UTF-8#q=Who+invented+the+digital+computer?) A moment later, her phone chirps in a feminine voice that evokes images of [Jeri Ryan in the late '90s](https://en.wikipedia.org/wiki/Seven_of_Nine):

> John Vincent Atanasoff (October 4, 1903 – June 15, 1995) was an American physicist and inventor, best known for being credited with inventing the first electronic digital computer. Atanasoff invented the first electronic digital computer in the 1930s at Iowa State College.

Google now does all of our unit conversions, finds delicious recipes, and sleuths out local events on our behalf. Between darn-good speech recognition, natural language processing, and Google's [semantic knowledge graph](https://ahrefs.com/blog/google-processes-queries-semantic-web-environment/), it almost feels like the computers are beginning to Understand.



As a scholar, I see the semantic web is a potentially powerful tool for dealing with the broader [data deluge](http://www.economist.com/node/15579717) of our era. Quite a bit of research over the past few years has been tied in some way to bibliographic (meta)data: records about the creators and contexts of scientific texts. The enormous volume of bibliographic records now available online (some [public](https://www.ncbi.nlm.nih.gov/pubmed/), some [hoarded by capitalists](https://webofknowledge.com/)) makes it a great time to work with these materials. But as any bibliometrically-inclined scholar will readily volunteer, those data (especially for papers prior to the 1990s) are extremely messy and riddled with ambiguities. For example, author name disambiguation -- figuring out precisely who in the world the name "Joe Bloggs" in a list of authors refers to -- remains a huge problem. My students and I often use semantic technology to develop metadata prior to text text analysis, e.g. by supplementing references to people and institutions with identifiers from [controlled authority services](https://erickpeirson.github.io/infrastructure/projects/2016/11/16/black-goat.html), re-writing metadata relations using widely-used ontologies like [FOAF](http://xmlns.com/foaf/spec/), and representing metadata using the [Resource Description Framework (RDF)](https://www.w3.org/RDF/) data model. This allows us to integrate data that we have already collected about those concepts (of people, places, species, things) from previous work, and re-use our data for different kinds of analysis and online presentation.

To be clear, this form of data enhancement isn't limited to bibliographic metadata. In the [History of the Marine Biological Laboratory](http://history.archives.mbl.edu/) project, for example, we're working to "semanticize" course attendance records from the late 19th century through last year, which will power Google-esque graph queries about the MBL and let us pull in historical data from other sources.


Other projects in history and philosophy of science are beginning to use semantic web technology to support research and presentation efforts. The [Isis Current Bibliography for the history of science](https://data.isiscb.org) (IsisCB)

When those kind of data curation activities are part of a web application development project, the way forward is relatively clear. Although complexities and edge-cases arise, your run-of-the-mill developer can quickly grasp the plethora of open standards and vocabularies for semantic data, and work with open APIs to retrieve and integrate data from wherever (DBPedia, VIAF, etc).



I won't dwell here on the basics of the semantic web, since [Google can tell you all about that](https://www.google.com/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#q=semantic+web). Instead, I want to focus on a problem that Berners-Lee did not (at least in print, that I have found) fully anticipate: how can data-oriented scholars with limited technical expertise effectively contribute to and benefit from the semantic web?
