# slite_sha1
Sqlite with SHA1

https://stackoverflow.com/questions/3179021/sha1-hashing-in-sqlite-how



sudo apt build-dep sqlite3 # fetches dependencies to compile sqlite3

mkdir sqlite-compilation
cd    sqlite-compilation

wget -O sqlite.tar.gz https://www.sqlite.org/src/tarball/sqlite.tar.gz?r=release

tar xzf sqlite.tar.gz

mkdir build
cd    build
  ../sqlite/configure
  make OPTS='-DSQLITE_ENABLE_LOAD_EXTENSION'
  ./sqlite3 -cmd 'pragma compile_options;' <<< .exit
cd -

cd sqlite/ext/misc
  # https://sqlite.org/src/file?name=ext/misc/sha1.c
  sed -i 's/int sqlite3_sha_init(/int sqlite3_extension_init(/' sha1.c # this is needed to give object file custom name, for example libSqlite3Sha1.so:
  gcc -g -O2 -shared -fPIC -I ../../../build -o libSqlite3Sha1.so ./sha1.c
  cp libSqlite3Sha1.so ../../../build/
cd -

