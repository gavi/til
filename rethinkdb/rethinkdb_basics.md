# RethinkDB Basics

RethinkDB is a great NoSQL database. Getting started with it is super simple.

Download the latest version for your platform here

http://www.rethinkdb.com

You could also very easily compile from source. I have tested it to work on Raspberry Pi 3 with no issues.

### Starting RethinkDB

Create a folder where you will keep your data files. In my case, its under `/Users/gavi/rethinkdb`. Then change directory to it and start it by typing `rethinkdb`

```
$mkdir -p /Users/gavi/rethinkdb
$cd /Users/gavi/rethinkdb
$rethinkdb
```

You will see messages like below

```
Recursively removing directory /Users/gavi/rethinkdb/rethinkdb_data/tmp
Initializing directory /Users/gavi/rethinkdb/rethinkdb_data
Running rethinkdb 2.2.6 (CLANG 4.2 (clang-425.0.28))...
Running on Darwin 15.4.0 x86_64
Loading data from directory /Users/gavi/rethinkdb/rethinkdb_data
warn: Cache size does not leave much memory for server and query overhead (available memory: 166 MB).
warn: Cache size is very low and may impact performance.
Listening for intracluster connections on port 29015
Listening for client driver connections on port 28015
Listening for administrative HTTP connections on port 8080
Listening on addresses: 127.0.0.1, ::1
```

By Default rethinkdb starts listening to `127.0.0.1`, the local loopback interface. If you would like to expose it over other interfaces you might have, use the following option

```
rethinkdb --bind all
```

If you look at the ports, `8080` is the web admin console. There is no username/password. It just logs you in as administrator. So security wise you need to be careful exposing it to the internet.

Two other ports of interest are `29015` for other rethinkdb instanes to connect to this server and `28015` for client connections. 

### RethinkDB data storage

Data is stored in the folder where you started the instance from under the folder `rethinkdb_data`. So in my case its under

```
/Users/gavi/rethinkdb/rethinkdb_data
```


### Stopping rethinkdb

To stop just kill the process by 

```
CNTRL + C
```


### Admin Console

http://localhost:8080

There is no username/password

### REPL

Install rethinkdb client for python

```
pip install rethinkdb
```

Then start interactive pythnon console

```
python
```

In python you can do the following

```
>>> import rethinkdb as r
>>> r.connect('localhost',28015).repl()
>>> list(r.db('rethinkdb').table('server_status').run())
```

You can write any ReQL now just using the `r` 






