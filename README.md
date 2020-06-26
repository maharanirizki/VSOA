# Vendor Selection and Order Allocation (VSOA) Tutorial


In supply chain management (SCM) creating a strategic partnership with suppliers/vendor plays an important role to maximize SCM performance. A VSOA happen when a company needs to choose which vendor to allocate orders for producing products. The main three objectives of VSOA are **maximizing perfomance, minimizing risk, and minimizing cost**. VSOA in supply chain located in the tactical level and strategic level. 

![Position of VSOA](https://user-images.githubusercontent.com/49055090/85646247-4b3c9080-b6ce-11ea-83e9-192e6fef70e5.PNG)

Position of VSOA in Supply Chain (Lee et al., 2013)

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
This tutorial use linear programming to find the optimal solution by using Gurobi and Python language. 

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

| Criteria | Weight | 
|:---:| ---: |
| Price | 0.2961 |
| Technical level | 0.1867 | 
| Defects | 0.1658 |
| Reliability | 0.0553 |
| On-time delivery | 0.0804 |
| Supply capacity | 0.0676 | 
| Repair turnaround | 0.0466 |
| Warranty period | 0.1015 |

Each supplier has a different performance in every criteria. To determine the overall weight for each supplier, a method called Analytic Hierarchy Process (AHP) is conducted. However, this is beyond the scope of this tutorial and the overall weight is already given. For further information about AHP you can read it through [Wikipedia page](https://en.wikipedia.org/wiki/Analytic_hierarchy_process). The overall weight for each supplier is presented as follows.

| Supplier | Price | Technical level | Defects | Reliability | On-time delivery | Supply capacity | Repair turnaround | Warranty period | Supplier weights |
|:---:| ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| A | 0.3380 | 0.2000 | 0.0732 | 0.2121 | 0.2392 | 0.2564 | 0.2353 | 0.1818 | 0.2273 |
| B | 0.2503 | 0.2000 | 0.1463 | 0.2242 | 0.2392 | 0.1805 | 0.2353 | 0.3636 | 0.2274 |
| C | 0.2209 | 0.3000 | 0.1951 | 0.2727 | 0.2738 | 0.2018 | 0.1765 | 0.2727 | 0.2404 |
| D | 0.1908 | 0.3000 | 0.5854 | 0.2909 | 0.2478 | 0.3613 | 0.3529 | 0.1818 | 0.3049 |
| Global Weight | 0.2961 | 0.1867 | 0.1658 | 0.0553 | 0.0804 | 0.0676 | 0.0466 | 0.1015 | |

The model consists of four parts:
* Sets and notation
* Decision variable
* Objective function
* Constraints

The model is written using Python language, solved using Gurobi solver and run in the Jupyter Notebook environment. Note that to run the code you need to install [Python](https://www.python.org/downloads/), [Jupyter Notebook](https://jupyter.readthedocs.io/en/latest/install.html), and [Gurobi](https://www.gurobi.com/gurobi-and-anaconda-for-windows/). 

### Sets and notation
Sets and notation of this problem is written here.

![sets](https://user-images.githubusercontent.com/49055090/85819093-1ea77800-b7a5-11ea-8d8e-a4441711acb2.PNG)

Note that in this tutorial there is only 1 item. Therefore, the index _i_ is always 1 in this problem. There are several parameter that I want to highlight in order to understand the problem setting better.
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

* __Defective rate of item _i_ offered by supplier _j___ related with the technical level criteria for each supplier.

| Supplier | Defective rate | 
|:---:| :---: |
| 1 | 0.08 |
| 2 | 0.04 | 
| 3 | 0.03 |
| 4 | 0.01 |

* __The buyer's minimum acceptable defective rate of item _i___ is the parameter about the threshold of the buyer to accept defective product. This parameter is related to quality.

* __On-time delivery rate of item _i_ offered by supplier _j___ related with the on-time delivery criteria for each supplier. The higher the number, the faster the delivery of the goods.

| Supplier | On-time delivery rate | 
|:---:| :---: |
| 1 | 0.83 |
| 2 | 0.83 | 
| 3 | 0.95 |
| 4 | 0.86 |

* Similar with the previous parameter, the buyer also have a threshold to patiently wait for the goods to be delivered. This parameter is __the buyer's minimum acceptable on-time delivery rate of item _i___.

### Decision variable
The decision variable for this problem is written here. 

![Decision variable](https://user-images.githubusercontent.com/49055090/85825509-f9bb0100-b7b4-11ea-8d13-76990fb3b23e.PNG)

There are two continuous variables; _Xij_ to indicate the number of goods and _Vij_ to represent the business volume in order to determine the discount interval. There is also one binary variable _yjr_ to indicate in which discount interval that the buyer bought the goods.

### Objective function
This problem has four objective.

1. Since in this problem the performance of supplier is represented by weight, therefore the objective is to __maximize the total weighted quantity of purchasing__.

![obj 1](https://user-images.githubusercontent.com/49055090/85828624-6933ef00-b7bb-11ea-8fd7-4a1bfc491324.PNG)

2. The buyer decision is to __minimize the total purchase cost__ by considering the cummulative price breaks. 

![obj 2](https://user-images.githubusercontent.com/49055090/85826300-eb6de480-b7b6-11ea-89ff-3bda9ca16c1b.PNG)

3. To improve product quality and satisfaction of customer, the buyer expects to __minimize the number of defective items__.

![obj 3](https://user-images.githubusercontent.com/49055090/85826443-2e2fbc80-b7b7-11ea-8fb6-75f0fb5fcc1e.PNG)

4. Due to production schedule, the buyer wants to __maximize the number of items delivered on time__. 

![obj 4](https://user-images.githubusercontent.com/49055090/85826591-6636ff80-b7b7-11ea-9b13-3cb823dbce70.PNG)

Since this problem is a multi-objective problem, we need to set Gurobi to be able to consider all of this objective. Therefore, this tutorial use a function by Gurobi to linearize the problem. 
```
model.setObjectiveN(expression, index, priority(opt))
```
The expression above shows the linearization of the objective function. This tutorial assumes that all of the objective function has the same priority.  

### Constraints
The constraints for this problem is divided into four major parts.
#### Capacity constraint
Each supplier has a different number of product that they can provide. This condition is bounded by the capacity constraint.

![capacity constraint](https://user-images.githubusercontent.com/49055090/85828686-8f598f00-b7bb-11ea-97b1-b9fd134aa7f8.PNG)

#### Discount constraint
This constraint represents the discount prices for the goods. 

![discount constraint](https://user-images.githubusercontent.com/49055090/85829479-473b6c00-b7bd-11ea-9241-e4b66336f9a5.PNG)

#### Demand constraint
The buyer has a demand that need to be satisfied. The sum of assigned order quantities has to satisfy the demand. This condition is constrained by a demand constraint.

![demand constraint](https://user-images.githubusercontent.com/49055090/85829799-fbd58d80-b7bd-11ea-85bb-760587e5d734.PNG)

#### Quality constraint
The buyer's has a quality requirement to be satisfied. To satisfy the quality constraint, an equation is formulated below.

![quality constraints](https://user-images.githubusercontent.com/49055090/85831155-6daed680-b7c0-11ea-8e2c-416f4d35bf93.PNG)

#### Delivery constraint
Similar like quality, the buyer also has a requirement for delivery which constrained by this constraint.

![delivery constraint](https://user-images.githubusercontent.com/49055090/85831229-9040ef80-b7c0-11ea-88cd-96a45f98ce18.PNG)

## Conclusion
The model is solved within 0.52 and Gurobi found an optimal solution. This is the objective value of the model.
```
Objective Function
Objective 1 (maximize the total weighted quantity of purchasing)
Z1 = 221.1321

Objective 2 (minimize the total purchase cost)
Z2 = 433764.8485

Objective 3 (minimize the number of defective items)
Z3 = 16.0000

Objective 4 (maximize the number of items delivered on time)
Z4 = 720.0000

```
The variable result is as follows.
```
(1, 1): 0.0000
(1, 2): 24.2424
(1, 3): 363.6364
(1, 4): 412.1212
```
From the result, we know that the buyer will use a mix of three suppliers in order to satisfy the demand. The buyer will order 24.2424 to supplier B, 363.6364 to supplier C, and 412.1212 to supplier C. 

![Result comparison](https://user-images.githubusercontent.com/49055090/85836870-dfd7e900-b7c9-11ea-9453-69c906730047.PNG)

We may see that the result is proportional to the overall weight of the supplier. Therefore, we may conclude that the it is important to know the priority of criteria and quantifying the performance of the supplier. There is a several method to determine the weight for each supplier. I recommend you to check [this page](https://github.com/LinBaiTao/Supplier_Selection_Methods) for further resource. Note that this tutorial is only for single item. For further practice, you may want to consider multiple item and add the extension to further research. 

## Important Papers
Lee, C. Y., & Chien, C. F. (2014). [Stochastic programming for vendor portfolio selection and order allocation under delivery uncertainty](https://link.springer.com/content/pdf/10.1007/s00291-013-0342-7.pdf). OR spectrum, 36(3), 761-797.

Xia, W., & Wu, Z. (2007). [Supplier selection with multiple criteria in volume discount environments](https://www.sciencedirect.com/science/article/pii/S0305048305001180?casa_token=Bj0ESyxOK8sAAAAA:ylopTXOi-RuQ-9JIqxQUO0Ln6bx_dR1r052sEuiZ5Bd8sST2tmDilfCgNedoTHZyGjiWaBzS13Y4). Omega, 35(5), 494-504.

## References
Nick Morris, [Supplier Selection](https://github.com/N-ickMorris/Supplier-Selection)

