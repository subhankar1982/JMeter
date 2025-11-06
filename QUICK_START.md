# JMeter Quick Reference Guide

## Quick Start

### 1. Run HTTP Performance Test
```bash
# GUI mode (for editing)
jmeter -t test-plans/http/http_performance_test.jmx

# Non-GUI mode (for execution)
jmeter -n -t test-plans/http/http_performance_test.jmx -l results/http_test.jtl -e -o results/http_report
```

### 2. Run REST API Test
```bash
# This test uses a public API (jsonplaceholder.typicode.com) - ready to run!
jmeter -n -t test-plans/api/rest_api_test.jmx -l results/api_test.jtl -e -o results/api_report
```

### 3. Run Database Test
```bash
# First: Configure your database connection in the test plan
jmeter -t test-plans/database/database_test.jmx

# Then run in non-GUI mode
jmeter -n -t test-plans/database/database_test.jmx -l results/db_test.jtl -e -o results/db_report
```

## Common Customizations

### Change Number of Users
```bash
jmeter -n -t test-plans/http/http_performance_test.jmx \
  -JNUM_THREADS=50 \
  -l results/http_test.jtl
```

### Change Test Duration
```bash
jmeter -n -t test-plans/http/http_performance_test.jmx \
  -JDURATION=600 \
  -l results/http_test.jtl
```

### Change Target URL
```bash
jmeter -n -t test-plans/http/http_performance_test.jmx \
  -JBASE_URL=http://your-server.com \
  -l results/http_test.jtl
```

### Multiple Parameters
```bash
jmeter -n -t test-plans/http/http_performance_test.jmx \
  -JBASE_URL=http://your-server.com \
  -JNUM_THREADS=100 \
  -JRAMP_TIME=120 \
  -JDURATION=600 \
  -l results/http_test.jtl \
  -e -o results/http_report
```

## Performance Test Execution Steps

1. **Start small** - Begin with 1-5 users
2. **Verify functionality** - Ensure tests pass with small load
3. **Increase gradually** - Incrementally increase user count
4. **Monitor resources** - Watch server CPU, memory, disk I/O
5. **Identify bottlenecks** - Analyze results for slow requests
6. **Optimize and repeat** - Make improvements and retest

## Key Metrics to Monitor

- **Response Time**: Average, median, 90th percentile, 95th percentile
- **Throughput**: Requests per second
- **Error Rate**: Percentage of failed requests
- **Active Threads**: Number of concurrent users
- **Bandwidth**: Network usage

## Result Analysis

### View HTML Report
After running a test with `-e -o results/report_dir`, open:
```
results/report_dir/index.html
```

### Key Sections in HTML Report
- **Dashboard**: Overview with key metrics
- **APDEX**: Application Performance Index
- **Requests Summary**: Per-request statistics
- **Statistics**: Detailed metrics table
- **Response Times Over Time**: Performance trends
- **Errors**: Failed requests and error messages

## Tips for Better Tests

1. **Use realistic think times** - Add delays between requests
2. **Parameterize data** - Use CSV files for varied test data
3. **Add assertions** - Verify response correctness
4. **Test gradually** - Ramp up load slowly
5. **Clean between runs** - Clear result files
6. **Use Non-GUI mode** - GUI consumes resources
7. **Monitor both sides** - Watch client and server resources

## Environment Variables

Set JMeter heap size for large tests:
```bash
export HEAP="-Xms2g -Xmx4g"
```

Or for a single run:
```bash
JVM_ARGS="-Xms2g -Xmx4g" jmeter -n -t test-plan.jmx -l results.jtl
```

## Troubleshooting

### Test fails immediately
- Check target URL/host is accessible
- Verify port numbers
- Check firewall settings

### OutOfMemoryError
- Increase heap size (see above)
- Use non-GUI mode
- Reduce logging level
- Disable unnecessary listeners

### Slow execution
- Use non-GUI mode
- Reduce number of listeners
- Don't save response data unless needed
- Use summary results instead of detailed logs

## Next Steps

1. Customize test plans for your specific application
2. Add more realistic test scenarios
3. Integrate with CI/CD pipeline
4. Set up monitoring and alerting
5. Establish performance baselines
6. Create load profiles based on production traffic
