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

Connect nodes to replicaset `rs0-0`:
```bash
rs.initiate();
rs.add('mongod.rs0-1.rs0-1');
rs.add('mongod.rs0-2.rs0-2');
rs.conf();
rs.status();
```

Connect nodes to replicaset `rs1-0`:
```bash
rs.initiate();
rs.add('mongod.rs1-1.rs1-1');
rs.add('mongod.rs1-2.rs1-2');
rs.conf();
rs.status();
```

Connect replicasets as shards to cluster:
```bash
sh.addShard('rs0/mongod.rs0-0.rs0-0.support.activator.gigantic.local:27017');
sh.addShard('rs1/mongod.rs1-0.rs1-0.support.activator.gigantic.local:27017');
ssh.startBalancer();
sh.status();
```
