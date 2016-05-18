
Orbit Async REST Client
============

[![Release](https://img.shields.io/github/release/orbit/orbit-rest-client.svg)](https://github.com/orbit/orbit-rest-client/releases)
[![Maven Central](https://img.shields.io/maven-central/v/cloud.orbit/orbit-rest-client.svg)](https://repo1.maven.org/maven2/cloud/orbit/orbit-rest-client/)
[![Javadocs](https://img.shields.io/maven-central/v/cloud.orbit/orbit-rest-client.svg?label=javadocs)](http://www.javadoc.io/doc/cloud.orbit/orbit-rest-client)
[![Build Status](https://img.shields.io/travis/orbit/orbit-rest-client.svg)](https://travis-ci.org/orbit/orbit-rest-client)
[![Gitter](https://img.shields.io/badge/style-Join_Chat-ff69b4.svg?style=flat&label=gitter)](https://gitter.im/orbit/orbit)

Allows calling JAX-RS REST interfaces with methods returning CompletableFuture or Orbit Task.
This enables writing REST client code in a fluent asynchronous way.    

The project orbit-rest-client depends only on the [JAX-RS standard](https://jax-rs-spec.java.net/) and it is in principle 
compatible with any client library that provides [javax.ws.rs.client.WebTarget](http://docs.oracle.com/javaee/7/api/javax/ws/rs/client/package-summary.html).
It has been tested with
[jersey-client](https://jersey.java.net/documentation/latest/modules-and-dependencies.html#client-jdk).

Developer & License
======
This project was developed by [Electronic Arts](http://www.ea.com) and is licensed under the [BSD 3-Clause License](LICENSE).

Examples
========
Using Orbit Tasks
-----

```java
public interface Hello
{
    @GET
    @Path("/")
    Task<String> getHome();
}

public static void main(String args[])
{
    WebTarget webTarget = getWebTarget("http://example.com");

    Hello hello = new RestClient(webTarget).get(Hello.class);

    Task<String> response = hello.getHome();

    response.thenAccept(x -> System.out.println(x)); 
    response.join();
}

// use any jax-rs client library to get javax.ws.rs.client.WebTarget
private static WebTarget getWebTarget(String host)
{
    ClientConfig clientConfig = new ClientConfig();
    Client client = ClientBuilder.newClient(clientConfig);
    return client.target(host);
}
```

Using CompletableFuture
-----

```java
public interface Hello
{
    @GET
    @Path("/")
    CompletableFuture<String> getHome();
}

public static void main(String args[])
{
    WebTarget webTarget = getWebTarget("http://example.com");

    Hello hello = new RestClient(webTarget).get(Hello.class);

    CompletableFuture<String> response = hello.getHome();
    
    response.thenAccept(x -> System.out.println(x)); 
    response.join();
}

// use any jax-rs client library to get javax.ws.rs.client.WebTarget
private static WebTarget getWebTarget(String host)
{
    ClientConfig clientConfig = new ClientConfig();
    Client client = ClientBuilder.newClient(clientConfig);
    return client.target(host);
}
```
