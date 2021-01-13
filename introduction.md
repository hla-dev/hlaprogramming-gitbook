# Introduction

This is a book about writing code against the C++ and Java APIs defined in the
High Level Architecture (HLA) Interface Specification. This book does not
attempt to teach you about the HLA, nor about distributed simulation, nor
federation design. It is purely focussed on providing some example code for
working with the available programming language APIs.

The chapters of this book progressively introduce the APIs as you might
encounter them when writing a federate. The chapters are presented per the
following template:

- Overview
- Pseudocode
- C++ code
- Java code
- API discussion

The core chapter of this book only present C++ and Java code since these are the
only languages that have defined APIs witihn the HLA Interface Specification. I
am interested in developing APIs in other languages and appendices for each
language will appear in due course.

To actually experience the code in this book you will need software that
actually implements the APIs. This software is known as a Runtime Infrastructure
(RTI) and there are a number of commmercial (with free trial versions) and free
and/or open source options available:

**Commercial RTIs**

- [MAK RTI](https://www.mak.com/products/link/mak-rti)
- [Pitch RTI](http://pitchtechnologies.com/prti/)

**FOSS RTIs**

- [CERTI](https://savannah.nongnu.org/projects/certi)
- [Portico](https://github.com/openlvc/portico)

TODO: add some instructions on getting up and running with one (or more) of
these RTIs.

TODO: Provide some context --- federation, federate, RTI, Ambassadors, Object
Model, User Code --- and restate that this book focusses on the Ambassador
interfaces.
