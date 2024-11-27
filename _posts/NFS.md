# NFS (Network File System)

- **111 - RPCbind/Portmapper**
- **2049 - NFS**

1. 접근 제어
2. 파일 권한
3. No_Root_Squash

```
---script='nfs-*'
showmount -e <ip>
```

**showmount** tool을 이용해서 nfs가 현재 노출하고 있는 directory와 접근 가능한 ip를 확인

```
sudo mount 172.31.217.167:/public-nfs /mnt/(dir)
```
mount 명령어를 sudo 권한으로 실행해 /mnt/(dir) 경로에 타겟 nfs에 노출되어 있던 directory를 연결시킴