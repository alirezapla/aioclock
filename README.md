# aioclock

[![Release](https://img.shields.io/github/v/release/ManiMozaffar/aioclock)](https://img.shields.io/github/v/release/ManiMozaffar/aioclock)
[![Build status](https://img.shields.io/github/actions/workflow/status/ManiMozaffar/aioclock/main.yml?branch=main)](https://github.com/ManiMozaffar/aioclock/actions/workflows/main.yml?query=branch%3Amain)
[![Commit activity](https://img.shields.io/github/commit-activity/m/ManiMozaffar/aioclock)](https://img.shields.io/github/commit-activity/m/ManiMozaffar/aioclock)
[![License](https://img.shields.io/github/license/ManiMozaffar/aioclock)](https://img.shields.io/github/license/ManiMozaffar/aioclock)

An asyncio-based scheduling framework designed for execution of periodic task with integrated support for dependency injection, enabling efficient and flexiable task management

- **Github repository**: <https://github.com/ManiMozaffar/aioclock/>

## Features

Aioclock offers:

- Async: 100% Async, very light, fast and resource friendly
- Scheduling: Keep scheduling tasks for yoo
- Group: Group your task, for better code maintainability
- Trigger: Already defined and easily extendable triggers, to trigger your scheduler to be started
- Easy syntax: Library's syntax is very easy and enjoyable, no confusing hierarchy
- Pydantic v2 validation: Validate all your trigger on startup using pydantic 2. Fastest to fail possible!
- **Soon**: Processor handlers, with pub/sub logic.

## Getting started

To Install aioclock, simply do

```
pip install aioclock
```

AioClock is very user friendly and easy to use, it's type stated library to use easily.
AioClock always have a trigger, that trigger the events.

```python
import asyncio

from aioclock import AioClock, At, Depends, Every, Forever, Once, OnShutDown, OnStartUp
from aioclock.group import Group

# groups.py
group = Group()


def more_useless_than_me():
    return "I'm a dependency. I'm more useless than a screen door on a submarine."


@group.task(trigger=Every(seconds=10))
async def every():
    print("Every 10 seconds, I make a quantum leap. Where will I land next?")


@group.task(trigger=At(tz="UTC", hour=0, minute=0, second=0))
async def at():
    print(
        "When the clock strikes midnight... I turn into a pumpkin. Just kidding, I run this task!"
    )


@group.task(trigger=Forever())
async def forever(val: str = Depends(more_useless_than_me)):
    await asyncio.sleep(2)
    print("Heartbeat detected. Still not a zombie. Will check again in a bit.")
    assert val == "I'm a dependency. I'm more useless than a screen door on a submarine."


@group.task(trigger=Once())
async def once():
    print("Just once, I get to say something. Here it goes... I love lamp.")


# app.py
app = AioClock()
app.include_group(group)


@app.task(trigger=OnStartUp())
async def startup():
    print(
        "Welcome to the Async Chronicles! Did you know a group of unicorns is called a blessing? Well, now you do!"
    )


@app.task(trigger=OnShutDown())
async def shutdown():
    print("Going offline. Remember, if your code is running, you better go catch it!")


# main.py
if __name__ == "__main__":
    asyncio.run(app.serve())
```

## TODOs

Ideally, producer and consumer should be on seperate process.
Because a function having CPU bound task, doesn't mean the task should be produced with delays.
So AioClock is aiming to give the user ability to easily setup how many process they want to use, and by default use 2 process, where one is consumer and one is producer. Producer doesn't need to be more than 1 process, unless the trigger is CPU intensive.

## Contribution

Feel free to contribute, we welcome new ideas, bug fixes or anything!
