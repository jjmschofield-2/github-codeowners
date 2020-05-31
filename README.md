# github-codeowners
A CLI tool for working with GitHub CODEOWNERS.

Things it does:
* Output who owns each file in your repo (ignoring files listed in `.gitignore`)
* Output who owns a single file
* Output who owns files based on a `git diff`
* Outputs in CSV and JSONL
* Outputs lots of lovely stats

## Installation
To install, first clone then:
```
$ npm i -g .
$ github-codeowners --help 
Usage: github-codeowners [options] [command]
```

## Commands
### Audit
Compares every file in your current (or specified) directory against your CODEOWNERS rules and outputs the result of who owns each file.
```
$ cd <your awesome project> 
$ github-codeowners audit
```

Output to JSONL to a target file:
```
$ github-codeowners audit -o jsonl > file-owners.jsonl 
```

Only unowned files:
```
$ github-codeowners audit -u 
```

Only files in a specific directory:
```
$ github-codeowners audit -i src/
```

Ownership stats:
```
$ github-codeowners audit -s
```

Full usage information:
```
$ github-codeowners audit --help
Usage: github-codeowners audit [options]

list the owners for all files

Options:
  -d, --dir <dirPath>          path to VCS directory (default: "/Users/jjmschofield/projects/github/github-codeowners")
  -c, --codeowners <filePath>  path to codeowners file (default: "<dir>/.github/CODEOWNERS")
  -o, --output <outputFormat>  how to output format eg: simple, jsonl, csv (default: "simple")
  -u, --unloved                unowned files only (default: false)
  -s, --stats                  output stats (default: true)
  -i, --include <partialPath>  paths begening with partial path only (default: '')
  -h, --help                   output usage information
```

### Who
Tells you who owns a given file: 
```
$ cd <your awesome project> 
$ github-codeowners who <file>
@some/team
```

Full usage:
```
$ github-codeowners who --help                   
Usage: github-codeowners who [options] <file>

lists owners of a specific file

Options:
  -d, --dir <dirPath>          path to VCS directory (default: "/Users/jjmschofield/projects/github/snyk/registry")
  -c, --codeowners <filePath>  path to codeowners file (default: "<dir>/.github/CODEOWNERS")
  -o, --output <outputFormat>  how to output format eg: simple, jsonl, csv (default: "simple")
  -h, --help                   output usage information
```

### Git
Provides a list of files with their owners between commits (against the **current** version of CODEOWNERS).

The stats option here can be super useful to help you understand if other people will have a lot of work in reviewing your PR. 

Owners  of unstaged changes:
```
$ github-codeowners git
```

Diff between a single commit and HEAD:
```
$ cd <your awesome project>
$ github-codeowners git <commit sha>
```

Diff between two commits:
```
$ github-codeowners git <commit sha> <commit sha>
```

Output stats:
```
$ github-codeowners git -s
```

Full usage:
```
$ github-codeowners git --help                                                                                       
Usage: github-codeowners git [options] [shaA] [shaB]

lists owners of files changed between commits, a commit against head or staged against head.

Options:
  -d, --dir <dirPath>          path to VCS directory (default: "/Users/jjmschofield/projects/github/snyk/registry")
  -c, --codeowners <filePath>  path to codeowners file (default: "<dir>/.github/CODEOWNERS")
  -o, --output <outputFormat>  how to output format eg: simple, jsonl, csv (default: "simple")
  -s, --stats                  output stats (default: false)
  -h, --help                   output usage information
```

## Output Formats
Check `github-codeowners <command> --help` for support for a given command, however generally the following outputs are supported:
* `simple` - tab delimited - terminal friendly output
* `jsonl` - line separated json - useful for streaming data to another command
* `csv` - csv delimited fields - useful to import into a spreadsheet tool of your choice

## Limits and Things to Improve
* It requires node
* It is not optimized
* It has 0 test coverage (there will be be bugs)
* It's not tested in the wild (YMMV based on OS and node versions etc)
* The output interface might change
* Some code could be better generalized (command setup, writers)

## Shout outs
Inspired by [codeowners](https://github.com/beaugunderson/codeowners#readme) but implemented in Typescript with extra bells and whistles.