
HHP (Heritage Health Prize) open claims dataset and Apache Drill


Vagrantfile to rev up Apache Drill and test its capabilities using HHP dataset. 

Get full dataset "HHP release 3" using following link:

http://www.heritagehealthprize.com/c/hhp/data

NOTE: a small sample Claims.csv dataset as a test is already included in this repository.


## Prerequisite

Vagrant,  Vagrantfile tested with virtual box as provider

   To launch VM run
  > vagrant up

## Test queries

To run queries on drill issue following command on shell after vagrant ssh:

cd /opt/apache-drill-0.7.0
bin/sqlline -u jdbc:drill:zk=local


## Example Queries

### Group by service speciality

    SELECT a.columns[5], count(*)  
       FROM dfs.`/vagrant/HHP_release3_dataset/Claims.csv` a 
       JOIN dfs.`/vagrant/HHP_release3_dataset/Members.csv` m 
         ON a.columns[0] = m.columns[0] where m.columns[0] <> 'MemberID' 
       GROUP 
          BY a.columns[5] order by 2 desc;

### Group by Geneder, Age Group , project avg. length of stay

    SELECT m.columns[2] as Gender, 
        m.columns[1] as AgeGroup, 
        Avg( cast( substring(a.columns[9],1,1) as double)) as AvgLengthOfStay, 
        count(*) as TotalNumberRecords
       FROM dfs.`/vagrant/HHP_release3_dataset/Claims.csv` a 
       JOIN dfs.`/vagrant/HHP_release3_dataset/Members.csv` m 
         ON a.columns[0] = m.columns[0] where substring(a.columns[9],1,1) not in ('D', '') 
       GROUP
          BY m.columns[2], m.columns[1] order by 1,2;

### OUTPUT
Output when using original Claims.csv present within HHP release 3:


    +------------+------------+-----------------+-------------+
    |   Gender   |  AgeGroup  | AvgLengthOfStay | TotalNumberRecords|
    +------------+------------+-----------------+-------------+
    | F          |            | 3.34            | 54872       |
    | F          | 0-9        | 2.21            | 40734       |
    | F          | 10-19      | 2.00            | 39756       |
    | F          | 20-29      | 2.49            | 47460       |
    | F          | 30-39      | 2.54            | 80264       |
    | F          | 40-49      | 2.76            | 126191      |
    | F          | 50-59      | 2.98            | 122099      |
    | F          | 60-69      | 3.30            | 173778      |
    | F          | 70-79      | 3.55            | 285468      |
    | F          | 80+        | 3.53            | 138526      |
    | M          |            | 3.13            | 43227       |
    | M          | 0-9        | 2.23            | 43186       |
    | M          | 10-19      | 1.74            | 35567       |
    | M          | 20-29      | 1.36            | 16369       |
    | M          | 30-39      | 1.87            | 41453       |
    | M          | 40-49      | 2.29            | 76292       |
    | M          | 50-59      | 2.74            | 88920       |
    | M          | 60-69      | 3.22            | 124307      |
    | M          | 70-79      | 3.52            | 215407      |
    | M          | 80+        | 3.58            | 82620       |
    | null       |            | 3.38            | 143586      |
    | null       | 0-9        | 2.87            | 22869       |
    | null       | 10-19      | 2.90            | 22088       |
    | null       | 20-29      | 2.94            | 23273       |
    | null       | 30-39      | 3.10            | 38665       |
    | null       | 40-49      | 3.29            | 63554       |
    | null       | 50-59      | 3.42            | 75073       |
    | null       | 60-69      | 3.55            | 102199      |
    | null       | 70-79      | 3.64            | 145547      |
    | null       | 80+        | 3.64            | 102870      |
    +------------+------------+-----------------+-------------+





### Misc. queries

    Group by gender
        SELECT m.columns[2], count(*) from dfs.`/vagrant/HHP_release3_dataset/Claims.csv` a join dfs.`/vagrant/HHP_release3_dataset/Members.csv` m on a.columns[0] = m.columns[0] group by m.columns[2];


    SELECT m.columns[2], variance( cast( substring(a.columns[9],1,1) as int)), count(*) from dfs.`/vagrant/HHP_release3_dataset/Claims.csv` a join dfs.`/vagrant/HHP_release3_dataset/Members.csv` m on a.columns[0] = m.columns[0] where substring(a.columns[9],1,1) not in ('D', '') group by  m.columns[2];

