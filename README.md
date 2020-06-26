# Vendor Selection and Order Allocation (VSOA) Tutorial


In supply chain management (SCM) creating a strategic partnership with suppliers/vendor plays an important role to maximize SCM performance. A VSOA happen when a company needs to choose which vendor to allocate orders for producing products. The main three objectives of VSOA are **maximizing perfomance, minimizing risk, and minimizing cost**. VSOA in supply chain located in the tactical level and strategic level. 

![Position of VSOA](https://user-images.githubusercontent.com/49055090/85646247-4b3c9080-b6ce-11ea-83e9-192e6fef70e5.PNG)

Figure 1. Position of VSOA in Supply Chain (Lee et al., 2013)

### Problem Characteristics
VSOA problem has several characteristics as follows:
* Long-term strategy and planning
* Multiple outsourcing vendors consideration
* Selecting fewer vendors
* Multiple criteria and objectives for evaluation
* Criteria including qualitative and quantitative, subjective or objective
* Determining suitable order quantity as the purpose
* High uncertainty and risk in selection procedure

# Table of Contents
  * [Methodology](#methodology)
  * [Problem Setting](#problem-setting)
    + [Sets and notation](#sets-and-notation)
    + [Decision variable](#decision-variable)
    + [Objective function](#objective-function)
    + [Constraints](#constraints)
  * [Conclusion](#conclusion)
  * [Important Papers](#important-papers)
  * [References](#references)

## Methodology
This tutorial use integer programming to solve the problem using Gurobi

## Problem Setting
The example used in this tutorial will be based on the model provided by Xia and Wu (2007) and the data is based on the report by Nick Morris to determine which vendor to be selected given several criteria. The problem setting for this tutorial is given four supplier that sold 1 item. The buyer has several criteria in order to select which supplier is chosen. The criteria are:
* Price
* Technical level
* Defects
* Reliability
* On-time delivery
* Supply capacity
* Repair turn-around
* Warranty period

The buyer has a different priority for each criteria. The priority is represented by weight.

![Global weight](https://user-images.githubusercontent.com/49055090/85816417-db95d680-b79d-11ea-8a51-80cc16633aac.PNG)

Each supplier has a different performance in every criteria. To determine the overall weight for each supplier, a method called Analytic Hierarchy Process (AHP) is conducted. However, this is beyond the scope of this tutorial and the overall weight is already given. For further information about AHP you can read it through [Wikipedia page](https://en.wikipedia.org/wiki/Analytic_hierarchy_process). The overall weight for each supplier is presented as follows.

![Supplier final weight](https://user-images.githubusercontent.com/49055090/85817451-98893280-b7a0-11ea-98f3-d7076ee76412.PNG)

The model consists of four parts:
* Sets and notation
* Decision variable
* Objective function
* Constraints

### Sets and notation
Sets and notation of this problem is written here.
![sets](https://user-images.githubusercontent.com/49055090/85819093-1ea77800-b7a5-11ea-8d8e-a4441711acb2.PNG)

There are several parameter that I want to highlight in order to understand the problem setting better.
* __Final weight of supplier _i___ is the weight from each supplier that we get from AHP.
* __Set of discount interval in supplier _j_ discount schedule__ related with the idea of __economies of scale__. Economies of scale indicate the idea of doing things more efficiently with increasing size. In this case, the more product that the buyer bought, the more discount that the supplier gives. The example of discount interval is given in the table below.

| _r_ | Business volume (in thousand $) | Discount (%) |
|:---:| :---:                           | :---:        |
| 1    | 0 to under 10000 | 0 |
| 2 | 10000 to under 20000 | 10 |
| 3 | 20000 and over | 20 |

* __Unit price of item _i_ quoted by supplier _j___ deal with the price offered by each supplier. In this tutorial, the price offered is given through the table below.

| Supplier | Price ($/unit) | 
|:---:| :---: |
| 1 | 411 |
| 2 | 555 | 
| 3 | 629 |
| 4 | 728 |

* __Defective rate of item _i_ offered by supplier _j___ related with the technical level criteria for each supplier

### Decision variable

### Objective function

### Constraints

The code for this case is:
```
import gurobi gurobi.py
```
The code is provided here. Note that to run the code you need to install [Python](https://www.python.org/downloads/), [Jupyter Notebook](https://jupyter.readthedocs.io/en/latest/install.html), and [Gurobi](https://www.gurobi.com/gurobi-and-anaconda-for-windows/). 

## Conclusion

## Important Papers
Lee, C. Y., & Chien, C. F. (2014). Stochastic programming for vendor portfolio selection and order allocation under delivery uncertainty. OR spectrum, 36(3), 761-797.

Xia, W., & Wu, Z. (2007). Supplier selection with multiple criteria in volume discount environments. Omega, 35(5), 494-504.

## References
Nick Morris, [Supplier Selection](https://github.com/N-ickMorris/Supplier-Selection)

