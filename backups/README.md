This container uses inotify to detect changes to /var/backup on the host
and backs up to Amazon S3. You can utilize a CoreOS unit file for such
a backup:

[Unit]
Description=Backup  
After=docker.service  
Requires=docker.service

[Service]
User=core  
TimeoutStartSec=5  
ExecStart=/bin/bash -c  ' \  
  /usr/bin/docker run \
    --volumes-from ghost-override \
    --name=backup-ghost \
    --rm \
    -e  AWS_ACCESS_KEY_ID=<ACCESS_KEY_ID> -e AWS_SECRET_ACCESS_KEY=<ACCESS_KEY> \
    --privileged  \
    --entrypoint "/bin/bash" \
    jetsnoc/backups \
    -c "mount -o bind /ghost-override /var/backup && /backup.sh s3+http://<BUCKET>.s3.amazonaws.com/ghost 60"'

ExecStop=/usr/bin/docker stop backup-ghost

[Install]
WantedBy=multi-user.target  

