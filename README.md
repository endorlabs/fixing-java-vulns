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

### 2. Scan for Vulnerabilities

We'll use `endorctl` to scan the project for known vulnerabilities.

#### Step-by-step:

1. **Initialize Endor Labs**  
   Run the following command to authenticate with Endor Labs and set up your environment:
   ```bash
   ./endorctl init --auth-mode <mode> --headless-mode
   ```
   Replace `<mode>` with your preferred authentication mode (e.g., `google`, `github`, etc.).

2. **Authenticate via Portal**  
   The command will output a URL. You can command-click (⌘+click) the link in your terminal to open the authentication portal.  
   Log in and copy the generated token.

3. **Complete Setup**  
   Paste the token back into your terminal.  
   You'll then be prompted to select a tenant—choose the one you just created.

4. **Run the Vulnerability Scan**  
   Once authenticated and configured, scan your codebase:
   ```bash
   ./endorctl scan
   ```
   This will analyze your project for security vulnerabilities.

### 3. Fix Vulnerabilities

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

### 4. Verify Fixes

After making changes:

1. Rebuild the project:

```bash
mvn clean install
```

2. Check dependencies:

```bash
mvn dependency:tree
```

3. Re-run vulnerability scan:

```bash
./endorctl scan -L pom.xml 
```
