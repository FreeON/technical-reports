---
title: Technical Report 1
---

# Manifesto

A majority use of supercomputers involves the broad application of electronic
structure, as many of the most interesting and important problems in
chemistry, biology and materials science can only be answered with non-trivial
quantum theories.  In addition to scalable parallelism, new opportunities for
quantum simulation have developed over the last two decades;  based on the
locality principle of electronic matter, modern electronic structure codes
have been able to achieve dramatically reduced computational complexities,
e.g.  from $$O(N^4) \rightarrow O(N)$$.   Also, multi-decade efforts into
theory of the single-determinant Kohn-Sham representation are beginning to
convincingly yield high quality solutions for the non-local static
correlation, based on the long-range Fock exchange; these solutions are
apparent in  range-separated exchange {% cite Scuseria2006 %}, and in
correlation on top of exchange methods\*.  These long-ranged effects are key
to understanding the chemistry and physics of metal oxide systems, from the
biological reaction center to engineered perovskite systems in the materials
genome.   Achieving scalable parallelism, into the high throughput, strong
scaling regime, obtaining reduced $$O(N^4) \rightarrow O(N)$$ complexities and
enabling transformative, high quality solutions to large problems involving
long range correlation effects is a grail-level enterprise.  We are writing to
propose some enabling principles & technologies that might underpin such an
endeavor.  Since our first contributions\*, the FreeON project has targeted
methods based primarily on the Fock exchange;  in the mid 90's, we introduced
the first $$O(N)$$ method for the two-electron integral (ERI) problem in
Hartree-Fock theory\*.  Later, we were the first to develop Gamma-point
methods for the Fock exchange\*,  the first to demonstrate an $$O(N)$$ solve
of the Coupled-Perturbed Hartree-Fock (CPSCF) static response problem\*,
through fourth order in the dielectric response\*, and also the first to
achieve an $$O(N)$$ solution for the Time-Dependent Hartree-Fock (TD-SCF/RPA)
matrix eigenvalue problem\*.

The FreeON project\* has been through many innovation cycles involving the
interdependence of five and more coupled solvers, demonstrating to us that
localized solvers optimization often inhibits the overall progress of the
computational ecosystem;  fast-solvers should interact holistically to give
the best overall performance in a science per watt (S/W) sense, the
corresponding lowest overall error,  provide a low barrier to entry, and
enable new and integrative science through evolution and extension.  Instead,
current solver ecosystems are an often a competing nest of impeding data
structures, perhaps having arisen with the bit-by-bit development and
optimization of individual capabilities.

Perhaps the most entrenched example of entangled and competing data structures
in modern electronic structure is the row-column framework embedded in
conventional sparse matrix formats\*, including for example the BCSR & DBCSR
formats introduced by us, together with a first described parallel sparse
matrix-matrix multiply\*.   In addition to uncontrolled matrix approximations,
e.g. truncation\*, row-column imposes one-dimensional lists-of-lists
structures on the computational kernels that employ them\*.  This
one-dimensional restriction limits flexibility catastrophically in domain
decomposition\*, preventing access of $$O(N)$$ methods to the strong scaling
limit\*.  Perhaps most significantly though, the ability to fully exploit
quantum locality through culling and occlusion is limited by heavyweight
overheads of the data structures.

The now universal Almlöf-Ahlrichs direct-SCF ERI screening protocol\* finds
only the pair-wise $$O(N^2)$$ most significant naive shell-pair interactions
(occlusion), avoiding the corresponding ERI cost (culling).   The granularity
imposed by the underlying symmetry and linear algebra was, at the time,
related to the extra x in the commonly given $$O(N^{2.x})$$ of that period\*.
We were the first to show that it is the $$O(N^2)$$ complexity of occlusion in
the direct SCF that dominates the exchange component\*, with the culled
integral cost coming in as expected at an $$O(N)$$ cost\*.  These skipout-list
methods\* remain deeply entangled with the row-column structure of the
underlying sparse matrix formats,  the matrix truncation approximation itself,
and still inefficient (and often non-rigorous) methods of occlusion due to
symmetry operations, blocking & etc.

After multiple revisions, we've achieved an innovative milestone with the
strong prototyping of all five SCF solvers within the recursive $$n$$-body
framework\*.   This framework is characterized by recursive task parallelism
and empowered by common, high performance runtime stacks, e.g. charm++ and
OpenMP 4.0, with fine levels of granularity enabled by light weight and
efficient scheduling\*.  The  generacity of the $$n$$-body framework has the
potential to simplify code bases, lower barriers to entry,  and otherwise
enable integrative scientific pursuits.  The $$n$$-body solver is based on
computation that is foremost a data science problem; we are interested in high
quality database solutions to the opportunities provided by locality
principles, including the optimization of data and task flow at the ecosystems
level, and enabling optimal queries that underly algorithms for occlusion and
culling, and for related technologies that enjoy kernel compression.  Good
spatial and temporal locality enhances fast kernel summation\*, distributed
and persistence load balancing\*,  caching\*, propagation of auxiliary data
channels\*, multi-time scale methods including tree-based averages\*, parallel
in time methods\* and the like.  $$N$$-body frameworks offer tree-based kernel
reductions related to data science problems encountering intense market
pressures, such as MapReduce {% cite apache:hadoop %} and spark {% cite apache:spark %}.   We are interested in the
alignment with commodity analytics because it represents a collaborative
groundswell that cannot be matched in managed environments, and because it
provides a flexible orientation for evolution within a rapidly changing
hardware environment, marked by much higher levels of concurrency at lower
levels of quality\*.   Also,  it aligns powerful new methods for linear
algebra\* with fast solvers for learning and classification\*.  In electronic
structure, these solvers have demonstrated the ability to achieve $$O(N)$$
scaling for the computation of ill-conditioned matrix inverses\*, where
truncation schemes fail impressively\*, and also access to strong parallel
scaling in the $$O(N)$$ regime, so far with access up to 500 cores/atom {% cite Bock2014 %}.

This is a coincidence of remarkable new developments:  (1) in DFT, the ability
of single determinant representations to capture strong, many center
correlation effects based on the Fock exchange, (2) in high performance
algorithms able to access the high throughput, strong scaling regime and
$$O(N)$$ complexities for more accurate but ill-conditioned models,
unobtainable with sparse matrix methods,  (3) in the emergence of accelerators
like the Intel KNL, heralding massively MIMD engines towards 100 Flop/Watt,
and (4) in the ability to re-frame solver collectives entirely within the
$$n$$-body model able to absorb massive heterogeneous parallelism.  Each of
these developments is disruptive.  The scientific and computational landscape
is changing so fast, it seems that only collaborative, peer driven development
can hope to keep pace.  We are writing to engage with the interested and like
minded in areas related to these developments, 1-4, as well as broadly in the
physical and data sciences.

It is important to establish veracity, performance and generacity metrics and
regressions for thin, high performance $$n$$-body libraries; we are leading at
github with the spammpack library\* for fast multiplication of matrices with
decay\*.  This sits on top of OpenMP and charm++ {% cite charmpp %} at the moment, and below
solvers for electronic structure, like the $$n$$-body Fock exchange\*.   With
a collaborative approach, we think tremendous predictive power can fit into a
4U form factor over the next decade.  $$N$$-body solvers for the Fock exchange
hole grid will bring high quality, single determinant B13-like solutions to
problems dominated by the physics of long range correlation, including  doped,
derivativized and defect engineered metal oxide slabs (1000-5000 atom systems
in the dilute limit), perhaps economically interesting from the perspective of
materials genomics.

Developing the generacity of $$n$$-body recursion, perhaps with domain
specific languages\*, will enable complex solver collectives that can absorb
large quantities of heterogeneous parallelism with minimal effort.  While we
don't know the precise form this heterogeneous parallelism will take, we
believe that a data-science orientation provides the best opportunities for
exploiting it, for achieving a simplified code base, lower barriers to entry
and otherwise future proofing collaborative developments.

# Bibliography

{% bibliography %}
