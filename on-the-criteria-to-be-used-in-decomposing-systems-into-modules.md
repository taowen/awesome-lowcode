# On the criteria to be used in decomposing systems into modules

D. L. Parnas

Department of Computer Science Carnegie-Mellon University

Pittsburgh, Pa.

August, 1971

This works as supported by the Advanced Research Projects Agency of the Office of the Secretary of Defense (F44620-70-C-0107) and is monitored by the Air Force Office of Scientific Research. This document has been approved for public release and sale; its distribution is unlimited.

# Abstract 

This paper discusses modularization as a mechanism for improving the flexibility and comprehensibility of a system while allowing the shortening of its development time. The effectiveness of a "modularization" is dependent upon the criteria used in dividing the system into modules. Two system design problems are presented, and for each, both conventional and unconventional decomposition are described. It is shown that the unconventional decompositions have distinct advantages for the goals outlined. The criteria used in arriving at the decompositions are discussed. The unconventional decomposition, if implemented with the conventional assumption that a module consists of one or more subroutines, will be less efficient in most cases. An alternative approach to implementation which does not have this effect is sketched.

# Introduction

If programmers sang hymns, some of the most popular would be hymns in praise of modular programming. A lucid statement of this philosophy is to be found in a new textbook on the design of system programs which we quote below:

> "A well-defined segmentation of the project effort ensures system modularity. Each task forms a separate, distinct program module. At implementation time each module and its inputs and outputs are well-defined, there is no confusion in the intended interface with other system modules. At checkout time the integrity of the module is tested independently; there are few scheduling problems in synchronizing the completion of several tasks before checkout can begin. Finally, the system is maintained in modular fashion; system errors and deficiencies can be traced to specific system modules, thus limiting the scope of detailed error searching."[1, paragraph 10.23]*

I must begin by saying that I am in complete agreement with this statement though I might not agree with some possible interpretations. Note, however, that nothing is said about the criteria to use in dividing the system into modules. Because the decision to divide a system into n modules of a given size does not determine the decomposition, this paper will discuss that issue and, by means of examples, suggest the type of criteria that should be used in decomposing the system into modules.

# A brief status report

The major progress in the area of modular programming has been the development of coding techniques and assemblers which (1) allow one module to be written with little knowledge of the **code** used in another module and, (2) allow modules to be reassembled and replaced without reassembly of the whole system. This facility is extremely valuable for the production of large pieces of code, but its use has not resulted in the expected benefits. In fact, the system most often used as examples of the problems involved in producing large systems are themselves highly modularized programs which make use of the sophisticated coding and assembly techniques mentioned above.

## Expected benefits of modular programming

The expected benefits of modular programming fall into three classes: (1) managerial -- development time could be shortened because separate groups would work on each module with little need for communication (and little regret afterward that eree had not been more communication); (2) product flexiibility -- it was hoped that it would be possible to make quite drastic changes or improvements in one module without changing others; (3) comprehensibility -- it was hoped that the system could be studied a module at a time with the result that the whole system could be better designed because it was better understood.

