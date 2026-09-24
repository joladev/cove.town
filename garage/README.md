# s3.cove.town

[Garage](https://garagehq.deuxfleurs.fr/) provides an S3 compatible object store, used by hold and spindle.

Admin UI hosted on https://garage.tailscalenet. To generate a new key, run `docker exec $(docker ps -qf label=com.docker.compose.service=garage) /garage admin-token create -q webadmin`
