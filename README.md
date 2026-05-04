# Test Automation for Chat Translator

## Overview
This script automates testing of the Pixels Suite Chat Translator by reading test cases from an Excel file, inputting them into the web application, capturing outputs, and comparing them with expected results.

## Files
- `test_automation.py` - Main test automation script
- `Assignment 1 - Test cases.xlsx` - Excel file containing test cases

## Excel File Structure
The Excel file should have the following columns:

| Column | Header | Purpose |
|--------|--------|---------|
| A | Singlish Input | Input test cases (Romanized Sinhala) |
| C | Expected output | Expected Sinhala translation |
| D | Actual output | Captured output from the translator (auto-filled) |
| E | Status | Test result: PASS, FAIL, COLLECTED, or UI Error (auto-filled) |

## Installation

### 1. Install Python Packages
```bash
pip install playwright openpyxl
```

### 2. Install Playwright Browsers
```bash
python -m playwright install
```

## Usage

### Basic Test Run
```bash
python test_automation.py
```

### Test with Column A Input (Singlish Input)
```bash
python test_automation.py --header-row 1 --input-col "Singlish Input" --wait-ms 8000 --retries 15 --timeout-ms 120000
```

### Visible Browser (Watch Testing)
```bash
python test_automation.py --header-row 1 --input-col "Singlish Input" --wait-ms 8000 --retries 15 --timeout-ms 120000 --slow-mo-ms 500
```

### Headless Mode (No Browser Window)
```bash
python test_automation.py --header-row 1 --input-col "Singlish Input" --wait-ms 8000 --retries 15 --timeout-ms 120000 --headless
```

### Save Progress Every N Rows
```bash
python test_automation.py --header-row 1 --input-col "Singlish Input" --wait-ms 8000 --retries 15 --save-every 5
```

## Command Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `--excel` | Auto-detected | Path to Excel file |
| `--sheet` | " Test cases" | Sheet name in Excel |
| `--header-row` | Auto-detected | Header row number (1-based) |
| `--input-col` | Auto-detected | Input column name |
| `--expected-col` | Auto-detected | Expected output column name |
| `--actual-col` | "Actual output" | Actual output column name |
| `--status-col` | "Status" | Status column name |
| `--url` | Chat Translator URL | Website URL to test |
| `--wait-ms` | 5000 | Wait time after input (milliseconds) |
| `--retries` | 8 | Number of retries if output is empty |
| `--retry-wait-ms` | 1000 | Wait between retries (milliseconds) |
| `--timeout-ms` | 60000 | Overall timeout per test (milliseconds) |
| `--slow-mo-ms` | 0 | Slow down browser interactions (milliseconds) |
| `--headless` | false | Run browser in headless mode |
| `--save-every` | 0 | Save Excel every N rows (0 = only at end) |
| `--keep-open` | false | Keep browser open after tests |

## Test Results

### Status Values
- **PASS** - Actual output matches expected output exactly
- **FAIL** - Actual output differs from expected output
- **COLLECTED** - Output was captured but no expected output provided
- **UI Error** - Error occurred during testing (browser closed, timeout, etc.)

## Troubleshooting

### Browser Closes After First Test
Use longer timeouts and more retries:
```bash
python test_automation.py --header-row 1 --input-col "Singlish Input" --wait-ms 8000 --retries 15 --timeout-ms 120000
```

### Empty Actual Output
Increase wait time:
```bash
python test_automation.py --header-row 1 --input-col "Singlish Input" --wait-ms 10000
```

### Cannot Find Input/Output Columns
Verify Excel headers match the column names and specify them explicitly:
```bash
python test_automation.py --header-row 1 --input-col "Singlish Input" --expected-col "Expected output"
```

### Website Not Loading
Check your internet connection or URL:
```bash
python test_automation.py --url "https://www.pixelssuite.com/chat-translator"
```

## Example Output
```
Starting Frontend-Only test with 50 rows...
Frontend loaded successfully.
Testing [Row 2]: Question forms
  -> FAIL
Testing [Row 3]: Question forms
  -> PASS
Testing [Row 4]: Command forms
  -> COLLECTED
```

## Notes
- First run may take time as Playwright downloads browser binaries
- The script automatically detects header rows and column positions
- Results are saved to the same Excel file
- Merged cells in Excel are handled properly
- Unicode/Sinhala characters are fully supported
