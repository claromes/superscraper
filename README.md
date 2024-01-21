# Volley Stats

[![PyPI](https://img.shields.io/pypi/v/volleystats)](https://pypi.org/project/volleystats/)

Command-line tool to scrape volleyball statistics from Data Project Web Competition websites (WCM).

Volley Stats facilitates the export of CSV data from volleyball matches and competitions hosted by entities using the Data Project. The tool streamlines the collection of individual matches, match lists, and automates individual match retrieval from the competition list.

Additionally, it provides the structure of URLs for Web Competition websites, simplifying the search for identifiers (mID, ID, PID). It also supplies acronyms for the main entities utilizing Data Project Management.

**This tool is not affiliated with Genius Sports Italy.**

## Installation

### Requirement

- Python 3.8+

```shell
pip install volleystats
```

# Documentation

- [Usage](#usage)
- [Usage examples](#usage-examples)
    - [Match](#match)
    - [Competition Matches](#competition-matches)
    - [Competition Matches with PID](#competition-matches-with-pid)
    - [Matches via Competition Matches file](#matches-via-competition-matches-file)
    - [Help](#help)
    - [Log](#log)
    - [Output messages](#output-messages)
- [Data Project Web Competition URLs structure](#data-project-web-competition-urls-structure)
    - Hostname
    - Pathnames and search parameters
- [Federations, Confederations and Leagues Acronym](#federations-confederations-and-leagues-acronym)
    - European Volleyball
    - South American Volleyball
- [Troubleshooting](#troubleshooting)
    - [Match files collected from batch file](#match-files-collected-from-batch-file)

## Usage

```
volleystats [--help] --fed FED (--match MATCH | --comp COMP | --batch CSV_FILE_PATH) [--pid PID] [--log]
```

- `--fed`, `-f`: Federation Acronym (required)
- `--match`, `-m`: Statistics of a single match (required, unless `--comp` or `--batch` are provided)
- `--comp`, `-c`: List of matches in a competition (required, unless `--match` or `--batch` are provided)
- `--pid`, `-p`: PID of the competition (optional, only when `--comp` is provided)
- `--batch`, `-b`: CSV file path with Match IDs (Competition Matches output) (required, unless `--match` or `--comp` are provided)
- `--log`, `-l`: View the logging during scraping
- `--help`, `-h`: Show help message

### Match

```shell
volleystats --fed FED --match MATCH
```

#### Examples

- Brazilian Volleyball Confederation
    - Data Project website: https://cbv-web.dataproject.com/MatchStatistics.aspx?mID=1623
    - Federation Acronym: CBV
    - Match ID: 1623
    - Command: $ `volleystats --fed cbv --match 1623`
    - Output files:
        ```
        data/cbv-1623-22-10-28-guest-barueri-volleyball-club.csv
        data/cbv-1623-22-10-28-home-fluminense.csv
        ```

- Lithuanian Volleyball Federation
    - Data Project website: https://lvf-web.dataproject.com/MatchStatistics.aspx?mID=2093
    - Federation Acronym: LVF
    - Match ID: 2093
    - Command: $ `volleystats --fed lvf --match 2093`
    - Output files:
        ```
        data/lvf-2093-2022-11-23-guest-jonavos-sc.csv
        data/lvf-2093-2022-11-23-home-svaja-viktorija-lsu.csv
        ```

### Competition Matches

```shell
volleystats --fed FED --comp COMP
```

#### Example

- Brazilian Volleyball Confederation
    - Data Project website: https://cbv-web.dataproject.com/CompetitionMatches.aspx?ID=18
    - Federation Acronym: CBV
    - Competition ID: 18
    - Command: $ `volleystats --fed cbv --comp 18`
    - Output file:
        ```
        data/cbv-18-2022-2023-competition-matches.csv
        ```

### Competition Matches with PID

In some competitions, PID can be used to distinguish between seasons, such as regular season and playoffs. Therefore, it is necessary to submit this value to obtain statistics separately.

```shell
volleystats --fed FED --comp COMP --pid PID
```

#### Examples

- Bundesliga
    - Data Project website: https://vbl-web.dataproject.com/CompetitionMatches.aspx?ID=162&PID=173
    - Federation Acronym: VBL
    - Competition ID: 162
    - PID: 173
    - Season: Regular
    - Command: $ `volleystats --fed vbl --comp 162 --pid 173`
    - Output file:
        ```
        data/vbl-162-173-2022-2023-competition-matches.csv
        ```
    ---
    - Data Project website: https://vbl-web.dataproject.com/CompetitionMatches.aspx?ID=162&PID=174
    - Federation Acronym: VBL
    - Competition ID: 162
    - PID: 174
    - Season: Playoffs
    - Command: $ `volleystats --fed vbl --comp 162 --pid 174`
    - Output file:
        ```
        data/vbl-162-174-2023-2023-competition-matches.csv
        ```

### Matches via Competition Matches file

```shell
volleystats --fed FED --batch CSV_FILE_PATH
```

#### Example

- Brazilian Volleyball Confederation
    - Data Project website: https://cbv-web.dataproject.com/MatchStatistics.aspx?mID=ID
    - Federation Acronym: CBV
    - CSV file path (output of the [Competition Matches](#competition-matches)): data/cbv-18-2022-2023-competition-matches.csv
    - Command: $ `volleystats --fed cbv --batch data/cbv-18-2022-2023-competition-matches.csv`
    - Output files:
        ```
        data/cbv-1623-22-10-28-guest-barueri-volleyball-club.csv
        data/cbv-1623-22-10-28-home-fluminense.csv
        data/cbv-1618-2022-11-01-guest-energis-8-s-o-caetano.csv
        data/cbv-1618-2022-11-01-home-esporte-clube-pinheiros.csv
        data/cbv-1619-2022-11-01-guest-abel-moda-volei.csv
        data/cbv-1619-2022-11-01-home-gerdau-minas.csv
        ...
        ```

### Help

```shell
volleystats --help
```

### Log
```shell
volleystats --fed FED (--match MATCH | --comp COMP | --batch CSV_FILE_PATH) --log
```

### Output messages

```
                    .
                    |`.
                    |  `.
                    |-_  `.
                    |  -_  `._
____________________|____-_ _|_______________,
',                         -_|                ',
  ',                         |                  ',
    ',                       |                    ',
      ',_____________________|______________________',

volleystats: started
volleystats: data/cbv-1623-22-10-28-home-fluminense.csv file was created
volleystats: data/cbv-1623-22-10-28-guest-barueri-volleyball-club.csv file was created
volleystats: finished
```

## Data Project Web Competition URLs structure

- Hostname: `<Fed_Acronym>`-web.dataproject.com

- Pathnames and search parameters:
    - /MainHome

    - /History?ID=`<Fed_ID>`

    - /CompetitionHome?ID=`<Category_ID>` (*could be Women, Men, Pro or Youth, e.g.*)

    - /CompetitionMatches?ID=`<Competition_ID>`&PID=`<PID>` (*PID could be regular season or playoffs, e.g.*)

    - /MatchStatistics?mID=`<Match_ID>`&ID=`<Competition_ID>`

## Federations, Confederations and Leagues Acronym

**European Volleyball**

- `fshv`: Albanian Volleyball Federation
- `osbih`: Bosnia and Herzegovina Volleyball Federation
- `bvf`: Bulgarian Volleyball Federation
- `bvl`: Baltic League
- `vbl`: Bundesliga
- `hos`: Croatian Volleyball Federation
- `cvf`: Czech Volleyball Federation
- `dvbf`: Danish Volleyball Federation
- `evf`: Estonian Volleyball Federation
- `fbf`: Faroe Islands Volleyball Association
- `eope`: Hellenic Volleyball Federation
- `hvf`: Hungary Volleyball Federation
- `bli`: Icelandic Volleyball Association
- `iva`: Israel Volleyball Association
- `fipav`: Italian Volleyball Federation
- `lvf`: Lithuanian Volleyball Federation
- `mva`: Malta Volleyball Association
- `nvbf`: Norwegian Volleyball Federation
- `fpv`: Portuguese Volleyball Federation
- `frv`: Romanian Volleyball Federation
- `ossrb`: Serbian Volleyball Federation
- `svf`: Slovak Volleyball Federation
- `ozs`: Slovenian Volleyball Federation
- `rfevb`: Spanish Volleyball Federation
- `svbf`: Swedish Volleyball Federation
- `swi`: Swiss Volley
- `tvf`: Turkish Volleyball Federation
- `pvlu`: Professional Volleyball League of Ukraine

**South American Volleyball**

- `feva`: Argentine Volleyball Federation
- `cbv`: Brazilian Volleyball Confederation
- `fcv`: Cordoba Volleyball Federation
- `fpdv`: Peruvian Volleyball Federation

## Troubleshooting

### Match files collected from batch file

In some cases, empty files may be returned, usually named as `<fed_acronym>-<match_id>-guest_stats.csv` and `<fed_acronym>-<match_id>-home_stats.csv`. This can happen due to the hiding of a match in the competition listing, either because it was canceled or incorrectly entered. The match is hidden from view, but it remains accessible in the HTML, causing the tool to return an empty file. In such cases, simply ignore and delete this file.

## Development

$ `git clone git@github.com:claromes/volleystats.git`

$ `cd volleystats`

$ `pip install -r requirements.txt`

$ `pip install --editable .`

## Author

[Claromes](https://claromes.com)
