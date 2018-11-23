The program `statusgo` has two arguments:

1.  `-pprof` to enable a connection using the `pprof` tool
2.  `-pprofport` to set the port to connect from `pprof` via HTTP
    (default is 52525)

Started with the first argument the `pprof` tool (via `go tool pprof` or
better after installing the newer version with `go get
github.com/google/pprof`) can connect via

`pprof -seconds 300 http://localhost:52525/debug/pprof/profile`

for a CLI or

`pprof -seconds 300 -http localhost:53535
http://localhost:52525/debug/pprof/profile`

for a web interface. `-seconds 300` tells to collect samples for 5
minutes, default are only 30 seconds.

Both interfaces are starting after the sampling time and allow to
analyse the sampled data. Some impressions from the web interface are
here: ![Top_View.png](Top_View.png "Top_View.png")
![Graph_View.png](Graph_View.png "Graph_View.png")
![Flame_Graph_View.png](Flame_Graph_View.png "Flame_Graph_View.png")