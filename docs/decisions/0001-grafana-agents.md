## Grafana agents - Are not mature

Reasons for not going with Grafana agents
- The documentation is confusing and the Agents and operator docs are not aligned.
- Some of the main selling points (integrations and traces) are not supported by the operator
- Generally there were less configuration options in the Grafana Agent Operators.
  - One example is the implementation of Promtail: The `fs.inotify.max_user_instances` value can be changed in Promtails chart, but is not mentioned in the Grafana Agents implementation.
  - It is also hard to make changes directly to one of the pods using arguments, you cannot add a argument, you have to provide **all** the arguments if you want to add or change one.

Grafana Agents was chosen initially as a way to avoid a bulky Prometheus instance, as we only need the `remote write` functionality, so we can send metrics to Cortex.

<br>

### Reasons for switching back to Prometheus and Promtail

- Prometheus has an experimental agent feature, and according to [this blog](https://prometheus.io/blog/2021/11/16/agent/) it has been tested at scale.
 It sounds like it would achieve the same goal of giving us a instance that can `remote write` to cortex, without storing metrics locally.
- Prometheus is more mature for the time being.
- The prometheus operator could also be worth looking at since it give us some of the same features as the agents does, while still providing more configuration options.
  (Also endorsment for prometheus-operator is higher - 6,7k stars vs 594 at the time of writing)
- For Promtail, there is currently an issues related to `filetarget.fsnotify.newwatcher: to many open files` which is said to be fixed by using `fs.inotify.max_user_instances` but the Grafana agents does not have a way of adding this to the logging pods, this promtail does [here](https://github.com/grafana/helm-charts/blob/e64169fab5fe248787bd62f4e1a990760fbf99a8/charts/promtail/values.yaml#L20)

### Recommendation

We should consider going back to using Prometheus and Promtail,

This is mainly due to the inconsistent documentation and early stage of the Grafana Agents, and we need something that is more mature and has more consistent documentation.

The best option is to use the Prometheus operator for convenience, and then to deploy Promtail separately from Loki for extra flexibility. This way we do not need to change the way we deploy Loki if we want to switch the logging agent at a later stage (e.g. to Grafana Agents when it is more mature).
