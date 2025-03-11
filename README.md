# Java Vulnerability Fix Guide

Your security team has identified a vulnerability https://github.com/advisories/GHSA-599f-7c49-w659 in this repository. They also have asked you to harden the packages by removing unused code, if there is any at all.

Its your job to fix it. Ready, go!

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

Here are two common vulnerability fixes:

#### 4.1 Remove Unused Dependencies

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

#### 4.2 Upgrade Vulnerable Dependencies

Example: Upgrading commons-text to a secure version:

In the `pom.xml` update the version of commons-text to a non-vulnerable version, such as 1.10.0.

```xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-text</artifactId>
    <version>1.10.0</version>
</dependency>
```

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
