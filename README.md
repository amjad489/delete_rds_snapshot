# delete_rds_snapshot
Boto script to delete rds snapshots older than X days

usage: delete_rds_snapshots.py [-h] region rds_instance_name days action


positional arguments:

      region              US-EAST-1 or US-WEST-2
      rds_instance_name   ex: db-master-1
      days                X number of days snapshots need to be deleted
      action              list: prints all the available snapshots to delete
                          delete: delete all the snapshots which are older than the x days
                     
optional arguments:

      -h, --help         show this help message and exit
  

for more scripts please visit https://cloudtutorialsblog.wordpress.com
