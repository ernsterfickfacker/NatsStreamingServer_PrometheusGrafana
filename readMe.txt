1. Create database in PosgreSql
postgre_db.go 
URL: connStr := "postgres://postgres:admin@localhost:5432/postgres?sslmode=disable" 
if DB exists: DROP table delivery, items, order_table, payment
createDB.sql
2. go run "L0-main\runner\main.go"
3. go run src/publisher.go

To run prometheus
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



Additional metrics
import (  

   "net/http" 
   "time"
   "github.com/prometheus/client_golang/prometheus"  
   "github.com/prometheus/client_golang/prometheus/promhttp"

)

 

var ( 

   version   = "1.0.0"  
   startTime = time.Now()

   requestsTotal = prometheus.NewCounterVec( 
       prometheus.CounterOpts{ 
           Name: "http_requests_total", 
           Help: "WEB INFO metric. Total number of HTTP requests.", 
       }, 
       []string{"method"}, 
   )

   requestDuration = prometheus.NewHistogramVec( 
       prometheus.HistogramOpts{ 
           Name: "http_request_duration_seconds", 
           Help: "WEB INFO metric. Duration of HTTP requests.", 
       }, 
       []string{"method"}, 

   )

   requestsByStatusCode = prometheus.NewCounterVec( 
       prometheus.CounterOpts{ 
           Name: "http_requests_status_total", 
           Help: "WEB INFO metric. Total number of HTTP requests by status code.", 
       }, 
       []string{"status_code"}, 

   )

 

   serviceInfo = prometheus.NewGaugeVec( 
       prometheus.GaugeOpts{ 
           Name: "service_info", 
           Help: "WEB INFO metric. Service information including version and uptime.", 

       }, 
       []string{"version", "start_time"}, 
   )
)

func init() { 
   prometheus.MustRegister(requestsTotal) 
   prometheus.MustRegister(requestDuration) 
   prometheus.MustRegister(requestsByStatusCode) 
   prometheus.MustRegister(serviceInfo)

}

 
func recordMetrics() { 
   go func() { 
       for { 
           serviceInfo.With(prometheus.Labels{ 
               "version":    version, 
               "start_time": startTime.Format(time.RFC3339), 
           }).Set(float64(time.Since(startTime).Seconds())) 
           time.Sleep(10 * time.Second) 
       } 
   }()
}

func handler(w http.ResponseWriter, r *http.Request) { 
   timer := prometheus.NewTimer(requestDuration.WithLabelValues(r.Method)) 
   defer timer.ObserveDuration() 
   statusCode := http.StatusOK    
   requestsTotal.WithLabelValues(r.Method).Inc() 
   requestsByStatusCode.WithLabelValues(http.StatusText(statusCode)).Inc()
   w.WriteHeader(statusCode)
}