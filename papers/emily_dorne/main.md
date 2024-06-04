---
# Ensure that this title is the same as the one in `myst.yml`
title: Using Satellite Imagery to Identify Harmful Algal Blooms and Protect Public Health
abstract: |
  A short version of the long version that is way too long to be written as a
  short version anyway.  Still, when considering the facts from first
  principles, we find that the outcomes of this introspective approach is
  compatible with the guidelines previously established.

  In such an experiment it is then clear that the potential for further
  development not only depends on previous relationships found but also on
  connections made during exploitation of this novel new experimental
  protocol.
---

## Introduction

Inland water bodies provide a variety of critical services for both human and aquatic life, including drinking water, recreational and economic opportunities, and marine habitats. A significant challenge water quality managers face is the formation of harmful algal blooms. One of the major types of HABs is cyanobacteria. HABs produce toxins that are poisonous to humans and their pets, and threaten marine ecosystems by blocking sunlight and oxygen.

While there are established methods for using satellite imagery to detect cyanobacteria in larger water bodies like oceans, detection in small inland lakes and reservoirs remains a challenge. Machine learning is particularly well-suited to this task because indicators of cyanobacteria are visible from free, routinely collected data sources. Whereas manual water sampling is time and resource intensive, machine learning models can generate estimates in seconds. This allows water managers to prioritize where water sampling will be most beneficial, and can provide a birds-eye view of water conditions across the state.

CyFi, short for Cyanobacteria Finder, is an open-source Python package that uses satellite imagery and machine learning to detect cyanobacteria levels. CyFi helps decision makers protect the public by flagging the highest-risk areas in lakes, reservoirs, and rivers quickly and easily.

The first section of this paper will give an overview of the origins of CyFi along with the underlying methods. The second portion will focus on CyFi's primary capabilities, intended uses, and areas for future work.


## Motivation

> There are tens of thousands of lakes that matter for recreation and drinking water. Cyanobacterial blooms pose real risks in many of them, and we really don't know when or where they show up, except in the largest lakes.
>
> -- Dr. Rick Stumpf, Oceanographer, NOAA, National Centers for Coastal Ocean Science

For each sampling point – a unique combination of date, latitude, and longitude – CyFi downloads recent cloud-free Sentinel-2 data from Microsoft's Planetary Computer, calculates a set of summary statistics for a bounding box around the sampling point, and passes that feature row into a LightGBM model which produces an estimated cyanobacteria density. 

CyFi is a command line tool that requires only two lines of code to run. Simply pip install the package and then run `cyfi predict` for batch estimates or `cyfi predict-point` for single point estimates.

### Running CyFi

CyFi is a command line tool that downloads relevant satellite data and runs a trained machine learning algorithm that estimates cyanobacteria density for specified sampling points. A sampling point is a unique combination of date, latitude, and longitude.

Detailed instructions can be found in the CyFi docs.

CyFi supports batch predictions as well as sin


### Data sources

CyFi relies on two data sources as input: Sentinel-2 satellite imagery and land cover classifications from the United Nations Food and Agriculture Organization's Land Cover Classification System (LCCS).

#### Sentinel-2

Sentinel-2 is a wide-swath, high-resolution, multi-spectral imaging mission. It supports the monitoring of vegetation, soil and water cover, as well as observation of inland waterways and coastal areas. The Sentinel-2 Multispectral Instrument (MSI) samples 13 spectral bands: four bands at 10 meters, six bands at 20 meters and three bands at 60 meters spatial resolution. The mission provides a global coverage of the Earth's land surface every 5 days. Sentinel-2 data is accessed through Microsoft's Planetary Computer.

#### Land cover
The Climate Research Data Package (CRDP) Land Cover Gridded Map (2020) classifies land surface into 22 classes, which have been defined using the United Nations Food and Agriculture Organization's Land Cover Classification System (LCCS). This map is based on data from the Medium Resolution Imaging Spectrometer (MERIS) sensor on board the polar-orbiting Envisat-1 environmental research satellite by the European Space Agency. This data comes from the CCI-LC database hosted by the ESA Climate Change Initiative's Land Cover project ([source](https://www.esa-landcover-cci.org/?q=node/164)).


### Feature processing


### Model


### Predictions



### Origin story

CyFi has its origins in a machine learning competition. Machine learning competitions are excellent for crowd-sourcing top approaches to complex problems. In coordination with NASA, DrivenData ran the [Tick Tick Bloom machine learning competition](https://www.drivendata.org/competitions/143/tick-tick-bloom/) from December 2022 to February 2023. The goal in that challenge was to detect and classify the severity of cyanobacteria blooms in small, inland water bodies using publicly available [satellite](https://www.drivendata.org/competitions/143/tick-tick-bloom/page/650/#satellite-imagery), [climate](https://www.drivendata.org/competitions/143/tick-tick-bloom/page/650/#climate-data), and [elevation](https://www.drivendata.org/competitions/143/tick-tick-bloom/page/650/#elevation-data) data.

The labels in the competition were manually collected ground samples that were analyzed for cyanobacteria density. Labels were sourced from 14 data providers across the U.S., shown in @fig:ttb_datasets.

:::{figure} ttb_datasets.png
:label: fig:ttb_datasets
Labeled samples used in the Tick Tick Bloom competition colored by dataset provider.
:::

Participants predicted a severity category for a given sampling point as shown in @tbl:severity_categories. These ranges were based on EPA guidelines (TODO: link).

```{list-table} Severity categories used in the Tick Tick Bloom competition
:label: tbl:severity_categories
:header-rows: 1
* - Severity level
  - Cyanobacteria density range (cells/mL)
* - 1
  - $\lt$20,000
* - 2
  - 20,000 – $\lt$100,000
* - 3
  - 100,000 – $\lt$1,000,000
* - 4
  - 1,000,000 – $\lt$10,000,000
* - 5
  - $\ge$10,000,000
```

One of the key goals of the competition was to identify the most useful data sources for cyanobacteria estimation in inland lakes and waterways. Sentinel-2 data proved to be higher value than Landsat, which was unsurpring given the small size of water bodies and the coarse resolution of Landsat.

:::{figure} ttb_winner_sources.png
:label: fig:ttb_winner_sources
Data sources used by Tick Tick Bloom competition winners
:::







Twelve hundred years ago — in a galaxy just across the hill...

This document should be rendered with MyST Markdown [mystmd.org](https://mystmd.org),
which is a markdown variant inspired by reStructuredText. This uses the `mystmd`
CLI for scientific writing which can be [downloaded here](https://mystmd.org/guide/quickstart).
When you have installed `mystmd`, run `myst start` in this folder and
follow the link for a live preview, any changes to this file will be
reflected immediately.


## Bibliographies, citations and block quotes

Bibliography files and DOIs are automatically included and picked up by `mystmd`.
These can be added using pandoc-style citations `[@doi:10.1109/MCSE.2007.55]`
which fetches the citation information automatically and creates: [@doi:10.1109/MCSE.2007.55].
Additionally, you can use any key in the BibTeX file using `[@citation-key]`,
as in [@hume48] (which literally is `[@hume48]` in accordance with
the `hume48` cite-key in the associated `mybib.bib` file).
Read more about [citations in the MyST documentation](https://mystmd.org/guide/citations).

If you wish to have a block quote, you can just indent the text, as in:

Other typography information can be found in the [MyST documentation](https://mystmd.org/guide/typography).

### DOIs in bibliographies

In order to include a DOI in your bibliography, add the DOI to your bibliography
entry as a string. For example:

```{code-block} bibtex
:emphasize-lines: 7
:linenos:
@book{hume48,
  author    =  "David Hume",
  year      = {1748},
  title     = "An enquiry concerning human understanding",
  address   = "Indianapolis, IN",
  publisher = "Hackett",
  doi       = "10.1017/CBO9780511808432",
}
```

### Citing software and websites

Any paper relying on open-source software would surely want to include citations.
Often you can find a citation in BibTeX format via a web search.
Authors of software packages may even publish guidelines on how to cite their work.

For convenience, citations to common packages such as
Jupyter [@jupyter],
Matplotlib [@matplotlib],
NumPy [@numpy],
pandas [@pandas1; @pandas2],
scikit-learn [@sklearn1; @sklearn2], and
SciPy [@scipy]
are included in this paper's `.bib` file.

In this paper we not only terraform a desert using the package terradesert [@terradesert], we also catch a sandworm with it.
To cite a website, the following BibTeX format plus any additional tags necessary for specifying the referenced content is recommended.
If you are citing a team, ensure that the author name is wrapped in additional braces `{Team Name}`, so it is not treated as an author's first and last names.

```{code-block} bibtex
:emphasize-lines: 2
:linenos:
@misc{terradesert,
  author = {{TerraDesert Team}},
  title  = {Code for terraforming a desert},
  year   = {2000},
  url    = {https://terradesert.com/code/},
  note   = {Accessed 1 Jan. 2000}
}
```

## Source code examples

No paper would be complete without some source code.
Code highlighting is completed if the name is given:

```python
def sum(a, b):
    """Sum two numbers."""

    return a + b
```

Use the `{code-block}` directive if you are getting fancy with line numbers or emphasis. For example, line-numbers in `C` looks like:

```{code-block} c
:linenos: true

int main() {
    for (int i = 0; i < 10; i++) {
        /* do something */
    }
    return 0;
}
```

Or a snippet from the above code, starting at the correct line number, and emphasizing a line:

```{code-block} c
:linenos: true
:lineno-start: 2
:emphasize-lines: 3
    for (int i = 0; i < 10; i++) {
        /* do something */
    }
```

You can read more about code formatting in the [MyST documentation](https://mystmd.org/guide/code).

## Figures, Equations and Tables

It is well known that Spice grows on the planet Dune [@Atr03].
Test some maths, for example $e^{\pi i} + 3 \delta$.
Or maybe an equation on a separate line:

```{math}
g(x) = \int_0^\infty f(x) dx
```

or on multiple, aligned lines:

```{math}
\begin{aligned}
g(x) &= \int_0^\infty f(x) dx \\
     &= \ldots
\end{aligned}
```

The area of a circle and volume of a sphere are given as

```{math}
:label: circarea

A(r) = \pi r^2.
```

```{math}
:label: spherevol

V(r) = \frac{4}{3} \pi r^3
```

We can then refer back to Equation {ref}`circarea` or
{ref}`spherevol` later.
The `{ref}` role is another way to cross-reference in your document, which may be familiar to users of Sphinx.
See complete documentation on [cross-references](https://mystmd.org/guide/cross-references).

Mauris purus enim, volutpat non dapibus et, gravida sit amet sapien. In at
consectetur lacus. Praesent orci nulla, blandit eu egestas nec, facilisis vel
lacus. Fusce non ante vitae justo faucibus facilisis. Nam venenatis lacinia
turpis. Donec eu ultrices mauris. Ut pulvinar viverra rhoncus. Vivamus
adipiscing faucibus ligula, in porta orci vehicula in. Suspendisse quis augue
arcu, sit amet accumsan diam. Vestibulum lacinia luctus dui. Aliquam odio arcu,
faucibus non laoreet ac, condimentum eu quam. Quisque et nunc non diam
consequat iaculis ut quis leo. Integer suscipit accumsan ligula. Sed nec eros a
orci aliquam dictum sed ac felis. Suspendisse sit amet dui ut ligula iaculis
sollicitudin vel id velit. Pellentesque hendrerit sapien ac ante facilisis
lacinia. Nunc sit amet sem sem. In tellus metus, elementum vitae tincidunt ac,
volutpat sit amet mauris. Maecenas[^footnote-1] diam turpis, placerat[^footnote-2] at adipiscing ac,
pulvinar id metus.

[^footnote-1]: On the one hand, a footnote.
[^footnote-2]: On the other hand, another footnote.

:::{figure} figure1.png
:label: fig:stream
This is the caption, sandworm vorticity based on storm location in a pleasing stream plot. Based on example in [matplotlib](https://matplotlib.org/stable/plot_types/arrays/streamplot.html).
:::

:::{figure} figure2.png
:label: fig:em
This is the caption, electromagnetic signature of the sandworm based on remote sensing techniques. Based on example in [matplotlib](https://matplotlib.org/stable/plot_types/stats/hist2d.html).
:::

As you can see in @fig:stream and @fig:em, this is how you reference auto-numbered figures.
To refer to a sub figure use the syntax `@label [a]` in text or `[@label a]` for a parenhetical citation (i.e. @fig:stream [a] vs [@fig:stream a]).
For even more control, you can simply link to figures using `[Figure %s](#label)`, the `%s` will get filled in with the number, for example [Figure %s](#fig:stream).
See complete documentation on [cross-references](https://mystmd.org/guide/cross-references).


We show the different quantities of materials required in
@tbl:materials.

Unfortunately, markdown can be difficult for defining tables, so if your table is more complex you can try embedding HTML:

:::{table} Area Comparisons (written in html)
:label: tbl:areas-html

<table>
<tr><th rowspan="2">Projection</th><th colspan="3" align="center">Area in square miles</th></tr>
<tr><th align="right">Large Horizontal Area</th><th align="right">Large Vertical Area</th><th align="right">Smaller Square Area<th></tr>
<tr><td>Albers Equal Area   </td><td align="right"> 7,498.7   </td><td align="right"> 10,847.3  </td><td align="right">35.8</td></tr>
<tr><td>Web Mercator        </td><td align="right"> 13,410.0  </td><td align="right"> 18,271.4  </td><td align="right">63.0</td></tr>
<tr><td>Difference          </td><td align="right"> 5,911.3   </td><td align="right"> 7,424.1   </td><td align="right">27.2</td></tr>
<tr><td>Percent Difference  </td><td align="right"> 44%       </td><td align="right"> 41%       </td><td align="right">43%</td></tr>
</table>
:::

or if you prefer LaTeX you can try `tabular` or `longtable` environments:

```{raw} latex
\begin{table*}
  \begin{longtable*}{|l|r|r|r|}
  \hline
  \multirow{2}{*}{\bf Projection} & \multicolumn{3}{c|}{\bf Area in square miles} \\
  \cline{2-4}
   & \textbf{Large Horizontal Area} & \textbf{Large Vertical Area} & \textbf{Smaller Square Area} \\
  \hline
  Albers Equal Area   & 7,498.7   & 10,847.3  & 35.8  \\
  Web Mercator        & 13,410.0  & 18,271.4  & 63.0  \\
  Difference          & 5,911.3   & 7,424.1   & 27.2  \\
  Percent Difference  & 44\%      & 41\%      & 43\%  \\
  \hline
  \end{longtable*}

  \caption{Area Comparisons (written in LaTeX) \label{tbl:areas-tex}}
\end{table*}
```

Perhaps we want to end off with a quote by Lao Tse[^footnote-3]:

> 

[^footnote-3]: $\mathrm{e^{-i\pi}}$
