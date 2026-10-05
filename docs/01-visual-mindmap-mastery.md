# 🧠 Python — Visual MindMap Mastery (Think in Python, Don't Memorize It)

> **The whole philosophy:** Don't ask *"what syntax do I need?"* Ask *"what problem am I solving, what's the input, what's the output, what are the steps?"* Syntax is the last 10%. This doc is built from **maps, analogies, memory cards, cheat sheets, decision trees, and active recall** — not a function list.

---

## 🗺️ THE MASTER PYTHON MIND MAP (home base)

```
                           PYTHON
                              │
       ┌──────────────────────┼──────────────────────┐
       ↓                      ↓                      ↓
     THINK                  STORE                   ACT
       │                      │                      │
   Variables              Lists                   Functions
   Conditions             Dictionaries            Modules
   Loops                  Sets                    Classes
   Logic                  Tuples                  APIs
       └──────────────┬───────┴──────────────────────┘
                      ↓
                    DATA ──► Files · APIs · Databases
                      ↓
                   PROCESS ──► Pandas / NumPy
                      ↓
        ANALYZE ─► AUTOMATE ─► TEST ─► DEPLOY ─► MONITOR
```
**Recall rule:** if you can redraw this from memory, you can rebuild most of Python.

## 🔁 THE PROBLEM-SOLVING LOOP
```
PROBLEM ─► UNDERSTAND ─► BREAK INTO SMALL STEPS ─► WRITE LOGIC ─► CODE
       ─► RUN ─► CHECK RESULT ─► DEBUG ─► TEST ─► IMPROVE
```
> Memory: **Understand → Break → Code → Test → Improve.**

## 💻 THE COMPUTER MENTAL MODEL
```
INPUT ─► PROCESS ─► OUTPUT
```
```python
sales = [100, 200, 300]   # INPUT
total = sum(sales)        # PROCESS
print(total)              # OUTPUT -> 600
```
The computer is a juicer: fruit in, squeeze, juice out. It never guesses — you describe exact steps.

---
---

# PART A — THINK · STORE · ACT (fundamentals, as memory cards)

Every card: **WHAT · WHY · PICTURE · TINY CODE · MEMORY · WATCH OUT.**

### 🟦 VARIABLE
```
WHAT a name pointing to a value · WHY remember/reuse data · CODE price = 100
MEM Variable = a label stuck on data · WATCH '=' means assign, not "equals"
```

### 🟦 DATA TYPES
```python
name="Ravi"; age=30; salary=50000.5; active=True
cities=["Pune","Mumbai"]; cust={"name":"Ravi","age":30}
```
Convert: `int() float() str()`. Check: `type(x)`.

### 🟦 STRING
```python
"Ravi".upper(); f"Ravi is {age} years old."; "  hi ".strip(); "a,b".split(",")
```
`MEM: f-string = drop values into text.` `WATCH: strip() for dirty data.`

### 🟦 NUMBER
`+ - * / //(floor) %(remainder) **(power)` · `WATCH: / gives float; guard divide-by-zero.`

### 🟦 BOOLEAN & COMPARISON
`== != > < >= <=` → True/False. `WATCH: == compares; = assigns.`

### 🟦 IF / ELIF / ELSE
```python
if sales > 100000: cat="High"
elif sales > 50000: cat="Medium"
else: cat="Low"
```
`WATCH: indentation (4 spaces) defines what's "inside".`

### 🟦 LOGICAL OPERATORS
`and` (both) · `or` (either) · `not` (flip).

### 🟦 LIST (ordered, changeable)
```python
a=[10,20,30]; a.append(40); a[0]; a[-1]; a[1:3]; a.sort()
```

### 🟦 TUPLE (frozen list)
```python
point=(19.99,73.78)   # can't be changed; usable as a dict key
```

### 🟦 SET (unique)
`list(set(x))` dedupes; `x in myset` is super fast.

### 🟦 DICTIONARY (KEY → VALUE — the workhorse)
```python
c={"name":"Ravi","age":30}
c["name"]; c.get("phone","NA"); c["phone"]="999"
for k,v in c.items(): ...
```
It's JSON, API responses, config, one row of data. `WATCH: c["missing"] crashes; c.get() is safe.`

### 🟦 PICK-ONE MAP
```
ordered + changeable → LIST · fixed → TUPLE · unique → SET · key→value → DICT
```

### 🟦 LOOPS
```python
for customer in customers: process(customer)
while not done: step()
```
`WATCH: a while loop must change its condition.` `break`=stop, `continue`=skip.

### 🟦 FUNCTION (reusable machine)
```python
def calculate_profit(revenue, cost):
    return revenue - cost
```
Ask: what input? what output? can I test it alone? can I reuse it?

### 🟦 PARAMETER vs ARGUMENT vs RETURN
```python
def good(p,q): return p*q     # gives back the number
x = good(100,5) * 2           # works
```
`WATCH (critical): print() SHOWS and returns None; return SENDS a value other code can use.`

### 🟦 SCOPE
`GLOBAL` visible everywhere · `LOCAL` only inside a function. Prefer passing in / returning out.

### 🟦 LIST COMPREHENSION
```python
squares=[x*x for x in nums]
big=[o for o in orders if o>10000]
```
`MEM: a loop folded onto one line.` Keep it readable.

### 🟦 CLASS & OBJECT (practical OOP)
```python
class Customer:
    def __init__(self, name): self.name = name   # constructor
    def shout(self): return self.name.upper()
c = Customer("Ravi"); c.shout()                    # 'RAVI'
```
`DECISION: class when state + behavior belong together; plain transform → a function.`

### 🟦 EXCEPTION (controlled failure)
```python
try: amount=float(row["amount"])
except ValueError: amount=None; log.warning("bad amount")
```
`WATCH: never bare 'except: pass' — it hides real bugs.`

### 🟦 MODULE & PACKAGE / ENVIRONMENT
`from utils import calculate_profit` · module = .py file, package = folder of modules.
```bash
python -m venv .venv; source .venv/bin/activate; pip install -r requirements.txt
```
`MEM: a venv = a clean room per project, so versions never fight.`

### 🟦 GENERATOR (one at a time)
```python
def read_big(path):
    with open(path) as f:
        for line in f: yield line     # hand one out, pause, resume
```
Perfect for files too big for RAM.

---
---

# PART B — DATA · PROCESS · AUTOMATE (the engineer's toolkit)

## 📁 FILES
```python
with open("sales.txt") as f: data=f.read()   # 'with' auto-closes safely
```

## 🧾 CSV / JSON / EXCEL
```python
import csv, json
# csv.DictReader(f) -> each row a dict; json.loads/dumps; pandas.read_excel (needs openpyxl)
```
`JSON object {} ⇄ Python dict`; `JSON array [] ⇄ Python list`.

## 🐼 PANDAS — the data engine
```python
import pandas as pd
df = pd.read_csv("sales.csv")
df[(df["units"]>0) & (df["region"]=="West")]      # filter (use &, not 'and')
df["revenue"] = df["units"] * df["unit_price"]     # new column (vectorized)
df.groupby("region")["revenue"].sum()              # aggregate
pd.merge(sales, customers, on="customer_id", how="left")   # join
df["units"].fillna(0); df.drop_duplicates("order_id")
```
`WATCH: pandas works in memory on ONE machine — huge data needs a DB/Spark.`

### PANDAS ⇄ SQL
```
SELECT cols → df[["a","b"]]   WHERE → df[df.x>5]   GROUP BY → df.groupby("x")
JOIN → pd.merge(a,b,on=,how=) ORDER BY → df.sort_values("x")   DISTINCT → df.drop_duplicates()
```

## 🔢 NUMPY
`np.array([1,2,3]) * 2` → vectorized fast math. pandas is built on NumPy.

## 🌐 APIs
```python
import requests
r = requests.get(url, params={...}, headers={"Authorization":f"Bearer {key}"}, timeout=10)
r.raise_for_status(); data = r.json()
```
`GET=read POST=create PUT/PATCH=update DELETE=remove` · `200 ok·400 bad·401 unauth·404 missing·429 rate·500 server`
`WATCH: always set a timeout; handle failures; retry with backoff; respect rate limits.`

## 🗄️ DATABASES + PYTHON
```python
import sqlite3, pandas as pd
with sqlite3.connect("sales.db") as conn:
    df.to_sql("sales", conn, if_exists="replace", index=False)
    top = pd.read_sql_query("SELECT customer_id, SUM(revenue) r FROM sales GROUP BY customer_id ORDER BY r DESC LIMIT ?", conn, params=(3,))
```
`WATCH (security): use parameters (?), never f-string values into SQL — SQL injection.`

## 🤖 AUTOMATION
```python
from pathlib import Path; import shutil
for csv in Path("data").glob("*.csv"):
    ...
    shutil.move(str(csv), f"archive/{csv.name}")
```

## 📣 LOGGING
```
DEBUG < INFO < WARNING < ERROR < CRITICAL
```
```python
import logging; logging.basicConfig(level=logging.INFO)
log=logging.getLogger("job"); log.info("started"); log.error("file missing")
```
`MEM: print is for learning; logging is for production.`

## 🔐 CONFIG & SECURITY
```python
import os; password=os.getenv("DB_PASSWORD")   # never hardcode/commit secrets
```

## ✅ TESTING
```python
def test_profit(): assert calculate_profit(200,120)==80
```
Test edges (zero, negative, empty, bad type). Run with `pytest -q`.

---
---

# PART C — CAPSTONE: SALES AUTOMATION & ANALYTICS

> *"Every morning, automatically collect sales data, clean it, identify bad records, calculate KPIs, store the results in a database, and generate a management report."*

```
BUSINESS REQUEST
  ─► EXTRACT (csv/excel/api/db)
  ─► VALIDATE + CLEAN (nulls, dupes, bad dates, negative sales, bad types)
  ─► TRANSFORM (revenue, cost, profit, margin, AOV, by customer/region/product/month)
  ─► BUSINESS RULES (profit<0→LOSS · margin>30%→HIGH_MARGIN · sales>target→MET)
  ─► LOAD (SQLite) ─► QUERY BACK ─► REPORT ─► EMAIL ─► LOG every step ─► handle every failure
```
**Production-readiness checklist:** error handling · logging · config · secrets via env · tests · docs/README · reusable functions · clean structure · requirements.txt · performance aware · parameterized SQL · monitoring/log trail.

---
---

# PART D — SCALE, STRUCTURE & SHIP

## ⚡ PERFORMANCE
```
Is it actually slow? ─► MEASURE ─► find bottleneck ─► optimize ─► MEASURE again
```
`set` lookup ≈ instant vs scanning a list · avoid needless loops · vectorize in pandas · generators for big streams · push work to the database · profile before guessing.

## 🧵 CONCURRENCY (concept)
I/O-bound (many API/file waits) → threads/async. CPU-bound (heavy math) → multiprocessing.

## 🏗️ PROJECT STRUCTURE
```
sales_automation/
├ src/   main.py extract.py transform.py load.py utils.py
├ tests/ · config/ · data/ · logs/
├ requirements.txt  README.md  .gitignore
```

## 🌿 GIT / 🚀 DEPLOYMENT / 🐳 DOCKER
`add/commit → branch → PR → review → merge` (never commit secrets). Deploy shapes: scheduled job (cron/Airflow/Databricks Job), server, container, cloud function. Docker = code+deps+env → one container that runs the same everywhere.

## 🔗 WHERE PYTHON SITS
```
SOURCES ─► PYTHON ─► SQL ─► DATA PLATFORM ─► BI (Power BI)
Python = glue/logic/automation/APIs · SQL = set-based · Pandas = local/moderate · PySpark/Databricks = big distributed
```

---
---

# PART E — RECALL ENGINE

## 🧭 DECISION TREE
```
store many ordered/changeable → LIST · unique → SET · fixed → TUPLE · key→value → DICT
repeat → FOR/WHILE · decide → IF · reuse logic → FUNCTION · tables → PANDAS
talk to a system → API · read a file → FILE/PANDAS · records → DATABASE+SQL
might fail → TRY/EXCEPT · run on a schedule → SCRIPT+SCHEDULER · huge data → DB/SPARK
```

## 🧩 BUSINESS-PROBLEM FRAMEWORK
```
BUSINESS QUESTION ─► INPUT? ─► OUTPUT? ─► STEPS? ─► DATA STRUCTURES? ─► FUNCTIONS?
 ─► EXTERNAL SYSTEMS? ─► WHAT CAN FAIL? ─► HOW TO TEST? ─► HOW TO DEPLOY?
```

## ❓ THE 10 QUESTIONS BEFORE WRITING CODE
1 What problem? 2 Input? 3 Output? 4 Split into functions? 5 Which data structure? 6 What can go wrong? 7 How to test? 8 Once or repeatedly? 9 Maintained by someone? 10 How runs in production?

## 🐞 DEBUGGING MIND MAP
```
BUG → ERROR (read message) / WRONG RESULT (check logic) → ISOLATE → FIX → TEST → PREVENT
```
Read a traceback **bottom-up**: last line = what broke; lines above = where.

## 📄 ONE-PAGE CHEAT SHEET
```
VARIABLE store · IF decide · FOR each · WHILE repeat · LIST ordered · TUPLE fixed
SET unique · DICT key→value · FUNCTION reusable · RETURN give back · CLASS blueprint
TRY/EXCEPT handle errors · MODULE reusable file · PANDAS tables · API program↔program
JSON structured · LOGGING app diary · TEST verify · ENV clean room · GENERATOR one-at-a-time
=assign · ==compare · is→only for None · &,| in pandas (not and/or)
```

## 🧠 MASTER RECALL MAP
```
PYTHON ─► PROBLEM ─► INPUT ─► DATA STRUCTURES ─► CONDITIONS ─► LOOPS ─► FUNCTIONS
 ─► FILES/APIs/DATABASES ─► PANDAS ─► ANALYSIS ─► AUTOMATION ─► ERROR HANDLING
 ─► TESTING ─► LOGGING ─► PROJECT STRUCTURE ─► DEPLOYMENT ─► PRODUCTION
```

## 🏁 PROJECT-READY TEST
You're ready not when you can define "list," but when handed an **unfamiliar** requirement you independently: understand it → define input/output → break into steps → choose data structures → write functions → read data → transform → connect API/DB → handle errors → log → test → debug → optimize → package → deploy → monitor. That reflex — **think in Python** — is the whole point. 🚀
