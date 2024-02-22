# Домашнее задание к занятию «Запуск приложений в K8S»

<br>

## Задание 1.

Проверка чтения общего файла из пода multitool, обновляемого записями контейнера busybox созданного деплоймента:

```
ditry@workbook:~/kuber-homeworks-2.2$ kubectl exec -ti deployments/busybox-multitool -c multitool -- bash
busybox-multitool-7f85db9ffd-hszv7:/# cat /mnt/volume1/stream 
Thu Feb 22 02:53:37 UTC 2024
busybox-multitool-7f85db9ffd-hszv7:/# cat /mnt/volume1/stream 
Thu Feb 22 02:53:42 UTC 2024
busybox-multitool-7f85db9ffd-hszv7:/# cat /mnt/volume1/stream 
Thu Feb 22 02:53:47 UTC 2024
...
ditry@workbook:~/kuber-homeworks-2.2$ cat /mnt/volume1/stream 
Thu Feb 22 02:54:02 UTC 2024
ditry@workbook:~/kuber-homeworks-2.2$ cat /mnt/volume1/stream 
Thu Feb 22 02:54:07 UTC 2024
```

Удаляем deployment и pvc:

```
ditry@workbook:~/kuber-homeworks-2.2$ kubectl delete deployments.apps busybox-multitool 
deployment.apps "busybox-multitool" deleted
ditry@workbook:~/kuber-homeworks-2.2$ kubectl delete persistentvolumeclaims local-vol-claim 
persistentvolumeclaim "local-vol-claim" deleted
ditry@workbook:~/kuber-homeworks-2.2$ ls /mnt/volume1/
stream
```

Файл остается на месте, поскольку pv до сих пор активен.
Удаляем pv:

```
ditry@workbook:~/kuber-homeworks-2.2$ kubectl delete persistentvolume local-volume 
persistentvolume "local-volume" deleted
ditry@workbook:~/kuber-homeworks-2.2$ ls /mnt/volume1/
stream
```

Файл так же остается на месте, поскольку `ReclaimPolitic - Delete` позволяет удалять ресурс с файлами после удаления pv, нотолько для внешних облачных хранилищ. В нашем случае хранилище - локальное.

<br>

## Задание 2.

Полсе запуска на хосте nfs сервера и пода-драйвера `csi-nfs` в ns `default`, запускаем storage и с помощью pvc выделяем для пода valume емкостью 1 ГБ. Драйвер `csi-nfs` подключен к сетевому каталогу на хосте - `/mnt/nfs`.

Проверка доступности, и записи в volume: (сет. каталог):

```
ditry@workbook:~/kuber-homeworks-2.2$ kubectl exec -ti deployments/multitool -- bash
multitool-7b95cdf4d9-lnvbc:/# mkdir /mnt/volume/new
multitool-7b95cdf4d9-lnvbc:/# ls /mnt/volume/
new
...
ditry@workbook:~/kuber-homeworks-2.2$ ls /mnt/nfs/pvc-3f985d49-7d67-4ae5-aab5-7a83846790c7/
new
```
