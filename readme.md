# FitIld

## Fit an intron legth distribution (ILD) to a statistical model

Author: Osamu Gotoh

Contact: Osamu Gotoh <o.gotoh@aist.go.jp>

Last updated: 11/01/2017

## Overview

Intron length distribution (ILD) is a specific feature of a genome. This suit includes four programs related to ILD: **fitild**, **compild**, **decompild**, and **plotild**.  
* Given an experimentally observed _ild_, **fitild** calculates the maximum likelihood estimate of parameters of a statistical model consisting of one, two, or three components of Frechet or lognormal distributions. The degree of fitness is evaluated by three statistics, _residual root mean square error_, _AIC_, and _BIC_. As an option, the fitness can be visually confirmed by a graph displayed in a separate window.  
* **Compild** calculates a distance matrix for a given set of experimental or model ILDs. Typically, the outputs of **fitild** are used as the model ILDs. In each run, one of seven different measures of distance may be chosen.<p>
* **Decompild** decomposes a model ILD into individual components and outputs 1) fractional contribution, 2) mode, 3) median, 4) width (= 3/4 quantile - 1/4 quantile) of each component, and 5) boundary between _i_-th and (_i_+1)-th components.  
* **Plotild** displays one or more experimental and/or model ILDs in a single window. Hence, **plotild** is useful to visually compare ILDs of several species.  

## Install

### Platform and requirement

This suit of programs is currently compiled only with g++ compiler on a Linux system. As outer resources, [Gnu Science Library (GSL)](https://www.gnu.org/software/gsl/) and [libLBFGS](http://www.chokkan.org/software/liblbfgs/) are required. In addition, [Gnuplot](http://www.gnuplot.info/) is assumed to be installed in the system.

### Contents

* bin: binaries
* doc: documents
* perl: perl scripts
* sample: sample ild files
* src: source codes
* table: text files used by programs and scripts
   * gnm2tab: list of genomes with some characteristics
   * IldModel_1.txt: list of one-component Frechet models
   * IldModel_2.txt: list of two-component Frechet models
   * IldModel_3.txt: list of three-component Frechet models

### Compile

1. `% ./configure [--help]`
2. `% make`
3. `% make install`

## ILD file

The input to **fitild** is a simple text file, each line of which should be:

_intron\_length  frequency_

The lines should be numerically sorted on _intron\_length_ in the ascending order. The header line is optional.

## Execution

### **fitild** in one of the two modes
* (A) `% fitild [-N|G] [other options] -d IldModel.txt ild`
* (B) `% fitild [-N|G] [other options] ild initial_parameter_values`

#### options: (default value)
  * `-a`: as is: no optimization is attempted.
  * `-b[1|2]`: BFGS (Broyden-Fletcher-Golodfarb-Shanno) method
  * `-c#`: degree of convergence (1e-4)
  * `-d IldModel.txt`:  list of template ild parameters
  * `-f`: Fletcher-Reeves conjugate method
  * `-g[out]`: graphic output (screen)
  * `-h`: display help
  * `-i`: use template of the same identifier as the input <i>ild</i> file
  * `-k#`: don't use template with sample size less than # (0)
  * `-l#`: lower bound of plot in log_10 (1)
  * `-m`: multiple methods in series (default)
  * `-n[1|2]`: Nelder-Mead simplex method (2)
  * `-p`: Polak-Ribi`ere conjugate method
  * `-r#`: maximum number of iterations (1000)
  * `-s#`: step size in GSL minimization function (1e-2)
  * `-u#`: upper bound of plot in log_10 (6)
  * `-x#`: number of bins (100)
  * `-G`: geometric model (Frechet model)
  * `-L#`: lower bound of intron length (0)
  * `-N`: lognormal model (Frechet model)
  * `-V[#]`: verbose output 
  * `-U#`: upper bound of intron length (inf)

#### _Initial\_parameter\_values_: space-separated numeric values
  * Frechet model: one of the following three ways, where _a_: amplitude, _m_: position, _t_: scale, and _k_: shape parameter.

     * 1.0 _m\_1 t\_1 k\_1_ (one component)
     * _a\_1 m\_1 t\_1 k\_1 m\_2 t\_2 k\_2_ (two components)
     * _a\_1 m\_1 t\_1 k\_1 m\_2 t\_2 k\_2 a\_2 m\_3 t\_3 k\_3_ (three components)

  * lognormal model: one of the following three ways, where a: amplitude, m: position, s: scale parameter.

     * 1.0 _m\_1 s\_1_ (one component)
     * _a\_1 m\_1 s\_1 k\_1 m\_2 s\_2_ (two components)
     * _a\_1 m\_1 s\_1 m\_2 s\_2 a\_2 m\_3 s\_3_ (three components)

  * geometric model: one of the following three ways, where a: amplitude, q: common ratio, d: shift parameter.
     * 1.0 _q\_1 d\_1_ (one component)
     * _a\_1 q\_1 d\_1 q\_1 d\_2 s\_2_ (two components)
     * _a\_1 q\_1 d\_1 q\_2 d\_2 a\_2 q\_3 d\_3_ (three components)

### **compild** in one of the two modes
* (A) `% compild [options] -d IldModel.txt`
* (B) `% compild [options] ild1 ild2 ...`

#### options: (default value)
  * `-c`: cosine distance.
  * `-e`: Euclid distance
  * `-f`: Euclid^2 distance
  * `-j`: Jaccard distance
  * `-k`: Kullback-Leibler distance
  * `-m`: Manhattan distance
  * `-s`: Jensen-Shannon distance
  * `-x#`: maximum intron length
  * `-H#`: minimum frequency
  * `-O`: output each pair in one line
  * `-Q#`: horizontal at # quantile points
  * `-i[a|e|f|g|l|p]`:	input mode (B) only
    * `a`: alternative
    * `e`: every pair (default)
    * `f`: first and others
    * `l`: last and others
    * `p`: alternative

### **decompild** [-N] IldModel.txt

#### options: (default)
  * `-N`: lognormal (Frechet)

### **plotild** [options] [ild1 ild2 ...] [-d IldModel.txt gid1 gid2 ...]

#### options: 
  * `-g out`: write into _out_ (display on screen)
  * `-l#`: lower bound of plot in log_10 (1)
  * `-s dir`: directory of _ild_ files
  * `-u#`: upper bound of plot in log_10 (6)
  * `-x#`: number of bins (100)
  * `-C`: plot cumulative probability (pdf)
  * `-L#`: shortest intron length (0)
  * `-G#`: geometric model (Frechet model)
  * `-N`: lognormal model (Frechet model)
  * `-U#`: longest intron length (inf) 

## Examples

  * `% fitild -g ascasuum.ild 1.0 -408.40 991.29 2.6236`
  * `% fitild -d IldModel_2.txt gallgall.ild`
  * `% compild -k -if acidrich.ild  ascasuum.ild  dicdis.ild`
  * `% compild -H1000 -d IldModel_1.txt`
  * `% decompild IldModel_3.txt`
  * `% plotild -u4 acidrich.ild dictdisc.ild -d IldModel_2.txt acidrich  dictdis`

## Reference

Gotoh, O. "Modeling one thousand intron length distributions" (in preparation).

