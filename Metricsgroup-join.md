## How to use results from one function to another
metric_2 * on(instance) group_left(host) metric_1{instance="abc"}
on(instance) => this is how to JOIN on label instance.

group_left(host) metric_1{instance="abc"} => means, keep the label host from metric_1 in the result.
