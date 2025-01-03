# LangGraph Checkpoint CosmosDB
Implementation of LangGraph Async CheckpointSaver for Azure Cosmos DB.

## Setup Azure CosmosDB
Create an azure cosmos db in your azure portal with two containers: one for checkpointer, the other for checkpoint writes.
Then you should be able to organize the config to be like below
```
URL="my-azure-cosmosdb-url",
KEY="my-key",
DATABASE="my-database-name",
CHECKPOINTS_CONTAINER="checkpoints",
CHECKPOINT_WRITES_CONTAINER="checkpoint_writes",
```
## How to Run
```python
import asyncio

from async_checkpoint_saver import (
    AsyncCosmosDBCheckpointSaver,
    AsyncCosmosDBCheckpointSaverConfig,
)

async def main():
    write_config = {"configurable": {"thread_id": "1", "checkpoint_ns": ""}}
    read_config = {"configurable": {"thread_id": "1"}}
    
    checkpoint_store_config = AsyncCosmosDBCheckpointSaverConfig(
        URL="my-azure-cosmosdb-url",
        KEY="my-key",
        DATABASE="my-database-name",
        CHECKPOINTS_CONTAINER="checkpoints",
        CHECKPOINT_WRITES_CONTAINER="checkpoint_writes",
    )
    checkpointer = AsyncCosmosDBCheckpointSaver(checkpoint_store_config)
    checkpoint = {
        "v": 1,
        "ts": "2024-07-31T20:14:19.804150+00:00",
        "id": "1ef4f797-8335-6428-8001-8a1503f9b875",
        "channel_values": {"my_key": "meow", "node": "node"},
        "channel_versions": {"__start__": 2, "my_key": 3, "start:node": 3, "node": 3},
        "versions_seen": {
            "__input__": {},
            "__start__": {"__start__": 1},
            "node": {"start:node": 2},
        },
        "pending_sends": [],
    }
    # store checkpoint
    await checkpointer.aput(write_config, checkpoint, {}, {})

    # load checkpoint
    await checkpointer.aget(read_config)

    # list checkpoints
    [c async for c in checkpointer.alist(read_config)]


if __name__ == "__main__":
    asyncio.run(main())

```
