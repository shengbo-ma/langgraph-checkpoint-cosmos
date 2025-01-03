# LangGraph Checkpoint CosmosDB
Implementation of LangGraph Async CheckpointSaver for Azure Cosmos DB.

```python
from .async_checkpoint_saver import AsyncCosmosDBCheckpointSaver

checkpoint_store_config = AsyncCosmosDBCheckpointSaverConfig(
    URL=...,
    KEY=...,
    DATABASE=...,
    CHECKPOINTS_CONTAINER=...,
    CHECKPOINT_WRITES_CONTAINER=...,
)
checkpointer = AsyncCosmosDBCheckpointSaver(checkpoint_store_config)
checkpoint = {
    "v": 1,
    "ts": "2024-07-31T20:14:19.804150+00:00",
    "id": "1ef4f797-8335-6428-8001-8a1503f9b875",
    "channel_values": {
        "my_key": "meow",
        "node": "node"
    },
    "channel_versions": {
        "__start__": 2,
        "my_key": 3,
        "start:node": 3,
        "node": 3
    },
    "versions_seen": {
        "__input__": {},
        "__start__": {
        "__start__": 1
        },
        "node": {
        "start:node": 2
        }
    },
    "pending_sends": [],
}

# store checkpoint
await checkpointer.aput(write_config, checkpoint, {}, {})

# load checkpoint
await checkpointer.aget(read_config)

# list checkpoints
[c async for c in checkpointer.alist(read_config)]
```
