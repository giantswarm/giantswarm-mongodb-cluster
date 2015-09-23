# giantswarm-mongodb-cluster example

Start three [config server](http://docs.mongodb.org/manual/tutorial/deploy-shard-cluster/#start-the-config-server-database-instances) at first:
```bash
swarm up cfgi.json --var i=0 -d
swarm up cfgi.json --var i=1 -d
swarm up cfgi.json --var i=2 -d
```

Then the [mongos](http://docs.mongodb.org/manual/tutorial/deploy-shard-cluster/#start-the-mongos-instances) instance:
```bash
swarm up mongos0.json -d
```

At last two [shards](http://docs.mongodb.org/manual/tutorial/deploy-shard-cluster/#add-shards-to-the-cluster):
```bash
swarm up rsi-j.json --var i=0 --var j=0 -d
swarm up rsi-j.json --var i=0 --var j=1 -d
swarm up rsi-j.json --var i=0 --var j=2 -d

swarm up rsi-j.json --var i=1 --var j=0 -d
swarm up rsi-j.json --var i=1 --var j=1 -d
swarm up rsi-j.json --var i=1 --var j=2 -d
```
