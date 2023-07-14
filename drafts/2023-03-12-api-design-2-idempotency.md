---
layout: post
title: "Web Service Part 2: Idempotency"
date: 2023-03-12 22:03:00 -0700
categories: design
excerpt_separator: <!--ex-->
---

# Idempotency, what is it?

An idempotent operation is one that can be run repeatedly and produce the same result. A good example is the hash map *put* method. You can call *put* twice with the same value.
```python
a_map = {}
a_map[1] = 1 # {1: 1}
a_map[1] = 1 # {1: 1}
```

Idempotency is important for a web service because in many distributed setups it's impossible to achieve exactly-once delivery. A basic client-server example is that, when a request from the client succeeded, the client may never recieve the successful resposne from the server due to network issues. A sensible client may decide to retry in that case, calling the server for a second time.

Granted idempotency is not needed in every scenario. Consider an email service with SendEmail API, calling SendEmail twice may send the same email twice. It's not great, but it's not the end of the world if that happens once in a while. On the other hand, if the service involves financial transactions that need be tracked accurately, adding the same transaction twice would mess up a lot of things.

# Ok I want idempotency
Ah, ambitious are we not.

How would you implement idempotency?
