mkdir -p ./data/db
module load apptainer/1.4.1
apptainer pull docker://mongo:latest
apptainer exec --bind ./data/db:/data/db mongo_latest.sif mongod --dbpath /data/db --bind_ip_all --port 27017
apptainer exec mongo_latest.sif mongosh --eval "show dbs"
apptainer exec mongo_latest.sif mongosh --eval "use books_db; db.dummy.insert({initialized: true})"
apptainer exec mongo_latest.sif mongosh --host localhost --port 27017


test> 
test> use books_db
# switched to db books_db
books_db> show collections
# 
book_db> db.createCollection("books")
{ ok: 1 }

##BEFORE SCRAPING
books_db> db.books.countDocuments()
0

##AFTER SCRAPING
books_db> db.books.countDocuments()
1000
books_db> db.books.deleteMany({})
{ acknowledged: true, deletedCount: 1000 }
books_db> db.books.countDocuments()
0
books_db> db.books.findOne()
{
  _id: '712ca12caf4cc95d75647013d2c9bd408b544e8133b7094eb601c4329151d338',
  url: 'catalogue/a-light-in-the-attic_1000/index.html',
  title: 'A Light in the Attic',
  price: '£51.77'
}
books_db> Object.keys(db.books.findOne())
[ '_id', 'url', 'title', 'price' ]
# Find all
books_db> db.books.find()
