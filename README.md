
<!-- README.md is generated from README.Rmd. Please edit that file -->

# PPI-Context

Contextualization of protein-protein interaction databases by cell line

#### Clone repository

    $ git clone https://github.com/montilab/ppi-context

#### Install requirements

    $ cd ppi-context
    $ pip install -r requirements.txt

#### The data

If you just want the data it’s easy to load into R…

    $ R

``` r
ppi <- read.delim("data/v_1_00/PPI-Context.txt", header=TRUE, sep="\t", stringsAsFactors=FALSE)
```

``` r
data.frame(sort(table(ppi$cell_name), decreasing=TRUE)) %>%
set_colnames(c("var", "freq")) %>%
head(30) %>%
ggbarplot(x="var", y="freq", fill="freq") +
labs(title="", x="Cell Line Name", y="PPI") +
scale_fill_viridis_c(option="inferno", begin=0, end=0.8) + 
theme(legend.position="none",
      axis.text.x=element_text(angle=45, hjust=1, size=12, face="bold"))
```

<img src="README_files/figure-gfm/unnamed-chunk-3-1.png" style="display: block; margin: auto;" />

#### Pre-processing the data

    | PPI - Context (v1.0)
    usage: ppictx.py [-h] [-r] [-d]
                       [-fh PATH_HIPPIE] 
                       [-fp PATH_PUBTATOR]
                       [-fc PATH_CELLOSAURUS]
    
    optional arguments:
      -h, --help            show this help message and exit
      -r, --run             run pipeline
      -d, --download        download raw data first
      -fh PATH_HIPPIE       path to downloaded Hippie data (optional)
      -fp PATH_PUBTATOR     path to downloaded Pubtator data (optional)
      -fc PATH_CELLOSAURUS  path to downloaded Cellosaurus data (optional)

In most cases you will need to download the latest bulk data first and
then process it…

``` bash
$ python ppictx.py --download --run
```

    | PPI - Context (v1.0)
    | Downloading raw data...
    | Processing raw data
      ~ [PPI]
      ~ [PID -> CLA]
      ~ [CLA -> CID]
      ~ [PPI -> PID -> CLA -> CID]

In other cases, you might have the previous versions of the data to
process…

``` bash
$ python ppictx.py --run \
                   -fh path/to/HIPPIE.mitab \
                   -fp path/to/PUBTATOR.gz \
                   -fc path/to/CELLOSAURUS.txt
```

#### Special considerations

  - Cell lines that are primarily used in research due to their
    efficiency as an expression vector (e.g. *HeLa, HEK, CHO, Sf9*) may
    not be useful representations of cell-specific protein dynamics.
    However it may be useful to filter out PPIs annotated with these
    cell lines.

  - Cellosaurus contains synonymous cell lines, therefore some
    annotations such as *HEK (CVCL\_M624)* and *HEK293 (CVCL\_0045)*
    refer to the same cell line. Users should be aware of synonymous
    cell lines relevant to their interests and filter accordingly.

## Cite

Federico A, Monti S (2021) Contextualized Protein-Protein Interactions.
*Patterns*. <https://doi.org/10.1016/j.patter.2020.100153>.
