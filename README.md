### **GE**nome **CO**mpression **Z** - **GECOZ** is a set of various NGS formats and algorithms.

At the moment it contains an efficient FM-Index implementation.  

To build the tool:
```
gecoz/java/> mvn install
```
This will build a **gecoz/java/nova-gecoz/target/gecotools.jar** file.
___
```
gecoz/java/nova-gecoz/target/> java -jar gecotools.jar -h
```
Will get the command line help:
```
gecotools -i file [optional params]

parameters:

-h (--help)           - this help message
-i (--input)          - either *.fa or *.gcz
-o [header][from][to] - depends on the input parameters
                        (*.fa -> *.gcz, *.gcz -> *.fa, *gcz -> *.seq)
-c [header] 'string'  - count string occurrences in the *.gcz file
-s [header] 'string'  - search string in the *.gcz file
-t                    - use n threads
-v [level]            - verbose (default = WARNING)

examples:

>java -Xmx8g -jar gecotools.jar -t 4 -i hg38.fa -o hg38.gcz
>java -jar gecotools.jar -t 4 -i hg38.gcz -o hg38.fasta
>java -jar gecotools.jar -i hg38.gcz -o chr15.seq chr15
>java -jar gecotools.jar -i hg38.gcz -c ATTAACCCATGAAAA
>java -jar gecotools.jar -i hg38.gcz -s ATTAACCCATGAAAA
>java -jar gecotools.jar -i hg38.gcz -s chr11 ATTAACCCATGAAAA
```
___
FASTA file may be compressed with GZIP. This would require more memory (~+3Gb for HG38) as all sequences must be uncompressed first.
Using many threads for genome indexing also requires proportionally more memory.
The most demanding part of indexing is a Suffix Array sorting which requires 5n memory.
Current algorithm merges smaller sequences together, but never exceeds the maximum sequence length.
For the human genome this means that all blocks are not bigger than the length of chromosome 1.
For single thread this means ~ 1.2Gb of memory.
When there is no enough memory, gecotools tries to decrease number of threads, decreasing memory requirements.
Gecotools doesn't use more threads than system actually has.
___
GECOZ has several maven submodules that someone may found useful:
- nova-io - basic input/output classes such as BitInputStream and BitOutputStream
- nova-gzip - pure and fast java GZIP implementation with BGZF support.
- nova-formats - simple BAM file reader. FASTA reader/writer
- nova-algo - FM-Index implementation: Suffix Array, Huffman, Deflate, Huffman Shaped Wavelet Tree.

These libraries can be imported via maven:
```xml
<dependencies>
  <dependency>
    <groupId>es.elixir.bsc.ngs</groupId>
    <artifactId>nova-algo</artifactId>
    <version>0.2-SNAPSHOT</version>
...
<repositories>
  <repository>
    <id>gecoz</id>
    <url>https://raw.github.com/inab/gecoz/maven/</url>
  </repository>
```
