# Wildfire Analysis
Brazilian wildfire analysis based on Apache Spark + Apache Kudu + Jupyter Notebook

# Setup and Run

```bash
git clone --branch latest https://github.com/arthursimas1/wildfire-analysis.git
cd wildfire-analysis
docker-compose up -d
```

Enter the *impala-shell* terminal, connect to the Kudu master and create the table.

```bash
docker exec -it impala impala-shell
```

```sql
connect;   -- (run again if not successful)

CREATE TABLE queimada (
  datahora STRING,
  latitude DECIMAL(8, 5),
  longitude DECIMAL(8, 5),
  ano INT,
  mes INT,
  satelite STRING,
  pais STRING,
  estado STRING,
  municipio STRING,
  bioma STRING,
  diasemchuva DECIMAL(10, 5),
  precipitacao DECIMAL(10, 5),
  riscofogo DECIMAL(10, 5),
  frp DECIMAL(10, 5),
  PRIMARY KEY(datahora, latitude, longitude)
)
STORED AS KUDU;

CREATE TABLE queimadas_bioma_mes (
  bioma STRING,
  ano INT,
  mes INT,
  count BIGINT,
  PRIMARY KEY(bioma, ano, mes)
)
STORED AS KUDU;

CREATE TABLE frp_estado_mes (
  estado STRING,
  ano INT,
  mes INT,
  max_frp DECIMAL(10, 5),
  PRIMARY KEY(estado, ano, mes)
)
STORED AS KUDU;

exit;
```


> 💡 You can stop *impala* as we only use it for creating the table.

> ```bash
> docker stop impala
> ```

Get the *JupyterLab* link.

```bash
docker logs pyspark-notebook
```

Access the JupyterLab, upload the notebook and the `data.zip`. Run.

Then you can stop the running containers:

```bash
docker-compose stop
```

Or remove them:

```bash
docker-compose rm
```
