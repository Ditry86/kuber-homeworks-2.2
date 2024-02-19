# Домашнее задание к занятию «Запуск приложений в K8S»

<br>

## Задание 1.

Проверка чтения общего файла из пода multitool, обновляемого записями контейнера busybox созданного деплоймента:

```
kubectl exec -ti deployments/busybox-multitool -c multitool -- bash
busybox-multitool-6788876585-j8hlm:/# cat /mnt/volume1/stream 
Tue Feb 13 12:39:13 UTC 2024
busybox-multitool-6788876585-j8hlm:/# cat /mnt/volume1/stream 
Tue Feb 13 12:39:18 UTC 2024
busybox-multitool-6788876585-j8hlm:/# cat /mnt/volume1/stream 
Tue Feb 13 12:39:23 UTC 2024
```

## Задание 2. 

Проверка доступа каталога ноды /var/log и чтения файла syslog из пода daemonset'а через созданый volume (hostPath): 

```
kubectl exec -ti daemonsets/multitool-daemon -- bash
multitool-daemon-kw9rw:/# ls /mnt/log
README                     auth.log                   boot.log.2                 btmp                       dist-upgrade               dpkg.log                   gpu-manager.log            kern.log.3.gz              sssd                       ubuntu-advantage.log.1
alternatives.log           auth.log.1                 boot.log.3                 btmp.1                     dmesg                      dpkg.log.1                 hp                         kern.log.4.gz              syslog                     ubuntu-advantage.log.2.gz
alternatives.log.1         auth.log.2.gz              boot.log.4                 cloud-init-output.log      dmesg.0                    dpkg.log.2.gz              installer                  lastlog                    syslog.1                   unattended-upgrades
alternatives.log.2.gz      auth.log.3.gz              boot.log.5                 cloud-init.log             dmesg.1.gz                 faillog                    journal                    openvpn                    syslog.2.gz                wtmp
apport.log                 auth.log.4.gz              boot.log.6                 containers                 dmesg.2.gz                 fontconfig.log             kern.log                   pods                       syslog.3.gz
apport.log.1               boot.log                   boot.log.7                 cups                       dmesg.3.gz                 gdm3                       kern.log.1                 private                    syslog.4.gz
apt                        boot.log.1                 bootstrap.log              cups-browsed               dmesg.4.gz                 gpu-manager-switch.log     kern.log.2.gz              speech-dispatcher          ubuntu-advantage.log
multitool-daemon-kw9rw:/# cat /mnt/log/syslog | tail
2024-02-13T21:46:09.994328+09:00 homework systemd[1]: run-containerd-runc-k8s.io-488eded3c63ea82c73a28bffe08e86181cc8a2a975d7176d88198f584683ec53-runc.vFx3IH.mount: Deactivated successfully.
2024-02-13T21:46:14.539371+09:00 homework microk8s.daemon-kubelite[1535548]: E0213 21:46:14.539067 1535548 pod_workers.go:1300] "Error syncing pod, skipping" err="failed to \"StartContainer\" for \"nginx-ingress-microk8s\" with ImagePullBackOff: \"Back-off pulling image \\\"registry.k8s.io/ingress-nginx/controller:v1.8.0\\\"\"" pod="ingress/nginx-ingress-microk8s-controller-6f5rn" podUID="dc90cffa-2d5f-4dd2-82dd-0cd71f6f0e3a"
2024-02-13T21:46:16.665193+09:00 homework systemd[1]: run-containerd-runc-k8s.io-d259343387e5304eccc59abf9846c866184a44c6b1e46df8f7acd5f60b0e3af7-runc.lWlJRQ.mount: Deactivated successfully.
2024-02-13T21:46:19.250203+09:00 homework systemd[1]: run-containerd-runc-k8s.io-488eded3c63ea82c73a28bffe08e86181cc8a2a975d7176d88198f584683ec53-runc.Sh9EgT.mount: Deactivated successfully.
2024-02-13T21:46:20.002257+09:00 homework systemd[1]: run-containerd-runc-k8s.io-488eded3c63ea82c73a28bffe08e86181cc8a2a975d7176d88198f584683ec53-runc.v8goHm.mount: Deactivated successfully.
2024-02-13T21:46:21.531285+09:00 homework systemd[1]: run-containerd-runc-k8s.io-d259343387e5304eccc59abf9846c866184a44c6b1e46df8f7acd5f60b0e3af7-runc.v18uBc.mount: Deactivated successfully.
2024-02-13T21:46:29.250041+09:00 homework systemd[1]: run-containerd-runc-k8s.io-488eded3c63ea82c73a28bffe08e86181cc8a2a975d7176d88198f584683ec53-runc.JaGC9Q.mount: Deactivated successfully.
2024-02-13T21:46:29.539370+09:00 homework microk8s.daemon-kubelite[1535548]: E0213 21:46:29.539068 1535548 pod_workers.go:1300] "Error syncing pod, skipping" err="failed to \"StartContainer\" for \"nginx-ingress-microk8s\" with ImagePullBackOff: \"Back-off pulling image \\\"registry.k8s.io/ingress-nginx/controller:v1.8.0\\\"\"" pod="ingress/nginx-ingress-microk8s-controller-6f5rn" podUID="dc90cffa-2d5f-4dd2-82dd-0cd71f6f0e3a"
2024-02-13T21:46:29.996675+09:00 homework systemd[1]: run-containerd-runc-k8s.io-488eded3c63ea82c73a28bffe08e86181cc8a2a975d7176d88198f584683ec53-runc.ny5qSp.mount: Deactivated successfully.
2024-02-13T21:46:31.561084+09:00 homework systemd[1]: run-containerd-runc-k8s.io-d259343387e5304eccc59abf9846c866184a44c6b1e46df8f7acd5f60b0e3af7-runc.sClH40.mount: Deactivated successfully.
```