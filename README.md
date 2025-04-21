# Airflow
<h2>PythonOperator( )</h2>
<p>The PythonOperator is used to execute Python functions (callables) within an Airflow workflow (DAG). This means that if you have a Python function defined, you can use the PythonOperator to execute it as a task in your workflow.</p>

```python
def print_context(ds, **kwargs):
    print(kwargs)
    print(ds)
    return 'Whatever you return gets printed in the logs'


run_this = PythonOperator(
    task_id='print_the_context',
    provide_context=True,
    python_callable=print_context,
    dag=dag,
)
```

<h3 class="" data-start="410" data-end="449"><strong>Parameters of the <code data-start="432" data-end="448">PythonOperator</code>:</strong></h3>
<ol data-start="451" data-end="2351">
<li class="" data-start="451" data-end="906">
<p class="" data-start="454" data-end="488"><strong data-start="454" data-end="487"><code data-start="456" data-end="485">task_id='print_the_context'</code></strong>:</p>
<ul data-start="492" data-end="906">
<li class="" data-start="492" data-end="650">
<p class="" data-start="494" data-end="650"><strong data-start="494" data-end="505">Purpose</strong>: This is a unique identifier for the task within the DAG. Every task in an Airflow DAG must have a <code data-start="605" data-end="614">task_id</code> to distinguish it from other tasks.</p>
</li>
<li class="" data-start="654" data-end="789">
<p class="" data-start="656" data-end="789"><strong data-start="656" data-end="667">Details</strong>: The <code data-start="673" data-end="682">task_id</code> must be unique within the DAG, and this ID will also be shown in the Airflow UI, logs, and other metadata.</p>
</li>
</ul>
</li>
<li>
<p class="" data-start="911" data-end="938"><strong data-start="911" data-end="937"><code data-start="913" data-end="935">provide_context=True</code></strong>:</p>
<ul data-start="942" data-end="1499">
<li class="" data-start="942" data-end="1041">
<p class="" data-start="944" data-end="1041"><strong data-start="944" data-end="955">Purpose</strong>: This tells Airflow to automatically pass context information to the Python callable.</p>
</li>
<li class="" data-start="1045" data-end="1334">
<p class="" data-start="1047" data-end="1334"><strong data-start="1047" data-end="1058">Details</strong>: If <code data-start="1063" data-end="1085">provide_context=True</code>, Airflow will provide some runtime context information (like execution date, task instance, etc.) to the Python function via the <code data-start="1215" data-end="1225">**kwargs</code> argument. This is useful for retrieving dynamic information, such as execution date (<code data-start="1311" data-end="1315">ds</code>), in the function.</p>
</li>
</ul>
</li>
<li>
<p class="" data-start="1504" data-end="1540"><strong data-start="1504" data-end="1539"><code data-start="1506" data-end="1537">python_callable=print_context</code></strong>:</p>
<ul data-start="1544" data-end="1978">
<li class="" data-start="1544" data-end="1628">
<p class="" data-start="1546" data-end="1628"><strong data-start="1546" data-end="1557">Purpose</strong>: This is the Python function that will be executed when the task runs.</p>
</li>
<li class="" data-start="1632" data-end="1864">
<p class="" data-start="1634" data-end="1864"><strong data-start="1634" data-end="1645">Details</strong>: The <code data-start="1651" data-end="1668">python_callable</code> parameter is where you specify the function you want to execute. In this case, it's <code data-start="1753" data-end="1768">print_context</code>. The function must be callable (a function or an object that implements the <code data-start="1845" data-end="1855">__call__</code> method).</p>
</li>
<li class="" data-start="1868" data-end="1978">
<p class="" data-start="1870" data-end="1978"><strong data-start="1870" data-end="1885">Example Use</strong>: When this task runs, it will call <code data-start="1921" data-end="1950">print_context(ds, **kwargs)</code> and pass the context to it.</p>
</li>
</ul>
</li>
</ol>
<p class="" data-start="2810" data-end="2908"><strong data-start="2810" data-end="2818"><code data-start="2812" data-end="2816">ds</code></strong>: This is the execution date (as a string), passed as part of the context to the function.</p>
<p class="" data-start="2911" data-end="3058"><strong data-start="2911" data-end="2923"><code data-start="2913" data-end="2921">kwargs</code></strong>: This is a dictionary that contains additional runtime context information, such as task instance, execution date, and other metadata.</p>
<p class="" data-start="3060" data-end="3207"><code data-start="3077" data-end="3106">print_context(ds, **kwargs)</code> prints the <code data-start="3118" data-end="3126">kwargs</code> dictionary, which contains the context information, and the execution date <code data-start="3202" data-end="3206">ds</code>.</p>
<h2>Passing in arguments</h2>
<p>Use the&nbsp;<code class="docutils literal notranslate"><span class="pre">op_args</span></code>&nbsp;and&nbsp;<code class="docutils literal notranslate"><span class="pre">op_kwargs</span></code>&nbsp;arguments to pass additional arguments to the Python callable.</p>
<p>Example:</p>

```python
from airflow.operators.python import PythonOperator
from pprint import pprint
from datetime import datetime

def print_context(ds, **kwargs):
    pprint(kwargs)  # Prints the kwargs dictionary, which includes Airflow context
    print(ds)  # Prints the execution date (ds)
    return 'Whatever you return gets printed in the logs'

# Assuming you have a DAG defined already, for example:
dag = DAG('example_dag', start_date=datetime(2023, 4, 21))

# Define the task using PythonOperator and pass additional kwargs using op_kwargs
run_this = PythonOperator(
    task_id='print_the_context',
    provide_context=True,  # Automatically passes Airflow context to the function
    python_callable=print_context,
    op_kwargs={'custom_param_1': 'value1', 'custom_param_2': 'value2'},  # Adding custom keyword args
    dag=dag,
)
```

<p class="" data-start="1101" data-end="1445"><strong data-start="1101" data-end="1173"><code data-start="1103" data-end="1171">op_kwargs={'custom_param_1': 'value1', 'custom_param_2': 'value2'}</code></strong>: This is where we pass additional keyword arguments to the <code data-start="1233" data-end="1248">print_context</code> function. The <code data-start="1263" data-end="1274">op_kwargs</code> parameter expects a dictionary, where the keys are the names of the parameters that will be passed to the Python function, and the values are the values you want to pass.</p>
<ul data-start="1461" data-end="1664">
<li class="" data-start="1461" data-end="1661">
<p class="" data-start="1463" data-end="1661">The function <code data-start="1476" data-end="1491">print_context</code> can now accept <code data-start="1507" data-end="1523">custom_param_1</code> and <code data-start="1528" data-end="1544">custom_param_2</code> in addition to the default <code data-start="1572" data-end="1576">ds</code> and <code data-start="1581" data-end="1589">kwargs</code> (which is automatically passed by Airflow when <code data-start="1637" data-end="1659">provide_context=True</code>).</p>
</li>
</ul>

<p class="" data-start="1700" data-end="1747"><code data-start="1705" data-end="1720">print_context</code> function will now receive:</p>
<ul data-start="1748" data-end="2004">
<li class="" data-start="1748" data-end="1843">
<p class="" data-start="1750" data-end="1843"><strong data-start="1750" data-end="1758"><code data-start="1752" data-end="1756">ds</code></strong>: The execution date passed by Airflow automatically (due to <code data-start="1819" data-end="1841">provide_context=True</code>).</p>
</li>
<li class="" data-start="1844" data-end="2004">
<p class="" data-start="1846" data-end="2004"><strong data-start="1846" data-end="1858"><code data-start="1848" data-end="1856">kwargs</code></strong>: This will include the Airflow context (<code data-start="1899" data-end="1914">task_instance</code>, <code data-start="1916" data-end="1925">dag_run</code>, etc.) plus the <code data-start="1942" data-end="1958">custom_param_1</code> and <code data-start="1963" data-end="1979">custom_param_2</code> values from <code data-start="1992" data-end="2003">op_kwargs</code>.</p>
</li>
</ul>
<p class="" data-start="2006" data-end="2053">&nbsp;</p>

<h3 class="" data-start="196" data-end="240">Why <code data-start="204" data-end="226">provide_context=True</code> is Important:</h3>
<p class="" data-start="242" data-end="567">When <code data-start="247" data-end="269">provide_context=True</code> is set, Airflow automatically passes a set of runtime parameters to your Python function, which are essential for understanding the state and context of the task when it runs. These parameters are passed in through the <code data-start="489" data-end="499">**kwargs</code> dictionary (which is essentially a collection of dynamic metadata).</p>
<h3 class="" data-start="569" data-end="609">What Does <code data-start="583" data-end="605">provide_context=True</code> Do?</h3>
<ul data-start="611" data-end="1012">
<li class="" data-start="611" data-end="778">
<p class="" data-start="613" data-end="778"><strong data-start="613" data-end="647">Without <code data-start="623" data-end="645">provide_context=True</code></strong>, your Python function would <strong data-start="676" data-end="684">only</strong> receive the arguments you explicitly pass to it via parameters like <code data-start="753" data-end="764">op_kwargs</code> or <code data-start="768" data-end="777">op_args</code>.</p>
</li>
<li class="" data-start="779" data-end="1012">
<p class="" data-start="781" data-end="1012"><strong data-start="781" data-end="812">With <code data-start="788" data-end="810">provide_context=True</code></strong>, in addition to the parameters you pass, Airflow will also automatically include <strong data-start="894" data-end="925">runtime context information</strong> like the execution date (<code data-start="951" data-end="955">ds</code>), task instance, and more, in the <code data-start="990" data-end="1000">**kwargs</code> dictionary.</p>
</li>
</ul>
<h3 class="" data-start="1014" data-end="1055">Contextual Parameters You Can Access:</h3>
<p class="" data-start="1057" data-end="1160">Some of the key runtime parameters that Airflow automatically provides when <code data-start="1133" data-end="1155">provide_context=True</code> are:</p>
<ul data-start="1162" data-end="1991">
<li class="" data-start="1162" data-end="1228">
<p class="" data-start="1164" data-end="1228"><strong data-start="1164" data-end="1172"><code data-start="1166" data-end="1170">ds</code></strong>: The execution date (in string format: <code data-start="1212" data-end="1226">'YYYY-MM-DD'</code>).</p>
</li>
<li class="" data-start="1229" data-end="1310">
<p class="" data-start="1231" data-end="1310"><strong data-start="1231" data-end="1251"><code data-start="1233" data-end="1249">execution_date</code></strong>: The actual datetime object for when the task is executed.</p>
</li>
<li class="" data-start="1311" data-end="1438">
<p class="" data-start="1313" data-end="1438"><strong data-start="1313" data-end="1332"><code data-start="1315" data-end="1330">task_instance</code></strong>: The TaskInstance object for the current task, which you can use to get additional task-related metadata.</p>
</li>
<li class="" data-start="1439" data-end="1528">
<p class="" data-start="1441" data-end="1528"><strong data-start="1441" data-end="1454"><code data-start="1443" data-end="1452">dag_run</code></strong>: The DAG run object, which provides access to metadata about the DAG run.</p>
</li>
<li class="" data-start="1529" data-end="1675">
<p class="" data-start="1531" data-end="1675"><strong data-start="1531" data-end="1554"><code data-start="1533" data-end="1537">ti</code> (TaskInstance)</strong>: You can access the TaskInstance directly as <code data-start="1600" data-end="1614">kwargs['ti']</code> to get task-specific details like its status, log, and more.</p>
</li>
<li class="" data-start="1676" data-end="1711">
<p class="" data-start="1678" data-end="1711"><strong data-start="1678" data-end="1687"><code data-start="1680" data-end="1685">dag</code></strong>: The DAG object itself.</p>
</li>
<li class="" data-start="1712" data-end="1750">
<p class="" data-start="1714" data-end="1750"><strong data-start="1714" data-end="1726"><code data-start="1716" data-end="1724">run_id</code></strong>: The ID of the DAG run.</p>
</li>
<li class="" data-start="1751" data-end="1795">
<p class="" data-start="1753" data-end="1795"><strong data-start="1753" data-end="1766"><code data-start="1755" data-end="1764">task_id</code></strong>: The ID of the current task.</p>
</li>
<li class="" data-start="1796" data-end="1889">
<p class="" data-start="1798" data-end="1889"><strong data-start="1798" data-end="1810"><code data-start="1800" data-end="1808">macros</code></strong>: Access to Airflow macros for dynamically generating values, e.g., <code data-start="1878" data-end="1888">{{ ds }}</code>.</p>
</li>
<li class="" data-start="1890" data-end="1991">
<p class="" data-start="1892" data-end="1991"><strong data-start="1892" data-end="1904"><code data-start="1894" data-end="1902">params</code></strong>: If you define any custom parameters in the DAG definition, they can be accessed here.</p>
</li>
</ul>
<p class="" data-start="2006" data-end="2053">&nbsp;</p>
