# Spark Notes

## Boilerplate
This boilerplate will allow us to access pyspark from the home folder without having to work strictly in the home folder. 

```python
import findspark
findspark.init('/home/kyle/spark-3.3.1-bin-hadoop3')
import pyspark
```
