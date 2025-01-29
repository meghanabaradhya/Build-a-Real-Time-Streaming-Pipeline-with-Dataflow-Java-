# Serverless Data Processing with Apache Beam and Dataflow  

This project demonstrates how to build a serverless ETL pipeline using Apache Beam and Google Cloud Dataflow. The pipeline processes synthetic web server log data in JSON format, transforms it into structured objects, and writes the output to a BigQuery table.

---

## Features  
- **Data Processing**: Reads web server log data from Google Cloud Storage.  
- **Data Transformation**: Parses JSON data into structured objects using Apache Beam's `ParDo`.  
- **BigQuery Integration**: Writes the transformed data into BigQuery using `BigQueryIO`.  
- **Serverless Execution**: Leverages Google Cloud Dataflow for scalable and cost-effective processing.  

---

## Prerequisites  
1. **Google Cloud Project** with billing enabled.  
2. **Java Development Kit (JDK)** version 8 or higher.  
3. **Apache Maven** installed.  
4. Enabled APIs:  
   - Dataflow API  
   - BigQuery API  
   - Cloud Storage API  

---

## Setup  
1. Clone the repository:  
   ```bash
   git clone <repo_url>
   cd <repo_name>
   ```
2. Navigate to the lab folder:  
   ```bash
   cd 1_Basic_ETL/labs
   ```
3. Add required dependencies to `pom.xml`:  
   ```xml
   <dependency>
      <groupId>org.apache.beam</groupId>
      <artifactId>beam-sdks-java-core</artifactId>
      <version>${beam.version}</version>
   </dependency>
   <!-- Add other Beam dependencies -->
   ```

4. Download dependencies:  
   ```bash
   mvn clean dependency:resolve
   ```

---

## Steps  
### 1. Generate Synthetic Data  
Run the following script to generate and upload the sample dataset to Cloud Storage:  
```bash
source create_batch_sinks.sh
bash generate_batch_events.sh
```

### 2. Write ETL Pipeline  
- Implement the pipeline in `MyPipeline.java`.  
- Use `TextIO.read()` to read data from GCS.  
- Apply `ParDo` transforms to parse JSON into structured objects.  

### 3. Run Pipeline Locally  
Run the pipeline using the DirectRunner:  
```bash
mvn compile exec:java -Dexec.mainClass=com.mypackage.pipeline.MyPipeline
```

### 4. Deploy to Dataflow  
Switch to `DataflowRunner` for scalable, serverless processing:  
```java
options.setRunner(DataflowRunner.class);
```

Deploy the pipeline:  
```bash
mvn compile exec:java -Dexec.mainClass=com.mypackage.pipeline.MyPipeline
```

---

## BigQuery Output  
The pipeline writes data to a BigQuery table with the following schema:  
| Field        | Type    | Description              |  
|--------------|---------|--------------------------|  
| user_id      | INTEGER | Unique user identifier   |  
| ip           | STRING  | IP address               |  
| timestamp    | STRING  | Timestamp of the event   |  
| http_request | STRING  | HTTP request details     |  
| num_bytes    | INTEGER | Size of the HTTP request |  

---

## Troubleshooting  
1. **Dependency Issues**: Run `mvn clean install` to resolve conflicts.  
2. **Pipeline Errors**: Check logs in the Dataflow job UI for debugging.  
3. **Environment Reset**: Use GCP's VM reset option if IDE issues arise.  

---

## Resources  
- [Apache Beam Documentation](https://beam.apache.org/documentation/)  
- [Google Cloud Dataflow](https://cloud.google.com/dataflow)  

---

## License  
This project is licensed under the MIT License.

