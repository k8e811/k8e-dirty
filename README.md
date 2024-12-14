# Dirty

Tools for dealing with dirty data sources (Like the dl.ncsbe.gov bucket)

## Usage (We hope)

### dmclean

```sh
[DMC_UNARCHIVE=unarchive] \
[DMC_ENCODING=encoding] \
[DMC_FORMAT=tabs|csv] \
dmclean file [part]`

* DMC\_UNARCHIVE is the unarchive command... 'unzip -p' is the default
* EMC\_ENCODING is the "from" encoding for inconv... 'UTF-8' is the default

### sqlite3load

```sh
[DMC_UNARCHIVE=unarchive] \
[DMC_ENCODING=encoding] \
sqlite3load dbfile file [part]
```

## Known dirty

* Most files are LATIN1, not UTF-8
* VR\_Snapshot has 'null' characters in null fields
* VR\_Snapshot uses UTF-16
* vipFeed is multi-part AND CSV, not TSV

## Quick checks

* `[ "$(unzip -p zipfile part| head -c 2)" == $'\xff\xfe' ]`
  is true when it's a UTF-16-LE BOM
* Looping through UTF-8 and LATIN1 on files that are not UTF-16 does the LATIN1
  conversion
* `sed -e 's/\o0,"",g'` turns null characters into empty strings
* file on that will show csv (How to normalize output)
* file <(

* UTF8 vs LATIN1
  * `unzip -p ncvoter.zip | iconv -f UTF-8 -t UTF-8 - > /dev/null` fails when
    file isn't LATIN1/UTF-8 subset
  * [Check for BOM](https://superuser.com/questions/121599/how-can-i-find-all-utf-16-encoded-text-files-in-a-directory-tree-with-a-unix-com)
