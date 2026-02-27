# Introduction</p>

The deployment medium is a very important part of the lifecycle for the application. GCP was selected as the target platform, </p> This is due to its features and attributed. A specific are of focus was the rich AI/ML echosystem.

### App-Engine:</p>

​Google App Engine (GAE) is a Platform-as-a-Service (PaaS), meaning Google manages the underlying operating system for you. You don't "host" App Engine on Ubuntu; rather, App Engine provides the environment where your code runs. </p>

### The "Custom Runtime" Approach (App Engine Flexible)<p>
​If you need specific Ubuntu packages, libraries, or a particular version of Ubuntu to run your app, you can use App Engine Flexible with a Custom Runtime.  
​How it works: You provide a Dockerfile that starts with an Ubuntu base image (e.g., FROM ubuntu:24.04).
​Customization: You can apt-get install any dependencies you need.
​The Catch: While the container is Ubuntu, you still don't have control over the host kernel or the "machine" itself. You are limited to what can run inside a Docker container.


### Rust Axum example</p>

```
// Example using Axum
let addr = std::net::SocketAddr::from(([0, 0, 0, 0], 8080));
println!("Listening on {}", addr);
axum::Server::bind(&addr)
    .serve(app.into_make_service())
    .await
    .unwrap();


```