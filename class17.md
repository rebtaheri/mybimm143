# class17
Rebecca Taheri (Pid : A17228385)

``` r
setwd("/Users/sarahitaheri/Desktop/class17")
getwd() 
```

    [1] "/Users/sarahitaheri/Desktop/class17"

``` r
library(tximport)


# setup the folder and file-names to read
folders <- dir(pattern="SRR21568*")
samples <- sub("_quant", "", folders)
files <- file.path( folders, "abundance.h5" )
names(files) <- samples

txi.kallisto <- tximport(files, type = "kallisto", txOut = TRUE)
```

    1 2 3 4 

``` r
head(txi.kallisto$counts)
```

                    SRR2156848 SRR2156849 SRR2156850 SRR2156851
    ENST00000539570          0          0    0.00000          0
    ENST00000576455          0          0    2.62037          0
    ENST00000510508          0          0    0.00000          0
    ENST00000474471          0          1    1.00000          0
    ENST00000381700          0          0    0.00000          0
    ENST00000445946          0          0    0.00000          0

``` r
colSums(txi.kallisto$counts)
```

    SRR2156848 SRR2156849 SRR2156850 SRR2156851 
       2563611    2600800    2372309    2111474 

``` r
sum(rowSums(txi.kallisto$counts)>0)
```

    [1] 94561

``` r
to.keep <- rowSums(txi.kallisto$counts) > 0
kset.nonzero <- txi.kallisto$counts[to.keep,]
```

``` r
keep2 <- apply(kset.nonzero,1,sd)>0
x <- kset.nonzero[keep2,]
```

``` r
pca <- prcomp(t(x), scale=TRUE)
```

``` r
summary(pca)
```

    Importance of components:
                                PC1      PC2      PC3   PC4
    Standard deviation     183.6379 177.3605 171.3020 1e+00
    Proportion of Variance   0.3568   0.3328   0.3104 1e-05
    Cumulative Proportion    0.3568   0.6895   1.0000 1e+00

``` r
plot(pca$x[,1], pca$x[,2],
     col=c("blue","blue","red","red"),
     xlab="PC1", ylab="PC2", pch=16)
```

![](class17.markdown_strict_files/figure-markdown_strict/unnamed-chunk-9-1.png)

``` r
library(ggplot2)
library(ggrepel)

pca_df <- as.data.frame(pca$x)
pca_df$sample <- rownames(pca_df)
pca_df$color <- c("blue","blue","red","red")
```

``` r
ggplot(pca_df, aes(PC1, PC2)) +
  geom_point(aes(color=color), size=4) +
  geom_text_repel(aes(label=sample, color=color)) +
  scale_color_identity() +
  labs(x="PC1", y="PC2")
```

![](class17.markdown_strict_files/figure-markdown_strict/unnamed-chunk-11-1.png)

``` r
ggplot(pca_df, aes(PC1, PC3)) +
  geom_point(aes(color=color), size=4) +
  geom_text_repel(aes(label=sample, color=color)) +
  scale_color_identity() +
  labs(x="PC1", y="PC3")
```

![](class17.markdown_strict_files/figure-markdown_strict/unnamed-chunk-12-1.png)

``` r
ggplot(pca_df, aes(PC2, PC3)) +
  geom_point(aes(color=color), size=4) +
  geom_text_repel(aes(label=sample, color=color)) +
  scale_color_identity() +
  labs(x="PC2", y="PC3")
```

![](class17.markdown_strict_files/figure-markdown_strict/unnamed-chunk-13-1.png)
