# s3fs
Ubuntu Trusty with [s3fs-fuse](https://github.com/s3fs-fuse/s3fs-fuse) and [supervisor](http://supervisord.org) as base image for other app containers.

## Quick Start
```
docker run --privileged --cap-add SYS_ADMIN --device /dev/fuse -d -e "BUCKETNAME=$BUCKETNAME" -e "AWSACCESSKEYID=$AWSACCESSKEYID" -e "AWSSECRETACCESSKEY=$AWSSECRETACCESSKEY" hiveoss/s3fs
```

## Configurations

#### Environment variables
The environment variables are used to indicate the bucket to mount and to provide credentials to access the bucket.
`$BUCKETNAME` - the AWS bucket name
`$AWSACCESSKEYID` - the AWS key id
`$AWSSECRETACCESSKEY` - the AWS secret token

#### s3fs-fuse
You can overwrite `scripts/runS3fs.sh` to change the parameters for the s3fs.
```
mkdir -p /home/shared/s3 && 
mkdir -p /tmp && 
s3fs $BUCKETNAME /home/shared/s3 -o use_cache=/tmp -o allow_other -o umask=0002 -o use_rrs -f
``` 

#### supervisor
You can overwrite `config/supervisord.conf` to change the parameters for the supervisord. But usually you do not need to unless you need to start additional processes. Noted that daemon is off by default for supervisor.

#### Default S3 folder
The default s3 bucket is mounted at `/home/shared/s3`.

## As Base Image
```
FROM hiveoss/s3fs

CMD ["/usr/bin/supervisord"]
```