# datix
UNIX structured database written in rust

## Idea
Datix will be structured in unix style, meaning that each component
will be separate process, connected to another by unix socket

Launching datix will start with executing coordinator process
which will than run elements of the DB specified in configuration

### Exemplary launch config
`
services:
    - endpoint:
        name: GRPC_endpoint
        startup:
            type: file
            path: ./bin/GRPS_endpoint
    - logger:
        name: file_logger
        startup:
            type: file
            path: ./bin/file_logger
    - executor:
        name: simple_column_executor
        startup:
            type: file
            path: ./bin/simple_column_executor
    - cache:
            name: redis_cacher
            startup:
                type: file
                path: ./bin/redis_cacher
    - bin_file_tables:
        name: compact_bin_file_tables
        startup:
            type: file
            path: ./bin/compact_bin_file_tables
    - json_objects_tables:
        name: BT_indexes
            startup:
                type: file
                path: ./bin/BT_indexes
    - indexes:
        name: BT_indexes
        startup:
            type: file
            path: ./bin/BT_indexes

connections:
    - endpoint:
        - executor
        - logger
    - executor:
        - bin_file_tables
        - json_objects_tables
        - indexes
        - cache
        - logger
`

## Goals
reason to start this project is to create database that
will be extremely configurable, high performance modular database
that will be fit to use cases that make you think that writing
your DB would be best solution. With datix you'll be able to choose
modules that best suit your need, maybe write one custom module
to suit your use case which because of well defined protocol's should
be as easy as possible.

I'll begin with single node database, but plan is to have
highly distributed one.

Another important point is to have query chaining, with output of
one being able to be input of another
