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
If we would like to compare the running time of a block of codes, we use the following (We us shift+enter between the lines):
```
%%timeit
hero_wts_lbs = []
for wt in wts:
  hero_wts_lbs.append(wt * 2.20462)
```
vs
```
%%timeit
wts_np = np.array(wts)
hero_wts_lbs_np = wts_np*2.20462
```
This shows that numpy broadcasting is much faster than list calculation.

### Use package to time code running time
Use line_profiler pacakge.
```
pip install line_profiler
```
After installation, we run on the code:
```
%load_ext line_profiler
%lprun -f FUNCTION_NAME FUNCTION_NAME(ARGUMENTS)
```
For example:
```
%lprun -f convert_units convert_units(heroes, hts, wts)
```
### Code profiling for memory usage
```
import sys
nums_list = [*range(1000)]
sys.getsizeof(nums_list)
```
This returns the memory in bits.
Use package memory_profiler to generate line-by-line memory usage:
```
pip install memory_profiler
```
After installation, we run the code:
```
%load_ext memory_profiler
%mprun -f convert_units convert_units(heroes, hts, wts)
```
