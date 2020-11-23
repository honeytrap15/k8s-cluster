# k8s-cluster sample

## to up virtual machines

```bash
$ vagrant up
```

## joining to cluster

```bash
$ vagrant ssh master
(master)$ sudo kubeadm token create --print-join-command
```

出力されたコマンドをコピーして各ノードで実行

```bash
$ vagrant ssh worker
(worker)$ sudo kubeadm join 192.168.33.10:6443 --token xxxxxx.xxxxxxxxxxxxxxxx --discovery-token-ca-cert-hash sha256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

```bash
$ vagrant ssh router
(router)$ sudo kubeadm join 192.168.33.10:6443 --token xxxxxx.xxxxxxxxxxxxxxxx --discovery-token-ca-cert-hash sha256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

## labeling to nodes

```bash
$ vagrant ssh master
```

```bash
(master)$ kubectl label node worker node-role.kubernetes.io/worker=worker
(master)$ kubectl label node router node-role.kubernetes.io/router=router
(master)$ kubectl taint nodes router key=value:NoSchedule
```

正しくラベリングされていることを確認する

```bash
(master)$ kubectl get nodes -o -wide
```
