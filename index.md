---
layout: default
---

# Proposed Method 1, GPEX
Gaussian process (GP) is a white-box model which has the potential to become globally faithful to a neural network, 
thereby unboxing the blackbox of deep learning [8]. 
Existing theoretical works put strict assumptions on a neural network to make it equivalent to a Gaussian process.
Accommodating those theoretical assumptions is hard in recent deep architectures, and those theoretical conditions need refinement as new deep architectures emerge.
We proposed a method that - given a neural network - adopts knowledge distillation to finds a GP which is equivalent to the neural network.
Since GP is a white-box model, the obtained GPs can provide insights about the underlying decision mechanisms of the given neural network. 
Our methods called GPEX was published in NeurIPS 2023 conference [2].

# The Initial Prediction Problem
OncotypeDX test is a genomic assay for specific cohorts of breast cancer patients. 
The output of the assay is a number between 0 and 100 for each patient, and in this article we refer to it as
recurrence score.
OncotypeDX recurrence score stratifies patients as follows:
- Recurrence-score between 0 and 18: low risk of distant recurrence in 10 years.
- Recurrence-score between 18 and 30: medium risk of distant recurrence in 10 years.
- Recurrence-score between 30 and 100: high risk of distant recurrence in 10 years.

The initial goal was to adopt machine learning to predict recurrence-score only from H&E-stained 
histopathology images, thereby bypassing the need for performing genomic assays. 
A dataset of roughly 600 breast cancer patients was collected.
The dataset contained the following information for each patient:
- One H&E-stained whole-slide image
- The result of OncotypeDX test for the patient (a number between 0 and 100)
- Estrogen/progesterone receptor (ER/PR) intensity and percentages, obtained by pathologist assessment (four ordinal integers for each patient). 

# Proposed Method 2
We evaluated the state-of-the-art method CLAM [6] as well as the well-known cell profiler [7] features in predicting recurrence score directly from H&E-stained breast cancer images.    
This was done mainly by N. Guruprasad and with participation of A. Akbarnejad.
These methods could achieve prediction performances of up to 82 in terms of AUC.


The state-of-the-art method CLAM [6] encodes a whole-slide image by feeding each patch to a **fixed** resnet backbone which is pretrained on imagenet.
One may ask: is the low prediction performance of CLAM related to using a fixed backbone and not training the backbone itself?


To answer this question we proposed a pipeline based on deep Fisher-vector encoding which is entirely trained in an end-to-end way.
The method was published in the ISBI conference in 2021 [4]. 
The performance of our pipeline was 


# References
[1] A. Akbarnejad and N. Ray and P. J Barnes and G. Bigras,
 “Predicting Ki67, ER, PR, and HER2 Statuses from H&E-stained Breast Cancer Images,” arxiv preprint: https://arxiv.org/abs/2308.01982.


[2] A. Akbarnejad and G. Bigras and N. Ray, “GPEX, A Framework For Interpreting Artificial Neural Networks,” Conference on Neural Information Processing Systems (NeurIPS) 2023.


[3] Y. Yang and A. Akbarnejad and N. Ray and G. Bigras, “Double adversarial domain adaptation for whole-slide-imageclassification,” Medical Imaging with Deep Learning (MIDL), 2021.


[4] A. Akbarnejad and N. Ray and G. Bigras, “Deep Fisher Vector Coding For Whole Slide Image Classification,” in International Symposium of Biomedical Imaging (ISBI), 2021.


[5] N. Guruprasad, A. Akbarnejad, G. Bigras, P. J. Barnes, and N. Ray, "A Closer Look at Weak Supervision's Limitations in WSI Recurrence Score Prediction", IEEE International Conference on Bioinformatics and Biomedicine
(BIBM), 2023.


[6] M. Y, Lu, D. F. Williamson, et al, "Data-efficient and weakly supervised computational pathology on
whole-slide images", Nature biomedical engineering, 5(6):555–570, 2021.


[7] A. E. Carpenter, T. R. Jones, et al, "CellProfiler: image analysis software for identifying and quantifying cell phenotypes", Genome Biol 7, R100 (2006). 


[8] https://blog.research.google/2020/03/fast-and-easy-infinitely-wide-networks.html


Text can be **bold**, _italic_, or ~~strikethrough~~.

[Link to another page](./another-page.html).

There should be whitespace between paragraphs.

There should be whitespace between paragraphs. We recommend including a README, or a file with information about your project.

# Header 1

This is a normal paragraph following a header. GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.

## Header 2

> This is a blockquote following a header.
>
> When something is important enough, you do it even if the odds are not in your favor.

### Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Octocat](https://github.githubassets.com/images/icons/emoji/octocat.png)

### Large image

![Branching](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
