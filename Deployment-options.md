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

### Alternative VM customization: </p>

```
runtime: custom
env: flex

# Customizing the VM resources
resources:
  cpu: 1
  memory_gb: 1.0  # Rust has a tiny memory footprint
  disk_size_gb: 10

# Rust apps are fast, but App Engine needs to know it's healthy
liveness_check:
  path: "/health"   # Create a /health route in your app
  check_interval_sec: 30
  timeout_sec: 4
  failure_threshold: 2
  success_threshold: 2

# Scale based on CPU usage
automatic_scaling:
  min_num_instances: 1
  max_num_instances: 5
  cpu_utilization:
    target_utilization: 0.6


```

### Docker file customization example:</p>


```

# --- STAGE 1: The Builder ---
FROM ubuntu:24.04 AS builder

# Prevent prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Install system dependencies for building Rust
RUN apt-get update && apt-get install -y \
    curl \
    build-essential \
    pkg-config \
    libssl-dev \
    && rm -rf /var/lib/apt/lists/*

# Install Rust (using the official rustup script)
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"

# Set the working directory
WORKDIR /usr/src/app
COPY . .

# Build your application for release
# Ensure your app is named 'my_rust_app' or update the name below
RUN cargo build --release

# --- STAGE 2: The Final Runtime ---
FROM ubuntu:24.04

# Install only the runtime libraries your app needs (e.g., SSL for web requests)
RUN apt-get update && apt-get install -y \
    ca-certificates \
    libssl3 \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy only the compiled binary from the builder stage
# Replace 'my_rust_app' with the 'name' field from your Cargo.toml
COPY --from=builder /usr/src/app/target/release/my_rust_app /app/my_rust_app

# App Engine Flexible expects traffic on port 8080
EXPOSE 8080

# Run the binary
CMD ["./my_rust_app"]


```