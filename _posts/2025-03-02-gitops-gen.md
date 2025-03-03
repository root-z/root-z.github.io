---
layout: post
title: "On System Feedback"
date: 2025-03-02 11:48:00 -0800
categories: infrastructure
excerpt_separator: <!--ex-->
---

It's common to use run GitOps pipeline, where you manage infrastructure based on code that's committed into a git repository. We use terraform as an example for infrastructure as code here. It is also not uncommon to generate some of that terraform code from a template to aid initial application setup.

![GitOps](/assets/gitops.png "Code Generation For GitOps")

This works fine oftentimes. However, code generation can get abused quite easily. For example, maybe you have some complex resource setups that requires editting multiple terraform repositories. Now you might think it's easy to extend the code generation to cover that.

![GitOps](/assets/gitops2.png "Generating Code For Multiple Repositories")

However, this gets complicated very quickly. The main issue with code generation into a git repo is that code generation isn't a clean abstraction and the feedback is usually missing. The user triggers the code generation, the code gets committed, but does the infrastructure deployment actually succeeds or does fail somewhere? When there is only one pipeline and one piece of infrastructure. The user maybe knows where to check. However, when there are multiple pipelines involved, it becomes too much for the user to track. They might not even have the right permission to look into all those infrastructure deployment pipelines.

The same issue of not having enough feedback in the system interface is often found in a lot of the asynchronous workflows. Perhaps you let the user trigger an airflow/prefect DAG without ever providing response about whether it succeeded or not, or perhaps you have some kind of weak feedback in your system where you simply throw the result of a workflow into Slack or Datadog. Why do I consider these weak feedbacks? Slack messages, metrics, and even alerts are hard to track and hard to automate on. If one builds an API that operates and async workflow, they should always offer some kind of programmatic response. A basic form of this could be just another API that the users can use to poll and check the results. Alternatively, the API can accept some kind of callback or webhooks (and probably have a timeout on its operations). These strong feedback mechanism provides predictable behavior and makes automation much easier. 