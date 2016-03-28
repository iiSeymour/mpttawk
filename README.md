# Hierarchical data at the command line

`mptt` is a simple script for converting an adjacency list to a modified preorder tree traversal file using AWK.

## Generating a modified preorder file

To generate a `.mptt` file from an adjacency list run the `mptt` command and use `sort` to order the file.

```bash
$ ./mptt FS=, OFS=, tech.csv | sort -nt, -k 2,2 > tech.mptt
```

## Queries

Running common on queries on `.mptt` is much simpler and more efficient than it would be on the original adjacency list.

List children of a node:

```bash
$ awk -F, '$1==node{lft=$2; rgt=$3} $2>lft && $3<rgt' node=sql tech.mptt
mysql,30,31
mssql,32,33
postgres,34,35
```

List parents of a node:

```bash
$ sort -nt, -k3 tech.mptt | awk -F, '$1==node{lft=$2; rgt=$3} $2<lft && $3>rgt' node=sql
databases,28,41
technologies,1,42
```

List the number of children for a given node:

```bash
$ awk -F, '$1==node{print ($3-$2-1)/2}' node=sql tech.mptt
3
```

## Notes

mptt is also commonly known as the [nested set model](https://en.wikipedia.org/wiki/Nested_set_model).


## License

MIT Copyright (c) 2016 Chris Seymour
