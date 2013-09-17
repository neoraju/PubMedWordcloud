PubMed wordcloud
==============================
Examples of how to create a word cloud of the abstract of your publications in PubMed

updated on Mon Sep 16 12:12:38 2013

**PubMedWordcloud** is avaliable on [CRAN](http://cran.r-project.org/web/packages/PubMedWordcloud/index.html)


```r
library(PubMedWordcloud)  # you need to install it first
```


1. retrieve PMIDs from PubMed
----------------------------------
Since my first paper was published in 2007, I will retrieve all PMIDs of my paper from 2007 to this year (2013). I used both 'Yan-Hui Fan' and 'Yanhui Fan' as my name, so I assigned PMIDs for these two names to 'pmid1' and 'pmid2', respectively.


```r
pmid1 = getPMIDs(author = "Yan-Hui Fan", dFrom = 2007, dTo = 2013, n = 10)
pmid1
```

[1] "22698742" "22693232" "22564732" "22301463" "22015308" "21283797"

```r
pmid2 = getPMIDs(author = "Yanhui Fan", dFrom = 2007, dTo = 2013, n = 10)
pmid2
```

[1] "20576513" "19412437"


There are six PMIDs in 'pmid1' and two PMIDs in 'pmid2'. 

2. edit PMIDs
--------------------------------
Note that 'pmid1' and 'pmid2' are vectors, so it is easy to add or delete PMIDs to 'pmid1' and 'pmid2', or combine them. I also write a function to do it, in case you do not want to find out how to do it.   

PMID "22698742" in 'pmid1' and "20576513" in 'pmid2' are published by others (have the same name with me). So I want to exclude them and then combine 'pmid1' and 'pmid2'.   


```r
rm1 = "22698742"
pmids1 = editPMIDs(x = pmid1, y = rm1, method = "exclude")

rm2 = "20576513"
pmids2 = editPMIDs(x = pmid2, y = rm2, method = "exclude")

pmids = editPMIDs(x = pmids1, y = pmids2, method = "add")
```


**Note: only unique PMIDs were kept**

3. download abstracts
------------------------------

```r
abstracts = getAbstracts(pmids)
```


4. clean abstracts
-----------------------------
clean data using paackage {tm}: remove Punctuations, remove Numbers, Translate characters to lower or upper case, remove stopwords, remove user specified words, Stemming words.   


```r
cleanAbs = cleanAbstracts(abstracts)
```


5. plot wordcloud using 'plotWordCloud'
-----------------------------------------

Plot with dafault parameters   

```r
plotWordCloud(cleanAbs)
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6.png) 


Do not rotate words.  

```r
plotWordCloud(cleanAbs, rot.per = 0)
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7.png) 


Plot using other colors.

```r
colors = colSets(type = "Paired")
plotWordCloud(cleanAbs, rot.per = 0, colors = colors)
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-8.png) 


Clean the data with Stemming words is TRUE and plot again.   

```r
cleanAbs2 = cleanAbstracts(abstracts, stemDoc = TRUE)
plotWordCloud(cleanAbs2)
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9.png) 


**Note: ** 'plotWordCloud' uasually will generate a lot of warnings. Many words could not be fit on page. 'pmWordCloud' can adjust the position and plot all words.

6.plot wordcloud using 'pmWordCloud'
---------------------------------------

```r
pmWordCloud(cleanAbs, scale = 0.2)
```

![plot of chunk unnamed-chunk-10](figure/unnamed-chunk-10.png) 



```r
pmWordCloud(cleanAbs, rot.per = 0, scale = 0.2)
```

![plot of chunk unnamed-chunk-11](figure/unnamed-chunk-11.png) 



```r
colors = colSets(type = "Set1")
pmWordCloud(cleanAbs, rot.per = 0, colors = colors, scale = 0.2)
```

![plot of chunk unnamed-chunk-12](figure/unnamed-chunk-12.png) 


Another example plot.    
![plot of chunk unnamed-chunk-13](figure/unnamed-chunk-13.png) 


Only plot 100 words (most frequent words).   
![plot of chunk unnamed-chunk-14](figure/unnamed-chunk-14.png) 


7. References
-----------------
[Shiny Pubmed Word Clouds](http://www.vesnam.com/Rblog/pubmedwordcloud/)    
[wordcloud](http://cran.r-project.org/web/packages/wordcloud/index.html)   
[GOsummaries: Word cloud summaries of GO enrichment analysis](http://cran.r-project.org/web/packages/GOsummaries/index.html)   
[How I used R to create a word cloud, step by step](http://georeferenced.wordpress.com/2013/01/15/rwordcloud/) 

