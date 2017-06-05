# pyspark-flame
Pyspark-flame hooks into Pyspark's existing profiling machinery to provide a
low-overhead stack-sampling profiler, that outputs performance data in a
format compatible with
[Brendan Gregg's FlameGraph Visualizer](https://github.com/brendangregg/FlameGraph).

Unlike the cProfile-based profiler included with Pyspark, pyspark-flame uses
stack sampling. It takes stack traces at regular (configurable) intervals,
which allows its overhead to be low and tunable, and doesn't skew results,
making it suitable for use in performance test environments.

## Installation

```bash
pip install pyspark-flame
```

## Usage

```python
from pyspark_flame import FlameProfiler
from pyspark import SparkConf, SparkContext

conf = SparkConf().set("spark.python.profile", "true")
conf = conf.set("spark.python.profile.dump", ".")  # Optional - if not, dumps to stdout
sc = SparkContext(
    'local', 'test', conf=conf, profiler_cls=FlameProfiler,
    environment={'pyspark_flame.interval': 0.25}  # Optional - default is 0.2 seconds
)
# Do stuff with Spark context...
sc.show_profiles()
# Or maybe
sc.dump_profiles('.')
```
