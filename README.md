# KnotAli

#### Description:
Software implementation of KnotAli.      
KnotAli is an algorithm for predicting the pseudoknotted secondary structures of RNA using relaxed Hierarchical Folding.

#### Supported OS: 
Linux, macOS


### Installation:  
Requirements: A compiler that supports C++11 standard (tested with g++ version 4.9.0 or higher), Pthreads, and CMake version 3.1 or greater.    

[CMake](https://cmake.org/install/) version 3.1 or greater must be installed in a way that HFold can find it.    
To test if your Mac or Linux system already has CMake, you can type into a terminal:      
```
cmake --version
```
If it does not print a cmake version greater than or equal to 3.1, you will have to install CMake depending on your operating system.

[Linux instructions source](https://geeksww.com/tutorials/operating_systems/linux/installation/downloading_compiling_and_installing_cmake_on_linux.php)

#### Steps for installation   
1. [Download the repository](https://github.com/mateog4712/KnotAli.git) and extract the files onto your system.
2. From a command line in the root directory (where this README.md is) run
```
cmake -H. -Bbuild
cmake --build build
```   
If you need to specify a specific compiler, such as g++, you can instead run something like   
```
cmake -H. -Bbuild -DCMAKE_CXX_COMPILER=g++
cmake --build build
```   
This can be useful if you are getting errors about your compiler not having C++11 features.

After installing you can move the executables wherever you wish, but you should not delete or move the simfold folder, or you must 
recompile the executables. If you move the folders and wish to recompile, you should first delete the created "build" folder 
before recompiling.

Help
========================================

```
Usage: KnotAli[options] [input file]
```

Read input file from cmdline; predict minimum free energy and optimum structure using the time- and space-efficient MFE RNA folding
algorithm for each sequence in the alignment.

```
  -h, --help             Print help and exit
  -V, --version          Print version and exit
  -v, --verbose          Turn on verbose
  -i, --input-type       Specify input file type (CLUSTAL or FASTA, base is FASTA)
  -o, --output-file      Specify output file
```

#### Example:
    Assume you are in the KnotAli directory
    ./build/src/KnotAli myinputfile.txt
    ./build/src/KnotAli -o outputfile.txt myinputfile.afa
    
    Example 1:
    
    
    Let's go through an example from the example folder: align_ex1.fa. The first three sequences from 
    this file are:
    
    >tRNA_tdbR00000184-Asterias_amurensis-7602-Lys-CUU
    --CUUUGAUAAGCUUAUAAUGGCA-AGCAUUAAACUCUUAAUUUAAAUCAAAGUGAUCUCACCACACUAUCAAAGACCA
    >tRNA_tdbR00000189-Rattus_norvegicus-10116-Lys-NUU
    ------CAUUGCGAAGC----UUAGAGCGUUAACCUUUUAAGUUAAAGUUAGAGACAACAAAUCUCCACAAUG--ACCA
    >tRNA_tdbR00000190-Bos_taurus-9913-Lys-NUU
    -CACUAAGAAGC--------UAUAUAGCACUAACCUUUUAAGUUAGAGAUUGAGAGCCAUAUACUCUCCUUGGUGACCA
    
    _(((((((___((____________))__(((((_x_____)))))_____((((_________)))))))))))_xxx (consensus structure)
    
    The consensus structure shown above is the result of the covariation portion of the algorithm. 
    The '_' represents nucleotides that are allowed to pair in the final step. 
    In contrast 'x' represents nucleotides which the covariation deemed unlikely to pair and thus 
    are restricted from pairing. 
    Each '(' and ')' an opening and closing base pair within the structure. The consensus can be 
    seen when using the -v (verbose) command.
    
    After running KnotAli, you will have your output file (in the case of our example: align_sol1.txt) 
    in the format of: 
    
    line 1: Sequence name
    line 2: Sequence
    line 3: Output Structure
    line 4: Output Energy
    
    We give the first three sequences from prior in this format after running: 
    
    >tRNA_tdbR00000184-Asterias_amurensis-7602-Lys-CUU
    CUUUGAUAAGCUUAUAAUGGCAAGCAUUAAACUCUUAAUUUAAAUCAAAGUGAUCUCACCACACUAUCAAAGACCA
    .((.(....[[[......]]].....(((((.......))))).....((((.........)))).).))......
    0.53
    
    >tRNA_tdbR00000189-Rattus_norvegicus-10116-Lys-NUU
    CAUUGCGAAGCUUAGAGCGUUAACCUUUUAAGUUAAAGUUAGAGACAACAAAUCUCCACAAUGACCA
    [[[[[.[..[[[...]]]........(((((.......))))).............].]]]]]....
    -2.38
    
    >tRNA_tdbR00000190-Bos_taurus-9913-Lys-NUU
    CACUAAGAAGCUAUAUAGCACUAACCUUUUAAGUUAGAGAUUGAGAGCCAUAUACUCUCCUUGGUGACCA
    (((((((..[([....])].(((((.......))))).....(((([.......])))))))))))....
    -11.61

    For each sequence, KnotAli gives the minimum free energy and structure given the restricted 
    structure found in the covariation step.
    
    
    Example 2:
    
    
    Let's now go through a CLUSTAL format example. The use of CLUSTAL format can be specified by -i parameter, 
    followed by CLUSTAL. An example of this is:
    
    ./build/src/KnotAli -i CLUSTAL alignment.aln
    
    Our third example alignment is in this such format. When looking at the file, we see that it is written like so:

    
    
    CLUSTAL W multiple sequence alignment


    Arab.thal._AC005275                                          -------------------------------------GUCGAGCGAAGUA
    Arab.thal._AC005662                                          -------------------------------------GUCGAGCUAAGUA
    Arab.thal._AL161496                                          -------------------------------------GUCGAGCUAAGUA
    ...
    
    
    Arab.thal._AC005275                                          CGGGUCCACGGG--------CCGGUUCUGUUGCUGGCAUGUUUCUGGGCU
    Arab.thal._AC005662                                          CGGGUCCACGGG--------CCGGUUCUGUUGUUGGCAUGUUUCUGGGCU
    Arab.thal._AL161496                                          CGGGUCCACGGG--------CCGGUUCUGUUGUUGGCAUGUUUCUGGGCU
    ...
    
    Each sequence has 50 nucleotides of its sequence following the name repeating for all the sequences. This continues 
    for the length of the alignment 50 nucleotides at a time.
    
    We find the consensus structure for this alignment to be 
    ___.________.____________________________________________________________(_____(_____))
    _________________(_(_______________)_________________(__(______)___)__________________)
    _____________________________________(_____________________(_(_________________________
    _____________________________________(_(____)_)_.______________________________________
    _____))____________(________________)_______)__________________________________________
    _____________________________________________________________________
    
    
    Similar to the first example, '(' and ')' denote base pairs within the structure. '_' denote unpaired and unrestricted
    bases while '.' are restricted unpaired bases.
    
    We again show the first three predicted structures by KnotAli within the file. The output format for KnotAli is the same
    regardless of the input format. In both cases, the final predictions are place in a file with FASTA formatting.
    
    >Arab.thal._AC005275
    GUCGAGCGAAGUAACAUGAGCUUGUAACCCAUGUGGGGACAUUAAGAUGGUGGAACACUGGUUCGGGUCCACGGGCCGGUUCUGUUGCUGGCAUGUUUCUGGGCUGCCCAGUCCAAGCUGUGAG
    UAAGACGUGUGUGUCAAGCGAAGGCUUGGCUCAAACGGCUGCUAAAGUUGGAGGGCAAUGCGUGAGGCUGGUUUCACAGAGCAGCGAUUACUUCCCGCUUACAGCAGUGGACGGAUCACAGUUU
    GGCGUCGCUCAGAACCACUAUGGCCUGCUGGUCCGAUCUCAUCUGAACCACCAUUUU
    ....................((([...[[[....]]].....])))[[[[[[[...[[[[[[[[[......]]]]]]]]]...((.((((...((...[[[[[...]]]]].(((((((....(
    (..[[[[....]]]]..))...)))))))..))..)))).))...((.((((((((...((...(((((((((((...((((...(((..((..[[[[[......]]]]][[........]]..
    )).))))))).)))..)))..)))))))..))))..)).)).))...]]]]]]]...
    -50.05

    >Arab.thal._AC005662
    GUCGAGCUAAGUAACAUGAGCUUGUAACCCAUGUGGGGACAUUUAGAUGGUGGAACACUGGUUCGGGUCCACGGGCCGGUUCUGUUGUUGGCAUGUUUCUGGGCUGCCCAGUCCAAGCUGUGAG
    UAAGACGUGUGUGUCAAGCGAAGGCUUGGCUCAAACGGCUUCUAAAGUUGGAGGGUAAUGCGUGAGGCUGGUUUCACAGAGCAGCGACUACUUCCCGCUUACAGCAGUGGACGGAUCACAGUUU
    AGCGUCGCUCAGAACCACUAUGGCCUGCUGGUCCGAUCUCAUAUGAACCACCAUUU
    ...[[[[[..[[..[[[..[[[.[[[[.....[[[[[[[[.[[[[[[....[[[..[[[[[[[[[......]]]]]]]]]]]]((((((((.......[[[[[...]]]]].(((((((....(
    (..[[[[....]]]]..))...))))))).))).))))).]]]]]]]](((..(((.....(((((((((((....((((((...(((..((]]]]]]]]]]]]].]]].]].......]]]]]
    )).)))))))..........))....)))))).....)))))....))).)))...
    -48.5

    >Arab.thal._AL161496
    GUCGAGCUAAGUAACAUGAGCUUGUAACCCAUGUGGGGACAUUUAGAUGGUGGAACACUGGUUCGGGUCCACGGGCCGGUUCUGUUGUUGGCAUGUUUCUGGGCUGCCCAGUCCAAGCUGUGAG
    UAAGACGUGUGUGUCAAGCGAAGGCUUGGCUCAAACGGCUUCUAAAGUUGGAGGGUAAUGCGUGAGGCUGGUUUCACAGAGCAGCGACUACUUCCCGCUUACAGCAGUGGACGGAUCACAGUUU
    AGCGUCGCUCAGAACCACUAUGGCCUGCUGGUCCGAUCUCAUAUGAACCACCAUUUU
    ..[[[[[[..........]]]]]]....[[[[.[[[[....]]]].]]]].[[[..[[[[[[[[[......]]]]]]]]]]]]((((((((.......[[[[[...]]]]].(((((((....(
    (..[[[[....]]]]..))...))))))).))).))))).[[[((((.(((..(((.....(((((((((((....((((((...(((..((..[[[[[......]]]]]..]]].........
    )).)))))))..........))....)))))).....)))))....))).)))))))
    -48.63
#### Example Info:

    The three examples were aligned using the MUSCLE software (doi:10.1186/1471-2105-5-113). The first example: align_ex1.fa was
    compiled of 100 tRNA sequences in FASTA format. The results of running KnotAli on this example can also be found in 
    align_sol1.txt. align_ex2.fa, also in FASTA format, is comprised of 100 RNaseP sequences and its structures are found in 
    align_sol2.txt. Lastly, align_ex3.aln is an example file in CLUSTAL format. It was comprised of 100 SRP sequences, and its 
    structure can found in align_sol3.txt


    
##### References:
    Robert C. Edgar. MUSCLE: a multiple sequence alignment method with reduced time and space complexity. BMC Bioinformatics, 5:113, Aug 2004
    
### To cite:
    The paper can be cited at https://www.doi.org/10.21203/rs.3.rs-700965/v1

    The code can be cited at http://doi.org/10.5281/zenodo.5794719   
    
    



   
