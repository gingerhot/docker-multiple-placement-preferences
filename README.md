# docker-multiple-placement-preferences
This establishes a hierarchy of preferences, so that tasks are first divided over one category, and then further divided over additional categories. One example of where this may be useful is dividing tasks fairly between datacenters, and then splitting the tasks within each datacenter over a choice of racks. To add multiple placement preferences, specify the --placement-pref flag multiple times. The order is significant, and the placement preferences will be applied in the order given when making scheduling decisions.  The following example sets up a service with multiple placement preferences. Tasks are spread first over the various datacenters, and then over racks (as indicated by the respective labels):


###
```
Step1 : Create 6 worker nodes and one manager node

Step2 : Join all nodes into docker swarm mode

Step3: Divide the worker nodes into three different zones
  
  
| S.No | Node Name               | ZONE  | Labels                                   |
|------|-------------------------|-------|------------------------------------------|
| 1    | Worker1 Worker2 Worker3 | dmz1  | Worker1,worker2 (web) worker3 (database) |
| 2    | Worker4 Worker5         | dmz2  | Worker4,Worker5 (web)                    |
| 3    | Worker6 Worker7         | dmz3  | Worker6 (web) Worker7 (db)               |


Step4: 

Update all the nodes with the corresponding zone values

       docker node update --label-add zone=1 worker1
       docker node update --label-add zone=1 worker2
       docker node update --label-add zone=1 worker3
       docker node update --label-add zone=2 worker4
       docker node update --label-add zone=2 worker5
       docker node update --label-add zone=3 worker6
       docker node update --label-add zone=3 worker7
       
Step5:

Update all the nodes with the corresponding server modes

     docker node update --label-add server.mode=web worker1
     docker node update --label-add server.mode=web worker2
     docker node update --label-add server.mode=db worker3
     docker node update --label-add server.mode=web worker4
     docker node update --label-add server.mode=web worker5
     docker node update --label-add server.mode=web worker6
     docker node update --label-add server.mode=db worker7

```
