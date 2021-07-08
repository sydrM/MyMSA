## MyMSA
----------------

![이미지](task.png)

# 1. GitRepository 생성
  * cf) git clone https://github.com/sydrM/MyMSA
# 2. Docker Image 만들기
```
cp -r dockers/nginx2 MyMSA/mynginx
cd MyMSA/mynginx/
docker build --rm -t msm86/mynginx:0.1 .
```
# 3. Docker Login
```
docker login
```
* 계정을 접속한 적이 있으면 패스워드를 묻지 않는다

# 4. Docker image push
```
docker run -d --name n1 -p 8888:80 msm86/mynginx:0.1
curl localhost:8888
docker rm -f n1
docker push msm86/mynginx:0.1
```

# 5. Yaml artiface 만들기
```
kubectl create deploy --image=msm86/mynginx:0.1 n1
kubectl expose deploy n1 --type="NodePort" --port 80
kubectl get deploy n1 -o yaml > ~ubuntu/n1_deploy.yaml
kubectl get svc n1 -o yaml > ~ubuntu/n1_svc.yaml
kubectl delete -f ~ubuntu/n1_svc.yaml
kubectl delete -f ~ubuntu/n1_deploy.yaml
chown ubuntu ~ubuntu/n1_deploy.yaml ~ubuntu/n1_svc.yaml
```
* console로 yml파일 가져오기. [실습용 console 서버에서 구현]
```
cd ~/MyMSA/
mkdir n1
cd n1
scp vm01:/home/ubuntu/n1*.yaml ./
```

#  7. GitHub Push
```
git add -A
git config --global user.name "Sydr M"
git config --global user.email "msm86@naver.com"
git commit -m "initial commit"
git push
```

# 8. Namespace 생성
* vm01에서
```
kubectl create ns n1
```

# 9. ARGOCD
* \+ NEW APP
```
Application Name : n1
Project : Default
Repository URL : https://github.com/sydrM/MyMSA
Revision : HEAD
Path : n1
Cluster URL : https://kubernetes.default.svc
Namespace : n1
```

END
