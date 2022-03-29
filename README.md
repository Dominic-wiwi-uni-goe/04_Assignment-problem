## Work assignment based on individual human factors

### The assignment problem in general

The assignment problem is a special case of a linear programming problem. It is one of the fundamental combinational optimization problems in the branch of optimization or operations research in mathematics. Its goal consists in assigning **m resources** (usually workers) to **n tasks** (usually jobs) on a **one to one** basis while minimizing assignment costs. As a general rule, all jobs must be performed by exactly one worker and every worker must be assigned exclusively to one job.

The basic notation of the assignment problem is

![image](https://user-images.githubusercontent.com/102478331/160347326-4c1d8298-65c8-4688-8930-c2cbfba0c973.png)

where

![image](https://user-images.githubusercontent.com/102478331/160347290-00f56fd9-7169-41ed-9094-1e304813d9a1.png)

We can use the following linear assignment problem to assign a given number of workers to a given number of batches. Note that the number of workers and the number of batches must be equal for the LP. **Any worker can be assigned to perform any job, incurring in some cost that may vary depending on the work-job assignment.**

Cost matrix:

![image](https://user-images.githubusercontent.com/102478331/160347754-c24f6bad-9561-49aa-aab0-8e6008b6b538.png)


### The assignment problem in R

```
lp.assign (cost.mat, direction = "min", presolve = 0, compute.sens = 0)
```

The `lp.assign` funciton uses the following arguemnts:

- `cost.mat`: Matrix of costs: the ij-th element is the cost of assigning source i to destination j.
- `direction`: Character vector, length 1, containing either "min" (the default) or "max"
- `presolve`: Numeric: presolve? Default 0 (no); any non-zero value means "yes." Currently ignored.
- `compute.sens`: Numeric: compute sensitivity? Default 0 (no); any non-zero value means "yes." In that case presolving is attempted.

**Note:** Presolve is a preprocess of the lp-model. It looks for ways to simplify it. For example it can delete unused variables and restrictions. Substitute fixed variable values by a constant and so on. The result is a new model that is less complex than the original model and likely solves faster. The result of presolve can be that there are less variables and/or constraints in the presolved model.

### Example for a general assignment problem in R

Expemplary cost matrix:

![image](https://user-images.githubusercontent.com/102478331/160347889-5833ee7a-4a43-4629-b786-49abec822998.png)

(1) Import the package with the `library` fucntion

```
library(lpSolve)
```
(2) Formualte costs matrix

```
costs <- matrix(c(15, 10, 9,
                  9, 15, 10,
                  10, 12 ,8), nrow = 3, byrow = TRUE)
```
or use 

```
costs <- matrix (c(15, 10, 9,
                    9, 15, 10, 
                    10,12,8), 3, 3)
```
We can than have a look at the final matric that we call `costs`:

```
view(costs)
```

After formulating the `costs` matrix, we can calcualte the final value for z:

```
lp.assign(costs)
```

And look at the solution for the assignment of **m resources** (usually workers) to **n tasks** (usually jobs):

```
# Variables final values
lp.assign(costs)$solution
```

### Incorporating human facotrs in the work assignment problem: Delivery routes assigned to professional truck drivers

#### The assignment problem translated to truck drivers and route durations

Frist, we recall the basic notation of the assignment problem 

![image](https://user-images.githubusercontent.com/102478331/160347326-4c1d8298-65c8-4688-8930-c2cbfba0c973.png)

where

![image](https://user-images.githubusercontent.com/102478331/160347290-00f56fd9-7169-41ed-9094-1e304813d9a1.png)

Now, we set the cost in the cost matrix equal to a duration time in minutes. To drive delivery route j=1 (=task in the assignment problem),  truck driver i=1 (worker in the assignment problem) takes c11 of time to fulfil the route. But how do we calculate these times to get our cost matrix?


#### The cost matrix calculated by the individual regression weights of the multi level model

Second, we recall the multi level model discussed in **chapter 4** noted as:

```
model25 <- lmer(Tourdauer_Ist ~ Anzahl_Stopps + GTE + hour + Plan_Kap + KM + (Anzahl_Stopps | Fahrer), data = dataset_final)
```

However, rather than giving out the summary of the model with the estimates, we are interested in the individual estomates per truck dirver by

```
coef(model25)$Fahrer
```

Where we receive the regression weights for each truck drivers. 

![image](https://user-images.githubusercontent.com/102478331/160670960-f74e4f67-e046-41cd-b0e2-0a47ed5b8763.png)

















**Sources**: 
- https://towardsdatascience.com/operations-research-in-r-assignment-problem-4a1f92a09ab
- http://lpsolve.sourceforge.net/5.5/Presolve.htm
