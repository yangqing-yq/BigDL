## **Precondition**
First of all, you need to obtain the BigDL libs. Refer to [Install from pre built](../ScalaUserGuide/install-pre-built.md)
or [Install from source code](../ScalaUserGuide/install-build-src.md) for more details

## **Remark**
- Only __Python 2.7__, __Python 3.5__ and __Python 3.6__ are supported for now.
- Note that __Python 3.6__ is only compatible with Spark 1.6.4, 2.0.3, 2.1.1 and 2.2.0. See [this issue](https://issues.apache.org/jira/browse/SPARK-19019) for more discussion.

## **Set Environment Variables**
Set **BIGDL_HOME** and **SPARK_HOME**:

* If you download BigDL from the [Release Page](../release-download.md)
```bash
export SPARK_HOME=folder path where you extract the spark package
export BIGDL_HOME=folder path where you extract the bigdl package
```

* If you build BigDL by yourself
```bash
export SPARK_HOME=folder path where you extract the spark package
export BIGDL_HOME=the dist folder generated by the build process, which is under the top level of the source folder
```

## **Update spark-bigdl.conf (Optional)**
If you have some customized properties in some files, which is used with the **--properties-file** option
in spark-submit/pyspark, add these customized properties into ${BIGDL_HOME}/conf/spark-bigdl.conf.

## **Run with pyspark**
```bash
${BIGDL_HOME}/bin/pyspark-with-bigdl.sh --master local[*]
```
* `--master` set the master URL to connect to
* `--jars` if there are extra jars needed.
* `--py-files` if there are extra python packages needed.

You can also specify other options available for pyspark in the above command if needed.

[Example code to verify if BigDL can run successfully](run-from-pip.md#code.verification)

## **Run with spark-submit**
A BigDL Python program runs as a standard pyspark program, which requires all Python dependencies
(e.g., NumPy) used by the program to be installed on each node in the Spark cluster. You can try
running the BigDL [lenet Python example](https://github.com/intel-analytics/BigDL/tree/master/pyspark/bigdl/models/lenet)
as follows:

```bash
${BIGDL_HOME}/bin/spark-submit-with-bigdl.sh --master local[4] lenet5.py
```

## **Run with Jupyter**
With the full Python API support in BigDL, users can use BigDL together with powerful notebooks
(such as Jupyter notebook) in a distributed fashion across the cluster, combining Python libraries,
Spark SQL / dataframes and MLlib, deep learning models in BigDL, as well as interactive
visualization tools.

__Prerequisites__: Install all the necessary libraries on the local node where you will run Jupyter, e.g., 
```bash
sudo apt install python
sudo apt install python-pip
sudo pip install numpy scipy pandas scikit-learn matplotlib seaborn wordcloud
```

Launch the Jupyter notebook as follows:
```bash
${BIGDL_HOME}/bin/jupyter-with-bigdl.sh --master local[*]
```
* `--master` set the master URL to connect to
* `--jars` if there are extra jars needed.
* `--py-files` if there are extra python packages needed.

You can also specify other options available for pyspark in the above command if needed.

After successfully launching Jupyter, you will be able to navigate to the notebook dashboard using
your browser. You can find the exact URL in the console output when you started Jupyter; by default,
the dashboard URL is http://your_node:8888/

[Example code to verify if BigDL can run successfully](run-from-pip.md#code.verification)

<a name="yarn.example"></a>
## **Run with virtual environment in Yarn**
If you already created BigDL dependency virtual environment according to [Yarn cluster guide in install without pip ](install-without-pip.md#yarn.cluster), you can run python program using BigDL as following examples.

- Note: please set BigDL_HOME, SPARK_HOME environment. Set VENV_HOME to the parent directory of venv.zip and venv directory. Replace VERSION with your BigDL version, like 0.5.0. If you don't install BigDL from source, replace ${BigDL_HOME}/pyspark/bigdl/examples/lenet/lenet.py with your python program which is using BigDL.
* Yarn cluster mode
```
    BigDL_HOME=
    SPARK_HOME=
    PYTHON_API_PATH=${BigDL_HOME}/dist/lib/bigdl-VERSION-python-api.zip
    BigDL_JAR_PATH=${BigDL_HOME}/dist/lib/bigdl-VERSION-jar-with-dependencies.jar
    PYTHONPATH=${PYTHON_API_PATH}:$PYTHONPATH
    VENV_HOME=
    
    PYSPARK_PYTHON=./venv.zip/venv/bin/python ${SPARK_HOME}/bin/spark-submit \
    --conf spark.yarn.appMasterEnv.PYSPARK_PYTHON=./venv.zip/venv/bin/python \
    --master yarn-cluster \
    --executor-memory 10g \
    --driver-memory 10g \
    --executor-cores 8 \
    --num-executors 2 \
    --properties-file ${BigDL_HOME}/dist/conf/spark-bigdl.conf \
    --jars ${BigDL_JAR_PATH} \
    --py-files ${PYTHON_API_PATH} \
    --archives ${VENV_HOME}/venv.zip \
    --conf spark.driver.extraClassPath=bigdl-VERSION-jar-with-dependencies.jar \
    --conf spark.executor.extraClassPath=bigdl-VERSION-jar-with-dependencies.jar \
    ${BigDL_HOME}/pyspark/bigdl/examples/lenet/lenet.py
```

* Yarn client mode
```
    BigDL_HOME=
    SPARK_HOME=
    PYTHON_API_PATH=${BigDL_HOME}/dist/lib/bigdl-VERSION-python-api.zip
    BigDL_JAR_PATH=${BigDL_HOME}/dist/lib/bigdl-VERSION-jar-with-dependencies.jar
    PYTHONPATH=${PYTHON_API_PATH}:$PYTHONPATH
    VENV_HOME=
    
    PYSPARK_DRIVER_PYTHON=${VENV_HOME}/venv/bin/python PYSPARK_PYTHON=./venv.zip/venv/bin/python ${SPARK_HOME}/bin/spark-submit \
    --master yarn \
    --deploy-mode client \
    --executor-memory 10g \
    --driver-memory 10g \
    --executor-cores 16 \
    --num-executors 2 \
    --properties-file ${BigDL_HOME}/dist/conf/spark-bigdl.conf \
    --jars ${BigDL_JAR_PATH} \
    --py-files ${PYTHON_API_PATH} \
    --archives ${VENV_HOME}/venv.zip \
    --conf spark.driver.extraClassPath=${BigDL_JAR_PATH} \
    --conf spark.executor.extraClassPath=bigdl-VERSION-jar-with-dependencies.jar \
    ${BigDL_HOME}/pyspark/bigdl/examples/lenet/lenet.py
 ```


## **BigDL Configuration**
Please check [this page](../ScalaUserGuide/configuration.md)