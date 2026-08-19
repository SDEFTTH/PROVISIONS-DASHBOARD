# BSNL FTTH Warangal OA Dashboard V5 – Pandas + External Masters

## Key design change

The application no longer contains built-in OLT IP, BBC, DE/Manager, Area/TIP or target mappings.

Keep these two Excel files in the **same GitHub repository folder as `app.py`**:

- `OLT_BBC_MAP.xlsx`
- `BBC_Master.xlsx`

When the mapping changes, replace those two files in GitHub and redeploy/restart Streamlit. **No Python code change is required.**

## Master file formats

### OLT_BBC_MAP.xlsx

Required columns:

| OLT IP | BBC Name |
|---|---|
| 10.x.x.x | Employee Name |

Accepted aliases include `OLT_IP`, `OLT`, `IP`, `BBC`, `BBM Name`, `Employee`.

### BBC_Master.xlsx

Required columns:

| BBC Name | DE / Manager | Area / TIP | Monthly Target |
|---|---|---|---:|
| Employee Name | Manager Name | TIP-1 | 100 |

Optional columns:

- `Manager Target`
- `Display Name`
- `S.No` / `Order`

Several common column-name variations are accepted automatically.

## Operational logic

The FTTH source workbook is read with **pandas**. Classification and aggregation are vectorized/groupby based.

Connection classification:

- `CLSR = ACTIVE` + Order ID starts `BFBNC` → **NPC**
- `CLSR = ACTIVE` + other Order ID → **RECONNECTION**
- `CLSR = CLSD` + Order ID starts `BFBDI` → **CLSNP**
- `CLSR = CLSV` + Order ID starts `BFBDI` → **CLSVO**

## Excel output

`FTTHDashboard` now ends with:

1. NET
2. **NPC**
3. **RECONNECTIONS**

The same columns are present in the standalone HTML dashboard.

Additional sheets:

- Dashboard
- FTTHDashboard
- Data
- Manager_Report
- Franchisee_Report
- OLT_BBC_Map
- BBC_Master
- Charts_BBC
- Charts_Operations

The generated workbook uses **Pandas + XlsxWriter**. Excel-native table filters are included.

## Run locally

```bash
pip install -r requirements.txt
streamlit run app.py
```

## Streamlit Cloud

Commit/push:

```text
app.py
report_processor.py
requirements.txt
OLT_BBC_MAP.xlsx
BBC_Master.xlsx
```

Set the Streamlit main file to `app.py`.

Whenever the master data changes, replace the two Excel master files in GitHub and redeploy/restart the app.
