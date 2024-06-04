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

# Introduction

Inland water bodies provide a variety of critical services for both human and aquatic life, including drinking water, recreational and economic opportunities, and marine habitats. A significant challenge water quality managers face is the formation of harmful algal blooms. One of the major types of HABs is cyanobacteria. HABs produce toxins that are poisonous to humans and their pets, and threaten marine ecosystems by blocking sunlight and oxygen.

While there are established methods for using satellite imagery to detect cyanobacteria in larger water bodies like oceans, detection in small inland lakes and reservoirs remains a challenge. Machine learning is particularly well-suited to this task because indicators of cyanobacteria are visible from free, routinely collected data sources. Whereas manual water sampling is time and resource intensive, machine learning models can generate estimates in seconds. This allows water managers to prioritize where water sampling will be most beneficial, and can provide a birds-eye view of water conditions across the state.

CyFi, short for Cyanobacteria Finder, is an open-source Python package that uses satellite imagery and machine learning to detect cyanobacteria levels. CyFi helps decision makers protect the public by flagging the highest-risk areas in lakes, reservoirs, and rivers quickly and easily. CyFi represents a significant advancement in environmental monitoring, providing higher resolution detection capabilities that can pinpoint areas at risk of cyanobacterial contamination. The strength of CyFi lies in its utilization of Sentinel-2 satellite data, a computationally efficient gradient boosted tree machine learning algorithm, and straightfoward command line interface.

The first section of this paper will give an overview of the origins of CyFi along with the underlying methods. The second portion will focus on CyFi's primary capabilities, intended uses, and areas for future work.

// This paper presents a detailed examination of the development of CyFi, exploring the algorithm's evolution, the dynamics of the prize competition that shaped it, and the broader implications of such competitions for scientific advancements. 

# Motivation

> There are tens of thousands of lakes that matter for recreation and drinking water. Cyanobacterial blooms pose real risks in many of them, and we really don't know when or where they show up, except in the largest lakes.
>
> -- Dr. Rick Stumpf, Oceanographer, NOAA, National Centers for Coastal Ocean Science

Harmful algal blooms are a pressing environmental and public health issue, characterized by the rapid and excessive growth of algae in water bodies, predominantly involving cyanobacteria, also known as blue-green algae. These blooms can produce toxins, such as microcystins and anatoxins, that pose severe risks to human health, aquatic ecosystems, and local economies. The toxins released by HABs can contaminate drinking water supplies, leading to acute and chronic health problems for communities. Exposure to these toxins through ingestion, skin contact, or inhalation can result in various health issues, including gastrointestinal illnesses, liver damage, neurological effects, and even death in extreme cases.

The ecological impacts of HABs are equally devastating. They can create hypoxic (low oxygen) conditions in water bodies, resulting in massive fish kills and the disruption of aquatic food webs. The dense algal mats can block sunlight from penetrating the water, inhibiting the growth of submerged vegetation essential for aquatic habitats. Furthermore, the decomposition of large algal blooms consumes significant amounts of dissolved oxygen, exacerbating the oxygen depletion and leading to dead zones where most aquatic life cannot survive.

Economically, HABs can cripple local industries reliant on water resources, such as fisheries, tourism, and recreation. Beaches and lakeside areas affected by algal blooms often face closures, leading to a loss of revenue from tourism and recreational activities. The cost of managing and mitigating the effects of HABs, including water treatment and healthcare expenses, places additional financial burdens on affected communities.

Despite the severe consequences of HABs, existing monitoring tools and methods are often insufficient. Traditional approaches, such as manual water sampling and laboratory analysis, are time-consuming, labor-intensive, and provide only localized snapshots of water quality. Existing satellite-based monitoring tools offer broad coverage but fall short in terms of spatial resolution, making them inadequate for monitoring smaller inland lakes, reservoirs, and rivers where HABs frequently occur. One of the leading satellite-based tools for cyanobacteria detection is the [CyAN index](https://oceancolor.gsfc.nasa.gov/about/projects/cyan/), which relies on the [Ocean and Land Colour Instrument (OCLI)](https://ladsweb.modaps.eosdis.nasa.gov/missions-and-measurements/olci/) on Sentinel-3. However, the 300m resolution of Sentinel-3 is too coarse to pick up many inland water bodies and therefore can't provide the data needed for effective early warning and rapid response to HAB outbreaks.

The limitations of current monitoring techniques highlight the urgent need for more advanced, precise, and accessible tools to detect and manage HABs. This gap in effective monitoring necessitates the development of innovative solutions that utilize higher resolution publicly available satellite data imagery and computationally efficient machine learning models to quickly and effectively detect the presence and extent of harmful algal blooms.

# CyFi overview

For each sampling point – a unique combination of date, latitude, and longitude – CyFi downloads recent cloud-free Sentinel-2 data from Microsoft's Planetary Computer, calculates a set of summary statistics for a bounding box around the sampling point, and passes that feature row into a LightGBM model which produces an estimated cyanobacteria density.

CyFi is a command line tool that requires only two lines of code to run. Simply pip install the package and then run `cyfi predict` for batch estimates or `cyfi predict-point` for single point estimates.

CyFi is a command line tool that downloads relevant satellite data and runs a trained machine learning algorithm that estimates cyanobacteria density for specified sampling points. A sampling point is a unique combination of date, latitude, and longitude.

Detailed instructions can be found in the CyFi docs.

CyFi supports batch predictions as well as sin

## Core functionality

## Under the hood

### Data sources

CyFi relies on two data sources as input: Sentinel-2 satellite imagery and land cover classifications from the United Nations Food and Agriculture Organization's Land Cover Classification System (LCCS).

#### Sentinel-2

Sentinel-2 is a wide-swath, high-resolution, multi-spectral imaging mission. It supports the monitoring of vegetation, soil and water cover, as well as observation of inland waterways and coastal areas. The Sentinel-2 Multispectral Instrument (MSI) samples 13 spectral bands: four bands at 10 meters, six bands at 20 meters and three bands at 60 meters spatial resolution. The mission provides a global coverage of the Earth's land surface every 5 days. Sentinel-2 data is accessed through Microsoft's Planetary Computer.

#### Land cover
The Climate Research Data Package (CRDP) Land Cover Gridded Map (2020) classifies land surface into 22 classes, which have been defined using the United Nations Food and Agriculture Organization's Land Cover Classification System (LCCS). This map is based on data from the Medium Resolution Imaging Spectrometer (MERIS) sensor on board the polar-orbiting Envisat-1 environmental research satellite by the European Space Agency. This data comes from the CCI-LC database hosted by the ESA Climate Change Initiative's Land Cover project ([source](https://www.esa-landcover-cci.org/?q=node/164)).


### Training data

The machine learning model underpinning CyFi was trained and evaluated using a comprehensive dataset of in-situ measurements of cyanobacteria density from across the United States, including 8,979 observations for training and 4,035 for testing and evaluation.

X observation for training, Y for testing
Train/test split criteria



### Feature processing

Each observation (or "sampling point") is a unique combination of date, latitude, and longitude. Satellite imagery feature generation for each observation is as follows:
  1. Identify relevant Sentinel-2 tiles based on a bounding box of 2,000m around the sampling point and a time range of 30 days prior to (and including) the sampling date.
  2. Select the most recent image that has a bounding box containing fewer than 5% of cloud pixels. If none of the images meet this criteria, no prediciton is made for that sampling point.
  3. Filter the pixels in the bounding box to the water area using the scene classification (SCL) band.
  4. Generate summary statistics (e.g., mean, max, min) and ratios (e.g, NDVI) using the 15 Sentinel-2 bands. Features include the mean, minimum, maximum, and range for 15 different bands, both visible and non-visible (AOT, B01, B02, B03, B04, B05, B06, B07, B08, B09, B11, B12, B8A, SCL, WVP), as well as normalized difference vegetation index calculations generated by combining the NIR band separately with the red band (B4) and three vegetation red bands (B5, B6, B7).
  
:::{figure} feature_creation.png
:label: fig:feature_creation
:width: 600px
Mock up of satellite data selection and processing. The dot represents the sample point; the square represents the 2,000km bounding box around the sample point; the multiple squares represents the multiple satellite image contenders within the lookback period; the orange outlined square indicates the selected, most-recent satellite image; the blue shaded area indicates the water pixels in the bounding box from which features are calculated. Note that not all features are represented in the columns.
:::

Finally, the land cover value for each sampling point is looked up from the static land cover map, and added to the satellite features.




### Model

LightGGM


### Predictions

Density estimates along with severity level based on WHO buckets.

## CyFi explorer


## Use cases

The general use cases for CyFi are as follows.

1) Provide a birds-eye view of conditions in lakes across the state

Some states rely on public reporting of blooms. CyFi can quickly and easily estimate cyanobacteria levels in water bodies across the state. This is particuarly important for regulatory and impairment work.

2) Confirm publicly reported blooms with more data

Some states rely on visual inspection of a submitted photo to confirm a bloom. CyFi can estimate the cyanobacteria levels and provide an additional data point, allowing the individual to make a more informed decision.

3) Identify places where sampling is not needed

Identifying water bodies that are not impaired can be just as helpful as identifying water bodies that are. CyFi can help confirm water bodies that are very likely not to have worrisome cyanobacteria levels, which can help prioritize limited ground sampling staff and resources.

4) Identify areas needing public health intervention

On the contrary, places likely to contain severe concentrations of cyanobacteria often times merit public health interventions like drinking water, swimming, and/or recreational advisories or prohibitions.


# Model performance

## Comparison to CyAN




# Origins

## Machine learning competition

CyFi has its origins in the [Tick Tick Bloom: Harmful Algal Detection Challenge](https://www.drivendata.org/competitions/143/tick-tick-bloom/), which ran from December 2022 to February 2023. This machine learning competition sought to harness the power of community-driven innovation and was created by DrivenData on behalf of NASA and in collaboration with NOAA, the U.S. Environmental Protection Agency, the U.S. Geological Survey, the U.S. Department of Defense Defense Innovation Unit, Berkeley AI Research, and Microsoft AI for Earth.

The goal in that challenge was to detect and classify the severity of cyanobacteria blooms in small, inland water bodies using publicly available [satellite](https://www.drivendata.org/competitions/143/tick-tick-bloom/page/650/#satellite-imagery), [climate](https://www.drivendata.org/competitions/143/tick-tick-bloom/page/650/#climate-data), and [elevation](https://www.drivendata.org/competitions/143/tick-tick-bloom/page/650/#elevation-data) data. The ground measurement labels were manually collected ground samples that were analyzed for cyanobacteria density. Labels were sourced from 14 data providers across the U.S., shown in @fig:ttb_datasets.

:::{figure} ttb_datasets.png
:label: fig:ttb_datasets
Labeled samples used in the Tick Tick Bloom competition colored by dataset provider.
:::

The full dataset containing 23,570 in-situ cyanobacteria measurements is publicly available through the [SeaBASS data archive](https://seabass.gsfc.nasa.gov/archive/NASA_HEADQUARTERS/SGupta/CAML/CAML_2013_2021).

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

TODO: competition results

## Carrying foward competition models

Machine learning competitions are excellent for crowd-sourcing top approaches to complex predictive modeling problems. The outputs of machine learning competitions include winning model code, trained model, and write ups. The desired outcome is regular predictions of cyanobacteria levels for given latitude and longitude points within small water bodies across the US. This enables ongoing detection of unsafe bacteria counts to inform advisories and protect public health and safety. There remains a “missing middle” of implementation that bridges the gap between static research code and the ability to use regularly generated predictions from the model. CyFi aims to bridge that gap.

// The goal of CyFi is to carry forward winning solutions from machine learning competitions. Machine learning competitions are great for exploring a large feature space, identifying which datasets are most useful, getting a sense of how well models can perform on a given task, and understanding why types of models/methodologies are most successful at this task. Where the primary goal is exploration, competitions are not set up to also productionize that code. Rather, winning submissions are closer to reserach code, surfacing novel approaches rather than robust, reusable code.

This gap exists for a few reasons. First, competitions rely on static data exported and processed once. Deployment requires repeated, automatic use with new data. Second, winning models are relatively unconstrained by the size and cost of their solutions. For ongoing use, efficiency matters. Third, competition code is validated once with anticipated, clean data. In the real world things break and change; use requires basic robustness, testing and configurability. Lastly, there is substantial variability in the clarity and organization of competition-winning code. Usable code requires others to be able to understand, maintain, and build on the codebase.

In carrying forward winning models, we aimed to assess performance & efficiency opportunities, simplify & restructure code to transform it into a runnable pipeline, and engineer clean, reproducible repository, access points for processing new data, tests, & continuous integration. The output (CyFi) is a clean, configurable code package capable of generating predictions on new input data

### User interviews

CyFi is not intended to be the end of the story. More work is needed to integrate CyFi in state-level dashboards and make it an integral part of decision-making workflow. That said, we conducted a number of user interviews that can inform the technical development of CyFi. We wanted to ensure that technical decisions we made in designing the package didn't hamstring future work of further integration.

We conducted human-centered design interviews with representatives from California, New York, Georgia, Louisiana, Florida, and Michigan. These states were selected as they present a varying range of geographic locations, number of water bodies in the region, HAB severity, investment in HABs / resource availability, and technical sophistication of current approaches. The focus of the user interviews was on understanding current water quality decision-making processes, including the data and tools used to support those decisions. We anticipated the learnings to inform the format for surfacing predictions, priorities in model performance, and computational constraints.

@tbl:interview_takeaways shows the core design decsisions for CyFi that were rooted in these interviews:

```{list-table} CyFi design decisions rooted in HCD interviews
:label: tbl:interview_takeaways
:header-rows: 1
* - Interview insight
  - CyFi design decision
* - States tend to have designated sampling locations or locations of reported blooms. Continuous coverage of a lake is nice but not necessary.
  - CyFi will expect sampling points as input rather than polygons and the output will be point-estimates rather than a gridded heatmap.
* - Thresholds are not universal and actions vary by state.
  - Prediction will be a density value rather than severity category.
* - While blooms in small water bodies can change quickly, the maximum cyanobacteria estimation cadence is daily
  - A sampling point will be a unique combination of date, latitude, and longitude. Additional time granularity is not needed. The CyFi model should be able to produce estimates for thousands of points in a day on a normal laptop.
* - Many states include a visual review of imagery (satellite or submitted photo) as part of the decision-making process.
  - Including a way to see the underlying satellite data for a given prediction point will help users build confidence and intution around the CyFi model.
* - States have their own tools for managing water quality data (e.g. ground samples and lab results).
  - CyFi will output a simple CSV file that includes identifying columns for joining with external data.
```

### Model experimentation

In carrying forward the winning approaches into a robust, generalizable, testing open source python package, additional model testing and experimentation and tested was needed to create a mmore generalizable solution. The table below summarizes the matrix of experiments that were conducted. The goal was to produce a more robust, accurate, and generalizable model. The core levers were the data sources, satellite data processing choice, and the target variable. We did not experiment with various model architectures given that the Tick Tick Bloom competition clearly surfaced the success of a gradient boosted tree model. We did follow best practices in tuning hyperparameters for the final model.

:::{figure} experiments.png
:label: fig:experiments
Model experimentation building blocks
:::

During experimentation, the model was trained on roughly 13,000 samples and evaluated on a holdout validation set of roughly 5,000 samples. Performance was evaluated based on a combination of root mean squared error, mean absolute error, mean absolute percentage error, regional root mean squared error, and visualizations of predictions.

#### Data decisions

**Sentinel-2 as sole satellite source**: We found that Landsat data primarily only added value for time period period to 20XX, when Sentinel-2 was not available. We chose to use Sentinel-2 as the sole data source as downloading satellite data is the slowest part of the prediction process and we expect CyFi primarily to be used in a forward looking way. Chosing to rely only on Sentinel-2 data meant that we removed any samples prior to the launch of Sentinel-2 from the training and evaluation sets. This decreased the dataset size by XX.

**Exclude climate and elevation features**: Similarly, we found that climate and elevation features primarily provided value for data points prior to the launch of Sentinel-2 and so are not used in the final CyFi model. We recognize that climate and elevation likely do have an impact on how cyanobacteria blooms form, and more sophisticated feature engineering with these data sources may add value in the future. This is a direction for future research.

**Include land cover**: Lastly, we found that including a land cover map, even at a coarse 300m resolution, added to model accuracy. The land cover map is likely capturing farmland areas that are more likely to have fertilizer runoff that contributes to blooms. We choose a static map from 2020 rather than a real-time satellite-derived product as this reduces the compute time and patterns in land use do not fluctuate daily.

**Remove geographic features to improve generalizability**: Generalizability is the degree to which the model applies well to new data in new time ranges and new locations. One of the risks in a machine learning competition is overfitting to the test set or picking up on patterns specific to the competition data that don't hold outside of the competition. We sought to identify and remove competition artifacts that would hamper the generalizability of the model in a open source package. We found that all winning solutions used a "longitude" feature in their solutions, which captured some underlying differences in sampling procedures by data providers. For example, California only conducts toxin analysis (which produces cyanobacteria cell density measures) for suspected blooms, so for the California sampling points in the competition data, most had high severity. With longitude as a feature, the model predicts high values for California data which does well in the competition setting (where most California points are high severity) but doesn't generalize to the real world, where most lakes in California are not experiencing high severity blooms at any given time. There are no geographic features used in the CyFi model.

**Remove likely mislabeled data points**: Training data is limited to samples that are estimated to be near a water body. There are known to be some samples with a location that appears to be far from a water body in the hand-labeled ground truth data. This can be caused by human error, GPS device error, or a lack of adequate precision in recorded latitude and longitude. During data pre-processing, the distance between each sample and the nearest water body was calculated using the [European Space Agency (ESA) WorldCover 10m 2021](https://developers.google.com/earth-engine/datasets/catalog/ESA_WorldCover_v200) product on Google Earth Engine. Samples farther than 550m from a water body are excluded. This helps ensure that the relevant water body falls within the bounding box so the model learns from examples that reflect real-world environmental characteristics of cyanobacteria blooms rather than patterns in human error.

#### Satellite processing decisions

**Filter to water area and use a larger bounding box**: Land pixels will generate falsely high cyanobacteria estimates so we filter them out. We find that the scene classification band (while not perfect) is sufficient for masking non-water pixels. Since ground sampling points are often near land (taken from the shore or the dock), a larger bounding box (2,000m) is used to ensure the relevant water pixels are included.

**Use a larger look-back window and filter to images with almost no clouds**: We use the scene classification band to calculate the percent of clouds in the bounding box and do not use any imagery that has greater than 5% clouds. Given the strict cloud threshold, we use a look-back window of 30 days before the sample. This increases the chances of getting a cloud-free image.

**Use only one image per sample point**: Some winning solutions average predictions over multiple images within a specified range. We find that this favors static blooms. We use only the most-recent cloud-free image to better detect short-lived blooms.

#### Target variable decisions

**Estimate density instead of severity**: We learned in the user interviews that states use different thresholds for action, so predicting density instead of severity categories supports a broader range of use cases. The winning competition models were trained to predict severity, so the first experiment we ran was predicting on density to see if there was enough signal in the feature data to suppor this.

**Train the model to predict log density**: We find transforming density into a log scale for model training and prediction yields better accuracy as the underlying data is log distributed. This helps the model learn that incorrectly estimating a density of 100,000 when the true density is 0 is much more important than incorrectly estimating a density of 1,100,000 when the true density is 1,000,000. The estimate a user sees has been converted back into (non-log) density.

# Future directions

# Conclusion

Hope CyFi is illustrative in showcasing the utility of machine learning competitions as a first step toward open source tools. Also that machine learning can be useful for cyanobacteria detection and plugging into human workflows.

Through a review of relevant literature and a discussion of the challenges faced in creating this independent, collaborative code package, this study aims to illustrate the potential of prize competitions and open-source tools in addressing critical environmental issues.


--------------------------


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
