# Kubernetes Resources

## 컨테이너 관련 리소스
## 네트워크 관련 리소스
## 정보 저장 관련 리소스
## 데이터 저장관련 리소스
## 배치 잡 관련 리소스

# Kubernetes Objects

## Object 정의
## 기본 Object
### Pod
### Volume
### Service
### Namespace

## Controller
### Controller 정의
### ReplicaSet
### Deployment
### StatefulSet
### DaemonSet
### Job
### CronJob

## Object 생성 방법

# Pods

## 특징

## 생성 방법

## Life Cycle

## Init Container

## Health Checks

# ReplicaSet

## 작동 방식

## 템플릿을 사용하지 않는 Pod의 획득

# Deployment

## 특징

## 배포 전략

### In-place Deployment
### Rolling Update Deployment
### Blue / Green Deployment
### Canary Deployment

## "Deployment" 오브젝트의 배포 전략

# DaemonSet

## DaemonSet 업데이트 전략

# Service

## 기능

## 서비스를 생성하는 방법

## Pod의 IP 주소를 기반으로 애플리케이션에 접근했을 때 문제점

## Types
### ClusterIP
### NodePort
### LoadBalancer
### ExternalName

# Ingress

## 개요
### 주요 기능
### 인그레스 컨트롤러

## 용어
## 인그레스란?

# Namespace

## Namespace를 사용해 논리적으로 구분할 수 있는 대상

## 기본 Namespace

## Use Cases

## 다른 Namespace에서의 접근

# ConfigMap & Secret

## ConfigMap
### 값을 생성, 조회, 사용하는 방법들

## Secret
### Secret을 생성, 사용하는 방법들
### Secret Types

# Job

## Job Controller의 특징

## Use Cases

## Job의 병렬성 관리

## 메시지 브로커와 잡 컨트롤러의 결합

# CronJob

## CronJob과 Job의 관계

## CronJob 설정

# Storage

## K8s Volumes

## Local Volumes

## Persistent Volumes
### NFS
### PV & PVC
# StatefulSet

## Deployment vs StatefulSet
