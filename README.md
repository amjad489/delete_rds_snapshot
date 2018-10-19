# AWS RDS BOTO Script to Delete Snapshots
A python script which uses BOTO SDK to delete rds snapshots older than X days

# Dependencies

    boto.rds2

# Features available in the script
This script you can use it to list all the snapshots which can be deleted. As well as you can use it to delete those snapshots. This script can easily be use to automate the RDS snapshot cleanup process. To Automate you just have to schedule it under cron job in Linux or on windows you can schedule from Task Scheduler
    
#How to run the script
Following is the snippet on how to run the script to get the list of available snapshots to be deleted

    python delete_rds_snapshot.py us-east-1 db-master-1 7 list
    
Below snippet on how to run the script to delete the snapshots for the X mentioned days

    python delete_rds_snapshot.py us-east-1 db-master-1 7 delete

# Script Usage
    usage: delete_rds_snapshots.py [-h] region rds_instance_name days action
    positional arguments:
        region              US-EAST-1 or US-WEST-2
        rds_instance_name   ex: db-master-1
        days                X number of days snapshots need to be deleted
        action              list: prints all the available snapshots to delete
                            delete: delete all the snapshots which are older than the x days
    optional arguments:
         -h, --help         show this help message and exit
  
  
