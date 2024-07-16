
# Snowpark Container Services Cheat Sheet

## Image Registries

### Manage Image Registries

#### Get Environment Tokens for Registry Authentication

```
snow spcs image-registry token --connection mytest
```

- **Use token to log in to Docker:**
```
snow spcs image-registry token --format=JSON | docker login <org>-<account>.registry.snowflakecomputing.com -u 0sessiontoken --password-stdin
```

#### Log in to an Image Registry

```
snow spcs image-registry login
```

#### Retrieve the URL for an Image Registry

```
snow spcs image-registry url
```

## Image Repositories

### Manage Image Repositories

#### Create an Image Repository

```
snow spcs image-repository create tutorial_repository
```

#### Retrieve the URL for an Image Repository

```
snow spcs image-repository url tutorial_repository
```

#### List Tags and Images in an Image Repository

- **List Images:**
```
snow spcs image-repository list-images images --database my_db
```

- **List Tags:**
```
snow spcs image-repository list-tags images --image_name "MY_DB/PUBLIC/images/cp-schema-registry" --database my_db
```

## Compute Pools

### Manage Compute Pools

#### Create a Compute Pool

```
snow spcs compute-pool create "pool_1" --min-nodes 2 --max-nodes 2 --family "CPU_X64_XS"
```

#### Suspend a Compute Pool

```
snow spcs compute-pool suspend tutorial_compute_pool
```

#### Resume a Compute Pool

```
snow spcs compute-pool resume tutorial_compute_pool
```

#### Set and Unset Compute Pool Properties or Parameters

- **Set:**
```
snow spcs compute-pool set tutorial_compute_pool --min-nodes 2 --max-nodes 4
```

- **Unset:**
```
snow spcs compute-pool unset tutorial_compute_pool --auto-resume
```

#### Stop All Services in a Compute Pool

```
snow spcs compute-pool stop-all "pool_1"
```

## Services

### Manage Services

#### Create a Snowpark Container Services Service

```
snow spcs service create "job_1" --compute-pool "pool_1" --spec-path "/some-dir/spec_file.yaml"
```

#### Suspend a Service

```
snow spcs service suspend echo_service
```

#### Resume a Service

```
snow spcs service resume echo_service
```

#### Get the Status of a Service

```
snow spcs service status "service_1"
```

#### List the Endpoints in a Service

```
snow spcs service list-endpoints echo_service
```

#### Set and Unset Service Properties or Parameters

- **Set:**
```
snow spcs service set echo_service --min-instances 2 --max-instances 4
```

- **Unset:**
```
snow spcs compute-pool unset tutorial_compute_pool --auto-resume
```

#### Display Logs for a Named Service

```
snow spcs service logs "service_1" --container-name "container_1" --instance-id "0"
```

#### Upgrade a Named Service

```
snow spcs service upgrade echo_service --spec-path spec.yml
```
