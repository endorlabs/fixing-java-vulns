# Java Vulnerability Fix Guide

This repository consists of two exersises to resolve Java vulnerabilities in Maven:

- Exersise 1: Direct Dependency Update
- Exersise 2: Direct Dependency Removal

Its your job to go fix the issues being raised to you by your security scanners.

## Quick Start

Clone the repository:

```bash
git clone https://github.com/endorlabs/fixing-java-vulns.git
cd fixing-java-vulns
```

## Vulnerability Assessment Process

### 1. Build the Project

Compile and package your project:

```bash
mvn clean install
```

### 2. Analyze Dependencies

Generate a dependency tree:

```bash
mvn dependency:tree
```

### 3. Scan for Vulnerabilities

1. Download [OSV-Scanner](https://github.com/google/osv-scanner/releases/latest) for your platform (macOS, Linux, Windows)

2. Make the scanner executable (Unix-based systems):

```bash
curl -LO https://github.com/google/osv-scanner/releases/download/v1.9.2/osv-scanner_linux_amd64
mv osv-scanner_linux_amd64 osv-scanner
chmod +x ./osv-scanner
```

1. Run the vulnerability scan:

```bash
./osv-scanner scan -L pom.xml 
```

### 4. Fix Vulnerabilities

**Exersise #1:**

Your security team has identified a vulnerability https://github.com/advisories/GHSA-599f-7c49-w659 in this repository. Its your job to fix it. Okay, go!


Example: Upgrading commons-text to a secure version:

In the `pom.xml` update the version of commons-text to a non-vulnerable version of commons-text. Version 1.9 has a known vulnerability. 

```xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-text</artifactId>
    <version>1.9</version>
</dependency>
```

Research which version you should upgrade to and perform the upgrade. Then rebuild and rescan the package.


**Exersise #2: Removing unused dependencies**

Example: Removing unused jackson-databind:

1. Search your code for Jackson usage
2. If unused, remove from `pom.xml`:
   
```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.x.x</version>
</dependency>
```

Then verify your fixes!

### 5. Verify Fixes

After making changes:

1. Rebuild the project:

```bash
mvn clean install
```

1. Check dependencies:

```bash
mvn dependency:tree
```

1. Re-run vulnerability scan:

```bash
./osv-scanner scan -L pom.xml 
```
