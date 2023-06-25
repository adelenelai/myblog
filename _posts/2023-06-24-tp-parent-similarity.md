---
title:  "Cheminformatics for Risk Assessment of Transformation Products"
---
## MCS Tanimoto Similarity, 6000+ Transformations, Resistance and Ecological Risk Assessment

*I'm back after a hiatus, and by hiatus I mean finishing my PhD (December 2022). I now work as a software developer at an environmental analytics company here in Luxembourg. More about that later.*

I came across an interesting [review](https://doi.org/10.1021/acs.est.2c09854) published just 6 days ago related to the **risk assessment of antimicrobials** in Environmental Science & Technology. This line in the abstract caught my eye: *we propose evaluation of structural similarity between parent compounds and TPs for TP risk assessment*.

Some environmental chemistry context: there is concern about increasing antimicrobial resistance worldwide, as it poses threats to the health of humans, animals, and the environment. Antimicrobial resistance (e.g., to antibiotics) has been growing because their widespread use, dating back to the discovery of penicilin in the 50s.

However, Löffler et al., focus not on antimicrobial compounds themselves, but rather their **transformation products (TPs)**. 

TPs are compounds that result from environmental transformations of so-called parent compounds, such as biodegradation and photolysis; basically, chemical reactions that happen when the (parent) compound enters the environment. (If you come from metabolomics, basically think of them as metabolites.)

The chemical structures of many TPs are still unknown, let alone identifiable in the environment, which is concerning specially since some of the TPs that *are* known have been demonstrated to be more bioactive (potentially toxic) than their parent compounds. 

Furthermore, in the context of antimicrobials, many conventional wastewater treatment plants have been ineffective in removing antimicrobial residues. Some may even produce "treatment TPs" that are discharged into aquatic environments, in addition to other TPs that would form in the environment.

In short - **many possible antimicrobial TPs may be present the environment, whose ecotoxicological risks may be hard to estimate or quantify**. Scientifically, a very important (and interesting!) topic, tackled in the review.

As I worked briefly on TPs during my PhD - trying to [curate them for PubChem to make TP data more FAIR](https://zenodo.org/record/7838005), and supervised an [internship](https://adelenel.ai/mentoring/) on this topic - I was naturally curious about the **data and cheminformatics approach** the authors used in their review. 

In a nutshell, the authors calculated molecular similarity for TPs and their parent compounds in order to **estimate various Risk Quotients and EC<sub>50</sub>s for risk assessment of antimicrobial resistance and ecological risk assessment**. 

I think many people are familiar with ECsub>50</sub>, so I'll briefly explain Risk Quotients. 

Risk Quotients are calculated by dividing the Measured Environmental Concentration (exposure level) by Predicted No Effect Concentrations (acceptable effect level) of a chemical. The value of a compound's Risk Quotient can have implications for chemical prioritisation and possible risk mitigation measures such as restriction of the compound.

## Chemical Data: Parents and Transformation Products
Löffler et al. worked with a total of 56 TPs manually curated from the literature. They list these TP compounds in their [Supplementary Table S1 and S2](https://pubs.acs.org/doi/suppl/10.1021/acs.est.2c09854/suppl_file/es2c09854_si_002.xlsx) with plenty of details on provenance and quantified concentrations in the environment - a huge feat in itself. 

However, as I wanted to quickly spin up some cheminformatics analysis, I think the way they conveyed this information could be improved in the future. (I wrote about this topic 2+ years ago [here](https://adelenel.ai/communicatingenvchem/).)


### Cheminformatics Approach: MCS Tanimoto Similarity between Parents and TPs
The authors calculated the **structural similarity between TPs and their respective parent compounds**, then classified them as either 'similar' or 'dissimilar' according to similarity score thresholds. In their approach, whether a parent-TP pair is similar or dissimilar would affect their Risk Assessment results (see below).

Löffler et al. calculated 2D similarity using the **MCS Tanimoto method** in ChemMine's [Similarity Workbench](https://chemminetools.ucr.edu/similarity/). 

MCS stands for Maximum Common Substructure, which is a well-known concept in cheminformatics. The name is relatively self-explanatory, but more formally, if you treat a molecule and its atoms as a graph made of edges (bonds) and nodes (atoms), the MCS is the largest common subgraph across the graphs of the query and target molecules. There are many different ways (algorithms) to calculate MCS implemented in various cheminformatics toolkits.

I had never heard of MCS Tanimoto before and was curious, especially since the original [ChemMine NAR paper](https://doi.org/10.1093/nar/gkr320) describes the underlying MCS algirithm as *often provid(ing) the most accurate and sensitive similarity measure, especially for compounds with large size differences*.

Inevitably, I went to the ChemMine [source code](https://github.com/girke-lab/chemminetools/blob/1896c2dd7362f44193528aef028390543a125921/similarity/funcs.py#L88), that also led me to the [vignette](https://www.bioconductor.org/packages/devel/bioc/vignettes/fmcsR/inst/doc/fmcsR.html#52_Compute_MCS) of the underlying fmcsR package. From what I understood, ChemMine's MCS Tanimoto is:

> $c / (a+b-c)$
>
> where c =  number of atoms in the MCS
>
> a = number of atoms of the smaller molecule (a.k.a. the query, presumably the TP), and
>
> b = number of atoms of larger molecule (a.k.a. the target, presumably the parent compound).

I won't get into the cheminformatics weeds of this for now, but might blog about it more in upcoming posts. 


### Chemical Similarity in Antimicrobial Resistance and Ecological Risk Assessment
In the review, TP-parent pairs that have MCS Tanimoto >0.95 are classified as 'similar', and <0.95 as 'dissimilar'. These classifications have implications for calculating:

1. the **Risk Quotient of Antimicrobial Resistance (RQ<sub>AMR</sub>)**: if a TP-parent pair is dissimilar, the Risk Quotient of the TP is calculated as being 10 times lower than that of a TP whose parent is deemed similar - "a lower effect potency was assumed". See the formula [here](https://pubs.acs.org/doi/10.1021/acs.est.2c09854?goto=supporting-info#eq2). 

2. **50% Effect Concentrations (EC<sub>50</sub>) and the Toxic Ratio in Ecological Risk Assessment**: the Toxic Ratio of a chemical, which is a quotient of baseline and experimental EC<sub>50</sub>s , describes its cytotoxicity relative to its baseline toxicity. Toxic Ratios more than or equal to 10 typically indicate specific toxicity. In the review, a factor of 10 is applied to the EC<sub>50,specific</sub> values of dissimilar TPs compared to similar TPs. 

The main takeaway here is:

> *how* molecular similarity is calculated affects whether a TP is deemed similar/dissimilar to its parent, which in turn affects Antimicrobial Resistance and Ecological Risk Assessment

In other words, the cheminformatics method used for calculating Similarity between TP and parent molecules matters (idea for another post?).


### First Steps in Risk Assessment of more Transformation Products
In an effort to try to further Risk Assessment of these compounds, I asked myself if and how we could apply some of these concepts to more TPs (assuming the applicability domains of these concepts extend beyond antimicrobials and can be applied to other chemicals in general).

In Section 2.2 of the paper, Löffler et al. mention that the threshold for similar/dissimilar classification is 0.95 (MCS Tanimoto Similarity) - "these values agree with the known activity loss of beta-lactam TPs via ring opening". 

I'd be curious to understand this a little better, but in general in cheminformatics, I don't think there is a widely accepted cutoff for 'traditional' Tanimoto similarity, especially because there are so many ways to calculate chemical fingerprints.

Nevertheless, for the purpose of this post, I wanted to demonstrate some **first steps towards applying Transformation Product Risk Assessment on a larger dataset of TPs**. 

More concretely, I will classify TPs as similar/dissimilar to their parents according to Löffler et al's 0.95 threshold, albeit using a different similarity metric. Below, I'll show how I downloaded, processed, calculated similarity, and classified over 6500 parent-TP pairs.

First, I downloaded the latest version of [Transformations in PubChem](https://doi.org/10.5281/zenodo.7838005), specifically the 'wExtraInfo.csv' version (April 2023) which currently has 6554 entries i.e., documented transformations. Importantly, each transformation has structural info on the parent and transformation product molecules.

After some preliminary data exploration, I then calculated Molecular Similarity using RDKit. As far as I know, RDKit does not have a built-in MCS Tanimoto similarity function identical to ChemMine's, so I used . 

(Note that unlike ChemMine's method that calculates Tanimoto based on MCS, this approach first calculates molecular fingerprints, then does the Tanimoto calculation using the equation above on the numbers of 'on' bits.)
 
Lastly, I classify TPs as being either similar or dissimilar to their parents using the same threshold of 0.95 described in Löffler et al. Ideally, I would evaluate the similarity of a beta lactam and its TP upon ring opening and use that as a benchmark for each different Similarity method, but I'll keep it simple for now.

Feel free to use the code, I'd be grateful if you could attribute me using this link: <LINK>.

And if you'd like to share or cite this post, here's a stable DOI: <LINK>. 

Let's discuss this further! Please leave a comment or send me an email :)






### Further reading
1. [Risk Assessment]((https://doi.org/10.2166/9781789061987) )
2. On MCS Tanimoto, see this [issue on Chemmine Github](https://github.com/girke-lab/chemminetools/issues/172), [Cao et al.](https://doi.org/10.1093/bioinformatics/btn186),[Zhang et al](https://doi.org/10.1007/s10822-015-9872-1), and this discussion in the RDKit community [here](https://github.com/rdkit/rdkit/discussions/6265). 
3. [RDKit Blog](https://greglandrum.github.io/rdkit-blog/) - full of awesome stuff, I learn a lot from it


*A shoutout to Beate Escher and Martin Scheringer, whose class introduced me to Environmental Risk Assessment almost a decade ago, and is a large reason why I went into environmental chemistry :)*












