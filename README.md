# Curator

Have time-series indices in Elasticsearch? This is the tool for you!

## Versioning

There are two branches for development - `master` and `0.6`. Master branch is
used to track all the changes for Elasticsearch 1.0 and beyond whereas 0.6
tracks Elasticsearch 0.90 and the corresponding `elasticsearch-py` version.

Releases with major version 1 (1.X.Y) are to be used with Elasticsearch 1.0 and
later, 0.6 releases are meant to work with Elasticsearch 0.90.X.

## Usage

Install using pip

    pip install elasticsearch-curator

See `curator --help` for usage specifics.

### Defaults

The default values for the following are:

    --host localhost
    --port 9200
    -t (or --timeout) 30
    -T (or --time-unit) days
    -p (or --prefix) logstash-
    -s (or --separator) .


If your values match these you do not need to include them.  The value of `prefix` should be everything before the date string, i.e. `--prefix .marvel-` would match index `.marvel-2014.05.27`, and all other indices beginning with `.marvel-` (don't forget the trailing hyphen!).

### [Documentation & Examples](http://github.com/elasticsearch/curator/wiki)

See the [Curator Wiki](http://github.com/elasticsearch/curator/wiki) on Github for more documentation

#### Commands

* alias - Add/Remove indices from existing aliases
* allocation - Add a 'require' tag to indices for routing allocation
* bloom - Disable bloom filter cache for expired indices
* close - Close indices
* delete - Delete indices
* optimize - Optimize (Lucene forceMerge) indices
* show - Show indices or snapshots
* snapshot - Snapshot indices to existing repository
    
### Timeouts
With some operations (e.g. `optimize` and `snapshot`) the default behavior is to wait until the operation is complete before proceeding with the next step.  Since these operations can take quite a long time it is advisable to set `--timeout` to a high value.  If you do not specify, a default of 6 hours will be applied (21,600 seconds).


## Contributing

* fork the repo
* make changes in your fork
* run tests
* send a pull request!

### Running tests

To run the test suite just run `python setup.py test`

When changing code, contributing new code or fixing a bug please make sure you
include tests in your PR (or mark it as without tests so that someone else can
pick it up to add the tests). When fixing a bug please make sure the test
actually tests the bug - it should fail without the code changes and pass after
they're applied (it can still be one commit of course).

The tests will try to connect to your local elasticsearch instance and run
integration tests against it. This will delete all the data stored there! You
can use the env variable `TEST_ES_SERVER` to point to a different instance (for
example 'otherhost:9203').

The repository tests all expect to run on a single local node.  These tests will fail if run against a cluster due to the unhappy mixup between unit tests and shared filesystems (total cleanup afterwards).  It is possible, but you would have to manually replace `/tmp/REPOSITORY_LOCATION` with a path on your shared filesystem.  This is defined in `test_curator/integration/__init__.py`

## Origins

Curator was first called `clearESindices.py` [1] and was almost immediately renamed to `logstash_index_cleaner.py` [1].  After a time it was migrated under the [logstash](https://github.com/elasticsearch/logstash) repository as `expire_logs`.  Soon thereafter, Jordan Sissel was hired by Elasticsearch, as was the original author of this tool.  It became Elasticsearch Curator after that and is now hosted at <https://github.com/elasticsearch/curator>

[1] <https://logstash.jira.com/browse/LOGSTASH-211>

