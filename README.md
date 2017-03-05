# About dburl

Package dburl provides a standardized way of processing database connection
strings for PostgreSQL, MySQL, SQLite, Oracle, Microsoft SQL Server, and SAP
HANA databases in the form of a URL.

Standard URLs are of the form `protocol+transport://user:pass@host/dbname?opt1=a&opt2=b`.

For example, the following URLs are recognized by a call to `Parse` or `Open`:
```
    postgres://user:pass@localhost/mydb
    pgsql://user:pass@pg-server123.example.com/anotherdb?sslmode=disable
    mysql://user:pass@localhost:8899/mydb
    oracle://user:pass@somehost.com/someOtherDatabase
    mssql://localhost/databaseName
    sqlserver://localhost/databaseName
    sqlite://path/to/mydatabase.sqlite3
    file://mydb.sqlite3
    saphana://user:pass@localhost:1442/adatabase
```

Additional driver aliases are provided for all of the databases in order to
facilitate better handling of URLs from various sources. Please see the
[GoDoc page](https://godoc.org/github.com/knq/dburl) for more information on
the all the available aliases.

**Note**: you will still need to import the related database driver package
into your code, as this package does not import any database drivers.

## Installation

Install in the usual Go fashion:

```sh
go get -u github.com/knq/dburl
```

## Usage

Please see [the GoDoc API page](http://godoc.org/github.com/knq/dburl) for a
full API listing.

The dburl package can be used similarly to the following:

```go
// example/example.go
package main

import (
	"fmt"
	"log"

	_ "github.com/denisenkom/go-mssqldb"
	"github.com/knq/dburl"
)

func main() {
	db, err := dburl.Open("sqlserver://user:pass@localhost/dbname")
	if err != nil {
		log.Fatal(err)
	}

	var name string
	err = db.QueryRow(`SELECT name FROM mytable WHERE id=10`).Scan(&name)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println(">> got: %s\n", name)
}
```

## Related Projects

The dburl package was built primarily to support these projects:

* [xo](https://github.com/knq/xo) - a command line tool to generate Go types from a database schema
* [usql](https://github.com/knq/usql) - a universal/utility command line tool to execute queries on databases
