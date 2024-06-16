# Nats-Streaming Server with Prometheus, Grafana metrics
1. Create database in PosgreSql
postgre_db.go 
URL: connStr := "postgres://postgres:admin@localhost:5432/postgres?sslmode=disable" 
if DB exists: DROP table delivery, items, order_table, payment
create script createDB.sql
2. go run "L0-main\runner\main.go"
3. go run src/publisher.go

To run prometheus and grafana
1. docker-compose up -d
2. go run "c:\Users\Мария\Desktop\L0\runner\main.go"
3. go run src/publisher.go
4. http://localhost:9090 - prometheus
5. http://localhost:3000/login sign in with admin login  ()
6. change password
7. sign in with new password
8. add data source prometheus
9. prometheus server URL * http://localhost:9090 or 127.0.0.1 or http://host.docker.internal:9090 
10.building dashboard

Пример отображения в Prometheus go_memstats_alloc_bytes
 ![image](https://github.com/ernsterfickfacker/NatsStreamingServer_PrometheusGrafana/assets/93219479/c56355e9-d8d4-43ef-bbec-7d1cde55ae61)
Дашборд в Grafana go_memstats_alloc_bytes
![image](https://github.com/ernsterfickfacker/NatsStreamingServer_PrometheusGrafana/assets/93219479/880cbf14-eb8f-47c1-82b2-b181922d5787)

Пример отображения в Prometheus http_request_duration_seconds_sum 
![image](https://github.com/ernsterfickfacker/NatsStreamingServer_PrometheusGrafana/assets/93219479/e4c7d1f0-5529-47d2-a26f-591c72f946df)
Дашборд в Grafana http_request_duration_seconds_sum 
![image](https://github.com/ernsterfickfacker/NatsStreamingServer_PrometheusGrafana/assets/93219479/f3ce3950-1577-4b2e-aea8-00f01b4125e7)




