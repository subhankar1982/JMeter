# Results Directory

This directory is used to store JMeter test results.

## File Types

- `*.jtl` - JMeter Test Log files containing raw test results
- `*.log` - JMeter log files
- `*.csv` - CSV formatted results (if configured)
- HTML reports - Generated HTML dashboard reports

## Note

This directory should be kept clean between test runs to avoid confusion and disk space issues. Result files are automatically ignored by git (see `.gitignore`).

## Generating HTML Reports

To generate an HTML dashboard report from a JTL file:

```bash
jmeter -g results/test_results.jtl -o results/html_report
```

This will create a comprehensive HTML report with graphs, statistics, and detailed analysis.
