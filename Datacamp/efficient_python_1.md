## Notes for Course Efficient Python

### Magic inline code to find time of running
For a single line of code:
```
%timeit nums_list_comp = [num for num in range(51)]
%timeit nums_unpack = [*range(51)]
```
The output automatically average the time spent for multiple runs with a error interval.
To set a specific number of runs and loops each time, in order to average out a specific number of runs and executions per run, we use:
```
%timeit -r5 -n25 set(heroes_list)
```
The calculates the time spent to convert the list of heroes to a set with 25 iterations each and 5 runs in total.

Using %timeit, we find that the time taken to define list using formal name (x=list()) is slower than using literal name (x=[]).
```
formal_list = list()
formal_time = %timeit -o formal_list
literal_list = []
literal_time = %timeit -o literal_list
Compare_time = formal_time.worst>literal_time.worst
```
Compare_time outputs the value of True.

