---
name: binary-analysis
description: >
  Analyze binaries, firmware, and container images with Black Duck Binary
  Analysis (BDBA): find an existing scan, review the components found by
  binary fingerprinting, and present the vulnerabilities by severity. Use
  when the user asks which open-source components or vulnerabilities a
  binary, firmware image, or embedded software contains.
---

# Binary Analysis Workflow

Guide for analyzing binaries and firmware using Black Duck BDBA.

## When to Use

- User wants to find open-source components in a binary or firmware image
- User wants to check vulnerabilities in a previously scanned binary
- User asks about component composition of embedded software

## Workflow

### 1. Check Connectivity

Verify BDBA is accessible:
- Call `health_check` to confirm connection

### 2. Search for Existing Scans

Find relevant BDBA groups/products:
- Use `search_scans` with relevant name patterns (omit `group_id` to search across all groups)
- If no results: inform user that the binary hasn't been scanned yet, and suggest uploading it to BDBA

### 3. Review Components

For a found group/product:
- Read `components://{product_id}` to get the BOM
- Note: components are identified via binary fingerprinting, not source scanning

### 4. Assess Vulnerabilities

Review findings:
- Identify components with critical/high vulnerabilities
- Check if newer component versions resolve the issues
- Consider the context (is the vulnerable function reachable?)

### 5. Present Results

Structure output as:
- Component count and notable libraries found
- Vulnerability summary by severity
- Top-priority items requiring attention
- Recommended next steps

## Important Notes

- BDBA identifies components through binary analysis (string matching, file hashing, etc.)
- Results may include partial matches — confidence levels vary
- Some components may be embedded within others (nested dependencies)
- Always verify critical findings against the actual binary context
