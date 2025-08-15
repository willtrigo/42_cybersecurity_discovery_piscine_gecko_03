# 42_cybersecurity_discovery_piscine_gecko_03

## Overview

A comprehensive hash analysis and password recovery toolkit utilizing John the Ripper for systematic hash format identification and multi-stage password cracking operations.

## Project Structure

```
.
├── hash.txt           # Target hash file for analysis
├── wordlist.lst       # Primary wordlist for initial attack
└── combined.lst       # Enhanced wordlist with combinations 
```

## Methodology

### Phase 1: Hash Format Identification

The project employs John the Ripper's hash identification capabilities to determine the cryptographic format of target hashes stored in `hash.txt`.

```bash
# Hash format detection with initial wordlist attempt
john --wordlist=wordlist.lst hash.txt
```

**Identification Results:**

```bash
Warning: detected hash type "Raw-SHA1", but the string is also recognized as "Raw-SHA1-AxCrypt"
Use the "--format=Raw-SHA1-AxCrypt" option to force loading these as that type instead
Warning: detected hash type "Raw-SHA1", but the string is also recognized as "Raw-SHA1-Linkedin"
Use the "--format=Raw-SHA1-Linkedin" option to force loading these as that type instead
Warning: detected hash type "Raw-SHA1", but the string is also recognized as "ripemd-160"
Use the "--format=ripemd-160" option to force loading these as that type instead
Warning: detected hash type "Raw-SHA1", but the string is also recognized as "has-160"
Use the "--format=has-160" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-SHA1 [SHA1 256/256 AVX2 8x])
Warning: no OpenMP support for this hash type, consider --fork=20
Note: Passwords longer than 18 [worst case UTF-8] to 55 [ASCII] rejected
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
0g 0:00:00:00 DONE (2025-08-15 12:09) 0g/s 800.0p/s 800.0c/s 800.0C/s liam..hacking
Session completed.
```

**Analysis:** Hash identified as `Raw-SHA1` format with initial wordlist attack unsuccessful (0 passwords cracked).

### Phase 2: Wordlist Combination Generation

Advanced password recovery utilizing combination rules and enhanced wordlist (`combined.lst`) for comprehensive coverage.

```bash
# Generate combinations of all wordlist entries
for w1 in $(cat wordlist.lst); do
  for w2 in $(cat wordlist.lst); do
    echo "${w1}${w2}"
  done
done > combined.lst
```

**Enhancement Strategies:**

- Word permutations and mutations
- Rule-based transformations
- Hybrid attacks combining multiple techniques
- Custom rule sets for target-specific patterns

### Phase 3: Enhanced Combination Attack

Execute the combination attack using the generated enhanced wordlist against the identified SHA1 hash format.

```bash
# Combination attack with enhanced wordlist
john --wordlist=combined.lst --format=raw-sha1 hash.txt
```

**Attack Parameters:**

- Wordlist: `combined.lst` (generated combinations)
- Target: `hash.txt`
- Method: Dictionary attack with combinations
- Format: `raw-sha1` (explicitly specified)

## Technical Implementation

### Workflow Execution

1. **Hash Analysis**

   ```bash
   # Place target hash in hash.txt
   echo "c967d488512ab5559b446f97843de1be0d615088" > hash.txt
   ```

2. **Initial Format Detection & Attack**

   ```bash
   john --wordlist=wordlist.lst hash.txt
   ```

3. **Combination Generation**

   ```bash
   for w1 in $(cat wordlist.lst); do
     for w2 in $(cat wordlist.lst); do
       echo "${w1}${w2}"
     done
   done > combined.lst
   ```

4. **Enhanced Attack Execution**

   ```bash
   john --wordlist=combined.lst --format=raw-sha1 hash.txt
   ```

5. **Results Verification**

   ```bash
   john --show hash.txt
   ```

### Command Optimization

```bash
# Fork processes for better performance
john --wordlist=combined.lst --format=raw-sha1 --fork=20 hash.txt

# Session management for long-running attacks
john --wordlist=combined.lst --format=raw-sha1 --session=gecko03 hash.txt

# Resume interrupted sessions
john --restore=gecko03
```

### Performance Optimization

- **Multi-threading**: Leverages available CPU cores
- **Memory Management**: Efficient wordlist handling
- **Progress Monitoring**: Real-time status updates
- **Session Management**: Resumable cracking sessions

## Configuration

### Environment Requirements

- John the Ripper (Community Enhanced or Jumbo versions)
- Adequate RAM for wordlist processing
- Multi-core CPU architecture (recommended)
- Storage space for session files and results

## Results Analysis

### Output Interpretation

```bash
# Display cracked passwords
john --show hash.txt
```

### Success Metrics

- **Hash Coverage**: Percentage of hashes successfully identified
- **Crack Rate**: Passwords recovered per attack phase
- **Time Efficiency**: Performance benchmarks per methodology
- **Resource Utilization**: CPU and memory consumption analysis

## Security Considerations

- **Ethical Usage**: Authorized penetration testing only
- **Data Protection**: Secure handling of sensitive hash data
- **Legal Compliance**: Adherence to applicable cybersecurity laws
- **Documentation**: Comprehensive audit trail maintenance

## Troubleshooting

### Common Issues & Solutions

| Issue | Cause | Solution |
|-------|-------|----------|
| `0g 0:00:00:00 DONE` | Wordlist exhausted without success | Generate larger combination wordlist |
| `Warning: no OpenMP support` | Single-threaded execution | Use `--fork=N` for multi-processing |
| `Session completed` | Normal termination | Check results with `john --show` |
| Memory exhaustion | Large wordlist size | Process in chunks or increase available RAM |

### Debug Commands

```bash
# Verbose output for debugging
john --wordlist=combined.lst --format=raw-sha1 hash.txt --verbosity=5
```
