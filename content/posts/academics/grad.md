DNFs: a group of boolean clauses, ie (A and B and ... F) or (G and H ... and I) or ...

Instead of looking at all the boolean clauses, we can just eliminate any boolean clauses that are too long, ie (A and B and ... Z) since the chances that any boolean clause of this length is satisfied is very low.

The error we get from eliminating these long boolean clauses is log(s/e), where s is the size of the DNF and e is the error

