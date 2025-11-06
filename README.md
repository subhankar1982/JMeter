# JMeter Performance Testing Scripts

This repository contains a collection of Apache JMeter test scripts for performance testing various types of applications including web applications, REST APIs, and databases.

## Directory Structure

```
.
├── test-plans/
│   ├── http/                 # HTTP/Web application tests
│   │   └── http_performance_test.jmx
│   ├── api/                  # REST API tests
│   │   └── rest_api_test.jmx
│   └── database/             # Database performance tests
│       └── database_test.jmx
├── config/                   # Configuration files
│   └── test.properties
└── results/                  # Test results directory (generated)
```

## Prerequisites

1. **Apache JMeter** - Download and install from [https://jmeter.apache.org/](https://jmeter.apache.org/)
2. **Java JDK** - JMeter requires Java 8 or higher
3. **JDBC Driver** (for database tests) - Download the appropriate JDBC driver for your database

## Test Plans

### 1. HTTP Performance Test (`test-plans/http/http_performance_test.jmx`)

This test plan simulates multiple users accessing a web application.

**Features:**
- Configurable number of threads (virtual users)
- Ramp-up time for gradual load increase
- Duration-based test execution
- Response code assertions
- Think time simulation

**Configuration Variables:**
- `HOST`: Target server hostname (default: `localhost`)
- `PORT`: Target server port (default: `8080`)
- `PROTOCOL`: Connection protocol (default: `http`)
- `NUM_THREADS`: Number of concurrent users (default: 10)
- `RAMP_TIME`: Time to ramp up all threads in seconds (default: 60)
- `DURATION`: Test duration in seconds (default: 300)

**Usage:**
```bash
# GUI Mode
jmeter -t test-plans/http/http_performance_test.jmx

# Non-GUI Mode (for actual test execution)
jmeter -n -t test-plans/http/http_performance_test.jmx -l results/http_results.jtl -e -o results/http_report

# Override variables
jmeter -n -t test-plans/http/http_performance_test.jmx \
  -JHOST=example.com \
  -JPORT=80 \
  -JPROTOCOL=http \
  -JNUM_THREADS=20 \
  -l results/http_results.jtl
```

### 2. REST API Test (`test-plans/api/rest_api_test.jmx`)

This test plan performs CRUD operations on a REST API with JSON payloads.

**Features:**
- GET, POST, PUT, and DELETE requests
- JSON payload handling
- Response code assertions
- JSON path assertions
- Header management

**Configuration Variables:**
- `API_HOST`: API server hostname (default: `jsonplaceholder.typicode.com`)
- `API_PORT`: API server port (default: 443)
- `NUM_THREADS`: Number of concurrent users (default: 5)
- `RAMP_TIME`: Ramp-up time in seconds (default: 30)
- `LOOP_COUNT`: Number of iterations per thread (default: 10)

**Usage:**
```bash
# GUI Mode
jmeter -t test-plans/api/rest_api_test.jmx

# Non-GUI Mode
jmeter -n -t test-plans/api/rest_api_test.jmx -l results/api_results.jtl -e -o results/api_report

# Override variables
jmeter -n -t test-plans/api/rest_api_test.jmx \
  -JAPI_HOST=api.example.com \
  -JNUM_THREADS=10 \
  -l results/api_results.jtl
```

### 3. Database Test (`test-plans/database/database_test.jmx`)

This test plan performs database operations using JDBC.

**Features:**
- SELECT, INSERT, UPDATE, and DELETE queries
- JDBC connection pooling
- Parameterized queries
- Response assertions

**Configuration Variables:**
- `DB_URL`: JDBC connection URL (default: `jdbc:mysql://localhost:3306/testdb`)
- `DB_USER`: Database username (default: `testuser`)
- `DB_PASSWORD`: Database password (default: `password`)
- `NUM_THREADS`: Number of concurrent connections (default: 10)
- `LOOP_COUNT`: Number of query executions per thread (default: 100)

**Setup:**
1. Download the JDBC driver for your database (e.g., MySQL Connector/J)
2. Place the driver JAR file in the `lib` directory of your JMeter installation

**Usage:**
```bash
# GUI Mode
jmeter -t test-plans/database/database_test.jmx

# Non-GUI Mode
jmeter -n -t test-plans/database/database_test.jmx -l results/db_results.jtl -e -o results/db_report

# Override variables
jmeter -n -t test-plans/database/database_test.jmx \
  -JDB_URL=jdbc:mysql://dbserver:3306/mydb \
  -JDB_USER=admin \
  -JDB_PASSWORD=secret \
  -l results/db_results.jtl
```

## Using Configuration Files

You can use the `config/test.properties` file to manage test parameters:

```bash
jmeter -n -t test-plans/http/http_performance_test.jmx \
  -p config/test.properties \
  -l results/http_results.jtl
```

## Viewing Results

### GUI Mode
- Open JMeter in GUI mode
- Load the test plan
- View results in the "View Results Tree" or "Summary Report" listeners

### Non-GUI Mode (Recommended for actual testing)
```bash
# Run test with HTML report generation
jmeter -n -t <test-plan.jmx> -l results/results.jtl -e -o results/html_report

# Open the generated HTML report
open results/html_report/index.html  # macOS
xdg-open results/html_report/index.html  # Linux
start results/html_report/index.html  # Windows
```

## Best Practices

1. **Always use Non-GUI mode for actual load testing** - GUI mode is resource-intensive
2. **Set realistic ramp-up times** - Avoid sudden load spikes
3. **Monitor system resources** - Use tools like `top`, `htop`, or monitoring solutions
4. **Use assertions wisely** - Too many assertions can impact performance
5. **Clean up result files** - Large result files can impact performance
6. **Test incrementally** - Start with a small load and gradually increase
7. **Use distributed testing** - For high load scenarios, use JMeter's distributed testing feature

## Common JMeter Commands

```bash
# Run test in GUI mode
jmeter

# Run test in non-GUI mode
jmeter -n -t <testplan.jmx> -l <results.jtl>

# Run test with property file
jmeter -n -t <testplan.jmx> -p <config.properties> -l <results.jtl>

# Generate HTML report from existing results
jmeter -g <results.jtl> -o <output_folder>

# Run in server mode (for distributed testing)
jmeter-server

# Run with custom JVM options
JVM_ARGS="-Xms512m -Xmx2048m" jmeter -n -t <testplan.jmx> -l <results.jtl>
```

## Troubleshooting

### Common Issues

1. **OutOfMemoryError**
   - Increase heap size: `export HEAP="-Xms1g -Xmx1g"`
   - Use non-GUI mode
   - Disable unnecessary listeners

2. **Connection Refused**
   - Verify target server is running
   - Check firewall settings
   - Verify URL/port configuration

3. **JDBC Connection Issues**
   - Ensure JDBC driver is in JMeter's lib directory
   - Verify database connection parameters
   - Check database server accessibility

## Contributing

Feel free to contribute additional test plans or improvements to existing ones.

## Resources

- [Apache JMeter Official Documentation](https://jmeter.apache.org/usermanual/index.html)
- [JMeter Best Practices](https://jmeter.apache.org/usermanual/best-practices.html)
- [JMeter Plugins](https://jmeter-plugins.org/)

## License

This project is open source and available under the MIT License.
