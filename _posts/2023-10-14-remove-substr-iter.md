---
title: "Removing Substructure Matches and Returning Fragments in RDKit"
---
### Some cheminformatics hacking inspired by a reader

A reader of this blog contacted me regarding a previous [post](https://adelenel.ai/deleteonesubstructure) I wrote about how to remove substructure matches, one-by-one, from a molecule.

Back then, 1+ year ago, I wanted to solve the following problem:
> *given a molecule and a pattern that was present in that molecule at multiple sites (i.e., multiple matches of the same substructure), remove one match while leaving the other(s) intact, intended **for subsequent removal**.*

I had solved this problem as part of my PhD research on [classifying families of polymers, a.k.a., homologous series within compound datasets](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-022-00663-y) - an algorithm I called OngLai that was built using the RDKit.

However, Reader came to me with the following question:

> *I have a molecule with a fragment that is matched in several places. I need to build all the structures that can be obtained by removing only one of these matches. As I saw in your example, there is no place to select which match to be deleted. Would it be possible for you to describe if I can perform this task based on your post and how?*

This problem was interesting as it called for more fine-grained control of substructure removal.

Here, one needs to be able to 'select' the specific substructure match, remove it, return the remaining fragment, and repeat (if there are multiple matches).

In comparison, with OngLai, I was interested in removing *all* substructure matches - regardless of which match was removed when. The matches had to be removed one-by-one for my particular use-case so as to conserve the core fragment. (See [here](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-022-00663-y) for more details)

However, the difference now is that for each substructure match, Reader wanted to remove just that match and get the core fragment each time. (And what was nice was that they showed me some code they had attempted too.)

I gave this problem a go one morning and my solution, two RDKit functions, is explained below.

Notably, I could not use the same method as [before](https://adelenel.ai/deleteonesubstructure) where I used dummy molecules to preserve the fragmentation site.

Instead, this time I had to develop a different approach and got to learn about using EditableMol as well as a trick for [removing atoms by atom ID](https://sourceforge.net/p/rdkit/mailman/rdkit-discuss/thread/18CBD7F4-0BCB-46F9-9F8C-77056B91FEC9%40icr.ac.uk/#msg29022394) (the trick is to remove in descending order of ID!). 

A nice way to dip my hands in some cheminformatics again :)

> *Feel free to use the code, I'd be grateful if you could give me a shoutout:*
> Lai, A. (2023). Remove Substructures One-by-one and Return Fragments in RDKit. Zenodo. https://doi.org/10.5281/zenodo.10005395.


{% gist c5e3e771786cc7d2bb222065b5e6a0d9 %}


[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fadelenel.ai%2Fremove-substr-iter%2F&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)



