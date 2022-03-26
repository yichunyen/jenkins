# CLOC Manual

### Get Started

Visit cloc [gihub](https://github.com/AlDanial/cloc) page, and select one of installation. On macOS here is the command below:

```
brew install cloc
```

Currently, I would like to know my Android project codes count include Java and Kotlin.

```bash
cloc . --exclude-dir=build,libs,assets,res --include-lang=Java,Kotlin
```

After execute the command, here is the result.

```bash
NUMBERS_OF_FILE text files.
    NUMBERS_OF_UNIQUE_FILE unique files.                                          
     NUMBERS_OF_FILES_IGNORED files ignored.

github.com/AlDanial/cloc XXXXXXXXXXXXXXXX
-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
Kotlin                         100           4000          120             4120
Java                           200           2000          100             2100
-------------------------------------------------------------------------------
SUM:                           300           6000          220             6220
-------------------------------------------------------------------------------
```

If you want to get every single file codes, add the `--by-file` option. Then Cloc would list the count for each file.

```bash
cloc . --by-file --exclude-dir=build,libs,assets,res --include-lang=Java,Kotlin
```

### CLOC on Jenkins

1. Following Get Started, install clock on the Jenkins server.
2. The CLOC job is triggered manually and I create the FreeStyle Project to execute the command.
3. Job setup 
    1. Given Git information (i.e. repository, credentials, branch etc.) in **Source Code Management**.
    2. In **Build** section, paste command.
4. Save the changes.
5. Build CLOC job.

### EXTENDED

If you want to show the code trending graph on Jenkins, please install [SLOCCOUNT](https://plugins.jenkins.io/sloccount/) plugin.
