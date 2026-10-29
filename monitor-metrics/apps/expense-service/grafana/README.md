 Make the app reachable. Add this to application.properties (D:\tmp\quarkus\DO378-apps\monitor-metrics\apps\expense-service\src\main\resources\). Dev mode reloads it:

quarkus.http.host=0.0.0.0

Apply it. Copy the file into the existing container and restart it. This keeps the container's stored data, whereas recreating it would lose the data:

cd D:\tmp\bob\ltg
podman cp grafana\prometheus.yaml otel-lgtm:/otel-lgtm/prometheus.yaml
podman restart otel-lgtm

The copy is lost if you remove and recreate the container. For a persistent setup, recreate it with the file mounted:

podman rm -f otel-lgtm
podman run -d --name otel-lgtm `
  -p 3000:3000 -p 4317:4317 -p 4318:4318 -p 9090:9090 `
  -v ${PWD}\grafana\prometheus.yaml:/otel-lgtm/prometheus.yaml:Z `
  docker.io/grafana/otel-lgtm:late