# Kubernetes Alert Runbooks

As Rob Ewaschuk [puts it](https://docs.google.com/document/d/199PqyG3UsyXlwieHaqbGiWVa8eMWi8zzAn0YfcApr8Q/edit#):
> Playbooks (or runbooks) are an important part of an alerting system; it's best to have an entry for each alert or family of alerts that catch a symptom, which can further explain what the alert means and how it might be addressed.

It is a recommended practice that you add an annotation of "runbook" to every prometheus alert with a link to a clear description of it's meaning and suggested remediation or mitigation. While some problems will require private and custom solutions, most common problems have common solutions. In practice, you'll want to automate many of the procedures (rather than leaving them in a wiki), but even a self-correcting problem should provide an explanation as to what happened and why to observers.

Matthew Skelton & Rob Thatcher have an excellent [run book template](https://github.com/SkeltonThatcher/run-book-template). This template will help teams to fully consider most aspects of reliably operating most interesting software systems, if only to confirm that "this section definitely does not apply here" - a valuable realization.

This page collects this repositories alerts and begins the process of describing what they mean and how it might be addressed. Links from alerts to this page are added [automatically](https://github.com/kubernetes-monitoring/kubernetes-mixin/blob/master/alerts/add-runbook-links.libsonnet).


### kubernetes-absent

##### KubeAPIDown
+ *Severity*: critical
+ *Message*: `KubeAPI has disappeared from Prometheus target discovery.`



##### KubeControllerManagerDown
+ *Severity*: critical
+ *Message*: `KubeControllerManager has disappeared from Prometheus target discovery.`



##### KubeSchedulerDown
+ *Severity*: critical
+ *Message*: `KubeScheduler has disappeared from Prometheus target discovery.`



##### KubeletDown
+ *Severity*: critical
+ *Message*: `Kubelet has disappeared from Prometheus target discovery.`



### kubernetes-apps

##### KubePodCrashLooping
+ *Severity*: critical
+ *Message*: `{{ $labels.namespace }}/{{ $labels.pod }} ({{ $labels.container }}) is restarting {{ printf "%.2f" $value }} / second`



##### KubePodNotReady
+ *Severity*: critical
+ *Message*: `{{ $labels.namespace }}/{{ $labels.pod }} is not ready.`



##### KubeDeploymentGenerationMismatch
+ *Severity*: critical
+ *Message*: `Deployment {{ $labels.namespace }}/{{ $labels.deployment }} generation mismatch`



##### KubeDeploymentReplicasMismatch
+ *Severity*: critical
+ *Message*: `Deployment {{ $labels.namespace }}/{{ $labels.deployment }} replica mismatch`



##### KubeStatefulSetReplicasMismatch
+ *Severity*: critical
+ *Message*: `StatefulSet {{ $labels.namespace }}/{{ $labels.statefulset }} replica mismatch`



##### KubeStatefulSetGenerationMismatch
+ *Severity*: critical
+ *Message*: `StatefulSet {{ $labels.namespace }}/{{ $labels.statefulset }} generation mismatch`



##### KubeDaemonSetRolloutStuck
+ *Severity*: critical
+ *Message*: `Only {{$value}}% of desired pods scheduled and ready for daemon set {{$labels.namespace}}/{{$labels.daemonset}}`



##### KubeDaemonSetNotScheduled
+ *Severity*: warning
+ *Message*: `A number of pods of daemonset {{$labels.namespace}}/{{$labels.daemonset}} are not scheduled.`



##### KubeDaemonSetMisScheduled
+ *Severity*: warning
+ *Message*: `A number of pods of daemonset {{$labels.namespace}}/{{$labels.daemonset}} are running where they are not supposed to run.`



##### KubeCronJobRunning
+ *Severity*: warning
+ *Message*: `CronJob {{ $labels.namespaces }}/{{ $labels.cronjob }} is taking more than 1h to complete.`

Check the cronjob using kubectl decribe cronjob <cronjob> and look at the pod logs using kubectl logs <pod> for further information.


##### KubeJobCompletion
+ *Severity*: warning
+ *Message*: `Job {{ $labels.namespaces }}/{{ $labels.job }} is taking more than 1h to complete.`

Check the job using kubectl decribe job <job> and look at the pod logs using kubectl logs <pod> for further information.


##### KubeJobFailed
+ *Severity*: warning
+ *Message*: `Job {{ $labels.namespaces }}/{{ $labels.job }} failed to complete.`

Check the job using kubectl decribe job <job> and look at the pod logs using kubectl logs <pod> for further information.


### kubernetes-resources

##### KubeCPUOvercommit
+ *Severity*: warning
+ *Message*: `Overcommited CPU resource requests on Pods, cannot tolerate node failure.`



##### KubeMemOvercommit
+ *Severity*: warning
+ *Message*: `Overcommited Memory resource requests on Pods, cannot tolerate node failure.`



##### KubeCPUOvercommit
+ *Severity*: warning
+ *Message*: `Overcommited CPU resource request quota on Namespaces.`



##### KubeMemOvercommit
+ *Severity*: warning
+ *Message*: `Overcommited Memory resource request quota on Namespaces.`



##### KubeQuotaExceeded
+ *Severity*: warning
+ *Message*: `{{ printf "%0.0f" $value }}% usage of {{ $labels.resource }} in namespace {{ $labels.namespace }}.`



### kubernetes-storage

##### KubePersistentVolumeUsageCritical
+ *Severity*: critical
+ *Message*: `The persistent volume claimed by {{ $labels.persistentvolumeclaim }} in namespace {{ $labels.namespace }} has {{ printf "%0.0f" $value }}% free.`



##### KubePersistentVolumeFullInFourDays
+ *Severity*: critical
+ *Message*: `Based on recent sampling, the persistent volume claimed by {{ $labels.persistentvolumeclaim }} in namespace {{ $labels.namespace }} is expected to fill up within four days. Currently {{ $value }} bytes are available.`



### kubernetes-system

##### KubeNodeNotReady
+ *Severity*: warning
+ *Message*: `{{ $labels.node }} has been unready for more than an hour`



##### KubeVersionMismatch
+ *Severity*: warning
+ *Message*: `There are {{ $value }} different versions of Kubernetes components running.`



##### KubeClientErrors
+ *Severity*: warning
+ *Message*: `Kubernetes API server client '{{ $labels.job }}/{{ $labels.instance }}' is experiencing {{ printf "%0.0f" $value }}% errors.'`



##### KubeClientErrors
+ *Severity*: warning
+ *Message*: `Kubernetes API server client '{{ $labels.job }}/{{ $labels.instance }}' is experiencing {{ printf "%0.0f" $value }} errors / sec.'`



##### KubeletTooManyPods
+ *Severity*: warning
+ *Message*: `Kubelet {{$labels.instance}} is running {{$value}} pods, close to the limit of 110.`



##### KubeAPILatencyHigh
+ *Severity*: warning
+ *Message*: `The API server has a 99th percentile latency of {{ $value }} seconds for {{$labels.verb}} {{$labels.resource}}.`



##### KubeAPILatencyHigh
+ *Severity*: critical
+ *Message*: `The API server has a 99th percentile latency of {{ $value }} seconds for {{$labels.verb}} {{$labels.resource}}.`



##### KubeAPIErrorsHigh
+ *Severity*: critical
+ *Message*: `API server is erroring for {{ $value }}% of requests.`



##### KubeAPIErrorsHigh
+ *Severity*: warning
+ *Message*: `API server is erroring for {{ $value }}% of requests.`



##### KubeClientCertificateExpiration
+ *Severity*: warning
+ *Message*: `Kubernetes API certificate is expiring in less than 7 days.`



##### KubeClientCertificateExpiration
+ *Severity*: critical
+ *Message*: `Kubernetes API certificate is expiring in less than 1 day.`




## Other Kubernetes Runbooks and troubleshooting
+ [Troubleshoot Clusters ](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-cluster/)
+ [Cloud.gov Kubernetes Runbook ](https://cloud.gov/docs/ops/runbook/troubleshooting-kubernetes/)
+ [Recover a Broken Cluster](https://codefresh.io/Kubernetes-Tutorial/recover-broken-kubernetes-cluster/)
