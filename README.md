# Kubernetes-Service-
Summary of the concept of Kubernetes Services





DAY 35
Kubernetes Service

Here’s a concise summary of the concept of Kubernetes Services based on your detailed transcript:

⸻

Summary: Understanding Kubernetes Services

In Kubernetes, a Service is a critical component that enables reliable communication between applications and ensures stable access to Pods—despite their dynamic nature.

Why Services Are Needed
	•	Pods are ephemeral and may be recreated at any time (e.g., when a pod crashes).
	•	When recreated, pods receive new IP addresses, breaking direct communication links.
	•	In production, you usually deploy Deployments, which create ReplicaSets, which then manage multiple Pods for scalability and fault tolerance.

Problems Without Services
	•	Sharing dynamic pod IPs with users or other services is unreliable.
	•	Auto-healing features restore Pods, but their IPs change, breaking client connections.
	•	Real-world apps (e.g., Google) do not expose IPs directly but instead use load balancers with stable endpoints.

What Kubernetes Services Do
	•	A Service provides a stable IP and DNS name to access a set of Pods.
	•	It acts as a load balancer, distributing traffic evenly across all healthy Pods.
	•	Services use labels to dynamically discover and target the right Pods.
	•	Internally, Services rely on components like kube-proxy to route traffic.

Analogy

Think of a Service like a reception desk in a large office:
	•	Visitors don’t go directly to individual employees (pods), whose desks (IP addresses) may change.
	•	Instead, they go to the reception (Service), which always knows who is available and where they’re seated.



Kubernetes Services – Part 2: Service Discovery and Label-Based Routing

In addition to load balancing, Kubernetes Services also solve the critical problem of service discovery—ensuring that applications can always reach the correct Pods, even when they are dynamically recreated.

Problem Without Service Discovery
	•	Pods can be destroyed and recreated, resulting in changing IP addresses.
	•	Even if a Service is routing traffic, if it relies on Pod IPs, it would also fail when those IPs change.
	•	Manually tracking Pod IPs is not scalable (especially with dozens or hundreds of Pods).

Solution: Labels and Selectors
	•	Kubernetes uses labels (key-value tags) on Pods and selectors in Services.
	•	Instead of keeping track of IPs, Services route traffic to Pods matching a specific label.
	•	Even if a Pod dies and a new one is created with a new IP, as long as it has the same label, the Service will discover it automatically.

Example
	•	You label your Pods with app: payment.
	•	The Service uses a selector like selector: app: payment.
	•	Now, the Service always targets Pods with that label—regardless of their IP addresses or how many times they restart.

DNS Naming in Kubernetes
	•	Each Service gets a stable DNS name like:
payment.default.svc.cluster.local
	•	payment: Service name
	•	default: Namespace
	•	svc: Indicates it’s a service
	•	Applications or users connect using this DNS name instead of an IP.

Summary of Benefits of Kubernetes Services
	1.	Stable Networking – Pods can change, but the Service IP or DNS remains the same.
	2.	Load Balancing – Evenly distributes traffic across healthy Pods.
	3.	Service Discovery – Automatically finds Pods using labels/selectors, not IPs.

⸻




Kubernetes Services – Part 3: Types of Services and Access Control

After understanding load balancing and service discovery, it’s crucial to know how users can access your applications based on the type of service you create in Kubernetes.

1. ClusterIP (Default)
	•	Scope: Internal-only
	•	Access: Only accessible from within the Kubernetes cluster.
	•	Use case: Backend microservices (e.g., databases or internal APIs) that shouldn’t be exposed to the public.
	•	Example: A billing service only accessed by other services within the cluster.

2. NodePort
	•	Scope: Internal + External (via Node IP)
	•	Access: Exposes the service on a static port (e.g., 30080) on each node’s IP.
	•	Use case: Quick and basic way to expose applications outside the cluster for testing or internal networks.
	•	Note: User must know the worker node IP to access the app.
	•	Example: Internal web app available to employees over the network.

3. LoadBalancer
	•	Scope: Public (Cloud-native)
	•	Access: Requests go through a cloud provider’s load balancer, which provides a public IP.
	•	Use case: Applications meant to be accessed by anyone over the internet.
	•	Component involved: Cloud Controller Manager interacts with the cloud provider (e.g., AWS, GCP) to provision the load balancer.
	•	Example: Public-facing site like Amazon.com.

⸻

Key Takeaways:
	•	Kubernetes Services provide three major benefits:
	1.	Load Balancing – distribute traffic across multiple Pods.
	2.	Service Discovery – automatically locate Pods using labels and selectors, not IPs.
	3.	Access Control / Exposure – expose applications based on Service type.
	•	Choosing the right Service type depends on your use case:
	•	Use ClusterIP for internal communication.
	•	Use NodePort for limited external access.
	•	Use LoadBalancer for public access.

Would you like a YAML example of how to create a Kubernetes Service for a Deployment?


Service Advantages
1. Load Balancer
2. Service discovery
3. Exposing application to the world

3 different types of service doeAccess 
1. Cluster IP
2. Load balancer 
3. Node port


