2 approaches for controlling networks (computing forward and flow tables)
1) **Per-router control**
	1) Routing algo present in each and every router
	2) Each router has routing component that communicates with routing components in other routers
2) **Logically Centralized Control**
	1) Logically centralized controller computes and distributes forwarding table
	2) Controller interacts with control agent (CA) in each router to configure and manage that router's flow table
	3) CA has limited functionality: job is to communicate with controller and do as controller commands