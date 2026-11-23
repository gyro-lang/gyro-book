# @distributed

The `@distributed` decorator enables a function to be executed across a remote cluster of worker nodes. It automatically differentiates between *master* and *worker* execution contexts using the `self.master()` and `self.slave()` methods.

When called from the master node, the decorated function can delegate parts of its workload to other nodes via `self.dispatch()`. Each `dispatch` call returns a promise, allowing asynchronous orchestration and result aggregation. When run on a worker, the function executes only the logic in the `self.slave()` branch, processing the task payload received from the master.

Task scheduling is coordinated through an internal queue. Workers signal their availability after completing a task, allowing the master to dispatch new jobs without waiting for all workers to finish.

```gyro
import { read } from "fs

@distributed(min_targets = 1)
fn process_image(image_path: string, filter: string): Image {
    if self.master() {
        let image = await read(image_path)
        let chunks = split_into_chunks(image, count=self.workers_count())

        // Dispatch each chunk to a worker
        let promises = chunks.map(chunk => self.dispatch({ chunk, filter }))

        // Wait for all results
        let processed_chunks = await all(promises)
        return merge_chunks(processed_chunks)
    }

    if self.slave() {
        let { chunk, filter } = self.data()
        return apply_filter(chunk, filter)
    }
}

var result = await process_image<targets=[
    127.0.0.1:4000,
    127.0.0.1:4001,
    127.0.0.1:4002
], master=127.0.0.1:4003>(
    "photo.jpg", "sharpen"
)

```

### Options

| Option            | Type       | Default               | Description                                                                                               |
| ----------------- | ---------- | --------------------- | --------------------------------------------------------------------------------------------------------- |
| `targets`         | `ipaddress[]` | `[]` (empty list)     | List of worker node addresses (IP\:port) where tasks can be dispatched.                                   |
| `master`          | `ipaddress`   | First target or local | Address of the master node coordinating the distributed execution.                                        |
| `min_targets`     | `int`      | `0`                   | Minimum number of worker nodes required to run the distributed function; otherwise fails or runs locally. |
| `timeout`         | `int` (ms) | `30000` (30 sec)      | Maximum wait time for worker responses before failing or retrying.                                        |
| `retry`           | `int`      | `3`                   | Number of retries for dispatching a task to a worker on failure.                                          |
| `load_balancer`   | `function` | Round-robin           | Custom function to select which worker to assign next task, defaults to round-robin scheduling.           |
| `max_concurrency` | `int`      | Number of targets     | Maximum number of concurrent tasks allowed per master call.                                               |
| `cache`           | `bool`     | `false`               | Whether to cache results of dispatched tasks to avoid recomputation.                                      |
| `on_worker_ready` | `function` | Internal              | Hook triggered when a worker signals availability for a new task.                                         |