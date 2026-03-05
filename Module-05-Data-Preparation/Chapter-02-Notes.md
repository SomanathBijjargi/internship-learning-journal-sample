# Data Preparation Using Unix Shell Commands

## Introduction

we explore how to perform **data preparation using Unix shell commands**.

Unix tools are powerful because they are: - **Fast** -- written in C -
**Agile** -- instant feedback while running commands -
**Parallelizable** - **Available everywhere** -- Linux, Mac, and
environments like Google Colab

Example command:

``` bash
!ls
```

Lists files in the current directory.

# Downloading Data Using curl

Example:

``` bash
curl --location --continue-at - --output s.net-April-2024.gz <URL>
```

### Important Options

  Option            Description
  ----------------- ------------------------------
  --location        Follow redirects
  --continue-at -   Resume interrupted downloads
  --output          Save the downloaded file

Check files:

``` bash
ls
```

Detailed view:

``` bash
ls -l
```

------------------------------------------------------------------------

# Decompressing Files

``` bash
gzip --decompress s.net-April-2024.gz
```

Compressed file (5.8 MB) may become much larger after decompression.

------------------------------------------------------------------------

# Viewing File Content

## First Lines

``` bash
head -n 5 s.net-April-2024
```

## Last Lines

``` bash
tail -n 5 s.net-April-2024
```

Typical log fields include: - IP address - Date/time - Request type -
HTTP response code - Response size - Referrer - User agent

------------------------------------------------------------------------

# Counting Requests

``` bash
wc -l s.net-April-2024
```

Counts number of requests (lines).

------------------------------------------------------------------------

# Extracting IP Addresses

``` bash
!cut --delimeter " " --fields 1 s.net-April-2024 | head -n 5
```

Options:

  Option   Meaning
  -------- --------------
  --delimiter       delimiter
  --fields      field number

Preview:

``` bash
!cut --delimeter " " --fields 1 s.net-April-2024 | head -n 5
```

------------------------------------------------------------------------

# Using Pipes


The `|` passes output from one command to the next.

------------------------------------------------------------------------

# Sorting Data

``` bash
!cut --delimeter " " --fields 1 s.net-April-2024 | sort | -n 5
```

------------------------------------------------------------------------

# Counting Unique Requests

``` bash
!cut --delimeter " " --fields 1 s.net-April-2024 | sort | uniq --count | head -n 30
```
u can see the 30 IPs with number of duplications

------------------------------------------------------------------------

# Finding Most Active IPs

``` bash
!cut --delimeter " " --fields 1 s.net-April-2024 | sort | uniq --count | sort --key -1n | tail
```

Options:

  Option   Meaning
  -------- ------------------
  -key      sort by column 1
  -1n      sort alphabetically 

------------------------------------------------------------------------

# Investigating Bots

Example:

``` bash
grep "^IP_ADDRESS" s.net-April-2024 | head
```

Bots may identify themselves in the user agent string.

Example:

    Mozilla/5.0 compatible DataForSEO bot

------------------------------------------------------------------------

# Searching With grep

``` bash
grep "bot" s.net-April-2024
```

Better regex search:

``` bash
grep -o "\S*bot\S*" s.net-April-2024
```

------------------------------------------------------------------------

# Finding Most Active Bots

``` bash
grep -o "\S*bot\S*" s.net-April-2024 | sort | uniq -c | sort -k1 -n | tail
```

Example bots discovered: - GoogleBot - AppleBot - BingBot - DataForSEO
Bot - PetalBot

------------------------------------------------------------------------

# Converting Log to CSV

Example log date:

    [10/Apr/2024:10:22:15 +0000]

Convert brackets to quotes:

``` bash
sed 's/\[\([^]]*\)\]/"\1"/g' s.net-April-2024 > log.csv
```

------------------------------------------------------------------------

# Preview Converted File

``` bash
head log.csv
```

------------------------------------------------------------------------

# Create Smaller Sample

``` bash
head -n 1000 log.csv > small.csv
```

------------------------------------------------------------------------

# Opening in Excel

Steps:

1.  Download file
2.  Rename `.csv` → `.txt`
3.  Open in Excel
4.  Select **Delimited**
5.  Choose **Space delimiter**

------------------------------------------------------------------------

# Handling Data Issues

Sometimes URLs contain quotes like:

    "devotional songs"

This can break CSV structure.

Solutions: - Clean rows manually - Preprocess quotes

------------------------------------------------------------------------

# Useful Commands

  Command   Purpose
  --------- ------------------
  ls        list files
  curl      download data
  gzip      decompress
  head      view first lines
  tail      view last lines
  wc        count lines
  cut       extract columns
  sort      sort values
  uniq      count duplicates
  grep      search text
  sed       modify text


