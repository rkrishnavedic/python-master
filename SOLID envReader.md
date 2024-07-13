### Summary

By applying the SOLID principles, we can create a robust and maintainable structure for reading, processing, and loading environment variables from a file. This structure ensures each class has a clear responsibility, is extendable, and can be easily maintained and tested.

```python
import os
import re

class EnvFileReader:
    """A class to read and preprocess environment variables from a file."""
    
    def __init__(self, file_path):
        self.file_path = file_path
    
    def read_lines(self):
        """Reads lines from the file."""
        with open(self.file_path, 'r') as file:
            return file.readlines()


class EnvLineProcessor:
    """A class to process lines and extract environment variables."""
    
    def __init__(self, lines):
        self.lines = lines
    
    def process_lines(self):
        """Processes lines and returns a list of key-value pairs."""
        processed_lines = []
        for line in self.lines:
            stripped_line = line.strip()
            
            if not stripped_line or stripped_line.startswith('#'):
                continue
            
            match = re.match(r'(?:export\s+|set\s+|Env:)?([^=]+)=(.*)', stripped_line)
            if match:
                key, value = match.groups()
                processed_lines.append((key.strip(), value.strip()))
        
        return processed_lines


class EnvLoader:
    """A class to load environment variables."""
    
    @staticmethod
    def load_env_vars(env_vars):
        """Loads the environment variables into the OS environment."""
        for key, value in env_vars:
            os.environ[key] = value


class EnvManager:
    """A class to manage the entire environment variable loading process."""
    
    def __init__(self, file_path):
        self.file_path = file_path
    
    def load_environment(self):
        """Reads, processes, and loads environment variables from a file."""
        reader = EnvFileReader(self.file_path)
        lines = reader.read_lines()
        
        processor = EnvLineProcessor(lines)
        env_vars = processor.process_lines()
        
        EnvLoader.load_env_vars(env_vars)


# Path to your .env file
env_file_path = '.env'

# Load environment variables with preprocessing
env_manager = EnvManager(env_file_path)
env_manager.load_environment()

# Access environment variables
var1 = os.getenv('VAR1')
var2 = os.getenv('VAR2')
var3 = os.getenv('VAR3')
var4 = os.getenv('VAR4')

print(f'VAR1: {var1}')
print(f'VAR2: {var2}')
print(f'VAR3: {var3}')
print(f'VAR4: {var4}')
```

### Explanation of SOLID Principles Applied

1. **Single Responsibility Principle (SRP)**:
   - Each class has a single responsibility. 
     - `EnvFileReader` reads lines from the file.
     - `EnvLineProcessor` processes the lines and extracts key-value pairs.
     - `EnvLoader` loads environment variables into the OS environment.
     - `EnvManager` coordinates the entire process.

2. **Open/Closed Principle (OCP)**:
   - Classes are open for extension but closed for modification. 
   - If new processing rules are needed, `EnvLineProcessor` can be extended without modifying existing code.

3. **Liskov Substitution Principle (LSP)**:
   - Not explicitly applied here, but each class can be replaced by any of its subclasses without affecting the program's correctness.

4. **Interface Segregation Principle (ISP)**:
   - Not explicitly applied since we are not dealing with interfaces here, but each class has focused and specific methods.

5. **Dependency Inversion Principle (DIP)**:
   - `EnvManager` depends on abstractions (`EnvFileReader`, `EnvLineProcessor`, `EnvLoader`) rather than concrete implementations.

### Example

#### .env File:

```env
# .env
export VAR1=value1

set VAR2=value2

# This is a comment
Env:VAR3=value3
VAR4=value4
```

#### Running the Script:

```bash
python my_script.py
```

#### Output:

```
VAR1: value1
VAR2: value2
VAR3: value3
VAR4: value4
```
