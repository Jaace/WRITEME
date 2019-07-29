## RocksDB: A Persistent Key-Value Store for Flash and RAM Storage

@abstr_hyperlink @abstr_hyperlink @abstr_hyperlink 

RocksDB is developed and maintained by Facebook Database Engineering Team. It is built on earlier work on @abstr_hyperlink by Sanjay Ghemawat (sanjay@google.com) and Jeff Dean (jeff@google.com)

This code is a library that forms the core building block for a fast key value server, especially suited for storing data on flash drives. It has a Log-Structured-Merge-Database (LSM) design with flexible tradeoffs between Write-Amplification-Factor (WAF), Read-Amplification-Factor (RAF) and Space-Amplification-Factor (SAF). It has multi-threaded compactions, making it specially suitable for storing multiple terabytes of data in a single database.

Start with example usage here: https://github.com/facebook/rocksdb/tree/master/examples

See the @abstr_hyperlink for more explanation.

The public interface is in `include/`. Callers should not include or rely on the details of any other header files in this package. Those internal APIs may be changed without warning.

Design discussions are conducted in https://www.facebook.com/groups/rocksdb.dev/

## License

RocksDB is dual-licensed under both the GPLv @abstr_number (found in the COPYING file in the root directory) and Apache @abstr_number . @abstr_number License (found in the LICENSE.Apache file in the root directory). You may select, at your option, one of the above-listed licenses.
