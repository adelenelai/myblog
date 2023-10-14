---
title: "Removing Substructure Matches One-by-One in RDKit"
---
### Some cheminformatics hacking inspired by a reader

A reader of this blog contacted me regarding a [post](https://adelenel.ai/deleteonesubstructure) I wrote 1+ year ago about how to remove substructure matches, one-by-one, from a molecule.

I called this post "Leave Tweedle-Dum alone", after the famously indistinguishable twins in Alice in Wonderland. 

The idea was that if you had a molecule and a pattern that was present in that molecule at multiple sites (i.e., multiple matches of the same substructure), you would be able to remove one match while leaving the other(s) intact **for subsequent removal** - *remove Tweedle-Dee, but leave Tweedle-Dum alone!*

(I had solved this problem as part of my PhD research on [classifying families of polymers, a.k.a., homologous series within compound datasets](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-022-00663-y) - an algorithm I called OngLai that was built using the RDKit.)

Reader came to me with the following question:

> *I have a molecule with a fragment that is matched in several places. I need to build all the structures that can be obtained by removing only one of these matches. As I saw in your example, there is no place to select which match to be deleted. Would it be possible for you to describe if I can perform this task based on your post and how?*

This problem was interesting - Reader wanted even more fine-grained control of substructure removal!

With OngLai, I was interested in the resulting molecular fragment after removing *all* substructure matches. The matches had to be removed one-by-one for my particular use-case, so as to conserve the core fragment. (See the paper for more details)

However, the difference now is that for each substructure match, Reader wants to remove just that match and get the core fragment each time. (And what was nice was that they showed me some code they had attempted too..)

I gave this problem a go one morning and my solution, two RDKit functions, is explained below.

Notably, I could not use the same approach as before, using dummy molecules in datamol.

A nice way to dip my hands in some cheminformatics again :)

*If you happen to use this code, I'd be grateful if you gave me a shout-out, either by mentioning this blog or citing [BLA]*


















[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fadelenel.ai%2Ficcm5%2F&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)




