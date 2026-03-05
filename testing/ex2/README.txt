mkdir -p ./data/db
module load apptainer/1.4.1
apptainer pull docker://mongo:latest
apptainer exec --bind ./data/db:/data/db mongo_latest.sif mongod --dbpath /data/db --
bind_ip_all --port 27017
apptainer exec mongo_latest.sif mongosh --eval "show dbs"
apptainer exec mongo_latest.sif mongosh --host localhost --port 27017

test> 
test> use books_db
switched to db books_db
books_db> show collections

