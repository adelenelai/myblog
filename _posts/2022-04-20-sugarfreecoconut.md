---
title:  "Exploring COCONUT Locally - Part 2 "
---
### Hidden goodies in COCONUT: NP Fingerprints, Sugar-free Forms, NP-Likeness, and more

In [Part 1](https://adelenel.ai/mongodbcoconut/), I showed how to download COCONUT, restore it from its MongoDB dump, then load it in a Jupyter Notebook via PyMongo, ready to be explored.

Now, I will show how to use PyMongo to query the extensive metadata and properties (the 'hidden' goodies) that have been calculated for all natural products (NPs) in COCONUT.

These goodies do not appear on COCONUT Web because, well, there are just too many of them to display.

According to [Maria and coauthors](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-020-00478-9):

> "...the number of the computed properties is quite big (73 fields in each document corresponding to one unique NP), only a selected fraction of them is displayed on the COCONUT web interface."

...hence, this post :)

*NB: full credit to colleagues at the [Steinbeck Group](https://cheminf.uni-jena.de/), in particular Maria Sorokina, Jonas Schaub, Kohulan Rajan, and Christoph Steinbeck for the hard work that went into what I'm about to show below.*

#### What's in COCONUT MongoDB?

Let's start with a basic query of COCONUT using PyMongo to see the range of metadata and properties available. We will query `uniqueNaturalProduct`, the main collection containing unified and curated NPs.

Data in MongoDB are stored as key-value pairs within documents that are organised into collections. Below are all the keys for the first document. Note that keys may be different from document to document. The full list is available in [Sorokina et al](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-020-00478-9/tables/2).

{% gist 67ab42663ff6a80b69df0d2f3de9599c %}


#### Natural Product Fingerprints: PubChem, Circular, Extended

Over time, more pre-calculated fingerprints for COCONUT's NPs became available in COCONUT MongoDB, calculated using built-in CDK libraries. Here's what they look like and how to download them.

{% gist 2b5d23dcf2c40f157075a97e57fcb320 %}

#### Sugarfree Natural Products

Schaub et al. worked extensively on [deglycosylation](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-020-00467-y) of NPs, including analysis of [COCONUT glycosides](https://www.mdpi.com/2218-273X/11/4/486).

In COCONUT, we can query which NPs have what kinds of sugars (*e.g.,* linear, ring), and even download their aglycons (sugar-free forms) as SMILES.

{% gist 19dd01c46df2ebf38fb2f0089f0120ee %}

#### Other goodies in COCONUT worth exploring (not in Web)

* Different NP-likeness scores, already pre-calculated *e.g.*, `npl_score`, `npl_noh_score` and `npl_sugar_score`.
* Ertl FG fragments in SMILES and their frequency - `ertlFunctionalFragments`

#### Update: COCONUT in PubChem
[COCONUT has recently been uploaded to PubChem](https://twitter.com/AdeleneLai/status/1511996631637450761). For now, the metadata I described in this post are not available on PubChem, but I guess more calculated properties and cross-linked data will be available soon.

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fadelenel.ai%2Fsugarfreecoconut%2F&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
