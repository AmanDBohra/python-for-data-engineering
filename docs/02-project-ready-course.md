# 🐍 From Layman to Production Data Engineer — The 6-Hour Python Course

> **Profile:** complete beginner · Python first language · SQL/PySpark/Databricks new · goal = Data Engineer (Databricks), plus analytics & automation.
> **How to use:** written companion to the live training. Read a section, then do the exercise in the running project (`ecommerce_pipeline/`). One running story — a fictional online store.

---

## 🗺️ The one mental model

```
   BUSINESS REQUIREMENT
          │
   INPUT ─► READ ─► VALIDATE ─► CLEAN ─► TRANSFORM ─► AGGREGATE
                                                          │
                                   OUTPUT ◄─ (log everything, handle every error)
                                      │
                       small data? ─► pandas        BIG data? ─► PySpark / Databricks
```
When lost: *"which brick am I holding, and where does it snap in?"*

**The Golden Rule:** Python is *understand the requirement → design → choose data structures → write functions → process data → handle errors → test → log → optimize → deploy → monitor.* If you write code you can't explain, you've memorized — stop and understand it.

## 🏪 Running project: the E-Commerce store
Five deliberately-dirty CSVs (customers, orders, products, payments, inventory) with blanks, a negative amount, `abc` where a number should be, a duplicate customer, an orphan order. **Mission (by Hour 6):** read → reject bad rows → compute each customer's revenue → flag VIPs → write a report → log every step → survive missing files → test it → scale with PySpark.

---
---

# ⏱️ HOUR 1 — FOUNDATIONS
*Variables, types, operators, strings, if/else, loops, lists, dicts.*

**Program = recipe.** `INPUT → PROCESS → OUTPUT`. Python reads top to bottom, extremely literally.

**Variables** = labeled boxes. `=` assigns (not "equals"). Dynamically typed. `lower_snake_case`.

**Types:** str, int, float, bool, None; collections list `[]`, tuple `()`, set `{}`, dict `{:}`. `type(x)`, `int("5")`.

**Operators:** `+ - * / % ** //` · comparison `== != > < >= <=` · logical `and or not` · membership `in` · identity `is` (**only** for `None`).
> `==` equal value; `is` same object. Use `is` only for `None`.

**Strings:** `name[0]`, `name[0:4]`, `.upper()`, `.strip()` (cleaning!), `"a,b".split(",")`, f-strings `f"{name} spent ₹{total}"`, `f"{total:,.2f}"`.

**if/elif/else** — indentation (4 spaces) defines what's inside; the #1 beginner bug.

**Loops:** `for` when you know the collection; `while` until a condition flips (must change the condition). `range(5)`, `range(2,10,2)`. `break`/`continue`/`pass`.

**Lists:** `append insert remove pop sort` · `a[0] a[-1] a[1:3] len(a)`. Comprehension: `[o*1.18 for o in orders if o>10000]`.

**Dictionaries** (DE workhorse): `customer["name"]` (crashes if missing) vs `customer.get("phone","NA")` (safe). `.keys() .values() .items()`. JSON, API responses, config, one row.

**list vs tuple vs set vs dict:** ordered/changeable? → list; fixed → tuple; unique → set; key/value → dict. `list(set(cities))` dedupes.

### 🏋️ Exercises
Order total · HIGH/NORMAL tag · highest in a list · sum ≥1000 · dedupe cities · safe dict get · count by status · comprehension >10000 · uppercase names · f-string summary.

---
---

# ⏱️ HOUR 2 — FUNCTIONS · FILES · CSV · JSON

**Functions** = machines: input → job → output.
```python
def calculate_total(price, quantity): return price * quantity
```
- **parameters** = names in the definition; **arguments** = real values passed; **return** = sends a value back; locals vanish when the function ends.

**Parameters:** positional, `quantity=1` default, keyword args, `*args` (tuple), `**kwargs` (dict).

**return vs print (critical):** `print` SHOWS and returns None; `return` SENDS a value other code can use. `good(100,5)*2` works; `bad(100,5)*2` crashes.

**Function design — small is strong:** `read_data() → validate_data() → transform() → write()`. One thing each, clear name, testable.

**Scope:** prefer passing in / returning out; avoid `global`.

**Lambda:** `sorted(customers, key=lambda c: c["revenue"], reverse=True)` — short one-liners only.

**map/filter/reduce:** transform/select/combine. A readable loop or comprehension is often clearer; know them for interviews.

**File handling:** `with open("orders.csv") as f: text=f.read()` — `with` auto-closes. Modes `r w a`.

**CSV:** `csv.DictReader(f)` (each row a dict), `csv.DictWriter(f, fieldnames=[...])`. Mini pipeline: CSV → read → validate → transform → write.

**JSON:** `json.loads` (str→dict), `json.dumps` (dict→str), `json.load/dump` (files). JSON `{}`↔dict, `[]`↔list.

### 🏋️ Exercises
`calculate_total` with default · `categorize` returning (not printing) · `clean_name` strip+title · DictReader print cities · count per status · dict↔JSON · list-of-dicts → CSV · refactor one big function into read/validate/transform/write · `sorted(key=lambda)` rank by revenue · explain parameter vs argument.

---
---

# ⏱️ HOUR 3 — ERRORS · DEBUGGING · LOGGING · OOP · MODULES · VENV

**Exceptions** — errors are normal in production (missing file, bad row, API down). Good code *expects* this.
```python
try: amount = float(row["amount"])
except ValueError: amount=None; log.warning("Bad amount: %s", row)
else: log.info("Parsed %s", amount)
finally: pass
```
**Common types:** ValueError, TypeError, KeyError, IndexError, FileNotFoundError, ZeroDivisionError, ImportError. Skill > memory: **read the error.**

**Debugging method:** read the LAST line (actual error) → find file+line → check variable values → reproduce tiny → fix → test → log. Read tracebacks **bottom-up**.

**Custom exceptions:** `class InvalidOrderError(Exception): ...` — callers react differently to *your* errors.

**Logging (MANDATORY):** DEBUG<INFO<WARNING<ERROR<CRITICAL.
```python
import logging
logging.basicConfig(level=logging.INFO, format="%(asctime)s | %(levelname)s | %(message)s")
log = logging.getLogger("pipeline"); log.info("Pipeline started")
```
Logging gives timestamps, levels, files, dial-down in prod. `print` can't.

**OOP (just what a DE needs):**
```python
class Customer:
    def __init__(self, customer_id, name): self.customer_id=customer_id; self.name=name
    def display(self): return f"{self.customer_id}: {self.name}"
```
`self` = this object; `__init__` sets up data; a method is a function on the object. Vocabulary: attribute, method, constructor, inheritance, encapsulation, composition.
> **Use a class when state + behavior belong together. For a plain input→output transform, a function is better.** Unnecessary classes are a code smell.

**Modules & packages:** `from reader import read_csv`; package = folder of modules (+ `__init__.py`) = your `src/`.

**Virtual environments (MANDATORY habit):**
```bash
python -m venv .venv; source .venv/bin/activate
pip install pandas pytest; pip freeze > requirements.txt; pip install -r requirements.txt
```
Reproducibility — "works on my machine" dies here. Databricks uses cluster libs / `%pip install` for the same reason.

### 🏋️ Exercises
try/except skip bad rows · trigger & handle FileNotFoundError · read a traceback · add logging · `Customer` class with `is_vip(threshold)` · function vs class with example · split one file into reader.py + main.py · venv + requirements.txt · custom `InvalidOrderError` · why logging beats print.

---
---

# ⏱️ HOUR 4 — PANDAS · APIs · AUTOMATION

**pandas** = tables (DataFrame) — "a spreadsheet you drive with code" / "SQL table in memory".
```python
df = pd.read_csv("data/orders.csv")
df.head(); df.shape; df.columns; df.dtypes; df.info(); df.describe()
df[(df["amount"]>10000) & (df["status"]=="COMPLETED")]   # & | , wrap each ()
df["amount"]                       # Series   df[["customer_id","amount"]]  # DataFrame
df["total"] = df["price"]*df["quantity"]                  # vectorized
df.groupby("customer_id")["amount"].sum()                 # GROUP BY
pd.merge(orders, customers, on="customer_id", how="inner")# JOIN (inner/left/right/outer)
df.isna().sum(); df["amount"].fillna(0); df.dropna(subset=["amount"])
df.drop_duplicates(subset=["customer_id"])                # DISTINCT
```
> Don't blindly `fillna(0)` — a missing price filled with 0 silently corrupts revenue.
**Reality:** pandas is in-memory, one machine. 50 GB file crashes a laptop → that's why PySpark exists.

**APIs with requests:**
```python
resp = requests.get(url, params={...}, headers={"Authorization": f"Bearer {token}"}, timeout=10)
resp.raise_for_status(); data = resp.json()
```
Codes: 200/201/400/401/404/429/500. Handle `requests.Timeout` and `requests.HTTPError`. **Always set a timeout** or a hang freezes the pipeline.

**Config & secrets:** `os.getenv("DB_PASSWORD")` — never hardcode (git history is forever).

**Automation:** `pathlib` over `os.path`. `Path("data").glob("*.csv")`, `Path("archive").mkdir(exist_ok=True)`, `shutil.move(...)`. Typical jobs: find new files → process → report → archive → log.

### 🏋️ Exercises
Load + shape/columns/dtypes · filter >10k COMPLETED · total + HIGH/NORMAL · groupby revenue sorted · merge (which how & why) · handle missing amount sensibly · dedupe customers · `requests.get` with params/headers/timeout/errors · API key from env · automate: glob CSVs, row counts, archive.

---
---

# ⏱️ HOUR 5 — PYSPARK · TESTING · CODE QUALITY · PROJECT STRUCTURE

**Why PySpark:** pandas = one machine's RAM; Spark splits data into **partitions** across a **cluster**. Same thinking: read, filter, group, join, write.

| pandas | PySpark |
|---|---|
| `df[df.x>5]` | `df.filter(col("x")>5)` |
| `df["y"]=...` | `df.withColumn("y",...)` |
| `df.groupby("id").sum()` | `df.groupBy("id").agg(spark_sum("x"))` |
| `pd.merge(a,b)` | `a.join(b, on="id")` |
| runs immediately | **lazy** until an action |

```python
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("ecom").getOrCreate()   # on Databricks: already exists
df = spark.read.csv("data/orders.csv", header=True, inferSchema=True)
```
**Transformations** (build a plan): select, filter(col()>n), withColumn, drop, groupBy().agg(), join, orderBy. **Actions** (run it): show, count, collect ⚠️ (pulls all rows to driver — crash risk), write.parquet.

**Lazy evaluation:** Spark waits, sees the whole chain, optimizes, then runs on the first action.

**Window functions:**
```python
from pyspark.sql.window import Window
from pyspark.sql.functions import row_number, col
w = Window.partitionBy("city").orderBy(col("amount").desc())
df.withColumn("rank_in_city", row_number().over(w))
```

**Performance basics:** partition = a chunk on one worker; shuffle = moving data between workers (groupBy/join — expensive, minimize); broadcast join = copy a small table to every worker; cache a reused DataFrame; repartition/coalesce; predicate pushdown (filter early). **Rule: filter early, shuffle rarely, broadcast small tables.**

**Testing with pytest:** unit (one function), integration (parts together), data test (no nulls in key, etc.).
```python
def test_categorize_orders():
    df = pd.DataFrame({"amount":[5000,15000]})
    out = categorize_orders(df, 10000)
    assert list(out["order_category"]) == ["NORMAL","HIGH"]
```
Test the edges (zero, negative, decimal, invalid), not just the happy path. **Mocking:** replace API/DB/cloud with a fake returning a canned response.

**Code quality:** PEP 8 (4-space, snake_case, spaces around operators); descriptive names; small functions; no copy-paste; type hints (document intent, not enforced at runtime); docstrings (explain *why*).

**Project structure:** `src/ tests/ spark/ data/ logs/ output/ requirements.txt README.md`.

### 🏋️ Exercises
Explain lazy eval simply · rewrite a pandas filter+groupby as PySpark · why collect() is dangerous · window rank by revenue within city · pytest for `categorize_orders` + edge · 5 things that make Spark slow + fixes · add type hints + docstring · spot 3 PEP-8 issues · unit vs integration vs data test · what to mock testing the API-reader.

---
---

# ⏱️ HOUR 6 — THE REAL JOB: PROJECT · DEBUGGING · INTERVIEW · SCORING

**Requirement (PM phrasing):** "Read customer and order data, validate and clean it, calculate business metrics, generate output, log execution, handle errors, migrate later to Databricks/PySpark."
```
BUSINESS ─► INPUT ─► VALIDATE ─► CLEAN ─► TRANSFORM ─► BUSINESS LOGIC ─► OUTPUT ─► LOGGING ─► ERROR HANDLING ─► MONITORING
```

**Tickets (the shipped `src/` is the reference):** T001 Read · T002 Validate · T003 Clean · T004 Transform (total/revenue/category/segment) · T005 Aggregate · T006 Write (CSV/JSON/**Parquet**) · T007 Logging · T008 Error handling · T009 Testing · T010 PySpark migration. *Logic identical; only the engine changes.*

**Debugging drills:** indentation · wrong variable · KeyError · NoneType · off-by-one · **mutable default arg** · swallowed exception · path problem · wrong filter · `collect()` on huge data.

**Gotchas:**
```python
def add(item, bucket=None):   # ✅ fix the mutable-default bug
    if bucket is None: bucket = []
    bucket.append(item); return bucket
```
`is` vs `==` (identity vs value; `is` only for None) · truthiness (`0,"",[],{},None` falsy; `if orders:` = non-empty) · `None` ≠ `False`.

**Memory model:**
```python
a=[1,2,3]; b=a; b.append(4); print(a)   # [1,2,3,4] — one list, two labels
```
Variable = a label pointing at an object. True copy: `a.copy()` / `copy.deepcopy(a)`.

**Common production mistakes:** hardcoded creds/paths · 300-line functions · no error handling/logging/tests · globals · duplicated code · poor naming · needless classes · inefficient loops · loading huge files into memory · no validation · ignoring API timeouts · `except: pass`.

**Performance:** measure → bottleneck → optimize → measure again. `set` lookup vs list scan · generators · vectorize · Spark tuning. Correct and readable first.

**Generators (one at a time):**
```python
def read_big_file(path):
    with open(path) as f:
        for line in f: yield line
```

**Python + SQL + Databricks:** SQL for set logic, Python/PySpark for control flow + APIs + orchestration, Delta Lake tables, notebooks, scheduled jobs.

## 🎤 INTERVIEW BANK (run live, scored)
R1 Fundamentals · R2 Data structures · R3 Functions · R4 OOP · R5 Pandas · R6 PySpark (15: Spark vs pandas, transformation vs action, lazy eval, partition, shuffle, broadcast, collect danger, cache, window, repartition vs coalesce, inferSchema cost, narrow vs wide, Databricks run, Delta, debug slow job) · R7 Debugging · R8 Real-world DE (15: daily ingest, schema change overnight, corrupt file mid-batch, idempotency, dedupe at scale, late data, partitioning, CSV/Parquet/JSON, secrets, monitoring, retry/backoff, small-files, backfill, DQ checks, test the pipeline).

## 🧪 CODING TESTS
Beginner 10 · Intermediate 15 · Advanced 10 (generator, timing decorator, custom exception hierarchy, class with validation, memoized Fibonacci, context manager, dedupe preserving order, group-by from scratch, safe nested config getter, small ETL) · Data engineering 15 (pandas revenue by customer/month, merge, dedupe, nulls, pivot, Parquet; PySpark groupby, join, window top-N, lazy-vs-action, partitioned write, broadcast, DQ check) · Debugging 10.

## 📋 CODE REVIEW RUBRIC
Correctness · Readability · Maintainability · Performance · Error handling · Testing · Security · Production-ready (each /10) → "would I approve this in a production PR?"

## 📚 SURVIVAL KIT — 26 CHEAT SHEETS (abbrev.)
Syntax · types · list · dict · set · tuple · functions · lambda · files · JSON · CSV · exceptions · logging · OOP · modules · venv · pandas · API · automation · PySpark · testing · debugging · performance · interview · DE-python · production checklist.

## 🧭 FLOWCHARTS
```
DEBUG: read error → file+line → check input → values → type → function → exception → reproduce → fix → test → log
PIPELINE: input? → schema? → data valid? → transform? → output written? → logging complete? → SUCCESS
```

## 📊 PROJECT-READINESS SCORE (/200, honest)
Fundamentals, data structures, functions, control flow, file handling, exceptions, logging, OOP, modules, venv, pandas, APIs, automation, testing, debugging, code quality, performance, PySpark, data engineering, production readiness (each /10).
`0–80 Beginner · 81–120 Learning · 121–145 Junior · 146–165 Project Ready · 166–185 Strong · 186–200 Advanced.`

## ✅ Final objective
Given *"Every day, ingest customer and order files, validate schema, reject invalid records, remove duplicates, calculate customer revenue, identify inactive customers, generate a daily report, write to storage, and scale to millions"* — your brain auto-draws the INPUT→READ→VALIDATE→CLEAN→TRANSFORM→AGGREGATE→OUTPUT→LOG→ERROR-HANDLING→(PySpark if large)→DEPLOY→MONITOR skeleton. That reflex is the whole point. 🚀
