Step by step how to run:

1. Build phpmyadmin image docker build --tag phpmyadmin . (optional for self built)
2. Run official mysql container: *docker run --name mysql -e MYSQL_ROOT_PASSWORD=mysecretpassword -d mysql:5.7.5-m15*
3. Run phpmyadmin: *docker run --rm --link mysql:mysql mssio/phpmyadmin:4.3.10-1* (optional method to create database)
4. Run backup script:

```sh
docker run --rm -it \
  -e "S3_BUCKET=your-bucket-name/your-path-to-mysql-backups/" \
  -e "AWS_ACCESS_KEY_ID=your-aws-access-key-id" \
  -e "AWS_SECRET_ACCESS_KEY=your-aws-secret-access-key" \
  -e "BACKUP_INTERVAL=3600" \
  -e "DB_NAME=mydb" \
  -e "DB_USER=root" \
  -e "DB_PASSWORD=some-mysecretpassword" \
  -e "PATH_DATEPATTERN=%Y/%m" \
  --link mysql:mysql \
  registry.giantswarm.io/yourusername/mysql-archiver
```

This project is created based on article in [Giant Swarm](http://docs.giantswarm.io/guides/mysql-backup/) 