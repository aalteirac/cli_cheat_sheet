
# Snowflake CLI Cheat Sheet

## Credential Management Cheat Sheet

### Adding Credentials

1. **Using Snowflake CLI Connection Command**:
   ```shell
   snow connection add
   ```
   - Enter the required details when prompted (e.g., connection name, account name, username, etc.).
   - Optional parameters: role, warehouse, database, schema, host, port, region.

2. **Using Configuration File**:
   - Edit the `config.toml` file, or create one if it doesn't exist.
   - Add connection definitions:
     ```toml
     [connections.myconnection]
     account = "myaccount"
     user = "johndoe"
     password = "hunter2"
     warehouse = "my-wh"
     database = "my_db"
     schema = "my_schema"
     ```
   - Ensure file permissions are set correctly on MacOS/Linux:
     ```shell
     chown $USER config.toml
     chmod 0600 config.toml
     ```

3. **Using Environment Variables**:
   - Define environment variables:
     ```shell
     export SNOWFLAKE_ACCOUNT="myaccount"
     export SNOWFLAKE_USER="johndoe"
     export SNOWFLAKE_PASSWORD="hunter2"
     ```
   - For specific connection parameters:
     ```shell
     export SNOWFLAKE_CONNECTIONS_MYCONNECTION_ACCOUNT="myaccount"
     ```

### Changing the Default Connection

- **In `config.toml`**:
  ```toml
  default_connection_name = "my_prod_connection"
  ```
- **Using Environment Variable**:
  ```shell
  export SNOWFLAKE_DEFAULT_CONNECTION_NAME="my_prod_connection"
  ```
- **Using CLI Command**:
  ```shell
  snow connection set-default "my_test_connection"
  ```

### Using a Private Key for Authentication

- **Configuration**:
  ```toml
  [connections.jwt]
  account = "my_account"
  user = "johndoe"
  authenticator = "SNOWFLAKE_JWT"
  private_key_path = "~/sf_private_key.p8"
  ```

### Using Multi-Factor Authentication (MFA)

- **Configuration**:
  ```toml
  authenticator = "snowflake"
  ```
- **Enable MFA Caching**:
  ```toml
  authenticator = "username_password_mfa"
  ```

### Using Single Sign-On (SSO)

- **Refer to SSO Configuration Documentation**.

### Testing and Temporary Connections

1. **Test a Connection**:
   ```shell
   snow connection test --connection="myconnection"
   ```

2. **Using a Temporary Connection**:
   ```shell
   snow sql -q "select 42;" --temporary-connection \
                             --account myaccount \
                             --user jdoe
   ```
   - Temporary connection options:
     ```shell
     --account myaccount
     --user jdoe
     --password "mypassword"
     --authenticator "auth_method"
     --private-key-path "/path/to/private_key"
     --database "dbname"
     --schema "schemaname"
     --role "rolename"
     --warehouse "warehouse"
     --temporary-connection [-x]
     ```

### Using a Different Configuration File

- **Specify Configuration File**:
  ```shell
  snow --config-file="my_config.toml" connection test
  ```

### Important Notes

- **MFA Requirement**: From Snowflake version 8.24, MFA can be mandatory.
- **File Permissions**: Ensure correct permissions for `config.toml` and `connections.toml` on MacOS/Linux.


# CLI for Snowpark Container Services 

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
