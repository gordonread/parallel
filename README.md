# parallel

Simple bash script to run commands in parallel.

```
$ parallel --help
Usage: parallel [options] [command [args]] [files]

Run command on items in stdin or following in parallel. File optionally represented in command with {}

ls | parallel gzip
ls *.gz | parallel --quiet --jobs 20 gzip -d
parallel --command "gzip -d" --files "*.gz"
parallel gzip -d *.gz
find . -type f -print0 | parallel -0 gzip
ls bzip2.* | parallel ls -l {} \; gzip -v {} \; ls -l {}.gz

Options:
  -h|--help            Display this help
  -0|--null            Input items separated by null
  -j|--jobs [arg]      Number of jobs, default 32,
  -l|--load [arg]      Maximum load,
  -s|--stop            Stop if a command errors,
  -k|--keep-going      Keep going even if command fails
  -c|--command [arg]   Run command given...
  -f|--files [arg]     ... On these items
  -q|--quiet           Don't echo commands
  -n|--dry-run         Dry run - print but don't execute commands
```

Can be used in a similar way to xargs by piping the result of a command to `parallel`:

| Command | Description |
| ------- | ----------- |
| `ls \| parallel gzip` | List all files in current directory and gzip them in parallel, result is the same as running `bzip *`, however`bzip` will be run in parallel for each individual file, not one after the other. bzipping in paralle is *usually) much quicker |
| `parallel gzip *` | Run `bzip` on all files, bzip will be run in parallel for each individual file, not one after the other |
| `find . -name \*.cpp \| parallel bzip` | All files located by the find command will be compressed by `bzip` |
| `find . -name \*.cpp -print0 \| parallel -0 bzip` | As above, find uses the separator `\0` and parallel will also use the separator `\0` - similar to `xargs -0` |
