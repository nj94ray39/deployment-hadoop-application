# Table Functions

## Overview

Table functions perform various operations on strings, lists, and maps to create their tabular representations.

## Reference

* [addTable for map](#addtable-for-map)
* [addTable for maps](#addtable-for-maps)
* [addTable for list](#addtable-for-list)
* [jsonToMaps](#jsontomaps)
* [jsonToLists](#jsontolists)


### `addTable` for map

```javascript
   addTable([] m, string f) string
```

This function prints the input map `m` as a two-column table in the specified format `f`.

The first column in the table contains map keys, whereas the second column contains the corresponding map values.

The input map `m` typically refers to map fields such as `tags`, `entity.tags`, or `variables`.

Supported formats:

* 'markdown'
* 'ascii'
* 'property'
* 'csv'
* 'html'

An empty string is returned if the map `m` is `null` or has no records.

Map records with empty or `null` values are ignored.

Numeric values are automatically rounded in web and email notifications and are printed `as is` in other cases.

The default table header is 'Name, Value'.

Examples:

* `markdown` format

```javascript
  addTable(property_map('nurswgvml007','disk::', 'today'), 'markdown')
```

```ls
| **Name** | **Value**  |
|:---|:--- |
| id | sda5 |
| disk_%busy | 0.6 |
| disk_block_size | 16.1 |
| disk_read_kb/s | 96.8 |
| disk_transfers_per_second | 26.0 |
| disk_write_kb/s | 8.1 |
```

* `csv` format

```javascript
  addTable(entity.tags, 'csv')
```

```ls
Name,Value
alias,007
app,ATSD
cpu_count,1
os,Linux
```

* `ascii` format

```javascript
  addTable(entity_tags(tags.host, true, true), 'ascii')
```

```ls
+-------------+------------+
| Name        | Value      |
+-------------+------------+
| alias       | 007        |
| app         | ATSD       |
| cpu_count   | 1          |
| os          | Linux      |
+-------------+------------+
```

* `html` format

The HTML format includes the response rendered as a `<table>` node with inline CSS styles for better compatibility with legacy email clients such as Microsoft Outlook.

```javascript
  addTable(property_map('nurswgvml007', 'cpu::*'), 'html')
```

```html
<table style="font-family: monospace, consolas, sans-serif; border-collapse: collapse; font-size: 12px; margin-top: 5px"><tbody><tr><th bgcolor="#f0f0f0" align="right" style="font-weight: bold;border: 1px solid #d0d0d0;padding: 4px;">Name</th><th align="left" style="border: 1px solid #d0d0d0;padding: 4px;">Value</th></tr>
<tr><td bgcolor="#f0f0f0" align="right" style="font-weight: bold;border: 1px solid #d0d0d0;padding: 4px;">id</td><td align="left" style="border: 1px solid #d0d0d0;padding: 4px;">1</td></tr>
<tr><td bgcolor="#f0f0f0" align="right" style="font-weight: bold;border: 1px solid #d0d0d0;padding: 4px;">cpu.idle%</td><td align="left" style="border: 1px solid #d0d0d0;padding: 4px;">91.5</td></tr>
<tr><td bgcolor="#f0f0f0" align="right" style="font-weight: bold;border: 1px solid #d0d0d0;padding: 4px;">cpu.steal%</td><td align="left" style="border: 1px solid #d0d0d0;padding: 4px;">0.0</td></tr>
</tbody></table>
```

* `property` format

```javascript
  addTable(excludeKeys(entity.tags, ['ip', 'loc_code', 'loc_area']), 'property')
```

```ls
alias=007
app=ATSD
cpu_count=1
os=Linux
```

### `addTable` for maps

```javascript
  addTable([[] m], string f[, [string h]]) string
```

The function prints a collection of maps `m` as a multiple-column table in the specified format `f`, with optional header `h`.

The first column in the table contains unique keys from all maps in the collection, whereas the second and subsequent columns contain map values for the corresponding key in the first column.

The default table header is 'Name, Value-1, ..., Value-N'.

If the header argument `h` is specified as a collection of strings, it replaces the default header. The number of elements in the header collection must be the same as the number of maps plus `1`.

Examples:

`property_maps('nurswgvml007','jfs::', 'today')` returns the following collection:

```ls
[
  {id=/, jfs_filespace_%used=12.8}, 
  {id=/dev, jfs_filespace_%used=0.0}, 
  {id=/mnt/u113452, jfs_filespace_%used=34.9}, 
  {id=/run, jfs_filespace_%used=7.5}, 
  {id=/var/lib/lxcfs, jfs_filespace_%used=0.0}
  ]
```

* `markdown` format

```javascript  
  addTable(property_maps('nurswgvml007','jfs::', 'today'), 'markdown')
```

```markdown
| **Name** | **Value 1** | **Value 2** | **Value 3** | **Value 4** | **Value 5**  |
|:---|:---|:---|:---|:---|:--- |
| id | / | /dev | /mnt/u113452 | /run | /var/lib/lxcfs |
| jfs_filespace_%used | 12.8 | 0.0 | 34.9 | 7.5 | 0.0 |
```

* `csv` format

```javascript  
  addTable(property_maps('nurswgvml007','jfs::', 'today'), 'csv')
```

```ls
Name,Value 1,Value 2,Value 3,Value 4,Value 5
id,/,/dev,/mnt/u113452,/run,/var/lib/lxcfs
jfs_filespace_%used,12.7,0.0,34.9,7.5,0.0
```

* `ascii` format

```javascript
  addTable(property_maps('nurswgvml007','jfs::', 'today'), 'ascii', ['property', 'root', 'dev', 'mount', 'run', 'var'])
```

```ls
+---------------------+------+------+--------------+------+----------------+
| property            | root | dev  | mount        | run  | var            |
+---------------------+------+------+--------------+------+----------------+
| id                  | /    | /dev | /mnt/u113452 | /run | /var/lib/lxcfs |
| jfs_filespace_%used | 12.8 | 0.0  | 34.9         | 7.5  | 0.0            |
+---------------------+------+------+--------------+------+----------------+
```

* `html` format

```javascript
  addTable(property_maps('nurswgvml007','jfs::', 'today'), 'html')
```

```ls
<table style="font-family: monospace, consolas, sans-serif; border-collapse: collapse; font-size: 12px; margin-top: 5px"><tbody><tr><th bgcolor="#f0f0f0" align="right" style="font-weight: bold;border: 1px solid #d0d0d0;padding: 4px;">Name</th><th align="left" style="border: 1px solid #d0d0d0;padding: 4px;">Value 1</th><th align="left" style="border: 1px solid #d0d0d0;padding: 4px;">Value 2</th><th align="left" style="border: 1px solid #d0d0d0;padding: 4px;">Value 3</th><th align="left" style="border: 1px solid #d0d0d0;padding: 4px;">Value 4</th><th align="left" style="border: 1px solid #d0d0d0;padding: 4px;">Value 5</th></tr>
<tr><td bgcolor="#f0f0f0" align="right" style="font-weight: bold;border: 1px solid #d0d0d0;padding: 4px;">id</td><td align="left" style="border: 1px solid #d0d0d0;padding: 4px;">/</td><td align="left" style="border: 1px solid #d0d0d0;padding: 4px;">/dev</td><td align="left" style="border: 1px solid #d0d0d0;padding: 4px;">/mnt/u113452</td><td align="left" style="border: 1px solid #d0d0d0;padding: 4px;">/run</td><td align="left" style="border: 1px solid #d0d0d0;padding: 4px;">/var/lib/lxcfs</td></tr>
<tr><td bgcolor="#f0f0f0" align="right" style="font-weight: bold;border: 1px solid #d0d0d0;padding: 4px;">jfs_filespace_%used</td><td align="left" style="border: 1px solid #d0d0d0;padding: 4px;">12.8</td><td align="left" style="border: 1px solid #d0d0d0;padding: 4px;">0.0</td><td align="left" style="border: 1px solid #d0d0d0;padding: 4px;">34.9</td><td align="left" style="border: 1px solid #d0d0d0;padding: 4px;">7.5</td><td align="left" style="border: 1px solid #d0d0d0;padding: 4px;">0.0</td></tr>
</tbody></table>
```

* `property` format

```javascript
  addTable(property_maps('nurswgvml007','jfs::', 'today'), 'property')
```

```ls
id=/=/dev=/mnt/u113452=/run=/var/lib/lxcfs
jfs_filespace_%used=12.8=0.0=34.9=7.5=0.0
```

### `addTable` for list

```javascript
  addTable([[string]] c, string f[, [string] | boolean h]) string
```

This function prints `c` (list of lists) as a multi-column table in the specified format `f`. Each nested list in the parent list `c` is serialized into its own row in the table.

The number of elements in each collection must be the same.

The default table header is 'Value-1, ..., Value-N'.

The header argument `h` can be used to customize the header.

If `h` is specified as a collection, its elements replace the default header. The size of the header collection must be the same as the number of cells in each row.

If `h` argument is specified as a boolean value `true`, the first row in the table will be used as a header.

An empty string is returned if the list `c` is empty.

Examples:

```javascript
query = 'SELECT datetime, value FROM http.sessions WHERE datetime > current_hour LIMIT 2'
```

`executeSqlQuery(query)` returns following collection:

```ls
[[datetime, value], [2018-01-31T12:00:13.242Z, 37], [2018-01-31T12:00:28.253Z, 36]]
```

* `markdown` format

```javascript
  addTable(executeSqlQuery(query), 'markdown', true)
```

```ls
| **datetime** | **value**  |
|:---|:--- |
| 2018-01-31T12:00:13.242Z | 37 |
| 2018-01-31T12:00:28.253Z | 36 |
```

* `csv` format

```javascript
  addTable([['2018-01-31T12:00:13.242Z', '37'], ['2018-01-31T12:00:28.253Z', '36']], 'csv', ['date', 'count'])
```

```ls
date,count
2018-01-31T12:00:13.242Z,37
2018-01-31T12:00:28.253Z,36
```

* `ascii` format

```javascript
  addTable(executeSqlQuery(query), 'ascii', true)
```  

```ls
+--------------------------+-------+
| datetime                 | value |
+--------------------------+-------+
| 2018-01-31T12:00:13.242Z | 37    |
| 2018-01-31T12:00:28.253Z | 36    |
+--------------------------+-------+
```

* `html` format

```javascript
  addTable(executeSqlQuery(query), 'html', true)
```

```html
<table style="font-family: monospace, consolas, sans-serif; border-collapse: collapse; font-size: 12px; margin-top: 5px"><tbody><tr><th bgcolor="#f0f0f0" align="right" style="font-weight: bold;border: 1px solid #d0d0d0;padding: 4px;">datetime</th><th align="left" style="border: 1px solid #d0d0d0;padding: 4px;">value</th></tr>
<tr><td bgcolor="#f0f0f0" align="right" style="font-weight: bold;border: 1px solid #d0d0d0;padding: 4px;">2018-01-31T12:00:13.242Z</td><td align="left" style="border: 1px solid #d0d0d0;padding: 4px;">37</td></tr>
<tr><td bgcolor="#f0f0f0" align="right" style="font-weight: bold;border: 1px solid #d0d0d0;padding: 4px;">2018-01-31T12:00:28.253Z</td><td align="left" style="border: 1px solid #d0d0d0;padding: 4px;">36</td></tr>
</tbody></table>
```

* `property` format

```javascript
  addTable(executeSqlQuery(query), 'property')
```
```ls
datetime=value
2018-01-31T12:00:13.242Z=37
2018-01-31T12:00:28.253Z=36
```

```javascript
  addTable(executeSqlQuery(query), 'property', true)
```
```ls
2018-01-31T12:00:13.242Z=37
2018-01-31T12:00:28.253Z=36
```

```javascript
  addTable(executeSqlQuery(query), 'property', false)
```
```ls
datetime=value
2018-01-31T12:00:13.242Z=37
2018-01-31T12:00:28.253Z=36
```

### `jsonToMaps`

```javascript
  jsonToMaps(string s) [map]
```

The function parses the input string `s` into a JSON document and returns a collection of maps containing keys and values from this ISON document.

The collection contains as many maps as there are leaf objects in the JSON document. Each map contains keys and values of the leaf object itself as well as keys and values from the parent objects.

The key names are created by concatenating the current field name with field names of its parents using `.` as a separator and `[i]` as an index suffix for array elements.

The common prefix until the first element array is discarded from key names.

### `jsonToLists`

```javascript
  jsonToLists(string s) [[string]]
```

The function parses the input string `s` into a JSON document and returns a collection of string lists of the same size containing field values from this JSON document.

The first list in the collection contains all possible key names in the leaf objects and their parents.

The key names are created by concatenating the current field name with field names of its parents using `.` as a separator and `[i]` as an index suffix for array elements.

The common prefix until the first element array is discarded from key names.

The subsequent lists in the collection contain field values of the associated leaf object itself as well as field values from the parent objects ordered by keys in the first list. If the key specified in the first list is absent in iterated object, the list on the given index will contain an empty string.