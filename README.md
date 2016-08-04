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
  
  

    import argparse
    import sys
    import boto.rds2.exceptions
    from datetime import datetime, timedelta
    def delete_rds_snapshots(connobj, rds_instance, duration, action):
        conn = connobj
        delete_time = datetime.utcnow() - timedelta(days=duration)
        rds_snapshots = conn.describe_db_snapshots(db_instance_identifier=rds_instance)
        list_rds_snapshots = rds_snapshots['DescribeDBSnapshotsResponse']['DescribeDBSnapshotsResult']['DBSnapshots']
        for snapshots in list_rds_snapshots:
            if snapshots["Status"] == "available":
                create_time = datetime.fromtimestamp(snapshots["SnapshotCreateTime"]).strftime('%Y-%m-%d %H:%M:%S')
                if create_time < delete_time.strftime('%Y-%m-%d %H:%M:%S'):
                    if action == "list":
                        print("Available snapshots: " + snapshots["DBSnapshotIdentifier"] +
                          ", " + datetime.fromtimestamp(snapshots["SnapshotCreateTime"]).strftime('%Y-%m-%d %H:%M:%S'))
                    if action == "delete":
                        conn.delete_db_snapshot(db_snapshot_identifier=snapshots["DBSnapshotIdentifier"])
                        print(snapshots["DBSnapshotIdentifier"] + " Snapshot deleted succesfully!!")


    def start_from_here(region, rds_instance_name, days, action):
        try:
            conn = boto.rds2.connect_to_region(region)
            delete_rds_snapshots(conn, rds_instance_name, int(days), action)
        except Exception as E:
            print E

    if __name__ == "__main__":
        arg_parser = argparse.ArgumentParser("Boto script to delete rds snapshots older than X days")
        arg_parser.add_argument("region", help="US-EAST-1 or US-WEST-2")
        arg_parser.add_argument("rds_instance_name", help="ex: db-master-1")
        arg_parser.add_argument("days", help="X number of days snapshots need to be deleted")
        arg_parser.add_argument("action", help="list:      prints all the available snapshots to delete\n"
                                           "delete:    delete all the snapshots which are older than the x days")
    if len(sys.argv) < 5:
        arg_parser.print_help()
        sys.exit(1)
    arguments = arg_parser.parse_args()
    start_from_here(arguments.region, arguments.rds_instance_name, arguments.days, arguments.action)


for more scripts please visit https://cloudtutorialsblog.wordpress.com
