POD:
======
With Kubernetes our core goal will be to deploy our applications in the form of containers on worker nodes in a k8s cluster. 
Kubernetes does not deploy containers directly on the worker nodes.
Container is encapsulated in to a Kubernetes Object named POD.
A POD is a single instance of an application.
A POD is the smallest object that we can create in Kubernetes. 

For creating pod create command
for updating pod appply command
ala kaknunda total info ni edit cheyyali antey - kubectl edit pod podname
e command run chesaka apply cheyyakkarledu automatic ga edit ipoddi
apiVersion: v1 # String
kind: Pod # String
metadata: # Dictionary,
    name: my-pod //components of metadata and name odf the pod
    labels: # Dictionary
       app: myapp2
spec:
  containers: # List
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', 'echo Hello Kubernetes! && sleep 3600']


NAMESPACES:
===========
objects ni categorize chesi organize cheydaniki name spaces use chestam
we can use name spaces whenevewr we use multiple apps on cluster or mutiple stages (dev, test , stage)
And also different application teams different application medha work chesinappudu
okokkaru okookka individual name space medha work cheydaniki NS use avuthai.
kubectl get namespaces
kubectl create ns app1 -> app1 is namespace here
kubectl get pods -n app1 -> get pods in particular namespace
kubectl delete pod my-app1ns-pod -n app1
kubectl describe pod my-app1ns-pod -n app1 - for describe pod
describe cmnd mainly used for troubleshooting.
last lo vunna events batti troubleshoot chestam

namespaces must and should be create in first then create all the pods/containers
bcz all the pods/container,sc,pvc etc...will create under namespace

CONTAINER CONFIGURATION:
========================
container conf antey pod lo con run chesinappudu k8s lo konni default commands vunnai
but manam yml file lo konni cmnds echi vatini run cheyyohchu dhanine con.conf antam.

same pod lo vunna containers okadhanitho okati communicate avuthai 
endukantey installation time lo waves links avi estham kabatti

kani different pods lo vunna cont communicate avvalantey containerport define cheyyali

CONFIGMAPS: with ENV variables
===============================
config maps endukantey pods/container etc.. vati nundi configuration ni -
seperate cheydaniki e config map use chestaru
dhinni 2 ways lo apply chestaru 1.env var 2 .volumes
you have manage configs easily with this
and also container image become portable bcz total conifg seperate ipothundi kabatti
edhi conig data ni key value format lo store chestadi
container lo config s/w ni run cheydanniki e cm ni use chestaru
container starting lo e cm ni apply chestam

does not provide encryption
while creating cm, in yml file the name of the configmap and name of configmapkey ref must be same

so 2 files create chesina tarvata pod run chestam, run chesaka telsukodaniki

kubectl logs podname esthey configuration values manaki display chesthundi

CONFIGMAPS: with mounted volumes
=================================

conifgdata, etcd folder lo locate avutadi

volume mounts antey  volume path estham

volumes section lo config map object file lo vunna name ref estham

kubectl exec podname -- ls /etc/config

kubectl exec podname -- cat /etc/config

SECRETS:
========

sensitve infor ni store cheydaniki use chestam
ex: db passwords, security keys etc..
configmaps container total configu ni handle chestadi

SECURITY CONTEXT:
==================
nodes os lo underlying securtiy mechanism lo containers 
ela interact avvadani constomize cheydaniki edhi use chestam

containers os tho interact inapppudu ela security evvalo edhi chesthundi

containers ki sec provide cheydaniki pod specific lo edhi use chetam

oka pod yokka security priviliges and access control list ni define chestadi

pod spec lo dhinni include chestam

kubectl delete pod mysecretpod mysecuritypod --now -> pod fast ga delete avvadaniki

edhi oka attribute

RESOURCE REQUESTS AND LIMITS:
=============================
k8s cluster lo app run avvadaniki e resources avarasaram vuntundi
con resources reqest pod spec lo specfiy cheydaniki k8s allow cheydaniki
container yokka cpu, memory ni reources antam

yml file lo ah particular image and container entha limit lo run avvalo mention chestam
okavela run avvakapotey limits ane section lo automatic ga increase avvadaniki inkoka values estham

we can setup this limit range for per container


MULTI CONTAINER PODS:
=====================
oka pod lo 2 or more containers vuntey multi containers pod antam
and they will work together
evi main container ki helper containers
additional func provide chestai
and how they interact eah other in pod are 3 types:

1.sharednetwork
pod lo vunna cont ki ports asign chesthey chalu
avi oka okdhanitho okati communicate avvadanii sn antaru.
no need to expose as a service.

2.sharedstoragevolumes
indhulo containers anni oka storage volumes medha create ayyi
andulo vunna mouted volume ni use cheskodam vall
okadanitho okati easy ga communicate avuthai.

3.sharedprocessnamespace
here no port and no storage class
e method lo signals dwara pod lo vunna conta anni communicate avuthai
dhaniki manam pod spec section lo e line evvali
shareProcessNamespace:true 
multi. conta pod ni ela design chestam antey
1.sidecar pod
indhulo 2 containers vuntai helper container vunna sare
adhi only code pull cheskodam alanti basic tasks chesi main
container ki code push cheydam elantivi chestadi edhi exm matrame
elantivi eadaina chestadi req batti
2.Ambassador pod:
indulo network traffic ambasidor traffic ki connect aie vuntadi
ambasidor traffic main cont ki connect aie vuntundi
so traffic amsidor cont ki ochinappudu adhi main cont ki forward chestadi.
3.Adaptor pod:
main cont lo vunna log out put ni change cheydaniki adapter cont use chestaru

OBSERVABILITY IN K8S:
======================
MANA CONTAINER STATUS NI CUSTMIZE CHESI DETERMINE CHEYDANIKI 
probes use chestam

livenessprobes: 
It caches a deadlock state of application, 
where your application is running and but it unable to make progress the app,
in such state this is used to restart the containers

readinessprobes:
when a pod is not ready, it is removed from
the service load balancer based on the this probe signal

CONTAINER LOGGING:
==================
CONTAINER LOGGING ANTEY container output container log loki veltadi

kubectl logs podname
if multiple pods/container running 
kubectl logs -c podname
to send all logs to particular file
kubectl logs podname > filename.log

DEBUGGING IN K8S:
=================
k8s lo vunna problem ni fix cheyadanni debugging antam
2 wyas of debugging
1.find a issue
2.fix the issue

kubectl describe command is used to find the issue

LABELS AND SELECTORS AND ANNOTATIONS:
======================================
description on codings at vscode

get the pods depends upon appbbbb
kubectl get pods  -l app=myapp
get the pods depends upon environment
kubectl get pods -l environment=production
get the pods not depends upon environment
kubectl get pods -l environment!=production

e cmnds annitini equality based selectors antaru


set based selectors antey ela estharu

kubectl get pods -l 'environment in(dev,prod)'
kubectl get pods  -l app=myapp,environment=production

annotations antey metadata descr lo estam,
object metadata ni store cheydaniki annota use chestam
external tools tho interact avvadaniki annotations use chestam


DEPLOYMENTS:
==============
DEPLOYMENT def in screenshot

deployment use chesi manam apps deployment cheyyochu pods loki
alagey appudaina pods kill itey e deployment kothhavi create chestadi automatic ga

kubectl edit deployment deploymentname

ROLLING UPDATE:
===============
appudaina manam application run chesinapppudu
manam previos version ki vellali antey e rolling updates use avuthai

to update image version or image updation
kubectl set image deployment/deploymentnaame nginx=nginx:1.7.9 --record

here record is used to store rollbacks info

to see the histoory of rollouts

kubectl rollout history deploymrnts/deploymentname

rollout command to previous version

kubectl rollout undo depployment/deply-name --to-revision=1

JOBS IN K8S:
============
k8s jobs 2 types :
1.run to completion
antey edhi 2 or more pods ni create chesi ah job finish ayyaka andulo vunna container 
exit ipoyelaga job execute chesthundi

2.cronjobs

jobs ki similar ga vuntadi edhi same linux lagane
oka schedule prakaram execute chestadi

VOLUMES:
========
E VOLUMES ANEVI K8'S INTERNAL STORAGE OF CONTAINERS, FMERAL STORAGE ANTEY

EDHI CONTAINERS TEMPORARAY STORAGE edhi container stop chesthey loss ipotham

ala kakunda cont stop ina adhi vundali antey e volumes vadali
eppudaitey pod delete/shift to another node ipothundo appudu e vol delete ipothai
otherwise container del/stop ina kuda evi delete avvavu

volumes endukantey eks lo nodes/pods containers eavi delete ipoiena
ah node instances delete ipoiena sare e volumes lo ah data vuntundi

we can use dataabase schema in config map section to
associste with app with depployment and service

here nodeport will give the access to internet

IMP COMMANDS OF K8S
===================

kubectl cluster-info
kubectl cluster-info dumb
kubectl config view
kubectl describe nodes
kubectl describe pods
kubectl get services --all-namespaces
kubectl apiresources -o wide

INGRESS:
========
Ingress is an advanced load balancer which provides Context path based routing, SSL, SSL Redirect and many more (Example: AWS ALB)

outside nundi ochina traffic ni access cheydaniki 2 ways
1.ALB
2.Ingress
both are same but here you can use one or more applications or one more stages
application can access with single ip address
ingress resources and ingress controller e 2 use chesi single ip tho 
same cluster lo multiple applications kani or multiple stages with same application kani manage chestadi
ALB tho ela avvadhu antha same kani akkada only one app ni matrame cheygalam only ingress tho matrame
ela multiple apps/stages ni same cluster tho different service files tho pods lo deploy cheydam avutadi.

DEAMON SETS:
==============
deamon sets endukantey nodes lo vunna pods and containers thaluka 
logs ni monitor cheydaniki deamonset use chestam.
prathi node server lo e deamon set monitoring ni create cheyyali
eppudaitey HPA lo new pods create avuthayo ah time lo automatic ga
e deamon sets kothha dashboard create chesestai new nods lo
nodes delete ipotey avi kuda delete ipothai

node monitor cheydaniki collectd use chestaru
log collection chuddaniki fluentd use chestaru


UPGRADES IN K8S:
================
upgrades in k8s antey kubernetes lo vunna
components annitini ela upgrade chestamo ani

kubectl get nodes -  to chck the versions of master and nodes
kubectl version --short
for update the cluster

sudo apt-mark unhold kubeadm kubelet
sudo apt install -y kubeadm=1.14.1-00
sudo apt-mark hold kubeadm

sudo kubeadm upgrade plan - it shows al the components versions

sudo kubeadm upgrade appply v1.14.1

USES OF ALB WITH EKS:
=====================

-> path based routing means domainname/app1, app2/, /usermngmt
-> content based routing means - ap1.cloudon.com, usrmngmt.cloudon.com.
-> If you want to setup ALB on EKS first you have to install AWS ALB on eks cluster with some IAM roles and polocies

USES OF SSL CERTIFICATE IN EKS
===============================
-> setup routing in route53 with buy a new domain
-> create new ssl certificaate in AWS service
-> add ssl annotattions on yml file

HEML
=====
Helm is a Kubernetes deployment tool for automating creation, packaging, configuration, and deployment of applications and services to Kubernetes clusters.

EXTERNAL NAME:
==============
To access externally hosted apps in k8s cluster (Example: Access AWS RDS Database endpoint by application present inside k8s cluster)

SETUP CI/CD PIPELINES FOR SPRINGBOOT APP USING JENKINS AND AWS EKS:

-> create amaz ec2 t2.medium server for setup
jenkins,maven and docker
and install maven and dokcer pipeline plugin
-> Launch a new t2.micro k8s manage server and install kubectl
-> create a role and attach to managed ec2
-> now create eks cluster using k8s management host with kubectl
-> Setup maven, java, and dockerhub credentials in jenkins server
-> Metric server is imp for HPA,
if you configure pod HPA without metric server it will not work

IMP interview questions:
-> what is some usecases in k8s
    1. microservice is one of the usecase, but for api access need to use api gateway
    but api gateway can communicate to eks with the help of ELB/NLB but
     not directly froom api gateway to EKS
    2. Jenkins on EKS
    3.GITLAB on EKS
    4. EMR on EKS
-> diff b/w deamonset and sidecar
     deamonsets arr used to some/all pods are scheduled and running on available node
     it runs copy of desired pods, whenever new node created all pods will be created automatically
     SIdecar - another container running within same pod alongside your app container
     diff in ip behaviour
-> diff b/w secret and configmap
A, compare both in above
-> why we use EKS rather than kubeadm VM
      EKS cluster is setup on diff availability zones automatically, no need to setup in 3 az's manually
      thats why it gives HA and scalability
      if you manage ec2 vms k8s cluster, need to maintain and setup cluster in all azs with proper ASG

      If no.of worker nodes are increased in future need to increase your ec2 instance type config but with eks there is no problem like this

      EKS gives one clieck version upgrade without app downtime

      what feature do you add in EKS ?
         metric server
-> etcd ?
    read theory
-> how do i take backup of k8s cluster
      generally we take 2 type of backups in k8s
      1.object , 2.configmap 3.app data store in Persistent Volume
      take back ups via k8s snapshot API
      ebs driver
      external tools - portworx, velero, kasten
-> k8s network policy ? how  is it diff than service mesh ?
       np is control the network control flow of 3 tier arch
       service mesh is works on layer 7 np works on layer 3,4
       service mesh has some add features like throttling,security, proxy to proxy,
        tracing etc...
-> diff b/w ELB and Ingress
learn from ppt
-> tell me some k8s tools
    logging - fulentd and fluentbit
    monitoring - datadog, newrelic
    cost - kubecost, cloudhealth
    storage - portworx, velero
    security - aqua, twistlock
    devops - jenkins, gitlab
-> k8s security best practices
      create minimal images,
      1.reduce packaging and reduce attack surface
      2.use muli stage builds to reduce size
      3.run dynamic scans on pods for vulenelabirities
      4.RBAC - specify seperate roles for seperate groups(admin,dev, test wtc....)
      5.use n/p to control pod traffic, control traffic by ip, label, namespaces
-> k8s archietecture explain
-> k8s operator? how it is diff from controller
-> what uese cases are not good in k8s
    dont use lambda cron job,
    simple ms, like alb, apiGW with ec2, Lambda
    event driven like SQS, Eventbridge, AMAZON MQ
-> k8s app devops flow/automate k8s app ?
   udemy course vr sham-nkar
-> How do you scale k8s ?
      HPA, cluster autoscaler

-> Why should i choose EKS ?
    it supports 4 versions of k8s
    this is certified versions of k8s
    you can easily run large scale enterprise apps
-> high-level workflow of k8s
      nothing but devops flow
-> What is eks add-ons ? name some

