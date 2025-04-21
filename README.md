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
