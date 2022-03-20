+++
title = "Simple PLA"
hascode = true
date = Date(2021, 01, 03)
rss = "Review machine learning and PLA. Xingyu Yan 2021-01-03."
+++
@def tags = ["julia", "code"]

# Simple PLA

\toc

## About PLA
This is only a simple/naive PLA that only on 2D for visualization, and didn't consider any noise or training/validation skills.

It follows the fundamental of PLA. Assume we have $points = [1, x, y]$ and $labels \in \{-1, +1\}$. A line $ax + by + c = 0, w = [c, a, b]$ separates points into two categories.

* Given $w$, we know it classifies a point $p$ by:
    $$sign(p^Tw)$$

* Given its lable $l$, if its classification is wrong, then:
    $$sign(p^Tw)\cdot l < 0$$

* Then we can update the $w$ / line by:
    $$w_{new} = w_{old} + p\cdot l$$


## Implementation
The core part of PLA is simple as:

```julia
function pla_train(points, labels, iter = 500)
    w = rand(3)
    for i in 1:iter
        for idx in 1:size(points)[2]
            u = points[:, idx]'w
            if u*labels[idx] < 0 
                w += points[:, idx]*labels[idx]
            end 
        end 
    end 
    return w
end

function pla_predict(points, w)
    labels = sign.(points'w)
end
```
Other part of the code, including generation of train/test data set is [here](https://github.com/i7242/IL-VENTE-D-OR/blob/master/ML/NaivePLA.jl).

## Test & Results
The points labeled by red/blue colours are ground truth labels.
![Ground Truth](/assets/pla_ground_truth.png)

After training, the training data set is classified as:
![Training Result](/assets/pla_test_on_training_data_set.png)

Use the testing data set, we got:
![Testing Result](/assets/pla_test_on_testing_data_set.png)

There were no miss classifications by counting:
```julia
sum(result_test .!= labels_test)
```
This was because the generated data is linear separatable, whithout noise. It's too simple 😹😹.

## Ref
[Machine Learning Fundations](https://www.coursera.org/learn/ntumlone-mathematicalfoundations?)
