# Reproduction

```
cd E:\fenta\Downloads\The Anticloud\ANTICLOUD_REPOS\ONLINE_RETAIL\PYTHON_API_CLIENT_PRIOR_DOCS\anticloud
python tools\run_bench.py --out E:\fenta\Downloads\The Anticloud\ANTICLOUD_REPOS\ONLINE_RETAIL\PYTHON_API_CLIENT_PRIOR_DOCS\ISOLATED_LAB_RESULTS\04_Evidence\16_checks_report.json
```

Run twice: pass 1 generates BENCH.json-cited evidence,
pass 2 records. Exit code 0 only when all 16 checks PASS. The
report JSON at the --out path carries each check's evidence.
