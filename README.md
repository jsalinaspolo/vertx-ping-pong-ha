# Vertx3 ping-pong high availbility and a bare instance example

## Environment configuration

I've created 3 different configurations to cover the different possible environments

1. Local (disable multicast, enable tcp-ip and overwrite interfaces to use local 127.0.0.1)
2. Development (disable multicast, enable tcp-ip and overwrite interfaces with the nodes)
3. Test (enable multicast, overwrite interfaces to fix problems when multiple network interfaces on the server)


## Build by environment

1. Local:
```mvn clean install -Plocal```

2. Dev:
```mvn clean install -Pdev```

3. Test:
```mvn clean install -Ptest```

## Upload jars to different machines

1. Copy distribution/pong-1.0-SNAPSHOT-fat.jar in one node
2. Copy distribution/ping-1.0-SNAPSHOT-fat.jar in other node
2. Copy distribution/bare-instance-1.0-SNAPSHOT-fat.jar in a different node

## Run the jars
* Pong:

 ```java -jar pong-1.0-SNAPSHOT-fat.jar -ha -cluster-host XXX.XXX.XXX.XX```

for example

```java -jar pong-1.0-SNAPSHOT-fat.jar -ha -cluster-host 127.0.0.1```

or

```java -jar pong-1.0-SNAPSHOT-fat.jar -ha -cluster-host 192.168.112.10```

* Ping:

```java -jar ping-1.0-SNAPSHOT-fat.jar -ha -cluster-host XXX.XXX.XXX.XX```

for example

```java -jar ping-1.0-SNAPSHOT-fat.jar -ha -cluster-host 127.0.0.1```

or

```java -jar ping-1.0-SNAPSHOT-fat.jar -ha -cluster-host 192.168.112.9```

* Bare instance

```java -cp 'distribution/*' com.jspcore.BareInstance bare```


## Testing High Availability

1. Scenario 1.
 * Do a kill -9 of some process ping or pong (kill -9 PID), then the bare instance deploy the verticle killed

2. Scenario 2.
 * Do a kill -9 of both process (kill -9 PID1 PID2), then the bare instance deploy sometimes one of them or none.

3. Scenario 3 (Deploying ping and pong in same machine and bare instance in a different machine)
 * Do a kill -9 of some process ping or pong (kill -9 PID), then the bare instance deploy the verticle killed in the machine
 where is running

4. Scenario 4 (Deploying ping and pong in same machine and bare instance in a different machine)
 * Shutdown the machine where ping/pong is running to simulate a crash, then the bare instance after 5 minutes deploy
sometimes one of them or none.


