# playground-of-everasdasdasdsda

## Vector Configuration

This repository contains a Vector configuration for log processing, migrated from Logstash.

### Overview

[Vector](https://vector.dev/) is a high-performance, end-to-end observability data pipeline. It's a modern replacement for Logstash that offers:
- Better performance and lower resource usage
- Native Rust implementation
- Simple TOML-based configuration
- Built-in transforms using VRL (Vector Remap Language)

### Configuration Files

- `vector.toml` - Main Vector configuration file
- `kafka_tls_cert.pem` - TLS certificate for Kafka connection
- `keyfile.key` - Private key for TLS authentication

### Pipeline Architecture

```
Kafka (source) → Parse JSON → Add Metadata → Filter Logs → Console/Elasticsearch (sink)
```

### Usage

1. Install Vector:
   ```bash
   curl --proto '=https' --tlsv1.2 -sSfL https://sh.vector.dev | bash
   ```

2. Validate the configuration:
   ```bash
   vector validate vector.toml
   ```

3. Run Vector:
   ```bash
   vector --config vector.toml
   ```

### Configuration Details

#### Source
- **Kafka**: Consumes messages from Kafka with TLS enabled
  - Bootstrap servers: `kafka.example.com:9093`
  - Topic: `logs`
  - Consumer group: `vector-consumer-group`

#### Transforms
1. **parse_json**: Parses incoming JSON messages
2. **add_metadata**: Adds processing timestamp and source information
3. **filter_logs**: Filters out debug-level logs

#### Sinks
- **Console**: Outputs processed logs to stdout (for debugging)
- Additional sinks (Elasticsearch, file) are available as comments

### Customization

Edit `vector.toml` to:
- Change Kafka bootstrap servers and topics
- Modify transform logic using [VRL](https://vector.dev/docs/reference/vrl/)
- Enable/configure additional sinks

### Migration Notes

This configuration was migrated from Logstash. Key differences:
- Configuration format: Ruby DSL → TOML
- Transform language: Logstash filters → VRL (Vector Remap Language)
- Plugin ecosystem: Logstash plugins → Vector components