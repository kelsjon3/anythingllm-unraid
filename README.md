# AnythingLLM Unraid Template (PostgreSQL Version)

This repository contains an Unraid Docker template for deploying [AnythingLLM](https://anythingllm.com/) with a PostgreSQL backend.

This template is designed for users who want to connect AnythingLLM to an existing PostgreSQL Docker container running on the same Unraid machine.

## Prerequisites

1.  An Unraid server.
2.  A running PostgreSQL Docker container on your Unraid server.
3.  The `pgvector` extension must be installed and enabled in your PostgreSQL database.

## Installation

1.  In your Unraid web UI, navigate to the **Docker** tab.
2.  Click on **Template Repositories**.
3.  Copy and paste the URL of this GitHub repository into one of the empty fields: `https://github.com/kelsjon3/anythingllm-unraid`
4.  Click **Save**.
5.  Go back to the **Docker** tab and click **Add Container**.
6.  You should now see `AnythingLLM-PG` in the template dropdown list.

## Configuration

When you add the container, you will see several fields to configure the database connection.

### Postgres Settings

*   **Postgres Host**: The hostname or IP address of your PostgreSQL container. If it's a Docker container on the same Unraid server, you can usually use the container's name (e.g., `postgres`).
*   **Postgres Port**: The port PostgreSQL is listening on (usually `5432`).
*   **Postgres User**: Your PostgreSQL username.
*   **Postgres Password**: The user's password.
*   **Postgres Database**: The name of the database AnythingLLM will use.

### `DATABASE_URL`

After filling in the fields above, you must manually construct the `DATABASE_URL` and paste it into this field. This is the final connection string that AnythingLLM will use.

The required format is:

`postgresql://<user>:<password>@<hostname>:<port>/<database>`

**Example:**

If you used the default values from the template, your `DATABASE_URL` would be:

`postgresql://llm_user:password@postgres:5432/anythingllm`

### `.env` File

This template mounts a `.env` file from your appdata directory (`/mnt/user/appdata/anythingllm/.env`) into the container. While you can set the `DATABASE_URL` directly in the Unraid template UI, you can use this `.env` file to configure other AnythingLLM settings.

Create the file at `/mnt/user/appdata/anythingllm/.env` on your Unraid server if you need to add other variables. Refer to the official [AnythingLLM `.env.example`](https://github.com/Mintplex-Labs/anything-llm/blob/master/.env.example) for a full list of available options.

### PGVector Requirement

AnythingLLM uses the `pgvector` extension for PostgreSQL to handle vector embeddings. You **must** enable this extension in your database. You can typically do this by connecting to your database and running:

```sql
CREATE EXTENSION IF NOT EXISTS vector;
```

If you are using the official `pgvector/pgvector` Docker image, this extension is usually available to be enabled. 