# network-latency-toolkit
# Network Latency Toolkit

A lightweight JavaScript utility for measuring network performance, diagnosing connectivity issues, and generating detailed latency reports.

[![npm version](https://img.shields.io/npm/v/@all-it-service/network-latency-toolkit.svg)](https://www.npmjs.com/package/@all-it-service/network-latency-toolkit)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Features

- **Simple Latency Testing**: Measure round-trip time to any endpoint
- **Comprehensive Network Analysis**: Get detailed insights on latency, packet loss, and jitter
- **Multi-Endpoint Testing**: Compare performance across multiple services or regions
- **Performance Grading**: Automatic quality assessment of network connections
- **Lightweight**: No external dependencies for core functionality
- **Browser & Node.js Compatible**: Works in both environments

## Installation

```bash
npm install @all-it-service/network-latency-toolkit
```

## Quick Start

```javascript
const NetworkLatencyToolkit = require('@all-it-service/network-latency-toolkit');

// Create a new instance
const toolkit = new NetworkLatencyToolkit();

// Run a quick latency test
async function runTest() {
  // Test a single endpoint with 10 measurements
  const results = await toolkit.runLatencyTest('google.com', 10);
  
  // Analyze the results
  const analysis = toolkit.analyzeNetworkPerformance();
  
  console.log('Results:', results);
  console.log('Analysis:', analysis);
  
  // Export the complete results
  const exportedData = toolkit.exportResults();
  console.log(exportedData);
}

runTest();
```

## API Reference

### Constructor

```javascript
const toolkit = new NetworkLatencyToolkit(options);
```

#### Options

- `timeout` (Number): Maximum time to wait for a response (default: 5000ms)
- `retries` (Number): Number of retries for failed requests (default: 3)
- `interval` (Number): Time between consecutive tests (default: 1000ms)

### Methods

#### `async measureLatency(url)`

Measures the round-trip time to a specific URL.

- `url` (String): The endpoint to test
- Returns: `Promise<number>` - The latency in milliseconds

#### `async runLatencyTest(url, count)`

Performs multiple latency tests to a URL.

- `url` (String): The endpoint to test
- `count` (Number): The number of tests to run (default: 10)
- Returns: `Promise<object>` - The test results including all measurements and summary statistics

#### `async testMultipleEndpoints(endpoints, testsPerEndpoint)`

Tests connectivity to multiple endpoints.

- `endpoints` (Array<string>): Array of URLs to test
- `testsPerEndpoint` (Number): Number of tests per endpoint (default: 5)
- Returns: `Promise<object>` - Comprehensive test results for all endpoints

#### `analyzeNetworkPerformance(testResults)`

Generates an analysis of network performance.

- `testResults` (Object, optional): Results from previous tests (uses internal results if not provided)
- Returns: `Object` - Network performance analysis with quality rating and recommendations

#### `exportResults(format)`

Exports the results to a formatted object.

- `format` (String): Export format (currently only 'json' is supported)
- Returns: `String|Object` - The exported results

## Examples

### Testing Multiple Endpoints

```javascript
const toolkit = new NetworkLatencyToolkit();

async function compareServices() {
  const endpoints = [
    'google.com',
    'amazon.com',
    'microsoft.com',
    'cloudflare.com'
  ];
  
  const results = await toolkit.testMultipleEndpoints(endpoints, 5);
  const analysis = toolkit.analyzeNetworkPerformance(results);
  
  console.log('Analysis:', analysis);
  
  // Find the lowest latency endpoint
  const endpointLatencies = {};
  
  for (const [endpoint, data] of Object.entries(results)) {
    if (data.summary && data.summary.avg !== null) {
      endpointLatencies[endpoint] = data.summary.avg;
    }
  }
  
  const fastestEndpoint = Object.entries(endpointLatencies)
    .sort((a, b) => a[1] - b[1])[0];
    
  console.log(`Fastest endpoint: ${fastestEndpoint[0]} (${fastestEndpoint[1].toFixed(2)}ms)`);
}

compareServices();
```

### Custom Network Quality Assessment

```javascript
const toolkit = new NetworkLatencyToolkit();

async function assessConnectionQuality() {
  // Run a comprehensive test
  await toolkit.runLatencyTest('api.example.com', 20);
  
  const results = toolkit.results;
  
  // Implement custom quality criteria
  const avgLatency = results.summary.avg;
  const packetLoss = results.summary.packetLoss;
  const jitter = results.summary.jitter;
  
  let quality = 'Unknown';
  
  if (avgLatency < 50 && packetLoss === 0 && jitter < 5) {
    quality = 'Excellent - Suitable for real-time applications';
  } else if (avgLatency < 100 && packetLoss < 1 && jitter < 20) {
    quality = 'Good - Suitable for most applications';
  } else if (avgLatency < 200 && packetLoss < 5 && jitter < 40) {
    quality = 'Fair - May experience issues with real-time applications';
  } else {
    quality = 'Poor - Consider network optimizations';
  }
  
  console.log(`Connection Quality: ${quality}`);
  console.log(`Average Latency: ${avgLatency.toFixed(2)}ms`);
  console.log(`Packet Loss: ${packetLoss.toFixed(2)}%`);
  console.log(`Jitter: ${jitter.toFixed(2)}ms`);
}

assessConnectionQuality();
```

## Common Use Cases

- **Service Performance Monitoring**: Test the responsiveness of your APIs
- **Connection Troubleshooting**: Diagnose network issues affecting application performance
- **CDN Evaluation**: Compare performance across different CDN providers
- **Regional Performance Testing**: Measure network quality across different geographic regions
- **VPN Performance Assessment**: Evaluate how VPN connections affect network latency

## Browser Usage

When using in a browser environment, include the package via CDN:

```html
<script src="https://unpkg.com/@all-it-service/network-latency-toolkit@1.0.0/dist/network-latency-toolkit.min.js"></script>
<script>
  const toolkit = new NetworkLatencyToolkit();
  
  async function checkLatency() {
    const results = await toolkit.runLatencyTest('api.example.com', 5);
    console.log(results);
  }
  
  checkLatency();
</script>
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Resources

For more information and tutorials, visit:
- [Network Performance Optimization Guide](https://allitservice.com/resources/network-latency-toolkit/optimization-guide)
- [Common Network Issues and Solutions](https://allitservice.com/resources/network-latency-toolkit/troubleshooting)
- [API Documentation](https://allitservice.com/resources/network-latency-toolkit/api)

## Author

Created and maintained by [AllIT Service](https://allitservice.com).
