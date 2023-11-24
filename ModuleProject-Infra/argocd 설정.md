```
```

1. argocd 설치
```bash
$ kubectl create namespace argocd
$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
2. 외부 접속 허용
```bash
$ kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

> service/argocd-server patched
```

3. argocd password 확인
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

iGVzcJS0WOk00UlZ
```

4. ```

```
kubectl get svc argocd-server -n argocd                               

NAME            TYPE           CLUSTER-IP      EXTERNAL-IP                                                 
              PORT(S)                      AGE
argocd-server   LoadBalancer   172.20.119.95   a5583fd2c7e104c118208c1c85f96148-1494652202.us-west-2.elb.amazonaws.com   80:31096/TCP,443:31935/TCP   13m
```

![](https://i.imgur.com/2yl8I00.png)