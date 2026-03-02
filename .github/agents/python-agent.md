# Python Agent Documentation

## Overview

This is a fake Python Agent designed for testing and demonstration purposes. A Python Agent is a runtime instrumentation library that monitors application behavior, tracks function execution, and provides insights into program performance and behavior.

## Features

- **Function Call Tracking**: Monitor function execution with timing and error handling
- **Performance Profiling**: Measure execution time and call counts
- **Call History**: Maintain a log of all tracked function calls
- **Module Inspection**: Analyze module structure and members
- **Memory Tracking**: Monitor memory usage during execution
- **Metrics Collection**: Gather runtime statistics and metadata
- **Decorator-based**: Easy integration using Python decorators

## Installation

```bash
# Clone or copy the agent
cp PythonAgent.py your_project/

# Or install from package (if distributed)
pip install python-agent
```

## Quick Start

### Basic Usage

```python
from PythonAgent import PythonAgent

agent = PythonAgent(config={
    'enable_logging': True,
    'track_calls': True
})

@agent.enable_call_tracking()
def my_function(x):
    return x * 2

result = my_function(5)
print(agent.get_metrics())
```

### Using Global Agent Instance

```python
from PythonAgent import get_agent

agent = get_agent()

@agent.enable_call_tracking()
def calculate(a, b):
    return a + b

calculate(10, 20)
```

## Configuration Options

- `enable_logging: bool` - Enable console logging (default: False)
- `track_calls: bool` - Enable function call tracking (default: True)
- `track_memory: bool` - Monitor memory usage (default: False)
- `enable_profiling: bool` - Enable performance profiling (default: True)
- `max_history_size: int` - Maximum call history entries (default: 10000)

## API Reference

### Methods

#### `enable_call_tracking()`

Decorator that tracks function calls, execution time, arguments, and errors.

```python
@agent.enable_call_tracking()
def expensive_operation():
    pass
```

#### `profile_function(func)`

Decorator for lightweight function profiling with call count and total time.

```python
@agent.profile_function()
def fast_function():
    pass
```

#### `get_metrics()`

Returns current agent metrics including total calls, uptime, and history size.

```python
metrics = agent.get_metrics()
print(metrics['total_calls'])
```

#### `get_call_history(limit=None)`

Retrieve recorded function calls, optionally limited to most recent N calls.

```python
history = agent.get_call_history(limit=10)
```

#### `clear_history()`

Clear the call history and reset counters.

```python
agent.clear_history()
```

#### `inspect_module(module)`

Analyze a module and return its functions, classes, and sub-modules.

```python
import mymodule
info = agent.inspect_module(mymodule)
print(info['functions'])
```

#### `stop()`

Stop the agent and log final metrics.

```python
agent.stop()
```

## Performance Characteristics

- **Memory Overhead**: ~1-5MB depending on history size
- **CPU Overhead**: <1% for standard tracking
- **Call Tracking**: ~0.1ms per tracked call
- **Startup Time**: ~10-50ms initialization

## Compatibility

- Python 3.6+
- Python 3.7+
- Python 3.8+
- Python 3.9+
- Python 3.10+
- Python 3.11+

## Use Cases

- **Development**: Track function execution during debugging
- **Testing**: Monitor test coverage and function calls
- **Performance Analysis**: Identify slow functions and bottlenecks
- **Learning**: Understand code execution flow
- **Monitoring**: Observe application behavior in production

## Examples

### Track Multiple Functions

```python
from PythonAgent import get_agent
import time

agent = get_agent()

@agent.enable_call_tracking()
def process_data(data):
    time.sleep(0.5)
    return [x * 2 for x in data]

@agent.enable_call_tracking()
def validate_data(data):
    return len(data) > 0

data = [1, 2, 3, 4, 5]
process_data(data)
validate_data(data)

print(agent.get_metrics())
# Output: Total calls: 2, uptime_seconds: 0.51, ...
```

### Monitor Performance

```python
agent = get_agent()

@agent.profile_function()
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

fibonacci(10)
print(f"Called {fibonacci._call_count} times")
print(f"Total time: {fibonacci._total_time}s")
```

### Error Tracking

```python
@agent.enable_call_tracking()
def risky_operation(value):
    if value < 0:
        raise ValueError("Value must be positive")
    return value ** 2

try:
    risky_operation(-5)
except ValueError:
    pass

# Check history for error records
history = agent.get_call_history()
print(history[-1]['status'])  # 'error'
print(history[-1]['error'])   # 'Value must be positive'
```

## Limitations

- Thread-safety considerations for multi-threaded applications
- Large history can consume significant memory
- Performance impact increases with number of tracked functions
- Not designed for production high-frequency call tracking

## Troubleshooting

### Memory Usage Growing

- Call `agent.clear_history()` periodically
- Reduce number of tracked functions
- Limit history size in configuration

### No Data Being Tracked

- Ensure decorator is applied before function definition
- Check that `enable_logging` is True for debugging
- Verify agent instance is the same across modules

## References

- [Python Profiling Documentation](https://docs.python.org/3/library/profile.html)
- [Python Decorators Guide](https://docs.python.org/3/glossary.html#term-decorator)
- [Instrumentation Best Practices](https://docs.python.org/3/library/sys.html#sys.setprofile)
