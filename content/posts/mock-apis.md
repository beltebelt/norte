---
author: ["Shubhendu Madhukar"]
title: "How To Mock APIs In Seconds"
date: "2025-02-19"
description: "HTTP Mocks or Stubs, are replacements for your actual APIs, which can be used for testing purposes. "
summary: "If you are a part of a testing community in a microservices world, you would often come across challenges in terms of environment availability, resource constraints, clash in release cycles and what not. In these scenarios, you can use a mock API to act as a stand in for application B and provide application A with dummy responses so that you can continue your tests without any dependency on the downstream."
tags: ["javascript", "tooling", "prototyping", "mocks"]
categories: ["tooling", "mocks"]
series: ["Tools"]
ShowToc: true
TocOpen: true
---

If you are a part of a testing community in a microservices world, you would often come across challenges in terms of environment availability, resource constraints, clash in release cycles and what not.

Consider an example where you are testing the application A. During one of your test flows, application A makes a downstream call to application B. And guess what? For one or more of the many reasons, application B is unavailable. You end up waiting for application B even though, it is just a dependency that you are not even testing.

In these scenarios, you can use a mock API to act as a stand in for application B and provide application A with dummy responses so that you can continue your tests without any dependency on the downstream.

### What are HTTP mocks?

HTTP Mocks or Stubs, are replacements for your actual APIs, which can be used for testing purposes. Mocks are great for several reasons:

1. You are building a frontend application but your backend isn't ready yet. Use mocks to quickly create an API which provides you a dummy response and test your frontend application without actually hitting a backend. Once your backend is ready just replace the mock server host with the actual server host in your configs and everything else remains the same.
2. You can have similar use cases while running unit tests, functional tests or even performance tests, where as long as the mock API can simulate the latency and provide a response similar to actual response, you don't need your complete backend and downstream to be ready for running tests in silos.
3. Mocks are also great for debugging purposes, when you are testing multiple microservices together. Even with advanced monitoring tools, sometimes it's hard to pinpoint the exact cause of the issue. With mocks you can plug and play and debug which component is causing problems

### How do you create mocks?

While there are a lot many tools available in open source world that allow you to create mocks, in this article I'll discuss a new tool I have been working on. Camouflage.

Camouflage works just as the name suggests. It allows you to create and use dummy APIs. And your front end or dependant applications wouldn't be able to tell the difference if the response is coming from a mock or an actual API.

Though Camouflage isn't an original idea (mockserver already implemented something similar), it has a lot of cool features and enhancements over existing tools that help you get up and running within seconds. Few of the prominent features are:

1. Camouflage has a near minimum learning curve. Create a directory mocks/hello-world. Place a file named GET.mock containing your raw HTTP Response. And you are done. Make a GET request to /hello-world and you get your expected response.
2. Camouflage heavily uses handlebars, which enables you to add character to your response. Insert dynamic random values that change on each invocation, fetch data from incoming request and send out a conditional response, simulate delays and lot more.
3. Camouflage comes in two modes, functional and performance. By default Camouflage runs in functional mode, which is sufficient for unit tests, frontend testing, even a small scale performance test. However if your machine has multiple CPUs and you are planning to run a performance test, why not make use of the full potential of your machine. You can use performance mode which lets Camouflage utilise multiple CPUs using node's cluster module.

### Enough talk. How do we create a mock?

Well, you follow five simple steps:

1. Install Camouflage: `npm install -g camouflage-server`
2. Create a mocks directory which will contain all your mocks. e.g. `~/mocks`
3. Start Camouflage: `camouflage -m ~/mocks`
4. Create another directory in the format your APIs basepath would be. For example: For an API `http://localhost:8080/hello/world`, create directories as `~/mocks/hello/world`
5. Create a file ${HTTP_METHOD}.mock and insert your HTTP raw response. e.g `vi ~/mocks/hello/world/GET.mock` and paste following content. (If you are on windows, simply use notepad.)

```javascript
HTTP/1.1 200 OK
X-Custom-Header: Custom-Value
Content-Type: application/json
{
     "greeting": "Hey! It works!"
}
```

And you are done, navigate to http://localhost:8080/hello/world, to see your mock in action.

### Conclusion

There are already a lot of mocking tools already available like Wiremock, mountebank etc. And those are really great tools, but in my experience, it took me sometime to familiarise myself with the tool, their JSON schema and other available options. The problem statement Camouflage tries to tackle is simple, how to shorten the learning curve and get started with mocks creation within seconds.

**Are you working on prototyping your app? Do you want to starting testing it and you are worried that not all your services are ready? [Talk to us](https://cal.com/utsuk-labs) about how you can start early testing without waiting for everything to be ready.**
