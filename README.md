Services:
  postgres:
    image: postgres: 13
    environment:
      POSTGRES_USER: airflow
      POSTGRES_PASSWORD: airflow
      POSTGRES_DB: airflow
    volumes:
      - postgres-db-volume:var/lib/postgresql/data
    healthcheck:
      test: ["CMD","pg_isready", "-U", "airflow"]
      interval: 5s
      retries: 5
    restart: always

docker run -it -e POSTGRES_USER="root" -e POSTGRES_PASSWORD=admin -e POSTGRES_DB="ny_taxi" -v "/c/users/abhis/git/zoomcamp/week_1/ny_taxi_postgres_data://var/lib/postgresql/data" -p 5431:5432 postgres:13

#for accessing the newyork taxi database password is "admin"
pgcli -h localhost -p 5431 -u root -d ny_taxi

https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page

https://www1.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_yellow.pdf

docker run -it \
  -e PGADMIN_DEFAULT_EMAIL = "admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD = "admin" \
  -p 8080:80 \
  --network=pg-network
  --name pgadmin-2
  dpage/pgadmin4

  #network info
docker network create pg-network

  docker run -it -e POSTGRES_USER="root" -e POSTGRES_PASSWORD=admin -e POSTGRES_DB="ny_taxi" -v "/c/users/abhis/git/zoomcamp/week_1/ny_taxi_postgres_data://var/lib/postgresql/data" -p 5431:5432 --network=pg-network --name pg-database postgres:13

URL="https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2021-01.csv"

python ingest_data.py \

  --user = root \
  --password = admin \
  --host = localhost \
  --port = 5430 \
  --db = ny_taxi \
  --table_name = yellow_taxi_data \ 
  --url = ${URL}

  docker build -t taxi_ingest:v001 .

  docker run taxi_ingest:v001 .
  --user = root \
  --password = admin \
  --host = localhost \
  --port = 5430 \
  --db = ny_taxi \
  --table_name = yellow_taxi_data \ 
  --url = ${URL}

  
  URL="http://192.168.1.2:8000/yellow_tripdata_2021-01.csv"
docker run -it --network=pg-network taxi_ingest:v001 --user=root --password=admin --host=pg-database --port=5430 --db=ny_taxi --table_name=yellow_taxi_trips --url=${URL}
