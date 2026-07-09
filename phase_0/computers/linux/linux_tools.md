# Linux Tools 
## Usage, explanation, and commands

## GREP
### GREP: Global Regular Expression Print
- Searches text files for lines matching a specified pattern (regex or literal strings).
- Returns the matching lines, allowing you to filter and extract relevant information from files or command output.
- Incredibly versatile for searching logs, source code, configuration files, and piping data through pipelines.
- Commands are:
```bash
grep "pattern" filename              # Search for pattern in a file
grep "pattern" *                     # Search for pattern in all files in current directory
grep -i "pattern" filename           # Case-insensitive search
grep -v "pattern" filename           # Invert match (show lines NOT matching pattern)
grep -n "pattern" filename           # Show line numbers
grep -c "pattern" filename           # Count matching lines
grep -r "pattern" /path/to/dir       # Recursive search in directories
grep -l "pattern" *                  # Show only filenames that match
grep -w "pattern" filename           # Match whole words only
grep -E "regex\_pattern" filename     # Use extended regex (same as egrep)
grep "^pattern" filename             # Match pattern at start of line
grep "pattern\$" filename             # Match pattern at end of line
grep "." filename                    # Match any single character
grep ".\*" filename                   # Match any characters (essentially all lines)
```

### Common Combinations:
```bash
grep -rn "function" /path/to/project                # Find function definitions with line numbers
grep -i "error" logfile.txt | wc -l                 # Count case-insensitive errors
grep -v "^#" config.txt                             # Remove comment lines (lines starting with #)
grep -E "^[0-9]{3}\\.[0-9]{3}" file.txt              # Match IP address patterns
grep -r --include="\*.js" "TODO" .                    # Search only JavaScript files recursively
grep -A 3 "pattern" filename                        # Show pattern plus 3 lines after
grep -B 2 "pattern" filename                        # Show pattern plus 2 lines before
grep -C 2 "pattern" filename                        # Show pattern plus 2 lines before and after
grep -o "pattern" filename                          # Show only the matched part (not whole line)
ps aux | grep "process\_name"                        # Find running processes
grep -E "pattern1|pattern2" filename                # Match multiple patterns (OR logic)
grep -i "warn" logfile.txt | grep -v "debug"        # Chain greps: warnings but exclude debug lines
```

