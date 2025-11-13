# Assignment 4 - Columnar Databases and HBase

## 7in7 - HBase - Day 1

### Part 1 - Interactive Reading

Read Day 1 and work through the examples in the chapter.
Save your final database in a directory `day1` as follows.

```
./save.bash wiki day1
```

You can use the same script to save your database between work sessions.
But you cannot save over an existing save. So use temporary names like:
`day1a`, `day1b`, etc. You can use the `./load.bash` script to restore
an empty database from a saved state.

Here are some additional tips:

* Skip **Configuring HBase**.
* Begin with the last command on p57: `version`.
* p59 - Do not run these in the shell, which is Ruby.
    These are illustrative examples in python.
* p64 - The last example is split over 2 lines. `hbase>` and `hbase*` are
    not part of the command.
* I have not been able to use the colons inside of quotes in the hbase commands. If they don't work with the colon, try them without.
* The command to allow unlimited  versions is:

`alter 'wiki', {NAME => 'text', VERSIONS => 2147483647 }`

### Part 2 - 7in7 - HBase Day 1 - Find

1. Figure our how to use the shell to do the following:

    * Delete individual column values in a row:

        ```
        hbase(main):014:0> delete 'wiki', 'Home', 'text:'
        0 row(s) in 0.0670 seconds

        hbase(main):015:0> get 'wiki', 'Home'
        COLUMN        CELL
        0 row(s) in 0.0150 seconds
        ```

    * Delete an entire row

        ```
        hbase(main):018:0> deleteall 'wiki', 'Home'
        0 row(s) in 0.0390 seconds

        hbase(main):019:0> get 'wiki', 'Home'
        COLUMN        CELL
        0 row(s) in 0.0060 seconds
        ```


2. Bookmark the HBase API documentation for the version of HBase you’re using.

    ```
    https://hbase.apache.org/2.4/apidocs/

    ```

### Part 3 - Create a family database

#### Step 1 - Create an hbase table that represents a family.

Specifically, you should have column families for the following:
* personal information: names of family members and birthdays
* favorites: foods and vacation locations
* location information: addresses including street, city, state, and zip. and phone numbers

Place the Hbase code to create the families below:
```
hbase(main):022:0> create 'family', { NAME => 'personal information' }, { NAME => 'favorites' }, { NAME => 'location information' }
0 row(s) in 2.2360 seconds

=> Hbase::Table - family
hbase(main):023:0> describe 'family'
Table family is ENABLED
family
COLUMN FAMILIES DESCRIPTION
{NAME => 'favorites', BLOOMFILTER => 'ROW', VERSIONS =
> '1', IN_MEMORY => 'false', KEEP_DELETED_CELLS => 'FA
LSE', DATA_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER',
 COMPRESSION => 'NONE', MIN_VERSIONS => '0', BLOCKCACH
E => 'true', BLOCKSIZE => '65536', REPLICATION_SCOPE =
> '0'}
{NAME => 'location information', BLOOMFILTER => 'ROW',
 VERSIONS => '1', IN_MEMORY => 'false', KEEP_DELETED_C
ELLS => 'FALSE', DATA_BLOCK_ENCODING => 'NONE', TTL =>
 'FOREVER', COMPRESSION => 'NONE', MIN_VERSIONS => '0'
, BLOCKCACHE => 'true', BLOCKSIZE => '65536', REPLICAT
ION_SCOPE => '0'}
{NAME => 'personal information', BLOOMFILTER => 'ROW',
 VERSIONS => '1', IN_MEMORY => 'false', KEEP_DELETED_C
ELLS => 'FALSE', DATA_BLOCK_ENCODING => 'NONE', TTL =>
 'FOREVER', COMPRESSION => 'NONE', MIN_VERSIONS => '0'
, BLOCKCACHE => 'true', BLOCKSIZE => '65536', REPLICAT
ION_SCOPE => '0'}
3 row(s) in 0.0270 seconds

hbase(main):024:0>
```

#### Step 2 - Load five rows of data.
Make sure to have at least one row with more than one favorite food and at least one row with more than one favorite vacation location. Place the Hbase code below:
```
#Row-1

hbase(main):024:0> put 'family', 'mom', 'personal information:name', 'Rani Prathipati'
0 row(s) in 0.0100 seconds

hbase(main):025:0> put 'family', 'mom', 'favorites:food1', 'Mudda Papu-Avakaya'
0 row(s) in 0.0080 seconds

hbase(main):026:0> put 'family', 'mom', 'favorites:vacation1', 'Guntur'
0 row(s) in 0.0110 seconds

hbase(main):027:0> put 'family', 'mom', 'favorites:vacation2', 'Chennai'
0 row(s) in 0.0220 seconds

hbase(main):028:0> put 'family', 'mom', 'location:city', 'PathaReddy Palem'

hbase(main):029:0> put 'family', 'mom', 'location information:city', 'Pathareddy Palem'
0 row(s) in 0.0160 seconds

hbase(main):030:0> scan 'family'
ROW               COLUMN+CELL
 mom              column=favorites:food1, timestamp=1762994146285
                  , value=Mudda Papu-Avakaya
 mom              column=favorites:vacation1, timestamp=176299470
                  1533, value=Guntur
 mom              column=favorites:vacation2, timestamp=176299477
                  2463, value=Chennai
 mom              column=location information:city, timestamp=176
                  2995098548, value=Pathareddy Palem
 mom              column=personal information:name, timestamp=176
                  2994059863, value=Rani Prathipati
1 row(s) in 0.0160 seconds

hbase(main):031:0> get 'family', 'mom'
COLUMN            CELL
 favorites:food1  timestamp=1762994146285, value=Mudda Papu-Avaka
                  ya
 favorites:vacati timestamp=1762994701533, value=Guntur
 on1
 favorites:vacati timestamp=1762994772463, value=Chennai
 on2
 location informa timestamp=1762995098548, value=Pathareddy Palem
 tion:city
 personal informa timestamp=1762994059863, value=Rani Prathipati
 tion:name
5 row(s) in 0.0220 seconds

hbase(main):032:0>

#Row-2

hbase(main):032:0> put 'family', 'dad', 'personal information:name', 'Rambabu Prathipati'
0 row(s) in 0.0100 seconds

hbase(main):033:0> put 'family', 'dad', 'favorites:food1', 'Idli'
0 row(s) in 0.0130 seconds

hbase(main):034:0> put 'family', 'dad', 'favorites:food2', 'Dosa'
0 row(s) in 0.0090 seconds

hbase(main):035:0> put 'family', 'dad', 'favorites:vacation1', 'Vizag'
0 row(s) in 0.0070 seconds

hbase(main):036:0> put 'family', 'dad', 'location information:city','Bheemli'
0 row(s) in 0.0100 seconds

hbase(main):037:0> scan 'family'
ROW               COLUMN+CELL
 dad              column=favorites:food1, timestamp=1762995416933
                  , value=Idli
 dad              column=favorites:food2, timestamp=1762995448151
                  , value=Dosa
 dad              column=favorites:vacation1, timestamp=176299548
                  2047, value=Vizag
 dad              column=location information:city, timestamp=176
                  2995529246, value=Bheemli
 dad              column=personal information:name, timestamp=176
                  2995359137, value=Rambabu Prathipati
 mom              column=favorites:food1, timestamp=1762994146285
                  , value=Mudda Papu-Avakaya
 mom              column=favorites:vacation1, timestamp=176299470
                  1533, value=Guntur
 mom              column=favorites:vacation2, timestamp=176299477
                  2463, value=Chennai
 mom              column=location information:city, timestamp=176
                  2995098548, value=Pathareddy Palem
 mom              column=personal information:name, timestamp=176
                  2994059863, value=Rani Prathipati
2 row(s) in 0.0230 seconds

hbase(main):038:0> get 'family', 'dad'
COLUMN            CELL
 favorites:food1  timestamp=1762995416933, value=Idli
 favorites:food2  timestamp=1762995448151, value=Dosa
 favorites:vacati timestamp=1762995482047, value=Vizag
 on1
 location informa timestamp=1762995529246, value=Bheemli
 tion:city
 personal informa timestamp=1762995359137, value=Rambabu Prathipa
 tion:name        ti
5 row(s) in 0.0080 seconds

hbase(main):039:0>

#Row-3

hbase(main):001:0> put 'family', 'child1', 'personal information:name', 'Divya Prathipati'
0 row(s) in 0.2960 seconds

hbase(main):002:0> put 'family','child1','favorites:food1','Pani Puri'
0 row(s) in 0.0060 seconds

hbase(main):003:0> put 'family','child1','favorites:vacation1','Hyderabad'
0 row(s) in 0.0090 seconds

hbase(main):004:0> put 'family','child1','location information:city','Sanathnagar'
0 row(s) in 0.0110 seconds

hbase(main):007:0> put 'family','child1','favorites:food2','Gongura mutton'
0 row(s) in 0.0050 seconds

hbase(main):009:0> scan 'family'
ROW               COLUMN+CELL
 child1           column=favorites:food1, timestamp=1762996002391
                  , value=Pani Puri
 child1           column=favorites:food2, timestamp=1762996286273
                  , value=Gongura mutton
 child1           column=favorites:vacation1, timestamp=176299602
                  4275, value=Hyderabad
 child1           column=location information:city, timestamp=176
                  2996143637, value=Sanathnagar
 child1           column=personal information:name, timestamp=176
                  2995968676, value=Divya Prathipati
 dad              column=favorites:food1, timestamp=1762995416933
                  , value=Idli
 dad              column=favorites:food2, timestamp=1762995448151
                  , value=Dosa
 dad              column=favorites:vacation1, timestamp=176299548
                  2047, value=Vizag
 dad              column=location information:city, timestamp=176
                  2995529246, value=Bheemli
 dad              column=personal information:name, timestamp=176
                  2995359137, value=Rambabu Prathipati
 mom              column=favorites:food1, timestamp=1762994146285
                  , value=Mudda Papu-Avakaya
 mom              column=favorites:vacation1, timestamp=176299470
                  1533, value=Guntur
 mom              column=favorites:vacation2, timestamp=176299477
                  2463, value=Chennai
 mom              column=location information:city, timestamp=176
                  2995098548, value=Pathareddy Palem
 mom              column=personal information:name, timestamp=176
                  2994059863, value=Rani Prathipati
3 row(s) in 0.0390 seconds

hbase(main):010:0> get 'family', 'child1'
COLUMN            CELL
 favorites:food1  timestamp=1762996002391, value=Pani Puri
 favorites:food2  timestamp=1762996286273, value=Gongura mutton
 favorites:vacati timestamp=1762996024275, value=Hyderabad
 on1
 location informa timestamp=1762996143637, value=Sanathnagar
 tion:city
 personal informa timestamp=1762995968676, value=Divya Prathipati
 tion:name
5 row(s) in 0.0280 seconds

hbase(main):010:0>

#Row-4

hbase(main):001:0> put 'family', 'dog1', 'personal information:name', 'Jackie'
0 row(s) in 0.2390 seconds

hbase(main):002:0> put 'family', 'dog1', 'favorites:food1', 'Pedigree'
0 row(s) in 0.0090 seconds

hbase(main):003:0> put 'family', 'dog1', 'favorites:food2', 'chicken'
0 row(s) in 0.0090 seconds

hbase(main):004:0> put 'family', 'dog1', 'favorites:vacation1', 'PathaReddy Palem'
0 row(s) in 0.0100 seconds

hbase(main):005:0> put 'family', 'dog1', 'location information:city', 'Guntur'
0 row(s) in 0.0100 seconds

hbase(main):007:0> scan 'family'
ROW               COLUMN+CELL
 child1           column=favorites:food1, timestamp=1762996002391
                  , value=Pani Puri
 child1           column=favorites:food2, timestamp=1762996286273
                  , value=Gongura mutton
 child1           column=favorites:vacation1, timestamp=176299602
                  4275, value=Hyderabad
 child1           column=location information:city, timestamp=176
                  2996143637, value=Sanathnagar
 child1           column=personal information:name, timestamp=176
                  2995968676, value=Divya Prathipati
 dad              column=favorites:food1, timestamp=1762995416933
                  , value=Idli
 dad              column=favorites:food2, timestamp=1762995448151
                  , value=Dosa
 dad              column=favorites:vacation1, timestamp=176299548
                  2047, value=Vizag
 dad              column=location information:city, timestamp=176
                  2995529246, value=Bheemli
 dad              column=personal information:name, timestamp=176
                  2995359137, value=Rambabu Prathipati
 dog1             column=favorites:food1, timestamp=1762996817141
                  , value=Pedigree
 dog1             column=favorites:food2, timestamp=1762996863427
                  , value=chicken
 dog1             column=favorites:vacation1, timestamp=176299691
                  8970, value=PathaReddy Palem
 dog1             column=location information:city, timestamp=176
                  2996976044, value=Guntur
 dog1             column=personal information:name, timestamp=176
                  2996741662, value=Jackie
 mom              column=favorites:food1, timestamp=1762994146285
                  , value=Mudda Papu-Avakaya
 mom              column=favorites:vacation1, timestamp=176299470
                  1533, value=Guntur
 mom              column=favorites:vacation2, timestamp=176299477
                  2463, value=Chennai
 mom              column=location information:city, timestamp=176
                  2995098548, value=Pathareddy Palem
 mom              column=personal information:name, timestamp=176
                  2994059863, value=Rani Prathipati
4 row(s) in 0.0820 seconds

hbase(main):008:0> get 'family', 'dog1'
COLUMN            CELL
 favorites:food1  timestamp=1762996817141, value=Pedigree
 favorites:food2  timestamp=1762996863427, value=chicken
 favorites:vacati timestamp=1762996918970, value=PathaReddy Palem
 on1
 location informa timestamp=1762996976044, value=Guntur
 tion:city
 personal informa timestamp=1762996741662, value=Jackie
 tion:name
5 row(s) in 0.0320 seconds

hbase(main):009:0>

#Row-5

hbase(main):009:0> put 'family', 'grandma', 'personal information:name', 'AnnaMary'
0 row(s) in 0.0110 seconds

hbase(main):010:0> put 'family', 'grandma', 'favorites:food1', 'Fish curry'
0 row(s) in 0.0100 seconds

hbase(main):011:0> put 'family', 'grandma', 'favorites:food2', 'pulihora'
0 row(s) in 0.0100 seconds

hbase(main):012:0> put 'family', 'grandma', 'favorites:vacation1', 'Ravipadu'
0 row(s) in 0.0090 seconds

hbase(main):013:0> put 'family', 'grandma', 'location information:city', 'Narsaraopet'
0 row(s) in 0.0110 seconds

hbase(main):014:0> scan 'family'
ROW               COLUMN+CELL
 child1           column=favorites:food1, timestamp=1762996002391
                  , value=Pani Puri
 child1           column=favorites:food2, timestamp=1762996286273
                  , value=Gongura mutton
 child1           column=favorites:vacation1, timestamp=176299602
                  4275, value=Hyderabad
 child1           column=location information:city, timestamp=176
                  2996143637, value=Sanathnagar
 child1           column=personal information:name, timestamp=176
                  2995968676, value=Divya Prathipati
 dad              column=favorites:food1, timestamp=1762995416933
                  , value=Idli
 dad              column=favorites:food2, timestamp=1762995448151
                  , value=Dosa
 dad              column=favorites:vacation1, timestamp=176299548
                  2047, value=Vizag
 dad              column=location information:city, timestamp=176
                  2995529246, value=Bheemli
 dad              column=personal information:name, timestamp=176
                  2995359137, value=Rambabu Prathipati
 dog1             column=favorites:food1, timestamp=1762996817141
                  , value=Pedigree
 dog1             column=favorites:food2, timestamp=1762996863427
                  , value=chicken
 dog1             column=favorites:vacation1, timestamp=176299691
                  8970, value=PathaReddy Palem
 dog1             column=location information:city, timestamp=176
                  2996976044, value=Guntur
 dog1             column=personal information:name, timestamp=176
                  2996741662, value=Jackie
 grandma          column=favorites:food1, timestamp=1762997303739
                  , value=Fish curry
 grandma          column=favorites:food2, timestamp=1762997371707
                  , value=pulihora
 grandma          column=favorites:vacation1, timestamp=176299742
                  0199, value=Ravipadu
 grandma          column=location information:city, timestamp=176
                  2997910290, value=Narsaraopet
 grandma          column=personal information:name, timestamp=176
                  2997208742, value=AnnaMary
 mom              column=favorites:food1, timestamp=1762994146285
                  , value=Mudda Papu-Avakaya
 mom              column=favorites:vacation1, timestamp=176299470
                  1533, value=Guntur
 mom              column=favorites:vacation2, timestamp=176299477
                  2463, value=Chennai
 mom              column=location information:city, timestamp=176
                  2995098548, value=Pathareddy Palem
 mom              column=personal information:name, timestamp=176
                  2994059863, value=Rani Prathipati
5 row(s) in 0.0990 seconds

hbase(main):015:0> get 'family', 'grandma'
COLUMN            CELL
 favorites:food1  timestamp=1762997303739, value=Fish curry
 favorites:food2  timestamp=1762997371707, value=pulihora
 favorites:vacati timestamp=1762997420199, value=Ravipadu
 on1
 location informa timestamp=1762997910290, value=Narsaraopet
 tion:city
 personal informa timestamp=1762997208742, value=AnnaMary
 tion:name
5 row(s) in 0.0230 seconds

hbase(main):016:0>

```

#### Step 3 – Create HBase queries for the items below.

Place the Hbase code **and the results** after each query.

**Query 1:** Get complete information for a specific family member.
```
hbase(main):016:0> get 'family', 'mom'
COLUMN            CELL
 favorites:food1  timestamp=1762994146285, value=Mudda Papu-Avaka
                  ya
 favorites:vacati timestamp=1762994701533, value=Guntur
 on1
 favorites:vacati timestamp=1762994772463, value=Chennai
 on2
 location informa timestamp=1762995098548, value=Pathareddy Palem
 tion:city
 personal informa timestamp=1762994059863, value=Rani Prathipati
 tion:name
5 row(s) in 0.0320 seconds

hbase(main):017:0>

```

**Query 2:** View only personal information for all family members.
```
hbase(main):018:0> scan 'family', { COLUMNS => 'personal information' }
ROW               COLUMN+CELL
 child1           column=personal information:name, timestamp=176
                  2995968676, value=Divya Prathipati
 dad              column=personal information:name, timestamp=176
                  2995359137, value=Rambabu Prathipati
 dog1             column=personal information:name, timestamp=176
                  2996741662, value=Jackie
 grandma          column=personal information:name, timestamp=176
                  2997208742, value=AnnaMary
 mom              column=personal information:name, timestamp=176
                  2994059863, value=Rani Prathipati
5 row(s) in 0.0190 seconds

hbase(main):019:0>

```

**Query 3:** Get the name, favorite foods, and vacation locations for one family member.
```
hbase(main):019:0> get 'family', 'dad', { COLUMNS => ['personal information:name', 'favorites'] }
COLUMN            CELL
 favorites:food1  timestamp=1762995416933, value=Idli
 favorites:food2  timestamp=1762995448151, value=Dosa
 favorites:vacati timestamp=1762995482047, value=Vizag
 on1
 personal informa timestamp=1762995359137, value=Rambabu Prathipa
 tion:name        ti
4 row(s) in 0.0150 seconds

hbase(main):020:0>
```

**Query 4:** Get a range of at least two family members.
```
hbase(main):020:0> scan 'family', { STARTROW => 'child1', STOPROW => 'dad~' }
ROW               COLUMN+CELL
 child1           column=favorites:food1, timestamp=1762996002391
                  , value=Pani Puri
 child1           column=favorites:food2, timestamp=1762996286273
                  , value=Gongura mutton
 child1           column=favorites:vacation1, timestamp=176299602
                  4275, value=Hyderabad
 child1           column=location information:city, timestamp=176
                  2996143637, value=Sanathnagar
 child1           column=personal information:name, timestamp=176
                  2995968676, value=Divya Prathipati
 dad              column=favorites:food1, timestamp=1762995416933
                  , value=Idli
 dad              column=favorites:food2, timestamp=1762995448151
                  , value=Dosa
 dad              column=favorites:vacation1, timestamp=176299548
                  2047, value=Vizag
 dad              column=location information:city, timestamp=176
                  2995529246, value=Bheemli
 dad              column=personal information:name, timestamp=176
                  2995359137, value=Rambabu Prathipati
2 row(s) in 0.0530 seconds

hbase(main):021:0>
```

**Query 5:** Get the addresses for all family members.
```
hbase(main):021:0> scan 'family', {COLUMNS => ['location information:street', 'location information:city', 'location information:state', 'location information:zip', 'location information:phone'] }
ROW               COLUMN+CELL
 child1           column=location information:city, timestamp=176
                  2996143637, value=Sanathnagar
 dad              column=location information:city, timestamp=176
                  2995529246, value=Bheemli
 dog1             column=location information:city, timestamp=176
                  2996976044, value=Guntur
 grandma          column=location information:city, timestamp=176
                  2997910290, value=Narsaraopet
 mom              column=location information:city, timestamp=176
                  2995098548, value=Pathareddy Palem
5 row(s) in 0.0330 seconds

hbase(main):022:0>
```

**Query 6:** Get the names of family members who like a specific favorite food (e.g., pizza).
```
hbase(main):022:0> scan 'family', { COLUMNS => 'favorites', FILTER => "ValueFilter(=, 'substring:Dosa')" }
ROW               COLUMN+CELL
 dad              column=favorites:food2, timestamp=1762995448151
                  , value=Dosa
1 row(s) in 0.0390 seconds

hbase(main):023:0>
```

**Query 7:** Create a vacation preference list with names.
```
hbase(main):023:0> scan 'family', { COLUMNS => ['personal information','favorites'], FILTER => "MultipleColumnPrefixFilter('name','vacation')" }
ROW               COLUMN+CELL
 child1           column=favorites:vacation1, timestamp=176299602
                  4275, value=Hyderabad
 child1           column=personal information:name, timestamp=176
                  2995968676, value=Divya Prathipati
 dad              column=favorites:vacation1, timestamp=176299548
                  2047, value=Vizag
 dad              column=personal information:name, timestamp=176
                  2995359137, value=Rambabu Prathipati
 dog1             column=favorites:vacation1, timestamp=176299691
                  8970, value=PathaReddy Palem
 dog1             column=personal information:name, timestamp=176
                  2996741662, value=Jackie
 grandma          column=favorites:vacation1, timestamp=176299742
                  0199, value=Ravipadu
 grandma          column=personal information:name, timestamp=176
                  2997208742, value=AnnaMary
 mom              column=favorites:vacation1, timestamp=176299470
                  1533, value=Guntur
 mom              column=favorites:vacation2, timestamp=176299477
                  2463, value=Chennai
 mom              column=personal information:name, timestamp=176
                  2994059863, value=Rani Prathipati
5 row(s) in 0.0360 seconds

hbase(main):024:0>
```

### Part 4 - 7in7 - Day 3 - Wrap Up

Read Day 3 - Wrap Up. Then answer the following.

1. List the pros of HBase as described in our text.

    ```
    i. Data lifecycle control is included in with TTL and max-versions per column family.

    ii. As tables expand, region rebalancing occurs automatically; regionservers can be added to scale linearly.

    ```

2. List the cons of HBase as described in our text.

    ```
    i. Numerous tiny cells and versions might slow the scans and inflate metadata.

    ii. Not the best option for OLTP with small datasets.

    iii. While recovery and reassignment can halt workloads, region server failover is safe.
    ```


### 7in7 - Day 2 - OPTIONAL

OPTIONAL - You may safely skip Day 2.

This section contains a code-heavy example of loading a large amount
of data into your HBase database. If you feel like a challenge and are
interested, feel free to work through it.

If you do this section, please **do not** use ./save.bash to save
your database, because it may become very large.

So if you are ready for the challenge, below are some instructions to
help you along. Good luck!

1. Download and extract the source code for the text: https://pragprog.com/titles/pwrdata/seven-databases-in-seven-weeks-second-edition/

2. Drag the following files into 02_hbase/local/scripts on GitPod.
    * source_code/02_hbase/import_from_wikipedia.rb
    * source_code/02_hbase/create_wiki_schema.rb

3. Start your database.

4. Run the following to create the wiki table.

    ```
    ./shell.bash create_wiki_schema.rb
    ```

5. Now you should be able to run the command below.
    BEFORE YOU DO... be ready to press CTRL+C to stop the process. This
    command will load a lot of data very fast.

    ```
    curl https://dumps.wikimedia.org/enwiki/latest/enwiki-latest-pages-articles.xml.bz2 | bzcat | ./shell.bash import_from_wikipedia.rb
    ```

6. After it appears to be working, press CTRL+C to stop it.

7. Connect to your database.

8. Run the command below to count the number of
    rows in your 'wiki' table.

    ```
    count 'wiki'
    ```

    Copy and paste the output of this command below.

9. Run the command below to get information about your database's regions.

    ```
    scan 'hbase:meta',{FILTER=>"PrefixFilter('wiki')"}
    ```

    Copy and past the output of this command below.
