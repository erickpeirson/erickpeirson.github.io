---
layout: post
title: "arXiv development update: reference extraction & linking"
date: 2017-07-27 17:52:51
categories:	arXiv
short: 	"In addition to tackling big-picture architectural questions, one of
the really fun projects that I've worked on over the past several weeks is
experimenting with extracting cited references from arXiv papers. Today I put
up a blog post that describes what we've done so far, user input, and thoughts
for the future."
---

In addition to tackling big-picture architectural questions, one of the really
fun projects that I've worked on over the past several weeks is experimenting
with extracting cited references from arXiv papers. Today I put up a
[blog post](https://blogs.cornell.edu/arxiv/2017/09/27/development-update-reference-extraction-linking/)
that describes what we've done so far, user input, and thoughts for the future.

> Based on user and stakeholder feedback, extracting cited references from arXiv papers and providing links to those references for readers was identified for development under the 2017 arXiv Roadmap, based on input from the arXiv user survey. We have also heard from our API consumers that access to cited references would be valuable. arXiv already detects arXiv identifiers in cited references (for LaTeX submissions), and converts those identifiers to hyperlinks to the corresponding arXiv paper in the final PDF.

> Over the past several weeks we’ve undertaken an exploratory project focused on reference extraction and possible scenarios for reference linking. This post is a brief snapshot of what we’ve done so far, what we’re hearing from users, and some thoughts about where we go from here.
