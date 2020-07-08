# Scheduling

**Contents**:

* [Use label selectors to schedule pods](#use-label-selectors-to-schedule-pods)
* [Understand the role of DaemonSets](#understand-the-role-of-daemonsets)

## Use label selectors to schedule pods

**Labels** are key/value pairs that are attached to objects, such as pods.

```yaml
"metadata": {
  "labels": {
    "key1" : "value1",
    "key2" : "value2"
  }
}
```

*Examples:*

* "release": "stable", "release": "canary".
* "environment": "dev", "environment": "qa", "environment": "production".
* "tier": "frontend", "tier": "backend", "tier" :"cache".
* "partition": "customerA", "partition": "customerB".
* "track": "daily", "track": "weekly".

**Label selectors** allow the client/user to identify a set of objects, by filtering labels.

```bash
kubectl get pods -l release=stable
```

**Nodes** can also be labelled:

```bash
kubectl label nodes node-with-fast-storage disktype=ssd
```

From here, one can assign a given pod to a set of nodes holding a pre-defined label using `nodeSelector`:

```yaml
apiVersion: v1
kind: Pod
...
spec:
...
  nodeSelector:
    disktype: ssd
```

## Understand the role of DaemonSets

A *DaemonSet* ensures that all (or some) Nodes run a copy of a Pod. This way, if nodes are added to the cluster, Pods are added to these newer nodes. Also, if nodes are removed from the cluster, the garbage collection mechanism will delete those pods.

Uses for DaemonSets:

* running a cluster storage daemon on every node.
* running a logs collection daemon on every node.
* running a node monitoring daemon on every node.
