# <img src="https://github.com/mostafa/xk6-kafka/blob/main/assets/xk6-kafka-logo.png" alt="xk6-kafka logo" style="height: 32px; width:32px;"/> xk6-kafka

[![GitHub Workflow Status](https://img.shields.io/github/workflow/status/mostafa/xk6-kafka/Build%20and%20publish%20xk6-kafka?logo=github)](https://github.com/mostafa/xk6-kafka/actions) [![Docker Pulls](https://img.shields.io/docker/pulls/mostafamoradian/xk6-kafka?logo=docker)](https://hub.docker.com/r/mostafamoradian/xk6-kafka) [![Coverage Status](https://coveralls.io/repos/github/mostafa/xk6-kafka/badge.svg?branch=main)](https://coveralls.io/github/mostafa/xk6-kafka?branch=main)

The xk6-kafka project is a [k6 extension](https://k6.io/docs/extensions/guides/what-are-k6-extensions/) that enables k6 users to load test Apache Kafka using a producer and possibly a consumer for debugging.

The real purpose of this extension is to test the system you meticulously designed to use Apache Kafka. So, you can test your consumers, hence your system, by auto-generating messages and sending them to your system via Apache Kafka.

You can send many messages with each connection to Kafka. These messages are arrays of objects containing a key and a value in various serialization formats, passed via configuration objects. Various serialization formats are supported, including strings, JSON, binary, Avro, and JSON Schema. Avro and JSON Schema can either be fetched from Schema Registry or hard-code directly in the script. SASL PLAIN/SCRAM authentication and message compression are also supported.

For debugging and testing purposes, a consumer is available to make sure you send the correct data to Kafka.

If you want to learn more about the extension, see the [article](https://k6.io/blog/load-test-your-kafka-producers-and-consumers-using-k6/) explaining how to load test your Kafka producers and consumers using k6 on the k6 blog.

## Supported Features

- Produce/consume messages as [String](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_json.js), [stringified JSON](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_json.js), [ByteArray](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_bytes.js), [Avro](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_avro_with_schema_registry.js) and [JSON Schema](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_jsonschema_with_schema_registry.js) format
- Support for user-provided [Avro](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_avro.js) and [JSON Schema](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_jsonschema_with_schema_registry.js) key and value schemas in the script
- Authentication with [SASL PLAIN and SCRAM](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_sasl_auth.js)
- Create, list and delete [topics](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_topics.js)
- Support for loading Avro schemas from [Schema Registry](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_avro_with_schema_registry.js)
- Support for [byte array](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_bytes.js) for binary data (from binary protocols)
- Support consumption from all partitions with a group ID
- Support Kafka message compression: Gzip, [Snappy](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_json_with_snappy_compression.js), Lz4 & Zstd
- Support for sending messages with [no key](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_avro_no_key.js)
- Support for k6 [thresholds](https://github.com/mostafa/xk6-kafka/blob/e1a810d52112f05d7a66c12740d9885ebb64897e/scripts/test_json.js#L21-L27) on custom Kafka metrics
- Support for [headers](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_json.js) on produced and consumed messages
- Lots of exported metrics, as shown in the result output of the [k6 test script](https://github.com/mostafa/xk6-kafka/blob/main/README.md#k6-test-script)

## Download Binaries

### The Official Docker Image

The [official Docker image](https://hub.docker.com/r/mostafamoradian/xk6-kafka) is available on Docker Hub. Before running your script, make the script available to the container by mounting a volume (a directory) or passing it via stdin.

```bash
docker run --rm -i mostafamoradian/xk6-kafka:latest run - <scripts/test_json.js
```

### The Official Binaries

The binaries are generated by the build process and published on the [releases page](https://github.com/mostafa/xk6-kafka/releases). Currently, binaries for the GNU/Linux, macOS, and Windows on `amd64` (`x86_64`) machines are available.

> **Note:**
> If you want to see an official build for your machine, please build and test xk6-kafka from [source](https://github.com/mostafa/xk6-kafka/blob/main/README.md#build-from-source) and then create an [issue](https://github.com/mostafa/xk6-kafka/issues/new) with details. I'll add the specific binary to the build pipeline and publish them on the next release.

## Build from Source

You can build the k6 binary on various platforms, each with its requirements. The following shows how to build k6 binary with this extension on GNU/Linux distributions.

### Prerequisites

You must have the latest Go version installed to build the k6 binary. The latest version should match [k6](https://github.com/grafana/k6#build-from-source) and [xk6](https://github.com/grafana/xk6#requirements). I recommend [gvm](https://github.com/moovweb/gvm) because it eases version management.

- [gvm](https://github.com/moovweb/gvm) for easier installation and management of Go versions on your machine
- [Git](https://git-scm.com/) for cloning the project
- [xk6](https://github.com/grafana/xk6) for building k6 binary with extensions

### Install and build the latest tagged version

Feel free to skip the first two steps if you already have Go installed.

1. Install gvm by following its [installation guide](https://github.com/moovweb/gvm#installing).
2. Install the latest version of Go using gvm. You need Go 1.4 installed for bootstrapping into higher Go versions, as explained [here](https://github.com/moovweb/gvm#a-note-on-compiling-go-15).
3. Install `xk6`:

    ```shell
    go install go.k6.io/xk6/cmd/xk6@latest
    ```

4. Build the binary:

    ```shell
    xk6 build --with github.com/mostafa/xk6-kafka@latest
    ```

> **Note**
> You can always use the latest version of k6 to build the extension, but the earliest version of k6 that supports extensions via xk6 is v0.32.0. The xk6 is constantly evolving, so some APIs may not be backward compatible.

### Build for development

If you want to add a feature or make a fix, clone the project and build it using the following commands. The xk6 will force the build to use the local clone instead of fetching the latest version from the repository. This process enables you to update the code and test it locally.

```bash
git clone git@github.com:mostafa/xk6-kafka.git && cd xk6-kafka
xk6 build --with github.com/mostafa/xk6-kafka@latest=.
```

## Example scripts

There are many examples in the [script](https://github.com/mostafa/xk6-kafka/blob/main/scripts/) directory that show how to use various features of the extension.

## How to Test You Kafka Setup

You can start testing your setup immediately, but it takes some time to develop the script, so it would be better to test your script against a development environment and then start testing your environment.

### Development environment

I recommend the [fast-data-dev](https://github.com/lensesio/fast-data-dev) Docker image by Lenses.io, a Kafka setup for development that includes Kafka, Zookeeper, Schema Registry, Kafka-Connect, Landoop Tools, 20+ connectors. It is relatively easy to set up if you have Docker installed. Just monitor Docker logs to have a working setup before attempting to test because the initial setup, leader election, and test data ingestion take time.

1. Run the Kafka environment and expose the ports:

    ```bash
    sudo docker run \
        --detach --rm \
        --name lensesio \
        -p 2181:2181 \
        -p 3030:3030 \
        -p 8081-8083:8081-8083 \
        -p 9581-9585:9581-9585 \
        -p 9092:9092 \
        -e ADV_HOST=127.0.0.1 \
        -e RUN_TESTS=0 \
        lensesio/fast-data-dev:latest
    ```

2. After running the command, visit [localhost:3030](http://localhost:3030) to get into the fast-data-dev environment.

3. You can run the command to see the container logs:

    ```bash
    sudo docker logs -f -t lensesio
    ```

> **Note:**
> If you have errors running the Kafka development environment, refer to the [fast-data-dev documentation](https://github.com/lensesio/fast-data-dev).

### The xk6-kafka API

All the exported functions are available by importing the module object from `k6/x/kafka`. The exported objects, constants and other data structures are available in the [`index.d.ts`](https://github.com/mostafa/xk6-kafka/blob/main/index.d.ts) file, and they always reflect the *latest* changes on the `main` branch. You can access the generated documentation at [`docs/README.md`](https://github.com/mostafa/xk6-kafka/blob/main/docs/README.md).

> ⚠️ **Warning:**
> The Javascript API is subject to change in future versions unless a new major version is released.

### k6 Test Scripts

The example scripts are available as `test_<format/feature>.js` with more code and commented sections in the [scripts](https://github.com/mostafa/xk6-kafka/blob/main/scripts/) directory. Since this project extends the functionality of k6, it has four stages in the [test life cycle](https://k6.io/docs/using-k6/test-life-cycle/).

1. To use the extension, you need to import it in your script, like any other JS module:

    ```javascript
    // Either import the module object
    import * as kafka from "k6/x/kafka";

    // Or individual classes and constants
    import { Writer, Reader, Connection, SOME_CONSTANT } from "k6/x/kafka";
    ```

2. You need to instantiate the classes in the `init` context. All the [k6 options](https://k6.io/docs/using-k6/k6-options/) are also configured here:

    ```javascript
    // Creates a new Writer object to produce messages to Kafka
    const writer = new Writer({
        // WriterConfig object
        brokers: ["localhost:9092"],
        topic: "my-topic",
    });

    const reader = new Reader({
        // ReaderConfig object
        brokers: ["localhost:9092"],
        topic: "my-topic",
    });

    const connection = new Connection({
        // ConnectionConfig object
        address: "localhost:9092",
    });

    if (__VU == 0) {
        // Create a topic on initialization (before producing messages)
        connection.createTopic({
            // TopicConfig object
            topic: "my-topic",
        });
    }
    ```

3. In the VU code, you can produce messages to Kafka or consume messages from it:

    ```javascript
    export default function() {
        // Fetch the list of all topics
        const topics = connection.listTopics();
        console.log(topics); // list of topics

        // Produces message to Kafka
        writer.produce({
            // ProduceConfig object
            messages: [
                // Message object(s)
                {
                    key: "my-key",
                    value: "my-value",
                },
            ],
        });

        // Consume messages from Kafka
        let messages = reader.consume({
            // ConsumeConfig object
            limit: 10
        });

        // your messages
        console.log(message);

        // You can use checks to verify the contents,
        // length and other properties of the message(s)
    }
    ```

4. In the `teardown` function, close all the connections and possibly delete the topic:

    ```javascript
    export function teardown(data) {
        // Delete the topic
        connection.deleteTopic("my-topic");

        // Close all connections
        writer.close();
        reader.close();
        connection.close();
    }
    ```

5. You can now run k6 with the extension using the following command:

    ```bash
    ./k6 run --vus 50 --duration 60s scripts/test_json.js
    ```

6. And here's the test result output:

    ```bash

              /\      |‾‾| /‾‾/   /‾‾/
         /\  /  \     |  |/  /   /  /
        /  \/    \    |     (   /   ‾‾\
       /          \   |  |\  \ |  (‾)  |
      / __________ \  |__| \__\ \_____/ .io

    execution: local
        script: scripts/test_json.js
        output: -

    scenarios: (100.00%) 1 scenario, 50 max VUs, 1m30s max duration (incl. graceful stop):
            * default: 50 looping VUs for 1m0s (gracefulStop: 30s)


    running (1m00.2s), 00/50 VUs, 13778 complete and 0 interrupted iterations
    default ✓ [======================================] 50 VUs  1m0s

        ✓ 10 messages are received
        ✓ Topic equals to xk6_kafka_json_topic
        ✓ Key is correct
        ✓ Value is correct
        ✓ Header equals {'mykey': 'myvalue'}
        ✓ Time is past
        ✓ Partition is zero
        ✓ Offset is gte zero
        ✓ High watermark is gte zero

        █ teardown

        checks.........................: 100.00% ✓ 124002       ✗ 0
        data_received..................: 0 B     0 B/s
        data_sent......................: 0 B     0 B/s
        iteration_duration.............: avg=217.98ms min=26.64ms med=216.88ms max=357.64ms p(90)=244.95ms p(95)=254.86ms
        iterations.....................: 13778   229.051752/s
        kafka.reader.dial.count........: 50      0.831223/s
        kafka.reader.dial.seconds......: avg=4.76µs   min=0s      med=0s       max=2.22ms   p(90)=0s       p(95)=0s
     ✓ kafka.reader.error.count.......: 0       0/s
        kafka.reader.fetch_bytes.max...: 1000000 min=1000000    max=1000000
        kafka.reader.fetch_bytes.min...: 1       min=1          max=1
        kafka.reader.fetch_wait.max....: 200ms   min=200ms      max=200ms
        kafka.reader.fetch.bytes.......: 0 B     0 B/s
        kafka.reader.fetch.size........: 0       0/s
        kafka.reader.fetches.count.....: 50      0.831223/s
        kafka.reader.lag...............: 7457    min=5736       max=14370
        kafka.reader.message.bytes.....: 27 MB   450 kB/s
        kafka.reader.message.count.....: 137830  2291.348744/s
        kafka.reader.offset............: 2740    min=11         max=2810
        kafka.reader.queue.capacity....: 1       min=1          max=1
        kafka.reader.queue.length......: 1       min=0          max=1
        kafka.reader.read.seconds......: avg=0s       min=0s      med=0s       max=0s       p(90)=0s       p(95)=0s
        kafka.reader.rebalance.count...: 0       0/s
        kafka.reader.timeouts.count....: 0       0/s
        kafka.reader.wait.seconds......: avg=7.44µs   min=0s      med=0s       max=3.17ms   p(90)=0s       p(95)=0s
        kafka.writer.acks.required.....: -1      min=-1         max=0
        kafka.writer.async.............: 0.00%   ✓ 0            ✗ 1377800
        kafka.writer.attempts.max......: 0       min=0          max=0
        kafka.writer.batch.bytes.......: 302 MB  5.0 MB/s
        kafka.writer.batch.max.........: 1       min=1          max=1
        kafka.writer.batch.size........: 1377800 22905.17521/s
        kafka.writer.batch.timeout.....: 0s      min=0s         max=0s
     ✓ kafka.writer.error.count.......: 0       0/s
        kafka.writer.message.bytes.....: 603 MB  10 MB/s
        kafka.writer.message.count.....: 2755600 45810.350421/s
        kafka.writer.read.timeout......: 0s      min=0s         max=0s
        kafka.writer.retries.count.....: 0       0/s
        kafka.writer.wait.seconds......: avg=0s       min=0s      med=0s       max=0s       p(90)=0s       p(95)=0s
        kafka.writer.write.count.......: 2755600 45810.350421/s
        kafka.writer.write.seconds.....: avg=1.02ms   min=79.29µs med=893.09µs max=24.26ms  p(90)=1.22ms   p(95)=1.74ms
        kafka.writer.write.timeout.....: 0s      min=0s         max=0s
        vus............................: 50      min=50         max=50
        vus_max........................: 50      min=50         max=50
    ```

### Troubleshooting

To avoid getting the following error while running the test:

```bash
Failed to write message: [5] Leader Not Available: the cluster is in the middle of a leadership election and there is currently no leader for this partition and hence it is unavailable for writes
```

You can now use `createTopic` method to create the topic in Kafka. The `scripts/test_topics.js` script shows how to list topics on all Kafka partitions, create a topic or delete one. You always have the option to create it using `kafka-topics` command:

```bash
$ docker exec -it lensesio bash
(inside container)$ kafka-topics --create --topic xk6_kafka_avro_topic --bootstrap-server localhost:9092
(inside container)$ kafka-topics --create --topic xk6_kafka_json_topic --bootstrap-server localhost:9092
```

> **Note:**
> If you want to test SASL authentication, look at [this commit message](https://github.com/mostafa/xk6-kafka/pull/3/commits/403fbc48d13683d836b8033eeeefa48bf2f25c6e), where I describe how to run a test environment.

## Contributions, Issues and Feedback

I'd be thrilled to receive contributions and feedback on this project. You're always welcome to create an issue if you find one (or many). I would do my best to address the issues. Also, feel free to contribute by opening a PR with changes, and I'll do my best to review and merge it as soon as I can.

## Backward Compatibility Notice

If you want to keep up to date with the latest changes, please follow the [project board](https://github.com/users/mostafa/projects/1). Also, since [v0.9.0](https://github.com/mostafa/xk6-kafka/releases/tag/v0.9.0), the `main` branch is the *development* branch and usually has the latest changes and might be unstable. If you want to use the latest features, you might need to build your binary by following the [build from source](https://github.com/mostafa/xk6-kafka/blob/main/README.md#build-from-source) instructions. In turn, the tagged releases and the Docker images are more stable.

I make no guarantee to keep the API stable, as this project is in *active development* unless I release a major version. The best way to keep up with the changes is to follow [the xk6-kafka API](https://github.com/mostafa/xk6-kafka/blob/main/README.md#the-xk6-kafka-api) and look at the [scripts](https://github.com/mostafa/xk6-kafka/blob/main/scripts/) directory.

## The Release Process

The `main` branch is the *development* branch, and the pull requests will be *squashed and merged* into the `main` branch. When a commit is tagged with a version, for example, `v0.10.0`, the build pipeline will build the `main` branch on that commit. The build process creates the binaries and the Docker image. If you want to test the latest unreleased features, you can clone the `main` branch and instruct the `xk6` to use the locally cloned repository instead of using the `@latest`, which refers to the latest tagged version, as explained in the [build for development](https://github.com/mostafa/xk6-kafka/blob/main/README.md#build-for-development) section.

## CycloneDX SBOM

Since [v0.9.0](https://github.com/mostafa/xk6-kafka/releases/tag/v0.9.0), CycloneDX SBOMs are generated for [go.mod](go.mod) and it can be accessed from the latest build of GitHub Actions for [a tagged release](https://github.com/mostafa/xk6-kafka/actions/runs/2275475853) or on the release [assets](https://github.com/mostafa/xk6-kafka/releases/tag/v0.11.0). The artifacts are only kept for 90 days, so I'll also add the SBOM to the release assets.

## Disclaimer

This project *was* a proof of concept but seems to be used by some companies nowadays. However, it isn't supported by the k6 team, but rather by [me](https://github.com/mostafa) personally, and the APIs may change in the future. USE AT YOUR OWN RISK!

This project was AGPL3-licensed up until 7 October 2021, and then we [relicensed](https://github.com/mostafa/xk6-kafka/pull/25) it under the [Apache License 2.0](https://github.com/mostafa/xk6-kafka/blob/master/LICENSE).
