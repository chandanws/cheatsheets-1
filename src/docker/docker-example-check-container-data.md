## Container private data ##

#### Goal

To show where a container puts its private data

#### Instructions

Just check what is inside Docker's system directories

	$ sudo tree -L 3 /var/lib/docker/containers; \
	sudo tree -L 3 /var/lib/docker/volumes; \
	sudo tree -L 3 /var/lib/docker/vfs 

The result depends on if and how you previously run docker. This time on my machine the folders are empty. 

	/var/lib/docker/containers

	0 directories, 0 files
	/var/lib/docker/volumes

	0 directories, 0 files
	/var/lib/docker/vfs
	bb b  dir

	1 directory, 0 files

Run a postgres instance with

	docker run --rm --name mypg -p 127.0.0.1:5432:5432 -e POSTGRES_PASSWORD=pwd postgres:9.4.0

While container is running check again for the folders.

	/var/lib/docker/containers
	bb b  6e556aa9af283a15f0960352bf4277eec74722d03a3aca37722e9976145589c0
	    bb b  6e556aa9af283a15f0960352bf4277eec74722d03a3aca37722e9976145589c0-json.log
	    bb b  config.json
	    bb b  hostconfig.json
	    bb b  hostname
	    bb b  hosts
	    bb b  resolv.conf

	1 directory, 6 files
	/var/lib/docker/volumes
	bb b  605d94e8be0d21fb3b8816b2dcf10dcc714ccb90f1d964f1c75bc9dc322c608d
	    bb b  config.json

	1 directory, 1 file
	/var/lib/docker/vfs
	bb b  dir
	    bb b  605d94e8be0d21fb3b8816b2dcf10dcc714ccb90f1d964f1c75bc9dc322c608d
		bb b  base
		bb b  global
		bb b  pg_clog
		bb b  pg_dynshmem
		bb b  pg_hba.conf
		bb b  pg_ident.conf
		bb b  pg_logical
		bb b  pg_multixact
		bb b  pg_notify
		bb b  pg_replslot
		bb b  pg_serial
		bb b  pg_snapshots
		bb b  pg_stat
		bb b  pg_stat_tmp
		bb b  pg_subtrans
		bb b  pg_tblspc
		bb b  pg_twophase
		bb b  PG_VERSION
		bb b  pg_xlog
		bb b  postgresql.auto.conf
		bb b  postgresql.conf
		bb b  postmaster.opts
		bb b  postmaster.pid

Stop the container <CTRL+C>

Check again in the folders.

	/var/lib/docker/containers

	0 directories, 0 files
	/var/lib/docker/volumes

	0 directories, 0 files
	/var/lib/docker/vfs
	bb b  dir

	1 directory, 0 files

#### Conclusion

run --rm allows to play around with a container removing all its data when it terminates.
