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

# Expected benefits of modular programming

The expected benefits of modular programming fall into three classes: (1) managerial -- development time could be shortened because separate groups would work on each module with little need for communication (and little regret afterward that eree had not been more communication); (2) product flexiibility -- it was hoped that it would be possible to make quite drastic changes or improvements in one module without changing others; (3) comprehensibility -- it was hoped that the system could be studied a module at a time with the result that the whole system could be better designed because it was better understood.

# What is a "modularization"?

In the sequel I give several partial system descriptions called "modularizations". In this contexst "module" is best considered to be a work assignment unit rather than a subprogram. The modularizations are intended to describe the deisgn decisions which must be made **before** the work on independent modules can begin. Although quite different decisions are made in each alternative, in all cases the intention is to describe all "system level" decisions (i.e., decisions which affect more than one module).

# Example system 1: a KWIC index production system

For those who may not know what a KWIC index is the following description will suffice for this paper. The KWIC index system accepts an ordered set of lines, each line is an ordered set of characters. Any line may be "circularly shifted" by repeatedly removing the first word and adding it to the end of the line. The KWIC index system outputs a listing of all circular shifts of all lines in alphabetical order. This is a samll sytem. Except under extreme circumstances (huge database, no supporting software), such a system could be produced by a good programmer within a week or two. Consequently it is a poor example in the none of the reasons motivating modular programming are important for this sytem. Because it is impractical to treat a large system thoroughly, we shall go through the eexercise of treating this problem as if it were a large project. We give two modularizations. One, we fell, typifies current projects; the other has been used successfully in an undergraduate class project.

## Modularization 1

We see the following modules:

**Module 1**: **Input**: This module contains a single main program which reads the data lines from the input medium and stores them in core for processing by the remaining modules. In core the characters are packed four to a word, and an otherwise unused character is used to indicate end of a word. An index is kept to show the starting address of each line.

**Module 2**: **Circular Shift**: This module is called after the input module as completed its work. Rather than store all of the circulaar shifts in core, it prepares an index which gives the address of the first character of each circular shift, and the original index of the line in the array made up by module 1. It leaves its output in core with words in pairs (original line number, starting address).

**Module 3**: **Alphabetizing**: This module takes as input the arrays produced by modules 1 and 2. It produces an array in the same format as that produced by module 2. In this case, however, the circular shifts are listed in another order (alphabetically).

**Module 4**: **Output**: Using the arrays produced by module 3 and module 1, this module produces a nicely formatted output listing all of the circular shifts. In a sophisticated system, the actual start of each line will be marked, pointers to further information may be inserted, the start of the circular shift may actually not be the first word in the line, etc., etc.

**Module 5**: **Master Control**: This module does little more than control t he sequencing among the other four modules. It may also handle error messages, space allocation, etc.

It should be clear that the above does not constitute a definitive document. Much more information would have to be supplied before work could start. The defining documents would include a number of pictures showing core formats, pointer conventions, calling conventions, etc., etc. Only when all of the interfaces between the four modules had been specified could work really begin.

This is a modularization in the sense menat by all proponents of modular programming. The system is divided into a number of relatively independent modules with well defined interfaces; each one is small enought and simple enough to be thoroughly understood and well programmed. Experiments on a small scale indicate that this is approximately the decomposition which would be proposed by most programmers for the task specified. Figure 1 gives a picture of the structure of the system.

Figure 1

Structure of KWIC index decomposition 1

![figure1](./figure1.drawio.svg)

## Modularization 2

We see the following modules:

**Module 1**: **Line Storage**: This module consists of a number of functions each one of which is given a precise specification in Figure 2. By calling these functions one may add a character to the end of the last word in the last line, start a new word, or start a new line. One may call other functions to find  the kth character of the kth word in the jth line. Other routines in this module may be called to reveal the number of lines, the number of words in a line, or the number of characters in any word. A precise definition of this module is  given in Figure 2. The method of specification haas been explained in [3].

**Module 2**: **Input**: This module reads the original lines from the input media **and calls the Line Storage module to have them stored internally**.

**Module 3**: **Circular Shifter**: This module contains a number of functions. CSSTUP cause the others to have defined values. The others are intended to be analogue of  the information giving functions in module 1. Using them one may refer to the kth character of jth word of  the ith circular shift, as well as getting the lengths of lines and words, etc. This is shown in Figure 3.

**Module 4**: **Alphabetizer**: This module consists principally of two functions. One, ALPH, must be called before the others will have a defined value. The second, ITH, will serve as an index. ITH(i) will give the index of the circular shift which comes ith in the alphabetical ordering. Formal definitions of these functions are given in Figure 4.

**Module 5**: **Output**: This module will give the desired printing of any circular shift. It calls upon Circular Shift functions.

**Module 6**: **Master Control**: Similar in function to the modularization above.

Figure 2

Definition of a "Line Storage" Module

**Introduction**: This definition specifies a mechanism which may be used to hold up to p1 line, each line consisting of up to p2 words, and each word may be up to p3 characters.

```
Function WORD
  Possible values: integers
  initial values: undefined
  parameters: l,w,c all integer
  effect:
    call ERLWEL* if 1 < 1 or 1 > p1
    call ERLWNL if 1 > LINES
    call ERLWEW if w < 1 or w > p2
    call ERLWNW if w > WORDS(1)
    call ERLWEC if c < 1 or c > p3
    call ERLWNC if c > CHARS(1, w)
    
---
* The routine named are to be written by the user of the module. The call
informs the user that he has violated a restriction on the module; the sub-
routine should contain his recovery instructions [3].
```

```
Function SETWRD
  possible values: none
  initial values: not applicable
  parameters: l,w,c,d all integers
  effect:
    call ERLSLE if l < 1 or l > p1
    call ERLSBL if l > 'LINES' + 1
    call ERLSBL if 1 < 'LINES'
    call ERLSWE if w < 1 or w > p2
    call ERLSBW if w > 'WORDS'(1) + 1
    call ERLSBW if w < 'WORDS'(1)
    call ERLSCE if c < 1 or c > p3
    if l = 'LINES' + 1 then LINES = 'LINES' + 1
    if w = 'WORD'(1) + 1 then WORDS(1) = w
    CHARS(1,w) = c
    WORD(1,w,c) = d
```

# Comparison of the Two Modularizations 

Both schemes will work. The first is quite conventional; the second has been used successfully in a class project [7]. Both will reduce the programming to the relatively independent programming of a number of small, manageable, programs. We must, however, look more deeply in order to investigate the progress we have made towards the stated goals. 

I must emphasize the fact that in the two decompositions I **may not** have changed any representations or methods. It is my intention to talk about two different ways of cutting up what **may** be the same object. A system built according to decomposition 1 could conceivably be identical after assembly to one built according to decomposition 2. The differences between the two systems are in the way that they are divided into modules, the definitions of those modules, the work assignments, the interfaces, etc. The algorithms used in both cases **might** be identical. I claim that the systems are substantially different even if identical in the runnable representation. This is possible becuase the runnable representation is used only for running; other representations are used for changing, documenting, understanding, etc. In those other representations the two systems will not be identical. 

## (1) Changeability. 

There are a number of design decisions which are questionable and likely to change under many circumstances. A partial list: 

1. Input format. 
1. The decision to have all lines stored in core. For large indices it may prove inconvenient or impractical to keep all of the lines in core at any one time. 
1. The decision to pack the characters four to a word. In cases where we are working with small indices it may prove undesirable to pack the characters, time will be saved by a
character per word layout. In other cases, we may pack, but in different formats. 


