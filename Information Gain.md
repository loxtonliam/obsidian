above : [[DS 141 Week 5 Notes]]
>[!info] measure of change in [[entropy]] due to new information being added

- parent set = original set before segmentation takes place
- children = after segmentation
- The amount of information gain depends on how much purer the children are than the parent set

mathematically
$$IG(parent,children) = entropy(parent) - [p(c_1)*entropy(c_1) + p(c_2)*entropy(c2) + ...]$$
- where IG = information gain
- can be calculated using the $attrEval()$ from the CORELearn package
